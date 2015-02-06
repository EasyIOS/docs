* Scenmode是怎么出发request 调用action的呢?demo里是scene观察tableview的page 
	
		scenemodel有一个成员属性叫action，scenemodel会把你的request提交给action,观察page是因为有个分页
* 触发的时机呢
	
		想要发起网络请求就[self SEND_NO_CACHE_ACTION:_goodsListRequest];
		
* 如果viewdidload里我不初始化tableview的page 是不会请求网络数据

		网络请求和page没关系。。。看你哪个页面要不要分页，如果不用，就没有page啊。如果有分页，那么我只要把page从1变成2，那么数据就来了，不是很爽么
		

* scene里声明了成名变量scenemodel 然后我在viewdidload里调用`[scenemodel SEND_NO_CACHE_ACTION]` 这样可行？

		这样是可以的。。不过就违背了mvvm思想,而且你在viewdidload里面调用，要是你别的地方还想要调用，就又要SEND_NO_CACHE_ACTION一次
  		

* 但是我要按mvvm的思想要做的话我是不是要在scenemodel里声明个变量然后我在viewdidload里改变scenemodel的变量来触发scenemodel调用 SEND_NO_CACHE_ACTION

		是的
		
* 但这个变量我好像没啥意义 只是为了触发才这么做？

		是的！，ReactiveViewModel，也是这样的，它定义了一个叫active属性，只要active是yes就干活,这个变量easyios里面没有定义.
		如果你实在是没有这个状态变量，就仿照ReactiveViewModel，定义一个active属性，每次修改成YES，执行完成后改成NO 
		
* [ReactiveViewModel项目地址](https://github.com/ReactiveCocoa/ReactiveViewModel)

* `RAC(self.homeSceneModel.request,page) = RACObserve(self.tableView,page);`这个大概是啥意思 没看懂

		观察 self.tableView,page 如果有修改，就同步到 self.homeSceneModel.request,page


