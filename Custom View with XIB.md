# 使用XIB来建立自定义View


> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本文主要想记录下，在使用Storyboard来构建UI可视化编程的情况下，如何为一个自定义的view创建一个抽离的可视化xib，以达到重用的作用。

### 结构  
- 如何将xib和view绑定
- 在storyboard中自由地使用view
- [Demo](https://github.com/GabrielleNDoris/CustomViewDemo/)
 
## 绑定过程
1. 先来新建一个自定义的View，继承自`UIView`（必须的了）![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2539414-0faa79ec0c8e3b6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 新建一个`xib`文件  
	在这里，为了更好地识别，我们把这个xib的名字命名成我们之前新建的那个view子类。![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2539414-e40f5959fba9c3c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 更改xib的`File's Owner` class，指向我们创建好的class![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2539414-35f3e26b67c4ade5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4. 为xib中已存在的view绑定`IBOutlet`，命名为`contentView`。类比tableview cell中content view的作用。![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2539414-67334e0919bcbe4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
5. 在`CustomView.m`中实现加载`Content View`    
      
       - (instancetype)initWithCoder:(NSCoder *)aDecoder
	   {
    		self = [super initWithCoder:aDecoder];
    
    		if (self) {
        		[[NSBundle mainBundle] loadNibNamed:@"CustomView" owner:self options:nil];
        		self.contentView.frame = self.bounds;
        		[self addSubview:self.contentView];
    		}
    
    		return self;
		}

	load nib的时候，owner为self。   
	**NOTE**: load的时机，需要在initWithCoder:或者awakeFromNib里，或者在自己写的convenience initializer里。

6. 在Content View中任意地添加子view，添加约束，愉快地玩耍吧~

## 使用自定义view
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;假设，需要在`Main.storyboard`中的`ViewController`里使用这个自定义的view。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在storyboard中新加入一个子view，将其class改为`CustomeView`，即可
