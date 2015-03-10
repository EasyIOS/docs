* 生成一个图形按钮方法

		NSString* icon = [IconFont icon:@"fa_angle_down" fromFont:fontAwesome];//获取字符
		
		UIButton *button = [IconFont buttonWithIcon:icon fontName:fontAwesome size:15.0f color:[UIColor blackColor]];//生成按钮
		

字体名称`fa_angle_down` 可以在demo字典中查找到：
目前提供了4种图片字体下面是他们的弘定义：

	#define fontAwesome @"FontAwesome"
	#define zocialRegularWebfont @"Zocial-Regular"
	#define ionIcons @"Ionicons"
	#define foundationIcons @"fontcustom"