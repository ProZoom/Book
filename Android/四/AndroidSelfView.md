

<!-- TOC -->

- [基础](#基础)
    - [自定义View分类](#自定义view分类)
    - [坐标系](#坐标系)
- [继承View & 继承viewGroup](#继承view--继承viewgroup)
    - [绘制流程](#绘制流程)
    - [构造函数](#构造函数)
    - [自定义View](#自定义view)
    - [定义属性](#定义属性)
    - [绘画流程](#绘画流程)
        - [onMeasure](#onmeasure)
        - [onLayout](#onlayout)
        - [onDraw](#ondraw)
- [自定义组合控件](#自定义组合控件)
    - [编写布局文件](#编写布局文件)
    - [实现构造方法](#实现构造方法)
    - [初始化UI](#初始化ui)
    - [提供对外的方法](#提供对外的方法)
    - [在布局当中引用该控件](#在布局当中引用该控件)
- [继承系统控件](#继承系统控件)
- [进阶](#进阶)
    - [Paint专题](#paint专题)
    - [Cavcas专题](#cavcas专题)
    - [Path专题](#path专题)
    - [Bitmap专题](#bitmap专题)
- [面试](#面试)

<!-- /TOC -->



<!--<details>
<summary>查看代码</summary>
<pre><code>



</code></pre>
</details>-->


---

#### 基础

##### 自定义View分类

| 类型|定义|
|----|----|
|自定义组合控件| 将多个组件组合在一起
|继承系统组件| 继承Android自带UI控件，进行功能拓展
|继承View|直接继承View，实现控件高度定制
|继承viewGroup|直接接触ViewGroup类控件，比如LinearLayout等等

##### 坐标系

在Android坐标系中，以屏幕左上角作为原点，向右为x轴正方向，向下为y轴正方向，如图:

![Android坐标系](https://i.loli.net/2019/08/19/4cqi9Gw2jmoRneP.png)





---

#### 继承View & 继承viewGroup


##### 构造函数

无论是我们继承系统View还是直接继承View，都需要对构造函数进行重写，构造函数有多个，至少要重写其中一个才行。


|构造函数|说明|
|----|---|
|DemoView(Context context)|在java代码里new的时候会用到|
|DemoView(Context context, AttributeSet attrs)|在xml布局文件中使用时自动调用|
|DemoView(Context context, AttributeSet attrs, int defStyleAttr)|不会自动调用，如果有默认style时，在第二个构造函数中调用|
|DemoView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) |只有在API版本>21时才会用到,不会自动调用，如果有默认style时，在第二个构造函数中调用
  
```java

public class DemoView extends View {
     /**
     * 在java代码里new的时候会用到
     * @param context
     */
    public DemoView(Context context) {
        super(context);
    }

    /**
     * 在xml布局文件中使用时自动调用
     * @param context
     */
    public DemoView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    /**
     * 不会自动调用，如果有默认style时，在第二个构造函数中调用
     * @param context
     * @param attrs
     * @param defStyleAttr
     */
    public DemoView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    /**
     * 只有在API版本>21时才会用到
     * 不会自动调用，如果有默认style时，在第二个构造函数中调用
     * @param context
     * @param attrs
     * @param defStyleAttr
     * @param defStyleRes
     */
    public DemoView(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }
}

```
  

```java

public class DemoView extends View {
     /**
     * 在java代码里new的时候会用到
     * @param context
     */
    public DemoView(Context context) {
        super(context);
    }

    /**
     * 在xml布局文件中使用时自动调用
     * @param context
     */
    public DemoView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    /**
     * 不会自动调用，如果有默认style时，在第二个构造函数中调用
     * @param context
     * @param attrs
     * @param defStyleAttr
     */
    public DemoView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    /**
     * 只有在API版本>21时才会用到
     * 不会自动调用，如果有默认style时，在第二个构造函数中调用
     * @param context
     * @param attrs
     * @param defStyleAttr
     * @param defStyleRes
     */
    public DemoView(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }
}

```

#####  自定义View

Android系统的控件以android开头的都是系统自带的属性。为了方便配置自定义View的属性，我们也可以自定义属性值。
Android自定义属性可分为以下几步:

- 自定义一个View
- 编写values/attrs.xml，在其中编写styleable和item等标签元素
- 在布局文件中View使用自定义的属性（注意namespace）
- 在View的构造方法中通过TypedArray获取



##### 定义属性

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <declare-styleable name="demo">
            <!--颜色-->
            <attr name = "textColor" format = "#000000" />
            <!--布尔值-->
            <attr name = "focusable" format = "boolean" />
            <!--尺寸值-->
            <attr name = "layout_width" format = "dimension" />
            <!--浮点值-->
            <attr name = "fromAlpha" format = "float" />
            <!--整型值-->
            <attr name="testAttr" format="integer" />
            <!--字符串-->
            <attr name="text" format="string" />
            <!--百分数-->
            <attr name = "pivotX" format = "fraction" />
            <!--枚举值-->
            <attr name="orientation">
                <enum name="horizontal" value="0" />
                <enum name="vertical" value="1" />
            </attr>
            <!--位或运算-->
            <attr name="gravity">
                <flag name="top" value="0x01" />
                <flag name="bottom" value="0x02" />
                <flag name="left" value="0x04" />
                <flag name="right" value="0x08" />
                <flag name="center_vertical" value="0x16" />
            </attr>
            <!--混合类型-->
            <attr name = "background" format = "reference|color" />
        </declare-styleable>
    </resources>

```


##### 获取XML定义属性

- TintTypedArray
- TypedArray

```


```


##### 绘画流程

![TIM图片20190820162213.png](https://i.loli.net/2019/08/20/ZlojWN4DiRIqnsL.png)

- ViewRootImpl

  位于Android源码 frameworks/base/core/java/android/view/ViewRootImpl.java
  
  ViewRootImpl是连接WindowManager和DecorView的桥梁，View的绘制流程开始于ViewRootImpl得performTraversals方法，只有经过measure、layout、draw三个流程才能最终绘制出View;
  如上图: performTraversals()依次调用performMeasure()、performLayout()和performDraw()三个方法，分别完成顶级View的绘制;
  其中performMeasure()会调用measure()，measure()中又调用onMeasure()，实现对其所有子元素的measure过程，这样就完成了一次measure过程；接着子元素会重复父容器的measure过程，如此反复至完成整个View树的遍历(layout和draw同理).


![TIM截图20190820163003.png](https://i.loli.net/2019/08/20/aUjsfOhwPZ9l763.png)

View绘制流程如上图，其中最重要的三个方法:
- onMeasure
- onLayout
- onDraw

###### onMeasure

- MeasureSpec

MeasureSpec主要方法:

方法/变量|说明|对应
----|---|---|
getMode(int mode)|获取测量模式
getSize(int size)|获取测量大小
makeMeasureSpec(int size,int mode)| 通过Mode和Size生成新的SpecMode
AT_MOST|最大模式，View的尺寸有一个最大值，View不可以超过MeasureSpec当中的Size值|match_parent
EXACTLY|精准模式，View需要一个精确值，这个值即为MeasureSpec当中的Size|wrap_content
UNSPECIFIED|无限制，View对尺寸没有任何限制，View设置为多大就应当为多大|一般系统内部使用


- setMeasuredDimension(int measuredWidth, int measuredHeight)
  该方法用来设置View的宽高，在我们自定义View时也会经常用到。
  
- getDefaultSize(int size, int measureSpec)

    该方法用来获取View默认的宽高
    
```java

    /**
     * Utility to return a default size. Uses the supplied size if the
     * MeasureSpec imposed no constraints. Will get larger if allowed
     * by the MeasureSpec.
     *
     * @param size Default size for this view
     * @param measureSpec Constraints imposed by the parent
     * @return The size this view should be.
     */
    public static int getDefaultSize(int size, int measureSpec) {
        int result = size;
        int specMode = MeasureSpec.getMode(measureSpec);
        int specSize = MeasureSpec.getSize(measureSpec);

        switch (specMode) {
        case MeasureSpec.UNSPECIFIED:
            result = size;
            break;
        case MeasureSpec.AT_MOST:
        case MeasureSpec.EXACTLY:
            result = specSize;
            break;
        }
        return result;
    }
    
    
```


###### onLayout

主要用于自定义ViewGroup

常用方法有
方法/变量|说明|对应
----|---|---|
getChildCount()|
getChildAt()|
measureChild()|

###### onDraw

- Step 1, draw the background, if needed
- skip step 2 & 5 if possible (common case)
    - Step 3, draw the content
    - Step 4, draw the children
    - Step 7, draw the default focus highlight
    - 
- Step 2, save the canvas' layers
- Step 3, draw the content
- Step 4, draw the children
- Step 5, draw the fade effect and restore layers
- Step 6, draw decorations (foreground, scrollbars)

```java

 public void draw(Canvas canvas) {
        final int privateFlags = mPrivateFlags;
        final boolean dirtyOpaque = (privateFlags & PFLAG_DIRTY_MASK) == PFLAG_DIRTY_OPAQUE &&
                (mAttachInfo == null || !mAttachInfo.mIgnoreDirtyState);
        mPrivateFlags = (privateFlags & ~PFLAG_DIRTY_MASK) | PFLAG_DRAWN;

        /*
         * Draw traversal performs several drawing steps which must be executed
         * in the appropriate order:
         *
         *      1. Draw the background
         *      2. If necessary, save the canvas' layers to prepare for fading
         *      3. Draw view's content
         *      4. Draw children
         *      5. If necessary, draw the fading edges and restore layers
         *      6. Draw decorations (scrollbars for instance)
         */

        // Step 1, draw the background, if needed
        int saveCount;

        if (!dirtyOpaque) {
            drawBackground(canvas);
        }

        // skip step 2 & 5 if possible (common case)
        final int viewFlags = mViewFlags;
        boolean horizontalEdges = (viewFlags & FADING_EDGE_HORIZONTAL) != 0;
        boolean verticalEdges = (viewFlags & FADING_EDGE_VERTICAL) != 0;
        if (!verticalEdges && !horizontalEdges) {
            // Step 3, draw the content
            if (!dirtyOpaque) onDraw(canvas);

            // Step 4, draw the children
            dispatchDraw(canvas);

            drawAutofilledHighlight(canvas);

            // Overlay is part of the content and draws beneath Foreground
            if (mOverlay != null && !mOverlay.isEmpty()) {
                mOverlay.getOverlayView().dispatchDraw(canvas);
            }

            // Step 6, draw decorations (foreground, scrollbars)
            onDrawForeground(canvas);

            // Step 7, draw the default focus highlight
            drawDefaultFocusHighlight(canvas);

            if (debugDraw()) {
                debugDrawFocus(canvas);
            }

            // we're done...
            return;
        }

        /*
         * Here we do the full fledged routine...
         * (this is an uncommon case where speed matters less,
         * this is why we repeat some of the tests that have been
         * done above)
         */

        boolean drawTop = false;
        boolean drawBottom = false;
        boolean drawLeft = false;
        boolean drawRight = false;

        float topFadeStrength = 0.0f;
        float bottomFadeStrength = 0.0f;
        float leftFadeStrength = 0.0f;
        float rightFadeStrength = 0.0f;

        // Step 2, save the canvas' layers
        int paddingLeft = mPaddingLeft;

        final boolean offsetRequired = isPaddingOffsetRequired();
        if (offsetRequired) {
            paddingLeft += getLeftPaddingOffset();
        }

        int left = mScrollX + paddingLeft;
        int right = left + mRight - mLeft - mPaddingRight - paddingLeft;
        int top = mScrollY + getFadeTop(offsetRequired);
        int bottom = top + getFadeHeight(offsetRequired);

        if (offsetRequired) {
            right += getRightPaddingOffset();
            bottom += getBottomPaddingOffset();
        }

        final ScrollabilityCache scrollabilityCache = mScrollCache;
        final float fadeHeight = scrollabilityCache.fadingEdgeLength;
        int length = (int) fadeHeight;

        // clip the fade length if top and bottom fades overlap
        // overlapping fades produce odd-looking artifacts
        if (verticalEdges && (top + length > bottom - length)) {
            length = (bottom - top) / 2;
        }

        // also clip horizontal fades if necessary
        if (horizontalEdges && (left + length > right - length)) {
            length = (right - left) / 2;
        }

        if (verticalEdges) {
            topFadeStrength = Math.max(0.0f, Math.min(1.0f, getTopFadingEdgeStrength()));
            drawTop = topFadeStrength * fadeHeight > 1.0f;
            bottomFadeStrength = Math.max(0.0f, Math.min(1.0f, getBottomFadingEdgeStrength()));
            drawBottom = bottomFadeStrength * fadeHeight > 1.0f;
        }

        if (horizontalEdges) {
            leftFadeStrength = Math.max(0.0f, Math.min(1.0f, getLeftFadingEdgeStrength()));
            drawLeft = leftFadeStrength * fadeHeight > 1.0f;
            rightFadeStrength = Math.max(0.0f, Math.min(1.0f, getRightFadingEdgeStrength()));
            drawRight = rightFadeStrength * fadeHeight > 1.0f;
        }

        saveCount = canvas.getSaveCount();

        int solidColor = getSolidColor();
        if (solidColor == 0) {
            if (drawTop) {
                canvas.saveUnclippedLayer(left, top, right, top + length);
            }

            if (drawBottom) {
                canvas.saveUnclippedLayer(left, bottom - length, right, bottom);
            }

            if (drawLeft) {
                canvas.saveUnclippedLayer(left, top, left + length, bottom);
            }

            if (drawRight) {
                canvas.saveUnclippedLayer(right - length, top, right, bottom);
            }
        } else {
            scrollabilityCache.setFadeColor(solidColor);
        }

        // Step 3, draw the content
        if (!dirtyOpaque) onDraw(canvas);

        // Step 4, draw the children
        dispatchDraw(canvas);

        // Step 5, draw the fade effect and restore layers
        final Paint p = scrollabilityCache.paint;
        final Matrix matrix = scrollabilityCache.matrix;
        final Shader fade = scrollabilityCache.shader;

        if (drawTop) {
            matrix.setScale(1, fadeHeight * topFadeStrength);
            matrix.postTranslate(left, top);
            fade.setLocalMatrix(matrix);
            p.setShader(fade);
            canvas.drawRect(left, top, right, top + length, p);
        }

        if (drawBottom) {
            matrix.setScale(1, fadeHeight * bottomFadeStrength);
            matrix.postRotate(180);
            matrix.postTranslate(left, bottom);
            fade.setLocalMatrix(matrix);
            p.setShader(fade);
            canvas.drawRect(left, bottom - length, right, bottom, p);
        }

        if (drawLeft) {
            matrix.setScale(1, fadeHeight * leftFadeStrength);
            matrix.postRotate(-90);
            matrix.postTranslate(left, top);
            fade.setLocalMatrix(matrix);
            p.setShader(fade);
            canvas.drawRect(left, top, left + length, bottom, p);
        }

        if (drawRight) {
            matrix.setScale(1, fadeHeight * rightFadeStrength);
            matrix.postRotate(90);
            matrix.postTranslate(right, top);
            fade.setLocalMatrix(matrix);
            p.setShader(fade);
            canvas.drawRect(right - length, top, right, bottom, p);
        }

        canvas.restoreToCount(saveCount);

        drawAutofilledHighlight(canvas);

        // Overlay is part of the content and draws beneath Foreground
        if (mOverlay != null && !mOverlay.isEmpty()) {
            mOverlay.getOverlayView().dispatchDraw(canvas);
        }

        // Step 6, draw decorations (foreground, scrollbars)
        onDrawForeground(canvas);

        if (debugDraw()) {
            debugDrawFocus(canvas);
        }
    }

```


- onDraw里重要方法

方法|说明
----|---|
postInvalidate()|1.重绘UI<br>2.调用postInvalidate()后系统会重新调用onDraw方法画一次
postInvalidate(int left, int top, int right, int bottom)|
postInvalidateDelayed(long delayMilliseconds)|
postInvalidateDelayed(long delayMilliseconds, int left, int top,int right, int bottom)|
||
invalidate()|1.在UI线程自身中使用<br>2.不能直接在线程中调用



---



---


---

#### 自定义组合控件

自定义组合控件就是将多个控件组合成为一个新的控件，主要解决多次重复使用同一类型的布局,如dailog等，我们都可以把他们组合成一个新的控件。基本流程如下:

- 1.编写布局文件
- 2.实现构造方法
- 3. 初始化UI
- 4. 提供对外的方法
- 5. 在布局当中引用该控件

##### 编写布局文件

编写布局文件dialog_friend.xml，源码如下图:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/dialog_bg">

    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@drawable/dialog_bg"
        android:orientation="vertical">

        <TextView
            android:id="@+id/tip"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@drawable/dialog_bg_top"
            android:gravity="center"
            android:padding="20dp"
            android:text="温馨提示"
            android:textColor="#FFD306"
            android:textSize="20dp" />


        <ScrollView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_above="@id/view"
            android:layout_below="@+id/tip"
            android:background="@color/colorWhite">

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="20dp"
                android:layout_marginRight="20dp"
                android:text="提示信息"
                android:textSize="20dp" />

        </ScrollView>


        <View
            android:id="@+id/view"
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:layout_above="@+id/ll_btn"
            android:layout_marginTop="0dp"
            android:background="#c0c0c0" />

        <LinearLayout
            android:id="@+id/ll_btn"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_alignParentBottom="true"
            android:background="@drawable/dialog_bg_bottom"
            android:orientation="horizontal">

            <TextView
                android:id="@+id/btn_know"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:gravity="center"
                android:paddingBottom="15dp"
                android:paddingTop="15dp"
                android:text="知道了"
                android:textColor="#FFD306"
                android:textSize="17dp" />

        </LinearLayout>


    </RelativeLayout>

</RelativeLayout>



```

##### 实现构造方法

```java

public class FriendDialog extends Dialog {


    private Timer timer;

    private int second = 8;

    private TextView tv_know;
    private LinearLayout linearLayout;

    ......
    
    public FriendDialog(@NonNull Context context, int width, int height, @IdRes int layout, int style) {
        super(context);
        initView(context);
        initTimer();
        
    }
    
    
    ......
    private void initView(Context context) {
        ...
    }
    
    private void initTimer() {
        ...
    }
    
}


```

##### 初始化UI

```java

    private void initView(Context context) {
        
         View view = getLayoutInflater().inflate(R.layout.dialog_friend, null);

        setContentView(view);
        Window window = getWindow();
        WindowManager.LayoutParams params = window.getAttributes();
        params.gravity = Gravity.CENTER;
        params.height = context.getResources().getDisplayMetrics().heightPixels - 60;
        params.width = context.getResources().getDisplayMetrics().widthPixels - 60;


        //去背景
        this.getWindow().setBackgroundDrawableResource(android.R.color.transparent);
        window.setAttributes(params);

        setCancelable(false);

        linearLayout = view.findViewById(R.id.ll_btn);

        linearLayout.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mHandler.sendEmptyMessage(destorytimer);
                dismiss();
            }
        });

        tv_know = view.findViewById(R.id.btn_know);
        tv_know.setText("知道了( " + second + "s )");

    }

```

##### 提供对外的方法

针对内部相关控件提供相关接口，比如文字大小颜色，按钮的使能与否等各种功能

##### 在布局当中引用该控件

和正常控件调用一样，也采用findViewById来获取，对外暴露相关需要的接口

---

#### 继承系统控件

- 继承系统的控件可以分为继承View子类（如TextVIew、Button等）
- 继承ViewGroup子类(如LinearLayout等)



---

#### 进阶



##### Paint专题

常量|说明
----|---|
ANTI_ALIAS_FLAG|
CURSOR_AFTER|
CURSOR_AT|
CURSOR_AT_OR_AFTER|
CURSOR_AT_OR_BEFORE|
CURSOR_BEFORE|
DEV_KERN_TEXT_FLAG|
DITHER_FLAG|
EMBEDDED_BITMAP_TEXT_FLAG|
END_HYPHEN_EDIT_INSERT_ARMENIAN_HYPHEN|
END_HYPHEN_EDIT_INSERT_HYPHEN|
END_HYPHEN_EDIT_INSERT_MAQAF|
|


---
方法|说明
----|---|
Paint(Paint paint)|
Paint(int flags)|
Paint()|
reset()|重置Paint 
setStyle(Style style)|设置绘制模式
setColor(int color)|设置颜色
setStrokeWidth(float width)|设置线条宽度,画笔样式为空心时，设置空心画笔的宽度
setTextSize(float textSize)|设置文字大小
.setAntiAlias(boolean aa) |设置抗锯齿开关
setAlpha(int a)|设置画笔的透明度[0-255]，0是完全透明，255是完全不透明
setColorFilter(ColorFilter filter)|设置图形重叠时的显示方式，下面来演示一下
setARGB(int a, int r, int g, int b) |设置画笔颜色，argb形式alpha，red，green，blue每个范围都是[0-255]
setTextScaleX(float scaleX)|设置字体的水平方向的缩放因子，默认值为1，大于1时会沿X轴水平放大，小于1时会沿X轴水平缩小
,setTypeface(Typeface typeface)|设置字体样式
setFakeBoldText(boolean fakeBoldText) |设置文本粗体
setStrikeThruText(boolean strikeThruText) |设置文本的删除线
setUnderlineText(boolean underlineText) |设置文本的下划线
setFlags(int flags)|设置一些标志，比如抗锯齿，下划线等等
setLetterSpacing(float letterSpacing)|设置行的间距，默认值是0，负值行间距会收缩
setStrokeMiter(float miter)|当style为Stroke或StrokeAndFill时设置连接处的倾斜度，这个值必须大于0，看一下演示结果
setDither(boolean dither)|设置是否抖动，如果不设置感觉就会有一些僵硬的线条，如果设置图像就会看的更柔和一些
setStrokeCap(Paint.Cap cap)|设置线冒样式，取值有Cap.ROUND(圆形线冒)、Cap.SQUARE(方形线冒)、Paint.Cap.BUTT(无线冒)
setStrokeJoin(Paint.Join join)|设置线段连接处样式，取值有：Join.MITER（结合处为锐角）、Join.Round(结合处为圆弧)、Join.BEVEL(结合处为直线)


##### Cavcas专题

---
方法|说明
----|---|
drawRect(RectF rect, Paint paint)|绘制区域，参数一为RectF一个区域
drawPath(Path path, Paint paint) |绘制一个路径，参数一为Path路径对象
drawBitmap(Bitmap bitmap, Rect src, Rect dst, Paint paint) |贴图，参数一就是我们常规的Bitmap对象，参数二是源区域(这里是bitmap)，参数三是目标区域(应该在canvas的位置和大小)，参数四是Paint画刷对象，因为用到了缩放和拉伸的可能，当原始Rect不等于目标Rect时性能将会有大幅损失。
drawLine(float startX, float startY, float stopX, float stopY, Paintpaint)|画线，参数一起始点的x轴位置，参数二起始点的y轴位置，参数三终点的x轴水平位置，参数四y轴垂直位置，最后一个参数为Paint 画刷对象
drawPoint(float x, float y, Paint paint)|画点，参数一水平x轴，参数二垂直y轴，第三个参数为Paint对象
drawText(String text, float x, floaty, Paint paint)|渲染文本，Canvas类除了上面的还可以描绘文字，参数一是String类型的文本，参数二x轴，参数三y轴，参数四是Paint对象
drawOval(RectF oval, Paint paint)|画椭圆，参数一是扫描区域，参数二为paint对象
drawCircle(float cx, float cy, float radius,Paint paint)|绘制圆，参数一是中心点的x轴，参数二是中心点的y轴，参数三是半径，参数四是paint对象
drawArc(RectF oval, float startAngle, float sweepAngle, boolean useCenter, Paint paint)|画弧，参数一是RectF对象，一个矩形区域椭圆形的界限用于定义在形状、大小、电弧，参数二是起始角(度)在电弧的开始，参数三扫描角(度)开始顺时针测量的，参数四是如果这是真的话,包括椭圆中心的电弧,并关闭它,如果它是假这将是一个弧线,参数五是Paint对象

##### Path专题
1.<br>2.


变量|说明
----|---|
Path.Direction.CW|顺时针
Path.Direction.CCW|逆时针
Path.FillType.WINDING|
Path.FillType.EVEN_ODD|
Path.FillType.INVERSE_WINDING|
Path.FillType.INVERSE_EVEN_ODD|
Path.Op.DIFFERENCE|
Path.Op.INTERSECT|
Path.Op.UNION|
Path.Op.XOR|
Path.Op.REVERSE_DIFFERENCE|





---



方法|说明
----|---|
lineTo(float x, float y)|1.从上一个点到参数坐标点之间连一条线<br>2.如果没有进行过操作则默认点为坐标原点
moveTo(float x, float y)|1.移动下一次操作的起点位置<br>2.不影响上一次操作，影响下一次操作
setLastPoint(float dx, float dy)|1.设置之前操作的最后一个点位置	<br>2.影响上一次操作，影响下一次操作
close()|1.连接当前最后一个点和最初的一个点(如果两个点不重合的话)，最终形成一个封闭的图形<br>2.close的作用是封闭路径，与连接当前最后一个点和第一个点并不等价。如果连接了最后一个点和第一个点仍然无法形成封闭图形，则close什么 也不做
addCircle(float x, float y, float radius, Direction dir)|1.<br>2.
addRect(float left, float top, float right, float bottom, Direction dir)|
reset()|
rewind()|
set(Path src)|
op(Path path, Op op)|
op(Path path1, Path path2, Op op)|
isConvex()|
setFillType(FillType ft)|
getFillType()|
isInverseFillType()|
toggleInverseFillType()|
isEmpty()|
isRect(RectF rect)|
computeBounds(RectF bounds, boolean exact)|
incReserve(int extraPtCount)|
rMoveTo(float dx, float dy)|
rLineTo(float dx, float dy)|
quadTo(float x1, float y1, float x2, float y2)|
rQuadTo(float dx1, float dy1, float dx2, float dy2)|
cubicTo(float x1, float y1, float x2, float y2, float x3, float y3)|
rCubicTo(float x1, float y1, float x2, float y2, float x3, float y3)|
arcTo(RectF oval, float startAngle, float sweepAngle, boolean forceMoveTo)|
arcTo(RectF oval, float startAngle, float sweepAngle)|
arcTo(float left, float top, float right, float bottom, float startAngle,float sweepAngle, boolean forceMoveTo)|
detectSimplePath(float left, float top, float right, float bottom, Direction dir)|
addRect(RectF rect, Direction dir)|
addRect(float left, float top, float right, float bottom, Direction dir)|
addOval(RectF oval, Direction dir)|
addOval(float left, float top, float right, float bottom, Direction dir)|
addCircle(float x, float y, float radius, Direction dir)|
addArc(RectF oval, float startAngle, float sweepAngle)|
addArc(float left, float top, float right, float bottom, float startAngle, float sweepAngle)|
addRoundRect(RectF rect, float rx, float ry, Direction dir)|
addRoundRect(float left, float top, float right, float bottom, float rx, float ry, Direction dir)|
addPath(Path src, float dx, float dy)|
addPath(Path src)|
addPath(Path src, Matrix matrix)|
offset(float dx, float dy, Path dst)|
offset(float dx, float dy)|
transform(Matrix matrix, Path dst)|
transform(Matrix matrix)|



##### Bitmap专题


---

#### 面试