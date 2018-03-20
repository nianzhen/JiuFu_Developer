# 二、UI与布局

1.【强制】布局中不得不使用ViewGroup多重嵌套时，不要使用LinearLayout嵌套， 改用RelativeLayout，可以有效降低嵌套数。

说明:

* Android应用页面上任何一个View都需要经过measure、layout、draw三个步骤才能被正确的渲染。

* 从xml layout的顶部节点开始进行measure，每个子节点都需 要向自己的父节点提供自己的尺寸来决定展示的位置，在此过程中可能还会重新measure\(由此可能导致measure的时间消耗为原来的2-3倍\)。

* 节点所处位置越深，套嵌带来的measure越多，计算就会越费时。这就是为什么扁平的View结构会性能更好。同时，页面拥上的View越多，measure、layout、draw所花费的时间就越久。

* 要缩短这个时间，关键是保持View的树形结构尽量扁平，而且要移除所有不需要渲染的View。

* 理想情况下，总共的measure，layout，draw时间应该被很好的控制在16ms以内，以保证滑动屏幕时UI的流畅。要找到那些多余的View\(增加渲染延迟的view\)，可以用Android Studio Monitor里的Hierarchy Viewer工具，可视化的查看所有的view。

正例:

```
<RelativeLayout
    android:id="@+id/apply_buy_rl"
    android:layout_width="match_parent"
    android:layout_height="50dp"
    android:layout_alignParentBottom="true"
    android:background="@color/white">

    <TextView
        android:id="@+id/select_tv"
        android:layout_width="95dp"
        android:layout_height="50dp"
        android:layout_centerVertical="true"
        android:background="@color/white"
        android:drawableTop="@drawable/icon_confirm_select"
        android:gravity="center"
        android:padding="6dp"
        android:text=“xxx"
        android:textColor="@color/color_81849F"
        android:textSize="10sp"/>

    <TextView
        android:id="@+id/buy_tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_marginLeft="95dp"
        android:background="@color/color_FF7351"
        android:gravity="center"
        android:text=“xxxx"
        android:textColor="@color/white"
        android:textSize="17sp"/>

</RelativeLayout>
```

反例:

```
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="50dp">

    <RelativeLayout
        android:id="@+id/select_rl"
        android:layout_width="95dp"
        android:layout_height="match_parent"
        android:background="@color/white">

        <TextView
            android:id="@+id/select_tv"
            android:layout_width="95dp"
            android:layout_height="wrap_content"
            android:layout_centerVertical="true"
            android:background="@color/white"
            android:drawablePadding="3dp"
            android:drawableTop="@drawable/icon_confirm_select"
            android:gravity="center"
            android:text=“XXX"
            android:textColor="@color/color_81849F"
            android:textSize="10sp"/>
    </RelativeLayout>


    <TextView
        android:id="@+id/buy_tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_toRightOf="@+id/select_rl"
        android:background="@color/color_FF7351"
        android:gravity="center"
        android:text=“XXXX"
        android:textColor="@color/white"
        android:textSize="17sp"/>
</RelativeLayout>
```

多重嵌套导致 measure 以及 layout 等步骤耗时过多。 扩展参考:

1\) [https://developer.android.com/studio/profile/hierarchy-viewer.html](https://developer.android.com/studio/profile/hierarchy-viewer.html)

2\) [http://mrpeak.cn/android/2016/01/11/android-performance-ui](http://mrpeak.cn/android/2016/01/11/android-performance-ui)

3\) [https://www.safaribooksonline.com/library/view/high-performance-android/9781491913994/ch04.html\#figure-story\_tree](https://www.safaribooksonline.com/library/view/high-performance-android/9781491913994/ch04.html#figure-story_tree)

2.【推荐】在Activity中显示对话框或弹出浮层时，尽量使用DialogFragment，而非Dialog/AlertDialog，这样便于随Activity

生命周期管理对话框/弹出浮层的生命周期。

正例:

```
public void showPromptDialog(String text){
    DialogFragment promptDialog = new DialogFragment() {
        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
            getDialog().requestWindowFeature(Window.FEATURE_NO_TITLE); View view = inflater.inflate(R.layout.fragment_prompt, container); return view;
        } };
    promptDialog.show(getFragmentManager(), text);

}
```

3.【推荐】源文件统一采用UTF-8的形式进行编码。

4.【强制】禁止在非ui线程进行view相关操作。

5.【推荐】文本大小使用单位dp，view大小使用单位dp。对于Textview，如果在文 字大小确定的情况下推荐使用wrap\_content

布局避免出现文字显示不全的适配问题。

6.【强制】禁止在设计布局时多次设置子view和父view中为同样的背景造成页面过 度绘制，推荐将不需要显示的布局进行及时隐藏。

7.【推荐】灵活使用布局，推荐Merge、ViewStub来优化布局，尽可能多的减少UI布局层级，推荐使用FrameLayout、LinearLayout、RelativeLayout次之。

8.【推荐】在需要时刻刷新某一区域的组件时，建议通过以下方式避免引发全局layout刷新:

* 设置固定的view大小的高宽，如倒计时组件等;

* 调用view 的layout 方式修改位置，如弹幕组件等；

* 通过修改canvas位置并且调用invalidate\(ini i, int t, int r, int b\)等方式限定刷新区域；

* 通过设置一个是否允许requestLayout 的变量，然后重写控件的requestlayout、onSizeChanged方法，判断空间的大小没有改变的情况下，当进入requestlayout的时候，直接返回而不调用super的requestLayout方法。

9.【推荐】不能在Activity没有完全显示时显示PopupWindow和Dialog。

10.【推荐】尽量不要使用AnimationDrawable，它在初始化的时候就将所有图片加载到内存中，特别占内存，并且还不能释放，释放之后下次进入再次加载时会报错。

说明:
	Android 的帧动画可以使用 AnimationDrawable 实现，但是如果你的帧动画中如果包含过多帧图片，一次性加载所有帧图片所导致的内存消耗会使低端机发生 OOM 异常。帧动画所使用的图片要注意降低内存消耗，当图片比较大时，容易出现 OOM。


扩展参考：

1\) [https://stackoverflow.com/questions/8692328/causing-outofmemoryerror-in-frame-by-frame-animation-in-android](https://stackoverflow.com/questions/8692328/causing-outofmemoryerror-in-frame-by-frame-animation-in-android)

2\) [http://blog.csdn.net/wanmeilang123/article/details/53929484](http://blog.csdn.net/wanmeilang123/article/details/53929484)

3\) [https://segmentfault.com/a/1190000005987659](https://segmentfault.com/a/1190000005987659)

4\) [https://developer.android.com/reference/android/graphics/drawable/AnimationDrawable.html](https://developer.android.com/reference/android/graphics/drawable/AnimationDrawable.html)

11.【强制】不能使用ScrollView包裹ListView/GridView/ExpandableListVIew;因为这样会把ListView的所有Item都加载到内存中，要消耗巨大的内存和cpu去绘制图 面。说明:

ScrollView中嵌套List或RecyclerView的做法官方明确禁止。除了开发过程中遇到
的各种视觉和交互问题，这种做法对性能也有较大损耗。ListView等UI组件自身有垂直滚动功能，也没有必要在嵌套一层ScrollView。目前为了较好的UI体验，更贴近
Material Design的设计，推荐使用NestedScrollView。

扩展参考:

1\)[https://developer.android.com/reference/android/widget/ScrollView.html](https://developer.android.com/reference/android/widget/ScrollView.html)

2\)[https://developer.android.com/reference/android/support/v4/widget/NestedScrollView.html](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView.html)
