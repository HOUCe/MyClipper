# 跳转到wps查看文件_业大神的博客-CSDN博客

source:: https://blog.csdn.net/qq_34723470/article/details/84226839

  最近我的项目用到了需要从服务器下载一些附件，然后支持本地去查看这些文件，最后确定的实现办法是跳转到wps app进行浏览。之前我让另一个同事将这个功能写成了一个小demo ，然后我想起来了就拿着他的Demo看了一遍然后就修改代码自己做成一个工具类集成到自己的app里面了。

1、需要依赖jar包 和 拷贝一个so文件：

jar包和so文件地址：[https://download.csdn.net/download/qq\_34723470/10792809](https://download.csdn.net/download/qq_34723470/10792809 "垃圾下载站")  （我是想免费的，结果c[sdn](https://so.csdn.net/so/search?q=sdn)资源上传的时候必须要选择资源下载分数1-5分，，，，哎，我深深的能体会到这个没有积分的痛苦啊。）

  jar包 和so 文件我会压缩，然后免费放到csdn 的下载里面，后面会加上连接。将两者添加到项目里面完毕之后如我下面截图所示：

![](https://img-blog.csdnimg.cn/20181119092112680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzIzNDcw,size_16,color_FFFFFF,t_70)

就是我红色标记的两个框，至于jar 包的名字是我随便取的。

2、工具类代码:

```
import android.app.Activity;import android.app.AlertDialog;import android.content.Intent;import android.content.pm.PackageInfo;import android.content.pm.PackageManager;import android.support.v4.content.FileProvider;import java.io.IOException;import java.util.concurrent.TimeUnit;import io.reactivex.Observable;import io.reactivex.android.schedulers.AndroidSchedulers;import io.reactivex.schedulers.Schedulers;import 你的包名.CommonDialog;public class BrowseFileUtil {private static final String ALLOW_PDF = "pdf";private static final String ALLOW_PNG = "png";private static final String ALLOW_JPEG = "jpeg";private static final String ALLOW_JPG = "jpg";private static final String ALLOW_DOC = "doc";private static final String ALLOW_DOCS = "docs";private static final String ALLOW_DOCX = "docx";private static final String ALLOW_XLS = "xls";private static final String ALLOW_XLSX = "xlsx";private static final String ALLOW_PPT = "ppt";private static final String ALLOW_PPTX = "pptx";private static final String ALLOW_DWG = "dwg";private static final String ALLOW_ZIP = "zip";private static final String ALLOW_TXT = "txt";private static final String ALLOW_WPS = "wps";public static final String TYPE_WORD = "application/msword";public static final String TYPE_TXT = "text/plain";public static final String TYPE_EXCEL = "application/vnd.ms-excel";public static final String TYPE_PPT = "application/vnd.ms-powerpoint";public static final String TYPE_PDF = "application/pdf";public static final String TYPE_PIC = "image/jpeg";private static final String TYPE_CAD = "CAD";private static final String TYPE_ZIP = "application/x-zip-compressed";private static final String TYPE_WPS = "application/vnd.ms-works";private static final String WPS_PACKAGE_NAME = "cn.wps.moffice_eng";private static final String CAD_PACKAGE_NAME = "com.gstarmc.android_80";private static final String PROVIDER = "你的包名.fileprovider";private static final String INTENT_ACTION = "android.intent.action.VIEW";private static final String INTENT_CATEGORY = "android.intent.category.DEFAULT";private static final String WPS_ADDRESS = "0811837d75804af2b362d443f87eb4c4FriOct1214:44:46CST2018.apk";private static final String CAD_ADDRESS = "3ab3bc939d7e4a4e9ffb2fecade682eaFriOct2615:13:26CST2018.apk";private static final String TAG = "BrowseFileUtil";private static String allowFileType(String path) {if (null == path || path.isEmpty()) {            ToastUtil.newInstance().showToast("本地路径为空，无法进行浏览附件!");            hzm = path.substring(path.lastIndexOf(".") + 1);                ToastUtil.newInstance().showToast("识别文件后缀名失败，无法打开文件!");                ToastUtil.newInstance().showToast("暂时不支持该浏览当前类型( " + hzm + " )的文件");public static void browseFile(String path, Activity activity) {String type = allowFileType(path);if (type.equals(TYPE_CAD)) {            alertDownloadWps(activity, 1);                packageInfo = Visor.getApplicationContext().getPackageManager().getPackageInfo(WPS_PACKAGE_NAME, 0);if (packageInfo != null) {Intent intent = new Intent(INTENT_ACTION);                    intent.addCategory(INTENT_CATEGORY);                    intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);if (Build.VERSION.SDK_INT < 24) {                        uri = Uri.fromFile(new File(path));                        intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);                        uri = FileProvider.getUriForFile(activity, PROVIDER, new File(path));                    intent.setDataAndType(uri, type);                    activity.startActivity(intent);                    alertDownloadWps(activity, 0);            } catch (PackageManager.NameNotFoundException e) {                alertDownloadWps(activity, 0);private static void alertDownloadWps(Activity activity, int type) {        AlertDialog.Builder builder = new AlertDialog.Builder(activity);        builder.setMessage(type == 0 ? "当前手机没有安装wps，是否去下载(支持内网下载)?" : "目前不支持dwg(CAD)文件查看，但是提供了去内网下载可以查看该类型文件的工具，是否去下载?");        builder.setPositiveButton("确认", (dialog, which) -> downloadWps(activity, type));        builder.setNegativeButton("取消", (dialog, which) -> ToastUtil.newInstance().showToast("已取消"));AlertDialog dialog = builder.show();        Observable.just(1).delay(60, TimeUnit.SECONDS).observeOn(AndroidSchedulers.mainThread()).subscribe(aLong -> {if (dialog != null && dialog.isShowing()) {private static void downloadWps(Activity activity, int type) {CommonDialog dialog = new CommonDialog(activity, type == 0 ? "正在下载wps，文件比较大,请耐心等待..." : "正在下载查看CAD工具，文件比较大,请耐心等待...");File file = FileUtil.getOutputMediaFile(9, type == 0 ? "wps.apk" : "cad.apk");        RestCreator.getDownService().downAcc(type == 0 ? WPS_ADDRESS : CAD_ADDRESS, "0")                .subscribeOn(Schedulers.io())                .unsubscribeOn(Schedulers.io())                .observeOn(Schedulers.computation())                .doOnNext(responseBody -> {                        FileUtil.writeFile(responseBody.byteStream(), file);                    } catch (IOException e) {                .observeOn(AndroidSchedulers.mainThread())                .subscribe(inputStream -> {                    InstallUtil.installApk(activity, file);if (null != dialog && dialog.isShowing()) {                    VisorLogger.e(TAG, "缓存完毕");if (null != dialog && dialog.isShowing()) {                    VisorLogger.e(TAG, e.getMessage());
```

3、调用和解释

调用：

```
BrowseFileUtil.browseFile(entity.getAddress(), getActivity());
```

解释：

(1)downWps() 方法 是用来下载wps的，但是这个下载办法不是通用的，你可以忽视掉，只用其他地方就好。

(2)还是有很多文件查看不了，你自己把握。

(3)我把涉及到我的项目的包名的地方全部替换成了"你的包名."，所以你可能拷贝我的类不能直接用，要稍作修改，你自己先看一下我的代码再拿有用的用。
