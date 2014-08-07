##SceneModel Class Reference 


##Tasks

  action `property`
  
* +SceneModel

* – handleActionMsg:

* – SEND_ACTION:

* – SEND_CACHE_ACTION:

* – SEND_NO_CACHE_ACTION:

* – initModel

##Properties

###action 

	@property (nonatomic, strong) Action *action
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
