# 从0手撸一个RxJava

[zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/440362959?utm_source=wechat_session&utm_medium=social&utm_oi=38400975437824&utm_campaign=shareopn)不上班行不行代码搬运工

### 前言

rxjava 可以很方便的进行线程切换， 那么rxjava是如何进行线程切换的呢？阅读本文可以了解下rxjava 是如何进行线程切换的及线程切换的影响点。

* * *

### 一个简单的代码：

    Observable.create(new ObservableOnSubscribe<String>() {
        @Override
        public void subscribe(ObservableEmitter<String> e) throws Exception {
            Log.d("WanRxjava ", "subscrib  td ==" + Thread.currentThread().getName());
            e.onNext("我在发送next");
            e.onComplete();
        }
    }).subscribeOn(Schedulers.io())
            .observeOn(AndroidSchedulers.mainThread())
            .subscribe(new Observer<String>() {
                @Override
                public void onSubscribe(Disposable d) {
                    Log.d("WanRxjava ", "onSubscribe td ==" + Thread.currentThread().getName());
                }
    
                @Override
                public void onNext(String value) {
                    Log.d("WanRxjava ", "onNext td ==" + Thread.currentThread().getName());
                }
    
                @Override
                public void onError(Throwable e) {
    
                }
    
                @Override
                public void onComplete() {
                    Log.d("WanRxjava ", "onComplete td ==" + Thread.currentThread().getName());
                }
            });

如上代码，实现了线程切换和观察者被观察者绑定的逻辑。我们分四部分看上述代码逻辑create、subscribeOn、observeOn、subscribe

-
**1.create**-
create 顾名思议是 创建被观察者，这里有一个参数是 ObservableOnSubscribe，这是个接口类，我们看下create 的源码：-

    @SchedulerSupport(SchedulerSupport.NONE)
    public static <T> Observable<T> create(ObservableOnSubscribe<T> source) {
        ObjectHelper.requireNonNull(source, "source is null");
        return RxJavaPlugins.onAssembly(new ObservableCreate<T>(source));
    }

将ObservableOnSubscribe 传入后 又调用了 new ObservableCreate(source)

    public final class ObservableCreate<T> extends Observable<T> {
        final ObservableOnSubscribe<T> source;
    
        public ObservableCreate(ObservableOnSubscribe<T> source) {
            this.source = source;
        }
    }

ObservableCreate 有一个变量是 source,这里只是将传入的ObservableOnSubscribe 赋值给source,也就是做了一层包装，然后返回。

-
**2.subscribeOn**-
调用完create后返回了 ObservableCreate（Observable），然后继续调用subscribeOn,传入了一个变量 [http://Schedulers.io()](https://link.zhihu.com/?target=http%3A//Schedulers.io%28%29)

    @SchedulerSupport(SchedulerSupport.CUSTOM)
    public final Observable<T> subscribeOn(Scheduler scheduler) {
        ObjectHelper.requireNonNull(scheduler, "scheduler is null");
        return RxJavaPlugins.onAssembly(new ObservableSubscribeOn<T>(this, scheduler));
    }

我们看到调用了new ObservableSubscribeOn(this, scheduler) 将自身和 scheduler 传入

    public final class ObservableSubscribeOn<T> extends AbstractObservableWithUpstream<T, T> {
        final Scheduler scheduler;
    
        public ObservableSubscribeOn(ObservableSource<T> source, Scheduler scheduler) {
            super(source);
            this.scheduler = scheduler;
        }
    }

ObservableSubscribeOn 将scheduler 和 create 返回的对象又包装了一层 返回ObservableSubscribeOn

### 3.observeOn

有一个参数是 Scheduler

    @SchedulerSupport(SchedulerSupport.CUSTOM)
    public final Observable<T> observeOn(Scheduler scheduler) {
        return observeOn(scheduler, false, bufferSize());
    }
    @SchedulerSupport(SchedulerSupport.CUSTOM)
    public final Observable<T> observeOn(Scheduler scheduler, boolean delayError, int bufferSize) {
        ObjectHelper.requireNonNull(scheduler, "scheduler is null");
        ObjectHelper.verifyPositive(bufferSize, "bufferSize");
        return RxJavaPlugins.onAssembly(new ObservableObserveOn<T>(this, scheduler, delayError, bufferSize));
    }

ObservableSubscribeOn（observable）又调用了observeOn，然后调用了new ObservableObserveOn(this, scheduler, delayError, bufferSize)

    public final class ObservableObserveOn<T> extends AbstractObservableWithUpstream<T, T> {
        final Scheduler scheduler;
        final boolean delayError;
        final int bufferSize;
        public ObservableObserveOn(ObservableSource<T> source, Scheduler scheduler, boolean delayError, int bufferSize) {
            super(source);
            this.scheduler = scheduler;
            this.delayError = delayError;
            this.bufferSize = bufferSize;
        }
    }

又是一个包装,将ObservableSubscribeOn 和 scheduler 包装成 ObservableObserveOn

-
**4.subscribe**-
上述最后一步即调用ObservableObserveOn.subscribe,传入参数是一个 observer

    //ObservableObserveOn.java
    @SchedulerSupport(SchedulerSupport.NONE)
    @Override
    public final void subscribe(Observer<? super T> observer) {
        ObjectHelper.requireNonNull(observer, "observer is null");
        try {
            observer = RxJavaPlugins.onSubscribe(this, observer);
    
            ObjectHelper.requireNonNull(observer, "Plugin returned null Observer");
    
            subscribeActual(observer);
        } catch (NullPointerException e) { // NOPMD
            throw e;
        } catch (Throwable e) {
            Exceptions.throwIfFatal(e);
            // can't call onError because no way to know if a Disposable has been set or not
            // can't call onSubscribe because the call might have set a Subscription already
            RxJavaPlugins.onError(e);
    
            NullPointerException npe = new NullPointerException("Actually not, but can't throw other exceptions due to RS");
            npe.initCause(e);
            throw npe;
        }
    }

可以看到调用subscribe 后调用了subscribeActual(observer);将observer 传入

我们看下 subscribeActual(observer)

    //ObservableObserveOn.java
    @Override
    protected void subscribeActual(Observer<? super T> observer) {
        if (scheduler instanceof TrampolineScheduler) {
            source.subscribe(observer);
        } else {
            Scheduler.Worker w = scheduler.createWorker();
    
            source.subscribe(new ObserveOnObserver<T>(observer, w, delayError, bufferSize));
        }
    }

上面的if 先不管，主要看下下面的逻辑，调用了 scheduler.createWorker()，这个scheduler 是 observeOn 传入的，然后调用-
new ObserveOnObserver(observer, w, delayError, bufferSize)；将worker /observer 又做了一次包装。

    //ObservableObserveOn 内部类
    static final class ObserveOnObserver<T> extends BasicIntQueueDisposable<T>
    implements Observer<T>, Runnable {
    
        private static final long serialVersionUID = 6576896619930983584L;
        final Observer<? super T> actual;
        final Scheduler.Worker worker;
        final boolean delayError;
        final int bufferSize;
    
        SimpleQueue<T> queue;
    
        Disposable s;
    
        Throwable error;
        volatile boolean done;
    
        volatile boolean cancelled;
    
        int sourceMode;
    
        boolean outputFused;
    
        ObserveOnObserver(Observer<? super T> actual, Scheduler.Worker worker, boolean delayError, int bufferSize) {
            this.actual = actual;
            this.worker = worker;
            this.delayError = delayError;
            this.bufferSize = bufferSize;
        }
    
        @Override
        public void onSubscribe(Disposable s) {
            if (DisposableHelper.validate(this.s, s)) {
                this.s = s;
                if (s instanceof QueueDisposable) {
                    @SuppressWarnings("unchecked")
                    QueueDisposable<T> qd = (QueueDisposable<T>) s;
    
                    int m = qd.requestFusion(QueueDisposable.ANY | QueueDisposable.BOUNDARY);
    
                    if (m == QueueDisposable.SYNC) {
                        sourceMode = m;
                        queue = qd;
                        done = true;
                        actual.onSubscribe(this);
                        schedule();
                        return;
                    }
                    if (m == QueueDisposable.ASYNC) {
                        sourceMode = m;
                        queue = qd;
                        actual.onSubscribe(this);
                        return;
                    }
                }
    
                queue = new SpscLinkedArrayQueue<T>(bufferSize);
    
                actual.onSubscribe(this);
            }
        }
    
        @Override
        public void onNext(T t) {
            if (done) {
                return;
            }
    
            if (sourceMode != QueueDisposable.ASYNC) {
                queue.offer(t);
            }
            schedule();
        }
    
        @Override
        public void onError(Throwable t) {
            if (done) {
                RxJavaPlugins.onError(t);
                return;
            }
            error = t;
            done = true;
            schedule();
        }
    
        @Override
        public void onComplete() {
            if (done) {
                return;
            }
            done = true;
            schedule();
        }
    
        @Override
        public void dispose() {
            if (!cancelled) {
                cancelled = true;
                s.dispose();
                worker.dispose();
                if (getAndIncrement() == 0) {
                    queue.clear();
                }
            }
        }
    
        @Override
        public boolean isDisposed() {
            return cancelled;
        }
    
        void schedule
            () {
            if (getAndIncrement() == 0) {
                worker.schedule(this);
            }
        }
    
        void drainNormal() {
            int missed = 1;
    
            final SimpleQueue<T> q = queue;
            final Observer<? super T> a = actual;
    
            for (;;) {
                if (checkTerminated(done, q.isEmpty(), a)) {
                    return;
                }
    
                for (;;) {
                    boolean d = done;
                    T v;
    
                    try {
                        v = q.poll();
                    } catch (Throwable ex) {
                        Exceptions.throwIfFatal(ex);
                        s.dispose();
                        q.clear();
                        a.onError(ex);
                        return;
                    }
                    boolean empty = v == null;
    
                    if (checkTerminated(d, empty, a)) {
                        return;
                    }
    
                    if (empty) {
                        break;
                    }
    
                    a.onNext(v);
                }
    
                missed = addAndGet(-missed);
                if (missed == 0) {
                    break;
                }
            }
        }
    
        void drainFused() {
            int missed = 1;
    
            for (;;) {
                if (cancelled) {
                    return;
                }
    
                boolean d = done;
                Throwable ex = error;
    
                if (!delayError && d && ex != null) {
                    actual.onError(error);
                    worker.dispose();
                    return;
                }
    
                actual.onNext(null);
    
                if (d) {
                    ex = error;
                    if (ex != null) {
                        actual.onError(ex);
                    } else {
                        actual.onComplete();
                    }
                    worker.dispose();
                    return;
                }
    
                missed = addAndGet(-missed);
                if (missed == 0) {
                    break;
                }
            }
        }
    
        @Override
        public void run
            () {
            if (outputFused) {
                drainFused();
            } else {
                drainNormal();
            }
        }
    
        boolean checkTerminated(boolean d, boolean empty
            , Observer<? super T> a) {
            if (cancelled) {
                queue.clear();
                return true;
            }
            if (d) {
                Throwable e = error;
                if (delayError) {
                    if (empty) {
                        if (e != null) {
                            a.onError(e);
                        } else {
                            a.onComplete();
                        }
                        worker.dispose();
                        return true;
                    }
                } else {
                    if (e != null) {
                        queue.clear();
                        a.onError(e);
                        worker.dispose();
                        return true;
                    } else
                    if (empty) {
                        a.onComplete();
                        worker.dispose();
                        return true;
                    }
                }
            }
            return false;
        }
    
        @Override
        public int requestFusion
            (int mode) {
            if ((mode & ASYNC) != 0) {
                outputFused = true;
                return ASYNC;
            }
            return NONE;
        }
    
        @Override
        public T poll() throws Exception {
            return queue.poll();
        }
    
        @Override
        public void clear() {
            queue.clear();
        }
    
        @Override
        public boolean isEmpty() {
            return queue.isEmpty();
        }
    }

包装完ObserveOnObserver后，调用了[source.subscribe](https://www.zhihu.com/search?q=source.subscribe&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A440362959%7D) 这里的source 即ObservableSubscribeOn.subscribe，进而调用ObservableSubscribeOn.subscribeActual

    //ObservableSubscribeOn.java
    @Override
    public void subscribeActual(final Observer<? super T> s) {
        final SubscribeOnObserver<T> parent = new scheduler<T>(s);
    
        s.onSubscribe(parent);
    
        parent.setDisposable(scheduler.scheduleDirect(new Runnable() {
            @Override
            public void run() {
                source.subscribe(parent);
            }
        }));
    }
    
    static final class SubscribeOnObserver<T> extends AtomicReference<Disposable> implements Observer<T>, Disposable {
    
        private static final long serialVersionUID = 8094547886072529208L;
        final Observer<? super T> actual;
    
        final AtomicReference<Disposable> s;
    
        SubscribeOnObserver(Observer<? super T> actual) {
            this.actual = actual;
            this.s = new AtomicReference<Disposable>();
        }
    
        @Override
        public void onSubscribe(Disposable s) {
            DisposableHelper.setOnce(this.s, s);
        }
    
        @Override
        public void onNext(T t) {
            actual.onNext(t);
        }
    
        @Override
        public void onError(Throwable t) {
            actual.onError(t);
        }
    
        @Override
        public void onComplete() {
            actual.onComplete();
        }
    
        @Override
        public void dispose() {
            DisposableHelper.dispose(s);
            DisposableHelper.dispose(this);
        }
    
        @Override
        public boolean isDisposed() {
            return DisposableHelper.isDisposed(get());
        }
    
        void setDisposable(Disposable d) {
            DisposableHelper.setOnce(this, d);
        }
    }

ObservableSubscribeOn.subscribeActual

**-
首先将传入的观察者封装成 SubscribeOnObserver**

**-
然后触发了 onSubscribe,接着调用 scheduler.scheduleDirect(new Runnable() 这里的scheduler 是 subscribeOn 传入的**

**-
最后调用了 scheduler.setsetDisposable方法。**

-
我们看到 run 的方法体即source.subscribe(parent);这里的source 即 ObservableCreate（ObservableOnSubscribe），传入了observer,然后调用 observer的OnNext 和 OnComplete 方法。

-
**5.小结：**-
**a. 调用Observer.OnSubscribe 方法是 不受线程调度影响的-
b.subscribeOn 影响的是发送事件的线程-
c.observerOn 影响的是观察者处理接受数据的线程，如果没有调用observeOn 则不会进行包装成ObserveOnObserver，也就是说不会执行观察者的线程切换，和 发送者的线程一致-
d.多次调用subscribeOn切换线程，每次都会new ObservableSubscribeOn，触发事件发送时会往上调用，也就是第一次调用的subscribeOn传入的线程 会执行发送事件，后面的线程切换无效-
e.Observer.OnSubscribe 只会执行一次，因为调用DisposableHelper.setOnce(this.s, s)-
f.处理完onComplete 或者onError 后就不会再发出事件，因为被观察者发送完这两个事件后 就会调用disposed**

**相关视频推荐**

[【Android手写RxJava架构】04rxjava中的线程切换是如何介入原框架的](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV17S4y1R7P4%3Fspm_id_from%3D333.999.0.0)

[Android(安卓)开发零基础从入门到精通](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1wU4y1u7r9%3Fspm_id_from%3D333.999.0.0)

[查看原网页: zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/440362959?utm_source=wechat_session&utm_medium=social&utm_oi=38400975437824&utm_campaign=shareopn)