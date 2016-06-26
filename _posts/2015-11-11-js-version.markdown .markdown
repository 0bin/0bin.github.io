---
layout:     post
title:      "RunLoop"
subtitle:   ""
date:       2015-10-20
author:     "Bin"
header-img: "img/home-bg-o.jpg"
tags:
    - iOS
    
---

> “ 开发随笔”

```
- 框架的设计：
1.先写需求，在根据需求写方法
2.图片等资源封装，改后缀名 .bundle，打包成资源包，在使用图片时加上 包名/图片名；
3.要考虑用户多次使用 问题；
4.写readme 用markable ；

- 尽力减少字符串操作，新对象创建，for循环
性能分析
－ 静态  product － analyze
－ 动态 product － profile

po 要看的东西名字
－regexkitlite

- 计算总行数/页数 = （总个数 + 每页最大数-1）/每页最大数

- 行列算法
int col ＝ （count >＝ 列数）？列数 ：count；
int row ＝ （count ＋ 列数 －1）／列数；

－lowercasestring 小写

- 设置系统cell图片尺寸
    UIImage *icon = [UIImageimageNamed:@"defaultUserIcon"];
    CGSize itemSize = CGSizeMake(30, 30);
    UIGraphicsBeginImageContextWithOptions(itemSize, NO,0.0);
    CGRect imageRect = CGRectMake(0.0, 0.0, itemSize.width, itemSize.height);
    [icon drawInRect:imageRect];
    cell.imageView.image = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();

- session下载代理可放在子线程

- 键盘操作
resignFirstResponder
当叫出键盘的那个控件(第一响应者)调用这个方法时，就能退出键盘
endEditing
只要调用这个方法的控件内部存在第一响应者，就能退出键盘

- xx. pathExtension获取文件后缀名

- weak对象不能直接赋值，要先alloc init创建在引用

- setNeedsDisplay方法用于绘图，而layoutSubViews方法计算位置

- xcode 文件突然消失  新建一个类就能恢复

- 时间对比： xx timeIntervalSinceDate：oo

- AFN 请求成功后显示到tabble，要保存数据再reload data；

- xib加载不要使用xib的view的尺寸，从xib获取的尺寸不对，用[UIScreen mainScreen]的

- 创建xib 就要立刻设置一个类方法加载

- load nib  owner:拥有者 写self 就是当前控制器能与xib建立联系

- 键盘处理 
    [self becomeFirstResponder];
    [self.view endEditing:YES];

- textfield  inputView   / inputAccessoryView

- 导航栏设置 
    [self.navigationController.navigationBar setBackgroundImage:[[UIImage alloc] init] forBarMetrics:UIBarMetricsDefault];
    self.navigationController.navigationBar.shadowImage = [[UIImage alloc] init];

- 通过重写 set frame  set bounds   控制控件尺寸不被修改

- 碰到不会的：类文件 - 父类 - 协议 - 代理 -  一个个找

- 设置textfield占位字颜色 mutualAttributesString  富文本 1：attributes 2.drawPlaceholder 3、添加label作为占位字符

- xib在右边属性栏可以通过kvc（第三个按钮）修改控件属性

- xib要先添加个view在添加其他控件；

- charles青花瓷中文无法显示 在VMOptions中加一项：-Dfile.encoding=UTF-8

- 设置cell分割线满屏的办法（自动布局捣的鬼）
    cell.layoutMargins = UIEdgeInsetsZero;
    cell.separatorInset = UIEdgeInsetsZero;
－ 去除cell分割线
[tableView setSeparatorStyle:UITableViewCellSeparatorStyleNone];

- 获取请求头内容
    NSHTTPURLResponse *response = (NSHTTPURLResponse *)task.response;
    NSDictionary *allHeaders = response.allHeaderFields;

-layoutSubviews在以下情况下会被调用：
1、init初始化不会触发layoutSubviews
2、addSubview会触发layoutSubviews
3、设置view的Frame会触发layoutSubviews，当然前提是frame的值设置前后发生了变化
4、滚动一个UIScrollView会触发layoutSubviews
5、旋转Screen会触发父UIView上的layoutSubviews事件
6、改变一个UIView大小的时候也会触发父UIView上的layoutSubviews事件

- 
UITextField *searchField = [[search subviews] lastObject];
[searchField setReturnKeyType:UIReturnKeyDone]
// set style of keyboard
[(UITextField *)searchBarSubview setKeyboardAppearance:UIKeyboardAppearanceAlert];

// always force return key to be enabled
[(UITextField *)searchBarSubview setEnablesReturnKeyAutomatically:NO];


- 判断给定的点是否被一个CGRect包含,可以用CGRectContainsPoint函数
  BOOL contains = CGRectContainsPoint(CGRect rect, CGPoint point);

- 判断一个CGRect是否包含再另一个CGRect里面,常用与测试给定的对象之间是否又重叠
    BOOL contains = CGRectContainsRect(CGRect rect1, CGRect rect2);

- 判断两个结构体是否有交错.可以用CGRectIntersectsRect
    BOOL contains = CGRectIntersectsRect(CGRect rect1, CGRect rect2);
    float float_ = CGRectGetMaxX(CGRect rect);返回矩形右边缘的坐标

CGRectGetMaxY返回矩形顶部的坐标
CGRectGetMidX返回矩形中心X的坐标
CGRectGetMidY返回矩形中心Y的坐标
CGRectGetMinX返回矩形左边缘的坐标
CGRectGetMinY返回矩形底部的坐标
CGRectContainsPoint 看参数说明，一个点是否包含在矩形中，所以参数为一个点一个矩形

- window默认是隐藏的 [window setHidden:NO];要释放自定义的window，直接设置_window = nil；window 优先级 Alert > statusBar > Normal

- 导航栏界面滑动时导航栏一半黑色，受到window背景色影响，修改window背景色。

- 遮盖效果 也可以用xib 上添加个button。
[cover mas_makeConstraints:^(MASConstraintMaker *make) {
make.left.equalTo(self.tableView.mas_left);
make.right.equalTo(self.tableView.mas_right);
make.top.equalTo(self.tableView.mas_top);
make.bottom.equalTo(self.tableView.mas_bottom);
}];

//通过search 隐藏键盘 触发代理方法，移除cover
[cover addGestureRecognizer:[[UITapGestureRecognizer alloc] initWithTarget:searchBar action:@selector(resignFirstResponder)]];
[searchBar setBackgroundImage:[UIImage imageNamed:@"bg_login_textfield_hl"]];
[cover setTag:XTCoverTag];
[self.navigationController setNavigationBarHidden:NO animated:YES];
[[self.view viewWithTag:XTCoverTag] removeFromSuperview];
[searchBar setBackgroundImage:[UIImage imageNamed:@"bg_login_textfield"]];

- KVC [数组 valueForKeyPath：@“属性”]，自动将数组内的属性 包装成一个新的数组；

- AudioServicesPlaySystemSound(kSystemSoundID_Vibrate); 震动

- cell没有值或尺寸没设置好，是不会进入cell方法的；1、cell 无值 2.autolayout原因

- 控件frame打印正确，显示不对，设置 控件的autoresizingMask = UIViewAutoresizingNone，或在xib 创建之初 去掉控件内 横竖约束（自动布局搞得）

- 将字典转成模型，并存储；模型为对象属性所以用keyarchiver, writetofile 针对字典，数组；
- 随机色
#define UIColorFromRGB(rgbValue) [UIColor \
colorWithRed:((float)((rgbValue & 0xFF0000) >> 16))/255.0 \
green:((float)((rgbValue & 0xFF00) >> 8))/255.0 \
blue:((float)(rgbValue & 0xFF))/255.0 alpha:1.0]

//十六进制的颜色
#define UIColorFromRGB(rgbValue) [UIColor \
colorWithRed:((float)((rgbValue & 0xFF0000) >> 16))/255.0 \
green:((float)((rgbValue & 0xFF00) >> 8))/255.0 \
blue:((float)(rgbValue & 0xFF))/255.0 alpha:1.0]

- 判断系统版本[[UIDevice currentDevice].systemVersion doubeValue]
if ([[UIDevicecurrentDevice].systemVersiondoubleValue] >= 9.0) {

} else if ([[UIDevice currentDevice].systemVersion doubleValue] >= 8.0){

} else if ([[UIDevice currentDevice].systemVersion doubleValue] >= 7.0)

- 重新容错方法 方法里啥也不写
- (id)valueForUndefinedKey:(NSString *)key
{
return nil;
}

- (void)setValue:(id)value forUndefinedKey:(NSString *)key
{

}

- 当你自定义avplayer时，如果你程序设置有Exceptions breakpoint，就很容易运行到avplyer时就crash。吧breakpoint暂时去掉就好了。

- 用autolayout时，如何提前取得适配后的真实frame来做操作
-    __weak typeof（p） weakp ＝ p；或   __unsafe_unretained typeof（p） weakp ＝ p;

－ 对于sb 和xib 上的控件，自定义初始化方法为：initwithcoder， aweakformnib

- 图片剪裁
[imageView setContentMode:UIViewContentModeScaleAspectFill];
[imageView setClipsToBounds:YES];

- 视图跟随pop视图大小
[self setPreferredContentSize:popview.size];

-collection 中 self.view ＝ self.collection.superview；

- 要监听某视图内部按钮的点击事件，只有一个按钮时，可以用addtarget 方法  替代 代理方法；

- 自动布局插件mas  要先添加到父控件，然后才能设置。

[iconButton setAdjustsImageWhenDisabled:NO];
[iconButton setAdjustsImageWhenHighlighted:NO];

- button 设置内容对齐， contentMode 用于image。
[button setContentHorizontalAlignment:UIControlContentHorizontalAlignmentLeft];

－ 判断横竖屏，控制器才能使用这种方法判断，非控制器用尺寸来判断横竖屏。
if (UIInterfaceOrientationIsLandscape(toInterfaceOrientation))

－字符串截取从左到右 最前的那个生效

－只有init方法才能赋值


- 时间格式 NSDateFormatter @“EEE MMM dd HH:mm:ss Z yyyy"
E 星期
M 月
d 日
H 24小时制
m 分
s 秒
Z 时区
y 年

－ 还要设置时区 local ＝ [[NSLocale alloc] initWithLocaleIdentifier:@“zh_CN”]
// 获取当前时间
NSDate *date = [NSDate date];
//日历 对比功能
NSCalendar *calendar = [NSCalendar currentCalendar];
NSCalendarUnit *unit = NSCalendarUnitYear | NSCalendarUnitMonth | NSCalendarUnitDay | NSCalendarUnitHour | NSCalendarUnitMinute | NSCalendarUnitSecond ;
[calendar components:unit fromDate:date toDate:<#(NSDate *)#> options:0];

－ 设置发布时间 重写 get方法 即属性.creattime。
－ 在对比部分时间段时，只设置对比时间段的格式，忽略不需要对比的时间段；
- 设置deteformatter， 取出格式字符串 stringfromdate ；将字符串转会时间dateformstring，这样无关对比的时间段会置零；然后再调用对比功能 省着算；


－ 当控制器的view互为父子关系，控制器也要为父子关系，addchildviewcontroller

－cell 由于是重复调用， 所有设置属性 在本次调用必须重新设置，上次yes，这次就no;

- 让主线程分时间 执行time

NSTimer *time = [NSTimer scheduledTimerWithTimeInterval:5 target:self selector:@selector(<#selector#>) userInfo:nil repeats:YES];
[[NSRunLoop mainRunLoop] addTimer:time forMode:NSRunLoopCommonModes];

- description @20 number 转为 @“20” nsstring

－ self.XX和_XX
前者调用该类的setter或getter方法，后者直接获取自己的实例变量。

- loadView 自定义视图用这方法，最初创建view
如果view是从代码中创建 系统会调用initWithFrom
如果view是从文件中创建 （xib 和storyboard） 系统先调用initWithCoder   再调用awakeFromNib

自定义控件
所以 一般自定义初始化都是写的 initWithCoder 和initWithFrame

- MJExtension转模型第三方
－objectWithKeyValues:dict  //字典转模型
- replacedKeyFromPropertyName // 重命名 例：id 可以设置为 myid
－objectClassInArray{
return     @{@“books”:[wbook class]}; //数组里面有哪些类，数组里面的字典转模型
}


- 切换控制器的手段，insertSubView atIndex
1.push：pop：依赖于UINavigationController，控制器的切换是可逆的，比如A切换到B，B又可以回到A
2.modal（present／dismiss）
3.切换window的rootViewController，会销毁被抛弃的那个控制器
4、切换视图add 和 remove 只隐藏 不销毁
- [parentView exchangeSubviewAtIndex:I withSubviewAtIndex:j]
- 可以使用bringSubviewToFront:或者sendSubviewToBack:将视图前移或者后移.

- 使用setTag:给子视图添加标记.做这个标记可以在父视图中调用[parentView viewWithTag:]可以搜索到该标记的子视图.也可以作为一个区分的意思(当前太多视图或者控件时候).

－ 这方法是随机存储的
[[NSUserDefaults standardUserDefaults] setValue:currentVersion forKey:key];
//synchronize立即执行存入，
[[NSUserDefaults standardUserDefaults] synchronize];

- textfield  border 属性

- IQKeyboardManager

- EdgeInsets: 自切
contentEdgeInsets:会影响按钮内部的所有内容（里面的imageView和titleLabel）
shareBtn.contentEdgeInsets = UIEdgeInsetsMake(10, 100, 0, 0);

- titleEdgeInsets:只影响按钮内部的titleLabel
shareBtn.titleEdgeInsets = UIEdgeInsetsMake(0, 10, 0, 0);

- imageEdgeInsets:只影响按钮内部的imageView
shareBtn.imageEdgeInsets = UIEdgeInsetsMake(20, 0, 0, 50);

－kvc  ［［x valueForKeyPath:@“类名.@sum.类属性”］doublevalue］；

－ document 存备份文件、小文件
大文件存储在library － caches

－ initialize第一次使用这个类调用这个方法，只调用一次
－ load加载内存调用此方法
－ xib 的使用 注意 顺序 和 名字，修改xib 要删除缓存 clean一下。
－bool 默认值 是no
－图片 contentmode 属性


- 代理：子类设置代理 要先遵守父类代理，注意代理名 不要用delegate
1  .h
@protocol  类名＋delegate <父协议或基协议>
@optional
－（）方法()需要传递的对象；
@end
@property  （weak）id<协议名（ 即类名＋delegate）> lbdelegate

2  .m
if ([self.LBdelegate respondsToSelector:@selector(dropDownMenuDidShow:)]) {
[self.LBdelegate dropDownMenuDidShow:self];
}
3  在要成为代理方 set delegate：self，并遵守协议名，实现相关方法。


- tabbar 设置
-     Class class = NSClassFromString(@"UITabBarButton");
if ([childView isKindOfClass:class])
－//tabbar 是 ReadOnly，用kvc；
[self setValue:[[LBTabbar alloc] init] forKeyPath:@"tabBar"];

－图片 属性 内 slicing 可设置水平和垂直拉伸， 可设置center 平铺tiles 还是拉伸stretches。

－ 默认情况下fram 是以父视图为标准，要改变其为窗口位标准，就要使用坐标轴转换
//坐标轴转换 bounds 相对自己，frame 相对父控件
CGRect newFrame = [from convertRect:from.bounds toView:window];
//修改frame
CGRect frame = self.frame;
frame.origin.y = CGRectGetMaxY(newFrame) ;
frame.origin.x = CGRectGetMaxX(newFrame)* 0.3;
self.frame = frame;


- button内文字图片位置设置， 默认情况下都使居中现实
使用以下设置后：
[self setTitleEdgeInsets:UIEdgeInsetsMake( 0.0,-backGroundImag.size.width, 0.0,0.0)];
[self setImageEdgeInsets:UIEdgeInsetsMake(0.0, 0.0,0.0, -self.titleLabel.bounds.size.width)];

若要title在图片的上方，则位置相对于图片来说，向上移动－80
[self setTitleEdgeInsets:UIEdgeInsetsMake( -80.0,-backGroundImag.size.width, 0.0,0.0)];
[self setImageEdgeInsets:UIEdgeInsetsMake(0.0, 0.0,0.0, -self.titleLabel.bounds.size.width)];

效果如下：

综上所述，若单独设置一个title或者image在button中的位置，UIEdgeInsets是相对于button的frame来计算的（上，左，下，右，），如果是刚才所描述的情况，则title是相对于image的frame设置的，而image的位置是相对于titel的位置设置的


－[[ UIApplication shareApplication ].window lastObjuect] 获取最上面的window

- xib 加载 使用自定义标识符cell  在右边属性栏identifier 填写cell。

- dictionaryWithContentsOfFile 根据指定路径 返回 一个字典
- NSDictionary *account = [NSDictionary dictionaryWithContentsOfFile:path];
－自定义对象存储 沙盒用ns keyed archiver，
要遵守《nscoding》，
－encode withcode 方法是归档前 调用的方法，说明哪些属性要存入沙盒；
－write to file 是字典与数组的方法
- 若用 stringByAppendingString  则需要手动在名称前加 “/”符号，而stringByAppendingPathComponent则不需要，它会自动添加形成文件路径。

－剪裁图片：CGImageCreateWithImageInRect 方法；

－imageRectForContentRect方法可以改变button内图片尺寸;

－创建对象可以先看看有 share  deforce

－删掉工程中main.storyboard 后要删除plist文件中对应的键值，否则会报如下错误： Could not find a storyboard named 'Main' in bundle NSBundle

－删除main.storyboard后，需要在AppDelegate.m中初始化一个window进行使用，否则应用程序没有window可用。
self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];

- 设置图片渲染模式 imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal 原来的颜色

－ 设置字体颜色  NSForegroundColorAttributeName

- TextFieldView的leftview 默认不显示的
[self setLeftView:searchIcon];
[self setLeftViewMode:UITextFieldViewModeAlways];


1.pch文件设置
将building setting中的precompile header选项的路径添加“$(SRCROOT)/项目名称/pch文件名”（例如：$(SRCROOT)/LotteryFive/LotteryFive-Prefix.pch），将Precompile Prefix Header为YES，预编译后的pch文件会被缓存起来，可以提高编译速度

- ios 启动程序时隐藏状态栏，启动后显示状态栏
1、在info.plist里面  Status bar is initially hidden 设置为 YES
2、在appDelagate里面 设置
[application setStatusBarHidden:NO withAnimation:UIStatusBarAnimationFade]; 即可。

／／xcode 插件更新代码！！！
find ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins -name Info.plist -maxdepth 3 | xargs -I{} defaults write {} DVTPlugInCompatibilityUUIDs -array-add `defaults read /Applications/Xcode.app/Contents/Info.plist DVTPlugInCompatibilityUUID`

```



