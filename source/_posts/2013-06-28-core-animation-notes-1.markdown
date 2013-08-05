---
layout: post
title: "Core Animation Notes 1"
date: 2013-06-28 08:49
keywords: ios, 动画, "core Animation",
comments: true
categories: 
---

##废话
看Core Animation相关的例子不下10次，只有最近一次才看得算是明白，然后又看了一点文档。我不是什么爱学习的人，也不想特意去先学习点什么东西以便留着以后用。我只是碰到了，我就看，看不明白拉倒，哪天碰到了再看，总有一天能看明白。

瞅了一眼Core Animation Basics，看完就忘记了，不得已一边再看一遍文档，一边写文章了。

##上一段代码
经常的，想实现一个动画，或者只是想看看这个动画怎么写的布局，那就看一下别人写的代码呗。一看，我了个去，看不明白啊。好吧，整个简单的代码先看看

```objective-c
- (void)scaleLabel:(UILabel *)label
{
    [label.layer removeAnimationForKey:@"transform.scale"];
    
    CABasicAnimation *pulseAnimation = [CABasicAnimation animationWithKeyPath:@"transform.scale"];
    pulseAnimation.duration = .3;
    pulseAnimation.toValue = [NSNumber numberWithFloat:1.05];
    pulseAnimation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionLinear];
    pulseAnimation.autoreverses = YES;
    
    [label.layer addAnimation:pulseAnimation forKey:nil];
}
```
<!--more-->
##它干了什么
这个代码简单些，大概知道干了什么事，是要进行放大操作，放大到1.05倍，时间0.3s，autoreverses意思是自己恢复，timingFunction的意思在文档里写的我没看懂，英语太次，google了一下，找到一个词，感觉挺形象，elastic function，可以直白的翻译成“松紧函数”，或者说是“弹性函数”。上面代码里用的是kCAMediaTimingFunctionEaseIn，放大动画的运行效果是会先慢后快（渐入），当然如果是EaseInEaseOut，那就是渐入渐出。elastic function这个词我取自[该文章](http://stackoverflow.com/questions/5161465/how-to-create-custom-easing-function-with-core-animation)，文章里有自定义elastic function的内容，值得阅读。

对于上面代码有一点要注意：就是首先要先removeAnimationForKey。

##什么时候调用
当你想让这个label执行一次动画时，就调用一次

##想加多个动画到这个Label上怎么办
先上一段代码

``` objective-c
- (void)animationGroup:(UILabel *)label
{
    [label.layer removeAnimationForKey:@"animationGroup"];
    
    CABasicAnimation *pulseAnimation = [CABasicAnimation animationWithKeyPath:@"transform.scale"];
    pulseAnimation.duration = 2.;
    pulseAnimation.toValue = [NSNumber numberWithFloat:1.15];
    
    CABasicAnimation *pulseColorAnimation = [CABasicAnimation animationWithKeyPath:@"backgroundColor"];
    pulseColorAnimation.duration = 1.;
    pulseColorAnimation.fillMode = kCAFillModeForwards;
    pulseColorAnimation.toValue = (id)[UIColorFromRGBA(0xFF0000, .75) CGColor];
    
    CABasicAnimation *rotateLayerAnimation = [CABasicAnimation animationWithKeyPath:@"transform.rotation"];
    rotateLayerAnimation.duration = .5;
    rotateLayerAnimation.beginTime = .5;
    rotateLayerAnimation.fillMode = kCAFillModeBoth;
    rotateLayerAnimation.toValue = [NSNumber numberWithFloat:DEGREES_TO_RADIANS(45.)];
    
    CAAnimationGroup *group = [CAAnimationGroup animation];
    group.animations = [NSArray arrayWithObjects:pulseAnimation, pulseColorAnimation, rotateLayerAnimation, nil];
    group.duration = 2.;
    group.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
    group.autoreverses = YES;
    group.repeatCount = 0;
    
    [label.layer addAnimation:group forKey:@"animationGroup"];
}
```

最后用一个CAAnimationGroup来管理所有的动画，自己去看一下CAAnimationGroup Class Reference，里面有些注意事项需要注意。

##提出问题
上面讲的这些都是直接在某个已经存在的view.layer上操作，如果想实现一个类似如下效果该怎么办

解决方法：
- 以动画所需要的内容创建一个layer
- 给layer添加一些动画
- 将layer添加到mainView上
- 动画结束，删除该layer

##只说解决方法的第一条
如何创建一个layer，并让该layer显示我们需要的内容，直接上代码
``` objective-c
- (void)createLayerWithContent:(UIView *)targetView InCoordinateSystem:(UIView *)coordinateSystemView
{
    UIGraphicsBeginImageContext(targetView.frame.size);
    CGContextRef context = UIGraphicsGetCurrentContext();
    // 将targetView的内容渲染到context中
    [targetView.layer renderInContext:context];
    // 从context中取得该图片
    CGImageRef imgRef = CGBitmapContextCreateImage(context);    
    // UIImage *contextImage = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    
    //使用imgRef创建一个layer
    CALayer *layer = [[CALayer alloc] init];
    layer.contents = (__bridge id)imgRef;
    
    // 指定其frame，并添加到coordinateSystemView.layer
    layer.frame = [coordinateSystemView convertRect:targetView.frame fromView:targetView.superview];
    [coordinateSystemView.layer addSublayer:transitionLayer];
    
    
    // TODO: some code to animate the layer
}

```
targetView就要是展现的动画层，新layer的内容就是从targetView中渲染过来的
coordinateSystemView应该是动画层所在的父view

对于CGContextRef也有很多可以利用的方法，比如裁剪，旋转，绽放，具体自己google
对于CALayer当然也有很多属性，比如设置透明度，背景色等。

##几大动画方式
- CABasicAnimation
- CAKeyframeAnimation
``` objective-c
UIGraphicsBeginImageContext(targetView.frame.size);
    CGContextRef context = UIGraphicsGetCurrentContext();
    [targetView.layer renderInContext:context];
    CGImageRef imgRef = CGBitmapContextCreateImage(context);
    UIGraphicsEndImageContext();

```

##贝赛尔曲线

##什么时候使用CATransaction


