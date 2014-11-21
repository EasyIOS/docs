#SVPullToRefresh + SVInfiniteScrolling

These UIScrollView categories makes it super easy to add pull-to-refresh and infinite scrolling fonctionalities to any UIScrollView (or any of its subclass). Instead of relying on delegates and/or subclassing `UIViewController`, SVPullToRefresh uses the Objective-C runtime to add the following 3 methods to `UIScrollView`:

	- (void)addPullToRefreshWithActionHandler:(void (^)(void))actionHandler;
	- (void)addPullToRefreshWithActionHandler:(void (^)(void))actionHandler position:(SVPullToRefreshPosition)position;
	- (void)addInfiniteScrollingWithActionHandler:(void (^)(void))actionHandler;
	
	
## Usage

### Adding Pull to Refresh

```
[tableView addPullToRefreshWithActionHandler:^{
    // prepend data to dataSource, insert cells at top of table view
    // call [tableView.pullToRefreshView stopAnimating] when done
}];
```
or if you want pull to refresh from the bottom

```
[tableView addPullToRefreshWithActionHandler:^{
    // prepend data to dataSource, insert cells at top of table view
    // call [tableView.pullToRefreshView stopAnimating] when done
} position:SVPullToRefreshPositionBottom];
```

If you’d like to programmatically trigger the refresh (for instance in `viewDidAppear:`), you can do so with:

```
[tableView triggerPullToRefresh];
```

You can temporarily hide the pull to refresh view by setting the `showsPullToRefresh` property:

```
tableView.showsPullToRefresh = NO;
```

#### Customization

The pull to refresh view can be customized using the following properties/methods:

```
@property (nonatomic, strong) UIColor *arrowColor;
@property (nonatomic, strong) UIColor *textColor;
@property (nonatomic, readwrite) UIActivityIndicatorViewStyle activityIndicatorViewStyle;

- (void)setTitle:(NSString *)title forState:(SVPullToRefreshState)state;
- (void)setSubtitle:(NSString *)subtitle forState:(SVPullToRefreshState)state;
- (void)setCustomView:(UIView *)view forState:(SVPullToRefreshState)state;
```

You can access these properties through your scroll view's `pullToRefreshView` property.

For instance, you would set the `arrowColor` property using:

```
tableView.pullToRefreshView.arrowColor = [UIColor whiteColor];
```

### Adding Infinite Scrolling

```
[tableView addInfiniteScrollingWithActionHandler:^{
    // append data to data source, insert new cells at the end of table view
    // call [tableView.infiniteScrollingView stopAnimating] when done
}];
```

If you’d like to programmatically trigger the loading (for instance in `viewDidAppear:`), you can do so with:

```
[tableView triggerInfiniteScrolling];
```

You can temporarily hide the infinite scrolling view by setting the `showsInfiniteScrolling` property:

```
tableView.showsInfiniteScrolling = NO;
```

#### Customization

The infinite scrolling view can be customized using the following methods:

```
- (void)setActivityIndicatorViewStyle:(UIActivityIndicatorViewStyle)activityIndicatorViewStyle;
- (void)setCustomView:(UIView *)view forState:(SVInfiniteScrollingState)state;
```

You can access these properties through your scroll view's `infiniteScrollingView` property. 

## Under the hood

SVPullToRefresh extends `UIScrollView` by adding new public methods as well as a dynamic properties. 

It uses key-value observing to track the scrollView's `contentOffset`.


## EasyIOS 新增方法

1.停止所有刷新动作

	[tableView endAllRefreshing];

2.自动分页（需要Pagination）类


	Pagination *pagination = [Pagination Model];
	pagination.page = @2;
	pagination.pageSize = @10;
	pagination.total = @19;
	pagination.isEnd = @1;
	
	self.tableview.pagination = pagination;
	
	self.dataArray = [pagination
					success:self.dataArray
	                newArray:self.sceneModel.list];
	                
	[self.tableView endAllRefreshing];
	
	                                             
                                             


