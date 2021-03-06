##  **Android蒙板控件（用户引导）**

### **效果图**
<img src="http://otjav6lvw.bkt.clouddn.com/17-7-29/92985778.jpg" width="300"/>
<img src="http://otjav6lvw.bkt.clouddn.com/17-7-29/66349083.jpg" width="300"/>
<img src="http://otjav6lvw.bkt.clouddn.com/17-7-29/75740996.jpg" width="300"/>
<img src="http://otjav6lvw.bkt.clouddn.com/17-7-29/3381654.jpg" width="300"/>

### **添加依赖**
root build.gradle
```
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```
app build.gradle
```
dependencies {
    compile 'com.github.Tan5253:ShowMask:v1.0'
}
```

### **使用方法**
### **栗子一**
> 最简单的使用方式 `(图1)`  </br> 

>方法解释：(set系列的方法都会有默认值)
1.setOnlyOneTag 用来标识只显示一次
2.setMaskColor  遮罩的颜色
3.setDismissOnTouch 是否触摸任意地方消失
4.setDuration 显示和消失的事件
5.setTargetPadding 能控制透明块的大小
6.addTarget 透明块要在哪个view上展示
7.addImage  添加图片，五个参数分别是图片资源、x轴的位置（0-10.0f）、y轴的位置（0-10.0f）、图片的缩放比例（当为1.0f时图片宽为屏幕的一半，高度会等比例缩放）、点击图片蒙版是否消失。
8.addShowcaseListener 监听显示和关闭的事件
```
new ShowcaseView.Builder(this)
	.setOnlyOneTag(MainActivity.class.getSimpleName())
        .setMaskColor("#88EECC33")
        .setDismissOnTouch(true)
        .setDuration(1000L, 1000L)
        .setTargetPadding(20)
        .addTarget(mTextView)
        .addImage(R.mipmap.img_showcase, 5.0f, 5.0f, 1.5f, true)
        .addShowcaseListener(new ShowcaseView.ShowcaseListener() {
            @Override
            public void onDisplay(ShowcaseView showcaseView) {
            }

            @Override
            public void onDismiss(ShowcaseView showcaseView) {
                Intent intent = new Intent(
                  MainActivity.this, SecondActivity.class);
                startActivity(intent);
            }
        })
        .build().show();
```
### **栗子二**
>  `图2 图3 图4` 是添加到队列再显示的方式（showQueue），也就是说当第一个蒙版消失后会接着显示第二个...直到把队列中的蒙版都显示完。addShowcaseQueue方法用来添加到队列，最后通过showQueue方法来依次显示。

> 特别注意：set系列的属性会在第一个addShowcaseQueue调用后保留下来，直到调用build为止，比如遮罩颜色等。
```
new ShowcaseView.Builder(this)
	.setOnlyOneTag(MainActivity.class.getSimpleName())
        .setMaskColor("#88EECC33")
        .setDismissOnTouch(true)
        .setDuration(1000L, 1000L)
        .setTargetPadding(20)
        .addTarget(mTextView, ShowcaseView.CIRCLE_SHAPE)
        .addShowcaseQueue()
        .setMaskColor("#66FFB6C1")
        .addTarget(mTextView2, ShowcaseView.OVAL_SHAPE)
        .addShowcaseQueue()
        .setMaskColor("#D8BFD8")
        .setDismissOnTouch(false)
        .addTarget(mTextView3, ShowcaseView.RECTANGLE_SHAPE)
        .addImage(R.mipmap.img_showcase, 5.0f, 8.0f, 1.0f, true)
        .addShowcaseListener(new ShowcaseView.ShowcaseListener() {
            @Override
            public void onDisplay(ShowcaseView showcaseView) {
                Toast.makeText(getApplication(),
                "最后一个展示啦",Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onDismiss(ShowcaseView showcaseView) {
                Toast.makeText(getApplication(),
                "最后一个消失啦",Toast.LENGTH_SHORT).show();
            }
        })
        .addShowcaseQueue()
        .build().showQueue();
```
