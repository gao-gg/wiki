1.FrameLayout:

FrameLayout是最简单的一个布局对象。它被定制为你屏幕上的一个空白备用区域，
之后你可以在其中填充一个单一对象 — 比如，一张你要发布的图片。
所有的子元素将会固定在屏幕的左上角；你不能为FrameLayout中的一个子元素指定一个位置。
后一个子元素将会直接在前 一个子元素之上进行覆盖填充，把它们部份或全部挡住（除非后一个子元素是透明的）。

```java
<FrameLayout
  android:id="@+id/index_miaosha_image_layout"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:layout_centerVertical="true"
  android:layout_margin="5dp" >

    <ImageView
     android:id="@+id/index_miaosha_image"
     android:layout_width="80dp"
     android:layout_height="80dp" />

    <ImageView
      android:id="@+id/index_miaosha_discount_icon"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_gravity="top|left"
      android:background="@drawable/app_limit_buy_sale" />
</FrameLayout>
```

代码将index_miaosha_discount_icon这张图片重叠到index_miaosha_image,产生热门的效果,图示如下:
![frameLayout_img1.png](https://github.com/wjch/wiki/blob/master/frameLayout_img1.png?raw=true)
<img src="https://github.com/wjch/wiki/blob/master/frameLayout_img2.png?raw=true" width="200" height="200">
