
##AutoLayoutCell

我们在实际开发中肯定遇到过自适应Cell的需求

####在IOS6或者更早以前
在IOS6或者更早以前，大都是通过程序计算出cell得高度，在进行渲染。
可是实际开发中使用`UITableView`，我们发现，`UITableView`是先请求Cell的高度再去渲染视图，这就需要我们在请求高度的时候模拟渲染获得高度，或者根据图片大小或者字符串长度计算出Cell的最小高度。这样就导致了计算不准确等等问题。

####在IOS8
IOS8中,`UITableView`有了self sizing cells 这样一个功能来解决动态Cell的问题。

为了兼容IOS7，并且更简单的实现动态Cell，EasyIOS封装了基于Autolayout的动态Cell布局的功能。

下面是一个例子：

* 首先我们需要声明一个ALTableView
```
    self.tableView = [[ALTableView alloc]init];
    self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
    self.tableView.delegate = self;
    self.tableView.dataSource = self;
    self.tableView.cellFactory.delegate = self;
    self.tableView.estimatedRowHeight = 250.0f;
    self.tableView.rowHeight = UITableViewAutomaticDimension;

```
*  实现`UITableViewDataSource`

```
- (CGFloat)tableView:(ALTableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    return [tableView.cellFactory cellHeightForIdentifier:@"RssCell" atIndexPath:indexPath];
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return self.rssSceneModel.dataArray.count;
}

- (UITableViewCell *)tableView:(ALTableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    return [tableView.cellFactory cellWithIdentifier:@"RssCell" forIndexPath:indexPath];
}
```

* 实现`ALTableViewCellFactoryDelegate`代理

```
- (void)tableView:(UITableView *)tableView configureCell:(RssCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath
{
    cell.delegate = self;
    [cell reloadRss:[self.rssSceneModel.dataArray objectAtIndex:indexPath.row]
               with:self.rssSceneModel.rssList.feed];
}
```
这里仅仅是导入需要渲染的数据

* 声明一个`ALBaseCell`的子类
```
@interface RssCell : ALBaseCell
@end
```

*  `ALBaseCell`子类的初始化入口

```
-(void)commonInit{
    [super commonInit];
    //在这里添加需要的控件到self.contentView
    [self.contentView addSubview:_feedTitle];
    [self loadAutolayout];
}
-(void)loadAutolayout{
    //这里使用AutoLayout对Cell进行布局
}
```
当然上面这些，都可以在xib中完成:)，特别要注意最底部的控件 要设置他与bottom的关系，否则ContentView无法知道他的最小高度比如
```
[_lastView alignBottom:@"-5" toView:self.contentView];
```
* OK,现在就可以实现动态的Cell了，具体实现可以参见EasyRSS这个项目

