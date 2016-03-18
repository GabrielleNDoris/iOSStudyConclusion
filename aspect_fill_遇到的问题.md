# Aspect Fill mode下返回上一个页面出现卡顿的问题

> 情况描述

在一张页面B上，有一张背景图imageView。作为View的subview，充斥整个View，并且imageView的mode为AspectFill。

当imageView中image的长宽和imageView的长宽不匹配时（即和View、screen的也不匹配），按照AspectFill，image会在固定长宽比的前提下，填充imageView。也就是说，在这种情况下，imageView只会显示image的一部分。

> 问题

在这张页面B上，使用
  
		[self.navigationController.popViewControllerAnimated:YES]

来返回到上一个页面A时，B在即将向右完全滑出屏幕的时候，会出现卡顿现象。大约还剩1/4的时候出现这个卡顿。

> 运行环境 : iOS 9.0 on iPhone 6s

> 解决方案

为imageView添加 `clipsToBounds` 为YES。

	self.imageView.clipsToBounds = YES;
	
> Root Cause

待续

> Demo

待续

> Screenshot

待续

> Referrence

http://stackoverflow.com/questions/19592141/uiviewcontentmodescaleaspectfill-expands-uiviews-frame