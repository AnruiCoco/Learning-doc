## H5端Textarea的坑



##### Textarea是常见的文本输入框，但是在IOS与Android端使用时有不同的效果



#### 不同之处

**IOS端**：输入超一屏的文本时，页面会自动上滑至当前输入的位置，保证文本输入的过程中文字不会被键盘遮挡

**Android端**：输入超一屏的文本时，页面不会上滑，文本输入的过程中文字会被键盘遮挡

**所以，一般情况下，ios不需要开发者再额外去处理交互的逻辑，但是android是必须要额外开发的**



#### 处理方法

针对android机型这种极其不佳的textarea使用感，我们常见的处理方法如下：

1、为了避免输入时文字被键盘遮挡，我们可以动态改变textarea的高度，当focus时textarea的高度为屏幕高度—键盘高度（一般写死，不同机型键盘高度不一致）；当blur时textarea的高度为屏幕高度

此处需要注意: 如果当前react项目依赖的是antd-mobile这个UI库的话，如果使用antd-mobile库中的textareaItem，是实现不了动态改变高度的（具体原因与其属性autoHeight相关，有兴趣的同学可以自行尝试与学习），因此目前项目中使用的是html自己的textarea

2、有些ios和android机器在输入时，会导致底部固定定位的按钮跑到键盘上面去，因此我们会监听当前的teaxtarea的focus和blur状态来控制按钮的显示与隐藏



#### 遗留问题

1、android在手动点击关闭键盘时，textarea依旧会保持在focus的状态，导致textarea的高度没有变更到屏幕高度，只能通过手动点击textarea外的区域后触发textarea的blur，然后变更高度（暂时无法解决）