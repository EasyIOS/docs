#Action Class Reference 

Action 负责网络请求的发起，设置请求的成功、失败和错误的状态及对应的回调方法


初始化：

	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
	    [Action actionConfigHost:@"maichong.08dream.com" client:@"easyIOS" codeKey:@"code" rightCode:0 msgKey:@"msg"];
	 }
    
    


===

##Tasks

  action `property`
  
* +Action:

* – initWithCache:

* – success:

* – error:

* – failed:

* – Send:

===

##Properties

###action 

	@property(nonatomic,strong)id<ActionDelegate> aDelegaete;

===
##Class Methods

###SceneModel 

	+ (id)Action;
	
生成一个实例的Action对象
	
##Instance Methods

###initWithCache: 

	- (id)initWithCache;
	
初始化对象，并且默认使用缓存
###success: 

	- (void)success:(Request *)msg;
设置请求的状态为成功，并且触发对应的回调
	
###error: 

	- (void)error:(Request *)msg;
设置请求的状态为失败，并且触发对应的回调
###failed: 

	- (void)failed:(Request *)msg;
	
设置请求的状态为失败，并且触发对应的回调
###Send 

	-(MKNetworkOperation*)Send:(Request *) msg;
	
发送请求，会根据msg中配置的方式（GET或POST）和参数发起相应的请求操作，请求被加入请求队列中。
