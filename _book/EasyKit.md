#EasyKit Class Reference


EasyKit 里面有很多实用便捷的小功能。😊 统一用$符号调用

+ (NSString *)homePath;//应用程序目录的路径，在该目录下有三个文件夹：Documents、Library、temp以及一个.app包！该目录下就是应用程序的沙盒，应用程序只能访问该目录下的文件夹！！！
+ (NSString *)desktopPath;//数据所存桌面的绝对路径
+ (NSString *)documentPath;// 文档目录，需要ITUNES同步备份的数据存这里
+ (NSString *)libPrePath; // 配置目录，配置文件存这里
+ (NSString *)libCachePath; // 缓存目录，系统永远不会删除这里的文件，ITUNES会删除
+ (NSString *)appPath;  // .app 程序相对目录，不能存任何东西
+ (NSString *)tmpPath;	// 缓存目录，APP退出后，系统可能会删除这里的内容
+ (NSString *)resourcePath; // .app 程序绝对目录，不能存任何东西
+ (BOOL)touchPath:(NSString *)path;//创建路径
+ (BOOL)touchFile:(NSString *)file;//创建文件

+ (RACSignal*) rac_didNetworkChanges （rac监控网络状态的改变。）

	使用示例：

	    [[$ rac_didNetworkChanges]
	     subscribeNext:^(Reachability* reach) {
	         if([reach isReachableViaWiFi]){
	             EZLog(@"正在使用Wi-Fi网络");
	         }else if ([reach isReachableViaWWAN]){
	             EZLog(@"正在使用移动数据网络");
	         }else{
	             EZLog(@"网络连接已断开");
	         }
	     }];