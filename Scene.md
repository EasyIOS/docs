#Scene Class Reference


Scene 继承UIViewController，EasyIOS里面仅仅负责视图的展示工作。


===
##Tasks

  parentScene `property`
  
* – showBarButton:title:fontColor:
* – showBarButton:imageName:
* – showBarButton:button:
* – leftButtonTouch
* – rightButtonTouch
* – setTitleText:
* – setTitleText:hidden:

===


##Properties

###parentScene 

	@property (nonatomic, retain) Scene *parentScene
	
===
##Instance Methods

###leftButtonTouch 

	- (void)leftButtonTouch
左按钮点击事件。
###rightButtonTouch 

	- (void)rightButtonTouch
右按钮点击事件。
###setTitleText: 

	- (void)setTitleText:(NSString *)str
//添加到导航条标题
###showBarButton:button: 

	- (void)showBarButton:(EzNavigationBar)position button:(UIButton *)button
	
设置导航条左右按钮，使用方法：

	[self showBarButton:NAV_RIGHT button:button]; //把某按钮设置为导航右按钮
	
###showBarButton:imageName: 

	- (void)showBarButton:(EzNavigationBar)position imageName:(NSString *)imageName
	
设置导航条左右按钮，使用方法：

	[self showBarButton:NAV_RIGHT imageName:@"image"]; //把某图片设置为导航右按钮
	
###showBarButton:title:fontColor: 

	- (void)showBarButton:(EzNavigationBar)position title:(NSString *)name fontColor:(UIColor *)color

设置导航条左右按钮，使用方法：

	[self showBarButton:NAV_LEFT title:@"left" fontColor:[UIColor blackColor]]; //设置左按钮为left，颜色为black



