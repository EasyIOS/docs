#SceneModel Class Reference 

SceneModel 负责数据的绑定，控制视图更新，网络请求的发起，以及Model的生成工作

===

##Tasks

  action `property`
  
* +SceneModel

* – handleActionMsg:

* – SEND_ACTION:

* – SEND_CACHE_ACTION:

* – SEND_NO_CACHE_ACTION:

* – initModel

===

##Properties

###action 

	@property (nonatomic, strong) Action *action

===
##Class Methods

###SceneModel 

	+ (id)SceneModel
	
##Instance Methods

###SEND_ACTION: 

	- (void)SEND_ACTION:(NSDictionary *)dict
###SEND_CACHE_ACTION: 

	- (void)SEND_CACHE_ACTION:(NSDictionary *)dict
###SEND_NO_CACHE_ACTION: 

	- (void)SEND_NO_CACHE_ACTION:(NSDictionary *)dict
###handleActionMsg: 

	- (void)handleActionMsg:(ActionData *)msg
###initModel 

	- (id)initModel
