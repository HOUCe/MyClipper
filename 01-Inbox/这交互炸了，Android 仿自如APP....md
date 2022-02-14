# 这交互炸了，Android 仿自如APP裸眼 3D 效果 OpenGL 版

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650840886&idx=1&sn=3df032ef654a1f7a382c88aa3c41b9ec&chksm=80b74ba8b7c0c2be08e2c93a906808624a4a7194a46b840168200aaa0f26e209c087be8060e2&mpshare=1&scene=1&srcid=1207NAhtAhfuKOISMaHAiDuf&sharer_sharetime=1638828268969&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)却把清梅嗅 鸿洋

本文作者

作者：**却把清梅嗅**

链接：

https://juejin.cn/post/7035645207278256165

本文由作者授权发布。

之前自如系列各个版本：

[自如App裸眼3D效果最近火爆了，各个版本齐了~](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650838915&idx=1&sn=68cfa06193d7903d3028c69efcb7acb9&chksm=80b7431db7c0ca0b8c3112d44e469b868be82b3323a3af975aaca9069334a5392d4a9ed6c1ee&scene=21#wechat_redirect)-

_1_

概述

之前看到 自如团队 发布的 自如客APP裸眼3D效果的实现 ，非常有趣，不久后，社区内 Android 的开发者们陆续提供了 Flutter、 Android 原生 、Android Jetpack Compose 等不同的实现版本。

_自如客APP裸眼3D效果的实现_

_https://juejin.cn/post/6989227733410644005_

_Flutter 版本：_

_https://juejin.cn/post/6991409083765129229_

_Android 原生_

_https://juejin.cn/post/6991840263362576421_

_Android Jetpack Compose_

_https://juejin.cn/post/6992169168938205191_

很快我看到了一个好玩的评论：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibM3RWpib6KmVTECg3fW2myibrxn6QAj2cVPTbQmcgVibKrRKdMXfUWoEeA%2F640%3Fwx_fmt%3Dother)

既然客户端都卷成这样了，干脆破罐破摔，把 Android OpenGL 的实现版本也补齐，毕竟 图形学或许会迟到，但绝不会缺席 。

实现效果如下（图片来源），这一波属实参与到社区内裸眼3D的 客户端大满贯 了 ：

_https://juejin.cn/post/6991409083765129229_

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_gif%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibONxL3eQoqkpILabGj3HfA8KaFto2D06OCsb5CQyuuyoiaGu2HXc51hQ%2F640%3Fwx_fmt%3Dgif)

_2_

原理简介 & OpenGL 的优势

> 裸眼 3D 原理其它文章都拆解非常清晰了，本着不重复造轮子的原则，这里引用 Nayuta 和 付十一 文章中的部分内容，再次感谢。
> 
> _https://juejin.cn/post/6991409083765129229_
> 
> _https://juejin.cn/post/6992169168938205191_

裸眼 3D 效果的本质是——将整个图片结构分为 3 层：上层、中层、以及底层。在手机左右上下旋转时，上层和底层的图片呈相反的方向进行移动，中层则不动，在视觉上给人一种 3D 的感觉：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibewjBNXmvcbDEicGt2ibFBOWv9uNZkTj7SUpYsibMeib5tDGfYeH0MkeQ9w%2F640%3Fwx_fmt%3Dother)

也就是说效果是由以下三张图构成的：

前景

中景(文字是白色的)

背景

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibTibMJicYLUBM0wf4E0fVRAt31ce14ppVsz23DHCamSLGNVBJujzD8ruA%2F640%3Fwx_fmt%3Dother)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibRdQSiaicjgrdcZw6WfVE6hQKeglzhqDBPicSheVyuXl7f5LBic2xygNMbg%2F640%3Fwx_fmt%3Dother)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibT2yJbdtzWSBkWKj3piaPGYaDJ1Cbs2txv3UJFkF5x2ELP3t6Hqvokrw%2F640%3Fwx_fmt%3Dother)

接下来，如何感应手机的旋转状态，并将三层图片进行对应的移动呢？当然是使用设备自身提供各种各样优秀的传感器了，通过传感器不断回调获取设备的旋转状态，对 UI 进行对应地渲染即可。

笔者最终选择了 Android 平台上的 OpenGL API 进行渲染，直接的原因是，无需将社区内已有的实现方案重复照搬。

另一个重要的原因是，GPU 更适合图形、图像的处理，裸眼3D效果中有大量的缩放和位移操作，都可在 java 层通过一个 矩阵 对几何变换进行描述，通过 shader 小程序中交给 GPU 处理 ——因此，理论上 OpenGL 的渲染性能比其它几个方案更好一些。

> 本文重点是描述 OpenGL 绘制时的思路描述，因此下文仅展示部分核心代码，对具体实现感兴趣的读者可参考文末的链接。

_3_

具体实现

### **1\. 绘制静态图片**

首先需要将3张图片依次进行静态绘制，这里涉及大量 OpenGL API 的使用，不熟悉的读可略读本小节，以捋清思路为主。

首先看一下顶点和片元着色器的 shader 代码，其定义了图像纹理是如何在GPU中处理渲染的：

    // 顶点着色器代码// 顶点坐标attribute vec4 av_Position;// 纹理坐标attribute vec2 af_Position;uniform mat4 u_Matrix;varying vec2 v_texPo;void main() {    v_texPo = af_Position;    gl_Position =  u_Matrix * av_Position;}

-

    // 顶点着色器代码// 顶点坐标attribute vec4 av_Position;// 纹理坐标attribute vec2 af_Position;uniform mat4 u_Matrix;varying vec2 v_texPo;void main() {    v_texPo = af_Position;    gl_Position =  u_Matrix * av_Position;}

-

定义好了 Shader ，接下来在 GLSurfaceView (可以理解为 OpenGL 中的画布) 创建时，初始化Shader小程序，并将图像纹理依次加载到GPU中：

    public class My3DRenderer implements GLSurfaceView.Renderer {  @Override  public void onSurfaceCreated(GL10 gl, EGLConfig config) {      // 1.加载shader小程序      mProgram = loadShaderWithResource(              mContext,              R.raw.projection_vertex_shader,              R.raw.projection_fragment_shader      );      // ...       // 2. 依次将3张切图纹理传入GPU      this.texImageInner(R.drawable.bg_3d_back, mBackTextureId);      this.texImageInner(R.drawable.bg_3d_mid, mMidTextureId);      this.texImageInner(R.drawable.bg_3d_fore, mFrontTextureId);  }}

-

接下来是定义视口的大小，因为是2D图像变换，且切图和手机屏幕的宽高比基本一致，因此简单定义一个单位矩阵的正交投影即可：

    public class My3DRenderer implements GLSurfaceView.Renderer {    // 投影矩阵    private float[] mProjectionMatrix = new float[16];    @Override    public void onSurfaceChanged(GL10 gl, int width, int height) {        // 设置视口大小，这里设置全屏        GLES20.glViewport(0, 0, width, height);        // 图像和屏幕宽高比基本一致，简化处理，使用一个单位矩阵        Matrix.setIdentityM(mProjectionMatrix, 0);    }}

-

最后就是绘制，读者需要理解，对于前、中、后三层图像的渲染，其逻辑是基本一致的，差异仅仅有2点：图像本身不同 以及 图像的几何变换不同。

    public class My3DRenderer implements GLSurfaceView.Renderer {    private float[] mBackMatrix = new float[16];    private float[] mMidMatrix = new float[16];    private float[] mFrontMatrix = new float[16];    @Override    public void onDrawFrame(GL10 gl) {        GLES20.glClear(GLES20.GL_COLOR_BUFFER_BIT);        GLES20.glClearColor(0.0f, 0.0f, 0.0f, 1.0f);        GLES20.glUseProgram(mProgram);        // 依次绘制背景、中景、前景        this.drawLayerInner(mBackTextureId, mTextureBuffer, mBackMatrix);        this.drawLayerInner(mMidTextureId, mTextureBuffer, mMidMatrix);        this.drawLayerInner(mFrontTextureId, mTextureBuffer, mFrontMatrix);    }    private void drawLayerInner(int textureId, FloatBuffer textureBuffer, float[] matrix) {        // 1.绑定图像纹理        GLES20.glBindTexture(GLES20.GL_TEXTURE_2D, textureId);        // 2.矩阵变换        GLES20.glUniformMatrix4fv(uMatrixLocation, 1, false, matrix, 0);        // ...        // 3.执行绘制        GLES20.glDrawArrays(GLES20.GL_TRIANGLE_STRIP, 0, 4);    }}

-

参考 drawLayerInner 的代码，其用于绘制单层的图像，其中 textureId 参数对应不同图像，matrix 参数对应不同的几何变换。

现在我们完成了图像静态的绘制，效果如下：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibP78bJVpHS63fda9RMmyz7Zurnzao37UdFeFGy0dEBpiah4jfAwGBejg%2F640%3Fwx_fmt%3Dother)

接下来我们需要接入传感器，并定义不同层级图片各自的几何变换，让图片动起来。

### **2\. 让图片动起来**

首先我们需要对 Android 平台上的传感器进行注册，监听手机的旋转状态，并拿到手机 xy 轴的旋转角度。

    // 2.1 注册传感器mSensorManager = (SensorManager) context.getSystemService(Context.SENSOR_SERVICE);mAcceleSensor = mSensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER);mMagneticSensor = mSensorManager.getDefaultSensor(Sensor.TYPE_MAGNETIC_FIELD);mSensorManager.registerListener(mSensorEventListener, mAcceleSensor, SensorManager.SENSOR_DELAY_GAME);mSensorManager.registerListener(mSensorEventListener, mMagneticSensor, SensorManager.SENSOR_DELAY_GAME);// 2.2 不断接受旋转状态private final SensorEventListener mSensorEventListener = new SensorEventListener() {    @Override    public void onSensorChanged(SensorEvent event) {        // ... 省略具体代码        float[] values = new float[3];        float[] R = new float[9];        SensorManager.getRotationMatrix(R, null, mAcceleValues, mMageneticValues);        SensorManager.getOrientation(R, values);        // x轴的偏转角度        float degreeX = (float) Math.toDegrees(values[1]);        // y轴的偏转角度        float degreeY = (float) Math.toDegrees(values[2]);        // z轴的偏转角度        float degreeZ = (float) Math.toDegrees(values[0]);        // 拿到 xy 轴的旋转角度，进行矩阵变换        updateMatrix(degreeX, degreeY);    }};

-

注意，因为我们只需控制图像的左右和上下移动，因此，我们只需关注设备本身 x 轴和 y 轴的偏转角度：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvib8sB2Ulpic4OcBVwKiaOEvLeDD3IQibxp02qlWx5NHyNOMc8JcwicDLxqLQ%2F640%3Fwx_fmt%3Dother)

拿到了 x 轴和 y 轴的偏转角度后，接下来开始定义图像的位移了。

但如果将图片直接进行位移操作，将会因为位移后图像的另一侧没有纹理数据，导致渲染结果有黑边现象，为了避免这个问题，我们需要将图像默认从中心点进行放大，保证图像移动的过程中，不会超出自身的边界。

也就是说，我们一开始进入时，看到的肯定只是图片的部分区域。给每一个图层设置 scale，将图片进行放大。显示窗口是固定的，那么一开始只能看到图片的正中位置。（中层可以不用，因为中层本身是不移动的，所以也不必放大)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibkm6j4pOlx2wricxdzPvebg8vxUrOs5110c15ZlA7PnJfQS6FDhOw8Zw%2F640%3Fwx_fmt%3Dother)

> 这里的处理参考自 Nayuta 的 这篇文章，内部已经将思路阐述的非常清晰，强烈建议读者进行阅读。
> 
> _https://juejin.cn/post/6991409083765129229#heading-4_

明白了这一点，我们就能理解，裸眼 3D 的效果实际上就是对 不同层级的图像 进行 缩放 和 位移 的变换，下面是分别获取几何变换的代码：

    public class My3DRenderer implements GLSurfaceView.Renderer {    private float[] mBackMatrix = new float[16];    private float[] mMidMatrix = new float[16];    private float[] mFrontMatrix = new float[16];    /**     * 陀螺仪数据回调，更新各个层级的变换矩阵.     *     * @param degreeX x轴旋转角度，图片应该上下移动     * @param degreeY y轴旋转角度，图片应该左右移动     */    private void updateMatrix(@FloatRange(from = -180.0f, to = 180.0f) float degreeX,                              @FloatRange(from = -180.0f, to = 180.0f) float degreeY) {        // ... 其它处理                                                        // 背景变换        // 1.最大位移量        float maxTransXY = MAX_VISIBLE_SIDE_BACKGROUND - 1f;        // 2.本次的位移量        float transX = ((maxTransXY) / MAX_TRANS_DEGREE_Y) * -degreeY;        float transY = ((maxTransXY) / MAX_TRANS_DEGREE_X) * -degreeX;        float[] backMatrix = new float[16];        Matrix.setIdentityM(backMatrix, 0);        Matrix.translateM(backMatrix, 0, transX, transY, 0f);                    // 2.平移        Matrix.scaleM(backMatrix, 0, SCALE_BACK_GROUND, SCALE_BACK_GROUND, 1f);  // 1.缩放        Matrix.multiplyMM(mBackMatrix, 0, mProjectionMatrix, 0, backMatrix, 0);  // 3.正交投影        // 中景变换        Matrix.setIdentityM(mMidMatrix, 0);        // 前景变换        // 1.最大位移量        maxTransXY = MAX_VISIBLE_SIDE_FOREGROUND - 1f;        // 2.本次的位移量        transX = ((maxTransXY) / MAX_TRANS_DEGREE_Y) * -degreeY;        transY = ((maxTransXY) / MAX_TRANS_DEGREE_X) * -degreeX;        float[] frontMatrix = new float[16];        Matrix.setIdentityM(frontMatrix, 0);        Matrix.translateM(frontMatrix, 0, -transX, -transY - 0.10f, 0f);            // 2.平移        Matrix.scaleM(frontMatrix, 0, SCALE_FORE_GROUND, SCALE_FORE_GROUND, 1f);    // 1.缩放        Matrix.multiplyMM(mFrontMatrix, 0, mProjectionMatrix, 0, frontMatrix, 0);  // 3.正交投影    }}

-

这段代码中还有几点细节需要处理。

### **3\. 几个反直觉的细节**

#### **3.1 旋转方向 ≠ 位移方向**

首先，设备旋转方向和图片的位移方向是相反的，举例来说，当设备沿 X 轴旋转，对于用户而言，对应前后景的图片应该上下移动，反过来，设备沿 Y 轴旋转，图片应该左右移动（没太明白的同学可参考上文中陀螺仪的图片加深理解）：

    // 设备旋转方向和图片的位移方向是相反的float transX = ((maxTransXY) / MAX_TRANS_DEGREE_Y) * -degreeY;float transY = ((maxTransXY) / MAX_TRANS_DEGREE_X) * -degreeX;// ...Matrix.translateM(backMatrix, 0, transX, transY, 0f); 

-

#### **3.2 默认旋转角度 ≠ 0°**

其次，在定义最大旋转角度的时候，不能主观认为旋转角度 = 0°是默认值。什么意思呢？Y 轴旋转角度为0°，即 degreeY = 0 时，默认设备左右的高度差是 0，这个符合用户的使用习惯，相对易于理解，因此，我们可以定义左右的最大旋转角度，比如 Y ∈ (-45°，45°)，超过这两个旋转角度，图片也就移动到边缘了。

但当 X 轴旋转角度为0°，即 degreeX = 0 时，意味着设备上下的高度差是 0，你可以理解为设备是放在水平的桌面上的，这个绝不符合大多数用户的使用习惯，相比之下，设备屏幕平行于人的面部 才更适用大多数场景（degreeX = -90）：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvib1p0qYX1BJB68PHhSalwbQ45GkuNjSAkDicpQbHibDICp2GUlaBdicAy2A%2F640%3Fwx_fmt%3Dother)

因此，代码上需对 X、Y 轴的最大旋转角度区间进行分开定义：

    private static final float USER_X_AXIS_STANDARD = -45f;private static final float MAX_TRANS_DEGREE_X = 25f;   // X轴最大旋转角度 ∈ (-20°，-70°)private static final float USER_Y_AXIS_STANDARD = 0f;private static final float MAX_TRANS_DEGREE_Y = 45f;   // Y轴最大旋转角度 ∈ (-45°，45°)

-

解决了这些 反直觉 的细节问题，我们基本完成了裸眼3D的效果。

### **4\. 帕金森综合征？**

还差一点就大功告成了，最后还需要处理下3D效果抖动的问题：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_gif%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibo2twhRwSYRGjASmdE15b6yJOYVsDpB82T16d5cvOZQHnyJiaL79bAAg%2F640%3Fwx_fmt%3Dgif)

如图，由于传感器过于灵敏，即使平稳的握住设备，XYZ 三个方向上微弱的变化都会影响到用户的实际体验，会给用户带来 帕金森综合征 的自我怀疑。

解决这个问题，传统的 OpenGL 以及 Android API 似乎都无能为力，好在 GitHub 上有人提供了另外一个思路。

熟悉信号处理的同学比较了解，为了通过剔除短期波动、保留长期发展趋势提供了信号的平滑形式，可以使用 低通滤波器，保证低于截止频率的信号可以通过，高于截止频率的信号不能通过。

因此有人建立了 这个仓库 ， 通过对 Android 传感器追加低通滤波 ，过滤掉小的噪声信号，达到较为平稳的效果：

_https://github.com/Bhide/Low-Pass-Filter-To-Android-Sensors_

    private final SensorEventListener mSensorEventListener = new SensorEventListener() {    @Override    public void onSensorChanged(SensorEvent event) {        // 对传感器的数据追加低通滤波        if (event.sensor.getType() == Sensor.TYPE_ACCELEROMETER) {            mAcceleValues = lowPass(event.values.clone(), mAcceleValues);        }        if (event.sensor.getType() == Sensor.TYPE_MAGNETIC_FIELD) {            mMageneticValues = lowPass(event.values.clone(), mMageneticValues);        }        // ... 省略具体代码        // x轴的偏转角度        float degreeX = (float) Math.toDegrees(values[1]);        // y轴的偏转角度        float degreeY = (float) Math.toDegrees(values[2]);        // z轴的偏转角度        float degreeZ = (float) Math.toDegrees(values[0]);        // 拿到 xy 轴的旋转角度，进行矩阵变换        updateMatrix(degreeX, degreeY);    }};

-

大功告成，最终我们实现了预期的效果：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_gif%2FMOu2ZNAwZwNoQrINiaHfuicDJY7R4pyZvibSicY8ka4tT57QvVyBr9BSXGnHicjREeYI1AWibSsTh8iaSHkZH6y9Z43RQ%2F640%3Fwx_fmt%3Dgif)

## **源码地址**

本文所有源码，请查看 这里 。

_https://github.com/qingmei2/OpenGL-demo/blob/master/app/src/main/java/com/github/qingmei2/opengl\_demo/c\_image\_process/processor/C06Image3DProcessor.java_

## _**参考**_

_最后是本文中提到的相关资料，再次感谢先驱者的付出实践。_

_自如客APP裸眼3D效果的实现 @自如大前端团队_

_https://juejin.cn/post/6989227733410644005_

_拿去吧你！Flutter 仿自如 App 裸眼 3D 效果 @Nayuta-
_

_https://juejin.cn/post/6991409083765129229_

_Compose版来啦！仿自如裸眼3D效果 @付十一_

_https://juejin.cn/post/6992169168938205191_

_GitHub: Low-Pass-Filter-To-Android-Sensors_

_https://github.com/Bhide/Low-Pass-Filter-To-Android-Sensors_

* * *

最后推荐一下我做的网站，玩Android: _wanandroid.com_ ，包含详尽的知识体系、好用的工具，还有本公众号文章合集，欢迎体验和收藏！

推荐阅读：

[Android 编译速度提升黑科技 - RocketX](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650840783&idx=1&sn=c844aaf40dc2d87535a0f4f81677110f&chksm=80b74a51b7c0c347c784f4d332e06fb539b2c7a550531df7ebee00e03c371ca2ef8c1a569330&scene=21#wechat_redirect)-

[Android 基础架构组，面试题问什么？](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650840709&idx=1&sn=ac368ae7a3dd3a178d8737a548ff6c54&chksm=80b74a1bb7c0c30d444753576eb0a755182cd57f01391eea0bd270932179dd5141eda5067c3f&scene=21#wechat_redirect)-

[Android 无所不能的 hook，让应用不再崩溃](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650840653&idx=1&sn=b862759352e0f8bb7c2675006d2e63af&chksm=80b74ad3b7c0c3c547f18e2a50571695b0f9568e6b9c5d995d468d9f140410cd4b9d70b3ad59&scene=21#wechat_redirect)-

**点击** 关注我的公众号-

如果你想要跟大家分享你的文章，欢迎投稿~

┏(＾0＾)┛明天见！

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650840886&idx=1&sn=3df032ef654a1f7a382c88aa3c41b9ec&chksm=80b74ba8b7c0c2be08e2c93a906808624a4a7194a46b840168200aaa0f26e209c087be8060e2&mpshare=1&scene=1&srcid=1207NAhtAhfuKOISMaHAiDuf&sharer_sharetime=1638828268969&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)