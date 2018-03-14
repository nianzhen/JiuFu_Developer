# 一、Android 资源文件命名与使用

1.【推荐】

资源文件需带模块前缀。

2.【推荐】

layout 文件的命名方式。

Activity 的 layout 以 activity 开头

Fragment 的 layout 以 fragment 开头

Dialog 的 layout 以 dialog 开头

include 的 layout 以 include 开头

ListView 的行 layout 以 list\_item 开头

RecyclerView 的 item layout 以 recycle\_item 开头

GridView 的行 layout 以 grid\_item 开头

模块名\_控件描述\_业务描述词如：

`list_item_orderlist`

3.【推荐】

drawable

资源名称以小写单词+下划线的方式命名，根据分辨率不同存放 在不同的

drawable

目录下，建议只使用一套，例如drawable-xhdpi。采用规则如下:

模块名\_业务功能描述\_控件描述\_控件状态限定词

如:

`login_btn_pressed, tabs_icon_home_normal`

4.

【推荐】

anim资源名称以小写单词+下划线的方式命名，采用以下规则:

模块名\_逻辑名称\_\[方向\|序号\]

frame动画资源:尽可能以模 块+功能命名+序号。如:

`loading_grey_001`

5.

【推荐】

color资源使用

\#AARRGGBB格式，写入colors.xml文件中，命名格式采用以下规则:

以设计规范为准则

如

`<color name="xxxxx”>#FF000000</color>`

6.

【推荐】dimen资源以小写单词+下划线方式命名，写入dimens.xml文件中， 采用以下规则:

模块名\_描述信息如:

`<dimen name="horizontal_line_height">1dp</dimen>`

7.【推荐】

style

资源采用小写单词+下划线方式命名，写入styles.xml文件中， 采用以下规则:

父style名称.当前style名称如:

`<style name="ParentTheme.ThisActivityTheme”>`

`...`

`</style>`

8.

【推荐】

string

资源文件或者文本用到字符需要全部写入

strings.xml文件中， 字符串以小写单词+

下划线的方式命名，采用以下规则:

模块名\_逻辑名称 如:

`login_tips,homepage_notice_desc`

9.【推荐】

Id资源原则上以驼峰法命名，

View组件的资源id需要以View的缩写作为 前缀。常用缩写表如下:

| 控件 | 缩写 |
| :--- | :--- |
| LinearLayout | ll |
| RelativeLayout | rl |
| ConstraintLayout | cl |
| ListView | lv |
| ScollView | sv |
| TextView | tv |
| Button | btn |
| ImageView | iv |
| CheckBox | cb |
| RadioButton | rb |
| EditText | et |

其它控件的缩写推荐使用小写字母并用下划线进行分割，

例如: ProgressBar 对应的缩写为 progress\_bar

```
     DatePicker 对应的缩写为 date\_picker
```

10.【推荐】大分辨率图片\(单维度超过1000\)大分辨率图片建议统一放在xxhdpi目录 下管理，否则将导致占用内存成倍数增加。说明 :

```
    为了支持多种屏幕尺寸和密度，Android
```

为多种屏幕提供不同的资源目录进行适配。 为不同屏幕密度提供不同的位图可绘制对象，可用于密度特定资源的配置限定符\(在

下面详述\) 包括ldpi\(低\)、mdpi\(中\)、hdpi\(高\)、xhdpi\(超高\)、xxhdpi\(超 超高\)和xxxhdpi\(超超超高\)。例如，高密度屏幕 的位图应使用

drawable-hdpi/。根据当前的设备屏幕尺寸和密度，将会寻找最匹配的资源，如果将高分辨率图片放入低密度目录，将会造成低端机加载过大图片资源，又可能造成OOM，同时也是资源浪费，没有必要在低端机使用大图。

正例:

将144\*144的应用图标PNG文件放在drawable-xxhdpi目录

反例:

将144\*144的应用图标PNG文件放在drawable-mhdpi

目录

扩展参考:

[https://developer.android.com/guide/practices/screens\_support.html?hl=zh-cn](https://developer.android.com/guide/practices/screens_support.html?hl=zh-cn)

