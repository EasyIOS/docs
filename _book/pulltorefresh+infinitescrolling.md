#SVPullToRefresh + SVInfiniteScrolling

1.addPullToRefreshWithActionHandler
	    
	[self.tableView addPullToRefreshWithActionHandler:^{
           self.sceneModel.request.page = @1;
           self.sceneModel.active = YES;
    }];

2.addInfiniteScrollingWithActionHandler

    [self.tableView addInfiniteScrollingWithActionHandler:^{
            self.sceneModel.request.page = [self.sceneModel.request.page increase:@1];
            self.sceneModel.active = YES;
    }];

3.触发刷新

	 [self.tableView triggerPullToRefresh];
	 [self.tableView triggerInfiniteScrolling];
4.停止所有刷新动作

	[tableView endAllRefreshing];

5.自动分页（需要Pagination）类

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
	
	                                             
                                             


