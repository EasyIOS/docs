Scene 继承UIViewController，EasyIOS里面仅仅负责视图的展示工作。

Scene.h 文件里提供了如下接口：

    - (void)handlePullLoader:(MJRefreshBaseView *)view state:(NSInteger)state;
    - (void)showBarButton:(EzNavigationBar)position title:(NSString *)name fontColor:(UIColor *)color;
    - (void)showBarButton:(EzNavigationBar)position imageName:(NSString *)imageName;
    - (void)showBarButton:(EzNavigationBar)position button:(UIButton *)button;
    - (void)leftButtonTouch;//左按钮点击事件。
    - (void)rightButtonTouch;//右按钮点击事件。
    - (void)setTitleText:(NSString *)str;//添加到导航条标题


- (void)handlePullLoader:(MJRefreshBaseView *)view state:(NSInteger)state;

这是下拉刷新和上拉加载模块的代理方法。需要在继承Scene类的- (void)viewDidLoad里面添加以下代码

    self.collectionView.SceneDelegate = self; //需要下拉刷新或者上拉加载需要设置代理
    self.collectionView.backgroundColor = [UIColor whiteColor];
    [self.collectionView addHeader];   //添加下拉刷新
    [self.collectionView addFooter];   //添加上拉加载
    [self.collectionView initPage]; //多页数据初始化分页
    
Scene子类需要实现handlePullLoader:state:方法进行对page的改变。
    
 - (void)showBarButton:(EzNavigationBar)position title:(NSString *)name fontColor:(UIColor *)color;
 - (void)showBarButton:(EzNavigationBar)position button:(UIButton *)button;
 
 设置导航条左右按钮的快捷方法，使用示例：
 
    [self showBarButton:NAV_LEFT title:@"left" fontColor:[UIColor blackColor]]; //设置左按钮为left，颜色为black
    [self showBarButton:NAV_RIGHT button:button]; //把某按钮设置为导航右按钮