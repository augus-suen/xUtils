## xUtils简介
* xUtils 包含了很多实用的android工具。
* xUtils 源于Afinal框架，对Afinal进行了适当的精简，和一些适度的扩展和重构。
* xUtils 具有Afinal的一些特性如：无需考虑bitmap在android中加载的时候oom的问题和快速滑动的时候图片加载位置错位等问题；
简洁，约定大于配置...


## 目前xUtils主要有四大模块：

* DbUtils模块：android中的orm框架，一行代码就可以进行增删改查。

* ViewUtils模块：android中的ioc框架，完全注解方式就可以进行UI绑定和事件绑定。

* HttpUtils模块：支持同步，异步方式的请求，支持大文件上传；支持GET,POST,PUT,MOVE,COPY,DELETE,HEAD请求，支持multipart上传设置subtype如related。

* BitmapUtils模块：加载bitmap的时候无需考虑bitmap加载过程中出现的oom和android容器快速滑动时候出现的图片错位等现象；内存管理使用lru算法，更好的管理bitmap内存；可配置线程加载线程数量，缓存大小，缓存路径，加载显示动画等...


----
## 使用xUtils快速开发框架需要有以下权限：

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

----
## DbUtils使用方法：

```java
DbUtils db = DbUtils.create(this);
User user = new User(); //这里需要注意的是User对象必须有id属性，或者有通过@ID注解的属性
user.setEmail("wyouflf@qq.com");
user.setName("wyouflf");
db.save(user); // 使用saveBindingId保存实体时会为实体的id赋值
```

----
## ViewUtils使用方法
* 修改自原来的FinalActivity, 但没有使用继承式的实用方式。
* 完全注解方式就可以进行UI绑定和事件绑定。
* 无需findViewById和setClickListener等。

```java
@ViewInject(id=R.id.button,click="btnClick") Button button;
@ViewInject(id=R.id.textView) TextView textView;
...
//在使用注解对象之前调用(如onCreate中)：
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main);

    ViewUtils.inject(this);

    ...
    textView.setText("some text...");
    ...
}
```

----
## HttpUtils使用方法：
### 普通get方法

```java
HttpUtils http = new HttpUtils();
http.send(HttpRequest.HttpMethod.GET,
    "http://www.lidroid.com",
    new RequestCallBack<String>(){
        @Override
        public void onLoading(long total, long current) {
            testTextView.setText(current + "/" + total);
        }

        @Override
        public void onSuccess(String result) {
            textView.setText(result);
        }

        @Override
        public void onStart() {
        }

        @Override
        public void onFailure((Throwable error, String msg) {
        }
});
```

----
### 使用HttpUtils上传文件 或者 提交数据 到服务器（post方法）

```java
RequestParams params = new RequestParams();
params.addHeader("name", "value");
params.addQueryStringParameter("name", "value");
params.addBodyParameter("name", "value");
params.addBodyParameter("file", new File("path"));
...

HttpUtils http = new HttpUtils();
http.send(HttpRequest.HttpMethod.POST,
    "updloadurl....",
    params,
    new RequestCallBack<File>() {

        @Override
        public void onStart() {
            testTextView.setText("conn...");
        }

        @Override
        public void onLoading(long total, long current) {
            testTextView.setText(current + "/" + total);
        }

        @Override
        public void onSuccess(File result) {
            testTextView.setText("downloaded:" + result.getPath());
        }

        @Override
        public void onFailure(Throwable error, String msg) {
            testTextView.setText(msg);
        }
});
```

----
### 使用HttpUtils下载文件：
* 支持断点续传，随时停止下载任务，开始任务

```java
HttpUtils http = new HttpUtils();
HttpHandler handler = http.download("http://apache.dataguru.cn/httpcomponents/httpclient/source/httpcomponents-client-4.2.5-src.zip",
    "/sdcard/httpcomponents-client-4.2.5-src.zip",
    new RequestCallBack<File>() {

        @Override
        public void onStart() {
            testTextView.setText("conn...");
        }

        @Override
        public void onLoading(long total, long current) {
            testTextView.setText(current + "/" + total);
        }

        @Override
        public void onSuccess(File result) {
            testTextView.setText("downloaded:" + result.getPath());
        }


        @Override
        public void onFailure(Throwable error, String msg) {
            testTextView.setText(msg);
        }
});

...
//调用stop()方法停止下载
handler.stop();
...
```

----
## BitmapUtils 使用方法

```java
BitmapUtils.create(this).display(testImageView, "http://bbs.lidroid.com/static/image/common/logo.png");
```

----
## 其他
### 输出日志 LogUtils

```java
LogUtils.d("wyouflf"); // 自动添加TAG，格式： className[methodName, lineNumber]
```

----
# 关于作者
* Email： <wyouflf@gmail.com>
* 有问题可以给我发邮件

# 关于Afinal
* <https://github.com/yangfuhai/afinal>


