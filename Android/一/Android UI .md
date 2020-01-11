## Android Res

[TOC]



### anim

定义渐变动画的 XML 文件

**属性动画也可以保存在此目录中，但是为了区分这两种类型，属性动画首选 animator/ 目录。**

Android除了提供drawable资源动画之外，还提供了另外两种动画体系：

* 视图动画(View Animation)

* [属性动画(Property Animation)]()

视图动画比较简单，只能应用于各种View，可以做一些位置、大小、旋转和透明度的简单转变。
属性动画则是在android 3.0引入的动画体系，提供了更多特性和灵活性，也可以应用于任何对象，而不只是View。本篇先讲视图动画。

视图动画可以通过xml文件定义，xml文件放于res/anim/目录下，根元素可以为：

* < alpha >
* < scale >
* < translate >
* < rotate >
* < set >

其中，<set>标签定义的是动画集，它可以包含多个其他标签，也可以嵌套<set>标签。默认情况下，所有动画会同时播放；如果想按顺序播放，则需要指定startOffset**属性；另外，还可以通过设置interpolator改变动画变化的速率，比如匀速、加速。



<alpha>

< alpha >可以实现透明度渐变的动画效果，也就是淡入淡出的效果，可通过设置下面三个属性来设置淡入或淡出效果：

* android:duration 动画从开始到结束持续的时长，单位为毫秒
* android:fromAlpha 动画开始时的透明度，0.0为全透明，1.0为不透明，默认为1.0
* android:toAlpha 动画结束时的透明度，0.0为全透明，1.0为不透明，默认为1.0

当设置开始时透明度为0.0，结束时为1.0，就能实现淡入效果；相反，当设置开始时透明度为1.0，结束时为0.0，那就能实现淡出效果。示例代码如下：
```xml
<!-- res/anim/fade_in.xml -->

<?xml version="1.0" encoding="utf-8"?>
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="1000"
    android:fromAlpha="0.0"
    android:toAlpha="1.0" />
```
将这动画效果添加到View上也只需要一行代码：

```java
view.startAnimation(AnimationUtils.loadAnimation(this, R.anim.fade_in));
```

如果需要重用这个动画，也可以将其抽离出来。<alpha>标签对应的动画类为AlphaAnimation，父类为Animation，以上代码将AlphaAnimation抽离后的代码可以如下：

```java
AlphaAnimation fadeInAnimation = (AlphaAnimation) AnimationUtils.loadAnimation(this, R.anim.fade_in);
view.startAnimation(fadeInAnimation);
```



<scale>

< scale >可以实现缩放的动画效果，主要的属性如下：
* android:duration 动画从开始到结束持续的时长，单位为毫秒
* android:fromXScale 动画开始时X坐标上的缩放尺寸
* android:toXScale 动画结束时X坐标上的缩放尺寸
* android:fromYScale 动画开始时Y坐标上的缩放尺寸
* android:toYScale 动画结束时Y坐标上的缩放尺寸
  PS：以上四个属性，0.0表示缩放到没有，1.0表示正常无缩放，小于1.0表示收缩，大于1.0表示放大
  android:pivotX 缩放时的固定不变的X坐标，一般用百分比表示，0%表示左边缘，100%表示右边缘
  android:pivotY 缩放时的固定不变的Y坐标，一般用百分比表示，0%表示顶部边缘，100%表示底部边缘
  示例代码如下：
```xml
<!-- res/anim/zoom_out.xml -->

<?xml version="1.0" encoding="utf-8"?>
<scale xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="1000"
    android:fromXScale="1.0"
    android:fromYScale="1.0"
    android:pivotX="0%"
    android:pivotY="100%"
    android:toXScale="1.5"
    android:toYScale="1.5" />
```
< scale >标签对应的类为ScaleAnimation，父类也是Animation，添加到View上的用法和AlphaAnimation一样，代码如下：

```java
ScaleAnimation zoomOutAnimation = (ScaleAnimation) AnimationUtils.loadAnimation(this, R.anim.zoom_out);
view.startAnimation(zoomOutAnimation);
```



<translate>

< translate >可以实现位置移动的动画效果，可以是垂直方向的移动，也可以是水平方向的移动。坐标的值可以有三种格式：从-100到100，以"%"结束，表示相对于View本身的百分比位置；如果以"%p"结束，表示相对于View的父View的百分比位置；如果没有任何后缀，表示相对于View本身具体的像素值。主要的属性如下：

* android:duration 动画从开始到结束持续的时长，单位为毫秒
* android:fromXDelta 起始位置的X坐标的偏移量
* android:toXDelta 结束位置的X坐标的偏移量
* android:fromYDelta 起始位置的Y坐标的偏移量
* android:toYDelta 结束位置的Y坐标的偏移量
  看示例吧，以下代码实现的是从左到右的移动效果，起始位置为相对于控件本身-100%的位置，即在控件左边，与控件本身宽度一致的位置；结束位置为相对于父控件100%的位置，即会移出父控件右边缘的位置。
```xml
<!-- res/anim/move_left_to_right.xml -->

<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="2000"
    android:fromXDelta="-100%"
    android:fromYDelta="0"
    android:toXDelta="100%p"
    android:toYDelta="0" />
```

< translate >标签对应的类为TranslateAnimation，父类也是Animation，添加到View上的代码如下：
```java
TranslateAnimation moveAnimation = (TranslateAnimation) AnimationUtils.loadAnimation(this, R.anim.move_left_to_right);
view.startAnimation(moveAnimation);
```

< rotate >可以实现旋转的动画效果，主要的属性如下：

* android:duration 动画从开始到结束持续的时长，单位为毫秒
* android:fromDegrees 旋转开始的角度
* android:toDegrees 旋转结束的角度
* android:pivotX 旋转中心点的X坐标，纯数字表示相对于View本身左边缘的像素偏移量；带"%"后缀时表示相对于View本身左边缘的百分比偏移量；带"%p"后缀时表示相对于父View左边缘的百分比偏移量
  android:pivotY 旋转中心点的Y坐标，纯数字表示相对于View本身顶部边缘的像素偏移量；带"%"后缀时表示相对于View本身顶部边缘的百分比偏移量；带"%p"后缀时表示相对于父View顶部边缘的百分比偏移量
  以下示例代码旋转角度从0到360，即旋转了一圈，旋转的中心点都设为了50%，即是View本身中点的位置。
```xml
<!-- res/anim/rotate_one.xml -->

<?xml version="1.0" encoding="utf-8"?>
<rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="2000"
    android:fromDegrees="0"
    android:toDegrees="360"
    android:pivotX="50%"
    android:pivotY="50%" />
```
< rotate >标签对应的类为RotateAnimation，父类也是Animation，添加到View上的代码如下：
```java
RotateAnimation rotateAnimation = (RotateAnimation) AnimationUtils.loadAnimation(this, R.anim.rotate_one);
view.startAnimation(rotateAnimation);
```



<set>



< set >标签可以将多个动画组合起来，变成一个动画集。比如想将一张图片缩放的同时也做移动，这时候就要用<set>标签组合缩放动画和移动动画了。示例代码如下：
```xml
<!-- res/anim/move_and_scale.xml -->

<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="2000">
    <translate
        android:fromXDelta="0"
        android:fromYDelta="0"
        android:toXDelta="200%"
        android:toYDelta="0" />
    <scale
        android:fromXScale="1.0"
        android:fromYScale="1.0"
        android:pivotX="0%"
        android:pivotY="100%"
        android:toXScale="1.5"
        android:toYScale="1.5" />
</set>
```

以上代码实现的动画效果为向右移动的同时也同步放大。< set >标签在视图动画中除了可以组合< alpha >, < scale >, < translate >, < rotate >这四种标签，也可以嵌套其他< set >标签。另外，< set >标签可嵌套的标签元素并不只有这几个，后面谈到属性动画时会再讲其他的标签及用法。



---

### animator 

用于定义属性动画的 XML 文件



Android除了提供drawable资源动画之外，还提供了另外两种动画体系：

* [视图动画(View Animation)]()

* 属性动画(Property Animation)

视图动画比较简单，只能应用于各种View，可以做一些位置、大小、旋转和透明度的简单转变。

属性动画则是在android 3.0引入的动画体系，提供了更多特性和灵活性，也可以应用于任何对象，而不只是View。

本篇讲属性动画：

视图动画只能作用于View，而且视图动画改变的只是View的绘制效果，View真正的属性并没有改变。比如，一个按钮做平移的动画，虽然按钮的确做了平移，但按钮可点击的区域并没随着平移而改变，还是在原来的位置。而属性动画则可以改变真正的属性，从而实现按钮平移时点击区域也跟着平移。通俗点说，属性动画其实就是在一定时间内，按照一定规律来改变对象的属性，从而使对象展现出动画效果。

属性动画是在android 3.0引入的动画体系，如果还想适配基本已经灭绝的2.x版本，只好绕道了。

属性动画和视图动画一样，可以通过xml文件定义，不同的是：
* 视图动画的xml文件放于res/anim/目录下，
* 属性动画的xml文件则放于res/animator/目录下

一个是anim，一个是animator，别搞错了。同样的，在Java代码里引用属性动画的xml文件时，则用R.animator.filename，不同于视图动画，引用时为R.anim.filename。

属性动画主要有三个元素：

| 标签               | 相对应的类     | 类介绍                                                       |
| ------------------ | -------------- | ------------------------------------------------------------ |
| < animator >       | ValueAnimator  | ValueAnimator是基本的动画类,处理值动画,通过监听某一值的变化,进行相应的操作 |
| < objectAnimator > | ObjectAnimator | ObjectAnimator是ValueAnimator**的子类，处理对象动画          |
| < set >            | AnimatorSet    | AnimatorSet则为动画集，可以组合另外两种动画或动画集          |

相应的三个标签元素的关系也一样。
样式开发主要还是用xml的形式，所以这里主要还是讲标签的用法。

## 



<animator>

< animator >标签与对应的ValueAnimator类提供了属性动画的核心功能，包括计算动画值、动画时间细节、是否重复等。执行属性动画分两个步骤：
* 1.计算动画值
* 2.将动画值应用到对象和属性上

ValuAnimiator只完成第一步，即只计算值，要实现第二步则需要在值变化的监听器里自行更新对象属性。
通过<animator>标签可以很方便的对ValuAnimiator**进行设置，可设置的属性如下：
* android:duration 动画从开始到结束持续的时长，单位为毫秒
* android:startOffset 设置动画执行之前的等待时长，单位为毫秒
* android:repeatCount 设置动画重复执行的次数，默认为0，即不重复；可设为-1或infinite，表示无限重复
* android:repeatMode 设置动画重复执行的模式，可设为以下两个值其中之一：
* restart 动画重复执行时从起点开始，默认为该值
* reverse 动画会反方向执行
* android:valueFrom 动画开始的值，可以为int值、float值或color值
* android:valueTo 动画结束的值，可以为int值、float值或color值
* android:valueType 动画值类型，若为color值，则无需设置该属性
* intType 指定动画值，即以上两个value属性的值为整型
* floatType 指定动画值，即以上两个value属性的值为浮点型，默认值
* android:interpolator 设置动画速率的变化，比如加速、减速、匀速等，需要指定Interpolator资源。

接着，用一个实例讲解具体的用法吧。在这个例子里，将一个按钮的宽度进行缩放，从100%缩放到20%。
xml文件的代码如下：
```xml
<!-- res/animator/value_animator.xml -->
<?xml version="1.0" encoding="utf-8"?>
<animator xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="3000"
    android:valueFrom="100"
    android:valueTo="20"
    android:valueType="intType" />
```

可看到，值的变化从100到20，动画时长3000毫秒，以下则是目标按钮的xml代码：
```xml
<Button
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="@drawable/bg_btn_normal"
    android:onClick="onScaleWidth"
    android:text="点我"
    android:textColor="@android:color/white" />
```
按钮默认是填充屏幕宽度的，点击时的执行方法为onScaleWidth，以下则是onScaleWidth方法的代码：

```java
public void onScaleWidth(final View view) {
    // 获取屏幕宽度
    final int maxWidth = getWindowManager().getDefaultDisplay().getWidth();
    ValueAnimator valueAnimator = (ValueAnimator) AnimatorInflater.loadAnimator(this, R.animator.value_animator);
    valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
        @Override
        public void onAnimationUpdate(ValueAnimator animator) {
            // 当前动画值，即为当前宽度比例值
            int currentValue = (Integer) animator.getAnimatedValue();
            // 根据比例更改目标view的宽度
            view.getLayoutParams().width = maxWidth * currentValue / 100;
            view.requestLayout();
        }
    });
    valueAnimator.start();
}
```

从View Animation篇中已经知道，视图动画是通过AnimationUtils**类的loadAnimation()方法获取xml文件相对应的Animation类实例，而属性动画则是通过AnimatorInflater类的loadAnimation()方法获取相应的Animator类实例。
另外，ValueAnimator通过添加AnimatorUpdateListener监听器监听值的变化，从而再手动更新目标对象的属性。
最后，通过调用valueAnimator.start()方法启动动画。





<objectAnimator>

< objectAnimator >标签对应的类为ObjectAnimator，为ValueAnimator的子类。< objectAnimator >标签与< animator >标签不同的是，< objectAnimator >可以直接指定动画的目标对象的属性。标签可设置的属性除了和< animator >一样的那些，另外多了一个：
* android:propertyName 目标对象的属性名，要求目标对象必须提供该属性的setter方法，如果动画的时候没有初始值，还需要提供getter方法
  还是用实例说明具体用法，还是用上面的例子，将一个按钮的宽度进行缩放，从100%缩放到20%，但这次改用<objectAnimator>实现。
  以下为xml文件的代码：
```xml
<!-- res/animator/object_animator.xml -->
<?xml version="1.0" encoding="utf-8"?>
<objectAnimator xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="3000"
    android:propertyName="width"
    android:valueFrom="100"
    android:valueTo="20"
    android:valueType="intType" />
```
与< animator >的例子相比，就只是多了一个android:propertyName**的属性，设置值为width。也就是说，动画改变的属性为width，值将从100逐渐减到20。另外，值是从setWidth()传递过去的，再从getWidth()获取。而且，这里设置的值代表的是比例值，因此，还需要进行计算转化为实际的宽度值。最后，对象实际的宽度值为view.getLayoutParams().width。因此，我将用一个包装类来包装原始的view对象，对其提供setWidth()和getWidth()方法，代码如下：
```java
private static class ViewWrapper {
    private View target; //目标对象
    private int maxWidth; //最长宽度值

    public ViewWrapper(View target, int maxWidth) {
        this.target = target;
        this.maxWidth = maxWidth;
    }

    public int getWidth() {
        return target.getLayoutParams().width;
    }

    public void setWidth(int widthValue) {
        //widthValue的值从100到20变化
        target.getLayoutParams().width = maxWidth * widthValue / 100;
        target.requestLayout();
    }
}
```
上面setWidth()的代码里，根据比例值转化为了实际的宽度值。最后，动画处理的代码如下：
```java
public void onScaleWidth(View view) {
    // 获取屏幕宽度
    int maxWidth = getWindowManager().getDefaultDisplay().getWidth();
    // 将目标view进行包装
    ViewWrapper wrapper = new ViewWrapper(view, maxWidth);
    // 将xml转化为ObjectAnimator对象
    ObjectAnimator objectAnimator = (ObjectAnimator) AnimatorInflater.loadAnimator(this, R.animator.object_animator);
    // 设置动画的目标对象为包装后的view
    objectAnimator.setTarget(wrapper);
    // 启动动画
    objectAnimator.start();
}
```

ObjectAnimator提供了属性的设置，但相应的需要有该属性的setter和getter方法。而ValueAnimator则只是定义了值的变化，并不指定目标属性，所以也不需要提供setter和getter方法，但只能在AnimatorUpdateListener监听器里手动更新属性。不过，也因为没有指定属性，所以其实更具灵活性了，你可以在监听器里根据值的变化做任何事情，比如更新多个属性，比如在缩放宽度的同时做垂直移动。

为了对View更方便的设置属性动画，Android系统也提供了View的一些属性和相应的setter和getter方法：
* alpha：透明度，默认为1，表示不透明，0表示完全透明
* pivotX 和 pivotY：旋转的轴点和缩放的基准点，默认是View的中心点
* scaleX 和 scaleY：基于pivotX和pivotY的缩放，1表示无缩放，小于1表示收缩，大于1则放大
* rotation、rotationX 和 rotationY：基于轴点(pivotX和pivotY)的旋转，rotation为平面的旋转，rotationX和rotationY为立体的旋转
* translationX 和 translationY：View的屏幕位置坐标变化量，以layout容器的左上角为坐标原点
* x 和 y：View在父容器内的最终位置，是左上角坐标和偏移量（translationX，translationY）的和



< set >标签对应于AnimatorSet类，可以将多个动画组合成一个动画集，如上面提到的在缩放宽度的同时做垂直移动，可以将一个缩放宽度的动画和一个垂直移动的动画组合在一起。
< set >标签有一个属性可以设置动画的时序关系：
android:ordering 设置动画的时序关系，取值可为以下两个值之一：
* together 动画同时执行，默认值
* sequentially 动画按顺序执行
  那如果想有些动画同时执行，有些按顺序执行，该怎么办呢？因为< set >标签是可以嵌套其他< set >标签的，也就是说可以将同时执行的组合在一个< set >标签，再嵌在按顺序执行的< set >标签内。
  看实例代码吧，以下为xml文件：
```xml
<!-- res/animator/animator_set.xml -->
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:ordering="together">
    <objectAnimator
        android:duration="3000"
        android:propertyName="width"
        android:valueFrom="100"
        android:valueTo="20"
        android:valueType="intType" />
    <objectAnimator
        android:duration="3000"
        android:propertyName="marginTop"
        android:valueFrom="0"
        android:valueTo="100"
        android:valueType="intType" />
</set>
```
以上代码可实现两个同时执行的动画，一个将width从100缩放到20，一个将marginTop从0增加到100。多了一个marginTop属性，那么，在ViewWrapper添加setMarginTop()方法，添加后的ViewWrapper类代码如下：
```java
private static class ViewWrapper {
    private View target;
    private int maxWidth;

    public ViewWrapper(View target, int maxWidth) {
        this.target = target;
        this.maxWidth = maxWidth;
    }

    public int getWidth() {
        return target.getLayoutParams().width;
    }

    public void setWidth(int widthValue) {
        target.getLayoutParams().width = maxWidth * widthValue / 100;
        target.requestLayout();
    }

    public void setMarginTop(int margin) {
        LinearLayout.LayoutParams layoutParams = (LinearLayout.LayoutParams) target.getLayoutParams();
        layoutParams.setMargins(0, margin, 0, 0);
        target.setLayoutParams(layoutParams);
    }
}
```
最后，动画处理的代码：
```java
public void onScaleWidth(View view) {
    // 获取屏幕宽度
    int maxWidth = getWindowManager().getDefaultDisplay().getWidth();
    // 将目标view进行包装
    ViewWrapper wrapper = new ViewWrapper(view, maxWidth);
    // 将xml转化为ObjectAnimator对象
    AnimatorSet animatorSet = (AnimatorSet) AnimatorInflater.loadAnimator(this, R.animator.animator_set);
    // 设置动画的目标对象为包装后的view
    animatorSet.setTarget(wrapper);
    // 启动动画
    animatorSet.start();
}
```
这样就搞定了，实现了宽度缩放和垂直移动的效果。



---

----



### drawable

位图文件（.png、.9.png、.jpg、.gif）或编译为以下 Drawable 资源子类型的 XML 文件：

位图文件
九宫格（可调整大小的位图）
状态列表
形状
动画 Drawable
其他 Drawable



Android有很多种drawable/mipmap类型,在以前，工程目录里只有drawable，现在用Android Studio开发的还有mipmap目录，其实，两者都可以添加图片，不过经常是建议mipmap放图片资源，drawable放xmlz资源，本篇文章将汇总介绍drawable资源。



图片是最常用的mipmap资源，格式包括：png(推荐)、jpg(可接受)、gif(不建议)。用图片资源需要根据不同屏幕密度提供多张不同尺寸的图片，它们的关系如下表：

| 密度分类 | 密度值范围 | 代表分辨率  | 图标尺寸  | 图片比例 |
| -------- | ---------- | ----------- | --------- | -------- |
| mdpi     | 120~160dpi | 320x480px   | 48x48px   | 1        |
| hdpi     | 160~240dpi | 480x800px   | 72x72px   | 1.5      |
| xhdpi    | 240~320dpi | 720x1280px  | 96x96px   | 2        |
| xxhdpi   | 320~480dpi | 1080x1920px | 144x144px | 3        |
| xxxhdpi  | 480~640dpi | 1440x2560px | 192x192px | 4        |

本来还有一个ldpi的，但现在这种小屏幕的设备基本灭绝了，所以不需要再考虑适配。如上表所示，一套图片一般需要提供5张不同比例的图片。还好有切图工具，可以让切图变得简单，这里推荐两款：Cutterman和Cut&Slice me，都是Photoshop下的插件，输出支持android、ios和web三种平台。
使用切图工具虽然方便了，但还是无法避免一套图片需要提供多张不同尺寸的图片，这会加大安装包的大小。另外，需要对图片做改动时，比如换个颜色，必须更换所有尺寸图片。所以，建议尽量减少引入图片，而通过使用shape、layer-list等自己画，易于修改和维护，也减少了安装包大小，适配性也更好。



----

### selector

 * 可以添加一个或多个item子标签，而相应的状态是在item标签中定义的。

 * 定义的xml文件可以作为两种资源使用：drawable和color。

 * 作为drawable资源使用时，一般和shape一样放于drawable目录下，item必须指定android:drawable属性；

 * 作为color资源使用时，则放于color目录下，item必须指定android:color属性。

   

那么，看看都有哪些状态可以设置呢

 * android:state_enabled:
    设置触摸或点击事件是否可用状态，一般只在false时设置该属性，表示不可用状态
 * android:state_pressed:
    设置是否按压状态，一般在true时设置该属性，表示已按压状态，默认为false
 * android:state_selected: 
    设置是否选中状态，true表示已选中，false表示未选中
 * android:state_checked:
    设置是否勾选状态，主要用于CheckBox和RadioButton，true表示已被勾选，false表示未被勾选
 * android:state_checkable:
    设置勾选是否可用状态，类似state_enabled，只是state_enabled会影响触摸或点击事件，而state_checkable影响勾选事件
 * android:state_focused:
    设置是否获得焦点状态，true表示获得焦点，默认为false，表示未获得焦点
 * android:state_window_focused:
    设置当前窗口是否获得焦点状态，true表示获得焦点，false表示未获得焦点，例如拉下通知栏或弹出对话框时，当前界面就会失去焦点；另外，ListView的ListItem获得焦点时也会触发true状态，可以理解为当前窗口就是ListItem本身
 * android:state_activated:
    设置是否被激活状态，true表示被激活，false表示未激活，API Level 11及以上才支持，可通过代码调用控件的setActivated(boolean)方法设置是否激活该控件
 * android:state_hovered:
    设置是否鼠标在上面滑动的状态，true表示鼠标在上面滑动，默认为false，API Level 14及以上才支持



那么，在使用过程中，有几点还是需要注意和了解的：



  * selector作为drawable资源时，item指定android:drawable属性，并放于drawable目录下；
  * selector作为color资源时，item指定android:color属性，并放于color目录下；
  * color资源也可以放于drawable目录，引用时则用@drawable来引用，但不推荐这么做，drawable资源和color资源最好还是分开；
  * android:drawable属性除了引用@drawable资源，也可以引用@color颜色值；但android:color只能引用@color；
  * item是从上往下匹配的，如果匹配到一个item那它就将采用这个item，而不是采用最佳匹配的规则；所以设置默认的状态，一定要写在最后，如果写在前面，则后面所有的item都不会起作用了。



另外，selector标签下有两个比较有用的属性要说一下，添加了下面两个属性之后，则会在状态改变时出现淡入淡出效果，但必须在API Level 11及以上才支持：

  * android:enterFadeDuration 状态改变时，新状态展示时的淡入时间，以毫秒为单位
  * android:exitFadeDuration 状态改变时，旧状态消失时的淡出时间，以毫秒为单位



最后，关于ListView的ListItem样式，有两种设置方式，一种是在ListView标签里设置android:listSelector属性，另一种是在ListItem的布局layout里设置android:background。但是，这两种设置的结果却有着不同。同时，使用ListView时也有些其他需要注意的地方，总结如下：



  * android:listSelector设置的ListItem默认背景是透明的，不管你在selector里怎么设置都无法改变它的背景。所以，如果想改ListItem的默认背景，只能通过第二种方式，在ListItem的布局layout里设置android:background。
  * 当触摸点击ListItem时，第一种设置方式下，state_pressed、**state_focused**和state_window_focused设为true时都会触发，而第二种设置方式下，只有state_pressed会触发。
  * 当ListItem里有Button或CheckBox之类的控件时，会抢占ListItem本身的焦点，导致ListItem本身的触摸点击事件会无效。那么，要解决此问题，有三种解决方案：
    * 将Button或CheckBox换成TextView或ImageView之类的控件
    * 设置Button或CheckBox之类的控件设置focusable属性为false
    * 设置ListItem的根布局属性android:descendantFocusability="blocksDescendants"

    第三种是最方便，也是推荐的方式，它会将ListItem根布局下的所有子控件都设置为不能获取焦点。android:descendantFocusability属性的值有三种，其中，ViewGroup是指设置该属性的View，本例中就是ListItem的根布局：
    * beforeDescendants：ViewGroup会优先其子类控件而获取到焦点
    * afterDescendants：ViewGroup只有当其子类控件不需要获取焦点时才获取焦点
    * blocksDescendants：ViewGroup会覆盖子类控件而直接获得焦点





----



### shape

- rectangle

    矩形，默认的形状，可以画出直角矩形、圆角矩形、弧形等




- solid: 设置形状填充的颜色，只有android:color一个属性
  - android:color 填充的颜色



- padding: 设置内容与形状边界的内间距，可分别设置左右上下的距离
   - android:left 左内间距
   - android:right 右内间距
   - android:top 上内间距
   - android:bottom 下内间距
- gradient: 设置形状的渐变颜色，可以是线性渐变、辐射渐变、扫描性渐变
   - android:type 渐变的类型
      - linear 线性渐变，默认的渐变类型
      - radial 放射渐变，设置该项时，android:gradientRadius也必须设置
      - sweep 扫描性渐变
   - android:startColor 渐变开始的颜色
   - android:endColor 渐变结束的颜色
   - android:centerColor 渐变中间的颜色
   - android:angle 渐变的角度，线性渐变时才有效，必须是45的倍数，0表示从左到右，90表示从下到上
   - android:centerX 渐变中心的相对X坐标，放射渐变时才有效，在0.0到1.0之间，默认为0.5，表示在正中间
   - android:centerY 渐变中心的相对X坐标，放射渐变时才有效，在0.0到1.0之间，默认为0.5，表示在正中间
   - android:gradientRadius 渐变的半径，只有渐变类型为radial时才使用
   - android:useLevel 如果为true，则可在LevelListDrawable中使用
- corners:设置圆角，只适用于rectangle类型，可分别设置四个角不同半径的圆角，当设置的圆角半径很大时，比如200dp，就可变成弧形边了
   - android:radius 圆角半径，会被下面每个特定的圆角属性重写
   - android:topLeftRadius 左上角的半径
   - android:topRightRadius 右上角的半径
   - android:bottomLeftRadius 左下角的半径
   - android:bottomRightRadius 右下角的半径

- stroke:设置描边，可描成实线或虚线。
   - android:color 描边的颜色
   - android:width 描边的宽度
   - android:dashWidth 设置虚线时的横线长度
   - android:dashGap 设置虚线时的横线之间的距离

- oval属性：椭圆形，用得比较多的是画正圆
   - size: 设置形状默认的大小，可设置宽度和高度
      - android:width 宽度
      - android:height 高度
- line属性

     线形，可以画实线和虚线


- ring属性:环形，可以画环形进度条
   - android:innerRadius 内环的半径
   - android:innerRadiusRatio 浮点型，以环的宽度比率来表示内环的半径，默认为3，表示内环半径为环的宽度除以3，该值会被android:innerRadius覆盖
   - android:thickness 环的厚度
   - android:thicknessRatio 浮点型，以环的宽度比率来表示环的厚度，默认为9，表示环的厚度为环的宽度除以9，该值会被android:thickness覆盖
   - android:useLevel 一般为false，否则可能环形无法显示，只有作为LevelListDrawable使用时才设为true
   - 如果想让这个环形旋转起来，变成可用的进度条，则只要在shape外层包多一个rotate元素就可以了。

```xml
<?xml version="1.0" encoding="utf-8"?>
<rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromDegrees="0"
    android:pivotX="50%"
    android:pivotY="50%"
    android:toDegrees="1080.0">
    <shape
        android:innerRadiusRatio="3"
        android:shape="ring"
        android:thicknessRatio="8"
        android:useLevel="false">
        ......
    </shape>
</rotate>
```



### layout

用于定义用户界面布局的 XML 文件

---

### menu

用于定义应用菜单（如选项菜单、上下文菜单或子菜单）的 XML 文件

Android系统自带的Menu很单一，但是它也支持自定义，接下来我们就来总结下Menu的用法

Android的Menu样式一般定义在res/menu/文件目录下，其中有一个根元素< menu >，他们只有两个子标签：

- < group >

- < item >

分别用于设置菜单项和分组

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">

	<group >
	    <item>
	    	......        
	    </item>

	    <item>
	    	......        
	    </item>
	</group>

	<item >

	</item>

</menu>

```

接下来各自介绍每个标签的属性和相应的值

#### group属性



用于设置分组

- android:checkableBehavior 表示菜单项是否带复选框。该属性可设计为true或false
- android:enabled 菜单项默认状态是否被激活，该属性可设计为true或false
- android:id group的id号
- android:menuCategory 同种菜单项的种类，该属性可取4个值：Container、system、secondary和alternative。通过menuCategroy属性可以控制菜单项的位置。例如将属性设为system，表示该菜单项是系统菜单，应放在其他种类菜单项的后面
- android:orderInCategory 同种类菜单的排列顺序。该属性需要设置一个整数值，每个item的优先级，值越大优先级越低，actionbar地方不够就会放到overflow中
- android:visible 菜单项默认状态是否可视，该属性可设计为true或false

#### item属性

用于设置菜单项

- android:visible 与<item>标签的同名属性含义相同。只是作用域为菜单组
- android:orderInCategory 与<item>标签的同名属性含义相同。只是作用域为菜单组
- android:menuCategory 与<item>标签的同名属性含义相同。只是作用域为菜单组
- android:id 表示菜单组的ID
- android:enabled 与<item>标签的同名属性含义相同。只是作用域为菜单组
- android:actionLayout
- android:actionProviderClass
- android:actionViewClass
- android:alphabeticModifiers
- android:alphabeticShortcut
- android:checkable 表示菜单项是否带复选框。该属性可设计为true或false
- android:checked 如果菜单项带复选框(checkable属性为true)，该属性表示复选框默认状态是否被选中。可设置的值为true或false
- android:contentDescription
- android:icon
- android:iconTint
- android:iconTintMode
- android:numericModifiers
- android:numericShortcut
- android:onClick
- android:title
- android:titleCondensed
- android:tooltipText
- app:showAsAction

#### Menu菜单重写

重写菜单需要重载以下一个方法：

```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
  //方法一：代码构建，不需要xml
  menu.add(Menu.NONE, Menu.NONE, 1, "菜单1");  
  menu.add(Menu.NONE, Menu.NONE, 2, "菜单2");
  //方法二：
  getMenuInflater().inflate(R.menu.menu文件, menu);  

  return super.onCreateOptionsMenu(menu);
}
```
#### Menu菜单响应

```java
 @Override
public boolean onOptionsItemSelected(MenuItem item){
  switch (item.getItemId()){
      case ......
  }
  return super.onOptionsItemSelected(item);
}
```


### mipmap

适用于不同启动器图标密度的 Drawable 文件

---

### values

包含字符串、整型数和颜色等简单值的 XML 文件。

其他 res/ 子目录中的 XML 资源文件是根据 XML 文件名定义单个资源，而目录中的 values/ 文件可描述多个资源。对于此目录中的文件，&lt;resources&gt; 元素的每个子元素均定义一个资源。例如，&lt;string&gt; 元素创建 R.string 资源，&lt;color&gt; 元素创建 R.color 资源。

由于每个资源均用其自己的 XML 元素定义，因此您可以根据自己的需要命名文件，并将不同的资源类型放在一个文件中。但是，为了清晰起见，您可能需要将独特的资源类型放在不同的文件中。 例如，对于可在此目录中创建的资源，下面给出了相应的文件名约定：




#### arrays.xml

用于资源数组（类型化数组）。

#### color.xml

用于定义颜色状态列表的 XML 文件,颜色值。

#### dimens.xml

尺寸值。

#### strings.xml

字符串值。

#### styles.xml

样式。

终于要讲到Android样式系列的终篇，整合所有资源，定义成统一的样式Style。

哪些该定义成统一的样式呢？举几个例子吧：

* 每个页面标题栏的标题基本会有一样的字体大小、颜色、对齐方式、内间距、外间距等，这就可以定义成样式；
* 很多按钮也都使用一致的背景、内间距、文字颜色、文字大小、文字的对齐方式等，这也可以定义成样式；
* 网络加载的进度条基本也都是一样的，同样可以定义成样式；
* 不喜欢系统的弹出框样式，那也可以自定义样式。
* 等等



Android的样式一般定义在res/values/styles.xml文件中，其中有一个根元素< resource >，而具体的每种样式定义则是通过<resource>下的子标签< style >来完成，< style >通过添加多个< item >来设置样式不同的属性。
另外，样式是可以继承的，可通过
< style >标签的parent**属性声明要继承的样式，也可通过点前缀 (.) 继承，点前面为父样式名称，后面为子样式名称。点前缀方式只适用于自定义的样式，若要继承Android内置的样式，则只能通过parent属性声明。
用个实例说明具体的用法吧，以下代码为Android 5.0系统默认的按钮样式：
```xml
<style name="Widget.Material.Button">
    <item name="background">@drawable/btn_default_material</item>
    <item name="textAppearance">?attr/textAppearanceButton</item>
    <item name="minHeight">48dip</item>
    <item name="minWidth">88dip</item>
    <item name="stateListAnimator">@anim/button_state_list_anim_material</item>
    <item name="focusable">true</item>
    <item name="clickable">true</item>
    <item name="gravity">center_vertical|center_horizontal</item>
</style>
```

其中，stateListAnimator指定状态改变时的动画，button_state_list_anim_material的代码如下：
```xml
<!-- res/anim/button_state_list_anim_material.xml -->

<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true" android:state_enabled="true">
        <set>
            <objectAnimator
                android:propertyName="translationZ"
                android:duration="@integer/button_pressed_animation_duration"
                android:valueTo="@dimen/button_pressed_z_material"
                android:valueType="floatType" />
            <objectAnimator
                android:propertyName="elevation"
                android:duration="0"
                android:valueTo="@dimen/button_elevation_material"
                android:valueType="floatType" />
        </set>
    </item>
    <!-- base state -->
    <item android:state_enabled="true">
        <set>
            <objectAnimator
                android:propertyName="translationZ"
                android:duration="@integer/button_pressed_animation_duration"
                android:valueTo="0"
                android:startDelay="@integer/button_pressed_animation_delay"
                android:valueType="floatType"/>
            <objectAnimator
                android:propertyName="elevation"
                android:duration="0"
                android:valueTo="@dimen/button_elevation_material"
                android:valueType="floatType" />
        </set>
    </item>
    <item>
        <set>
            <objectAnimator
                android:propertyName="translationZ"
                android:duration="0"
                android:valueTo="0"
                android:valueType="floatType"/>
            <objectAnimator
                android:propertyName="elevation"
                android:duration="0"
                android:valueTo="0"
                android:valueType="floatType"/>
        </set>
    </item>
</selector>
```

可以看到，每种状态的动画为属性动画集，属性动画的用法请参考[Property Animation篇]()。
现在我想继承Widget.Material.Button样式，改变背景和文字颜色，那么，代码如下：

```xml
<!-- res/values/styles.xml -->
<resources>
    <style name="ButtonNormal" parent="@android:style/Widget.Material.Button" >
        <item name="android:background">@drawable/bg_btn_selector</item>
        <item name="android:textColor">@color/text_btn_selector</item>
    </style>
</resources>
```
其中，@drawable/bg_btn_selector和@color/text_btn_selector**的实现请参照[selector篇]()。
有些按钮，我只想改变文字颜色，但背景想让它透明，这时就可以用点前缀的方式继承以上的样式，代码如下：

```xml
<!-- res/values/styles.xml -->
<resources>
    <style name="ButtonNormal" parent="@android:style/Widget.Material.Button">
        <item name="android:background">@drawable/bg_btn_selector</item>
        <item name="android:textColor">@color/text_btn_selector</item>
    </style>

    <style name="ButtonNormal.Transparent">
        <item name="android:background">@drawable/bg_btn_transparent</item>
        <item name="android:textColor">@color/text_btn_selector</item>
    </style>
</resources>
```

引用的时候只要在相应的Button里添加style就可以了，代码如下：
```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="onAction"
    android:text="@string/btn_action"
    style="@style/ButtonNormal.Transparent" />
```

有时候，定义的样式太多，如果都放在styles.xml文件里，那这文件也太臃肿了。因此，可以将样式分类拆分成多个文件。Android系统本身也拆分为多个文件存放的，如下列表全都是样式文件：
```
styles.xml
styles_device_defaults.xml
styles_holo.xml
styles_leanback.xml
styles_material.xml
styles_micro.xml
themes.xml
themes_device_defaults.xml
themes_holo.xml
themes_leanback.xml
themes_material.xml
themes_micro.xml
```
其中，主要分为两大类，styles定义了简单的样式，而themes则定义了主题。



以上的简单例子只用于单个View，这是样式最简单的用法。但样式的用法不只是用于单个View，也能用于Activity或整个Application，这时候需要在相应的< activity >标签或 < application >标签里设置android:theme属性，引用的其实也是style，但一般称为主题。

Android系统提供了多套主题，查看Android的frameworks/base/core/res/res/values目录，就会看到有以下几个文件(目前为止)：

* themes.xml：低版本的主题，目标API level一般为10或以下
* themes_holo.xml：从API level 11添加的主题
* themes_device_defaults.xml：从API level 14添加的主题
* themes_material.xml：从API level 21添加的主题
* themes_micro.xml：应该是用于Android Wear的主题
* themes_leanback.xml： 还不清楚什么用

不过在实际应用中，因为大部分都采用兼容包的，一般都会采用兼容包提供的一套主题：Theme.AppCompat。AppCompat主题默认会根据不同版本的系统自动匹配相应的主题，比如在Android 5.0系统，它会继承Material主题。不过这也会导致一个问题，不同版本的系统使用不同主题，就会出现不同的体验。因此，为了统一用户体验，最好还是自定义主题。
自定义主题也很简单，只要继承某一父主题，然后在<activity>标签或<application>中引用就可以了。
主题的定义示例如下：
```xml
<resources>
    <style name="AppTheme" parent="Theme.AppCompat">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
        <item name="windowAnimationStyle">@style/WindowAnimation</item>
    </style>

    <!-- Standard animations for a full-screen window or activity. -->
    <style name="WindowAnimation" parent="@android:style/Animation.Activity">
        <item name="activityOpenEnterAnimation">@anim/activity_open_enter</item>
        <item name="activityOpenExitAnimation">@anim/activity_open_exit</item>
        <item name="activityCloseEnterAnimation">@anim/activity_close_enter</item>
        <item name="activityCloseExitAnimation">@anim/activity_close_exit</item>
    </style>
</resources>
```
其中，WindowAnimation重新指定了Activity的转场动画，以下为activity_close_exit的示例代码：
```xml
<!-- res/anim/activity_close_exit.xml -->
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:shareInterpolator="false"
    android:zAdjustment="top">
    <alpha
        android:fromAlpha="0.0"
        android:toAlpha="1.0"
        android:interpolator="@interpolator/decelerate_quart"
        android:fillEnabled="true"
        android:fillBefore="false"
        android:fillAfter="true"
        android:duration="200" />
    <translate
        android:fromYDelta="8%"
        android:toYDelta="0"
        android:fillEnabled="true"
        android:fillBefore="true"
        android:fillAfter="true"
        android:interpolator="@interpolator/decelerate_quint"
        android:duration="350" />
</set>
```

这是比较简单的视图动画，视图动画具体用法可参考View Animation篇。
接着，若要使用到整个Application，则在AndroidManifest.xml的< application >标签设置android:theme**属性，示例代码如下：
```xml
<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme">
    <!-- activity here -->
</application>
```

---

### raw

要以原始形式保存的任意文件。要使用原始 InputStream 打开这些资源，请使用资源 ID（即 R.raw.<em>filename</em>）调用Resources.openRawResource()。

但是，如需访问原始文件名和文件层次结构，则可以考虑将某些资源保存在 assets/ 目录下（而不是 res/raw/）。assets/ 中的文件没有资源 ID，因此您只能使用 AssetManager 读取这些文件。

---

### xml

可以在运行时通过调用 Resources.getXML() 读取的任意 XML 文件。各种 XML 配置文件（如可搜索配置）都必须保存在此处。
