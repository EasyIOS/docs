#Request

---
Request为初始化网络请求数据的类

* ###GET

	如果你想发这样一个GET请求
`http://test-leway.zjseek.com.cn:8000/api/goods/goodsList?type=1&pageSize=10&page=1&categoryId=-1&areaName=浙江`

	你可以这样定义一个GoodsListRequest

		#import "Request.h"
		@interface GoodsListRequest : Request
		@property(nonatomic,retain) NSString *type;
		@property(nonatomic,retain) NSNumber *page;
		@property(nonatomic,retain) NSNumber *pageSize;
		@property(nonatomic,retain) NSString *categoryId;
		@property(nonatomic,retain) NSString *areaName;
		@end
		
		
		#import "GoodsListRequest.h"
		@implementation GoodsListRequest
		/**
		 *  初始化 GoodsListRequest 的参数
		 */
		-(void)loadRequest{
		    [super loadRequest];
		    //self.HOST = @"test-leway.zjseek.com.cn:8000"; 不设置host 就采用宏定义的全局host
		    self.PATH = @"/api/goods/goodsList";
		    self.METHOD = @"GET";
		    self.type = @"1";
		    self.categoryId = @"-1";
		    self.areaName = @"浙江";
		    self.pageSize = @10;
		}
		@end
		
	loadRequest方法里面初始化HOST，PATH，METHOD以及各种变量。EasyIOS会自动进行拼接为`http://test-leway.zjseek.com.cn:8000/api/goods/goodsList?type=1&pageSize=10&page=1&categoryId=-1&areaName=浙江`
	
* ###POST
	如果你想POST文件
	你可以这样定义一个ImagePostRequest
	
		#import "Request.h"
		@interface ImagePostRequest : Request
		@property(nonatomic,retain) NSString *name;
		@end
		
		
		#import "ImagePostRequest.h"
		@implementation ImagePostRequest
		/**
		 *  初始化 GoodsListRequest 的参数
		 */
		-(void)loadRequest{
		    [super loadRequest];
		    //self.HOST = @"test-leway.zjseek.com.cn:8000"; 不设置host 就采用宏定义的全局host
		    self.PATH = @"/api/goods/goodsList";
		    self.METHOD = @"POST"; //设置为post
		    self.name = @"图片";
		}
		
		-(NSDictionary *)requestFiles{
		    NSString *imageName1 = @"face1.png";
		    NSString *localPath1 = [NSString stringWithFormat:@"%@/%@",$.documentPath,imageName1];
		    
		    NSString *imageName2 = @"face2.png";
		    NSString *localPath2 = [NSString stringWithFormat:@"%@/%@",$.documentPath,imageName];
		    
		    return @{@"image1":localPath1,
		    		 @"image2":localPath2};
		}
		@end
	
	requestFiles方法里面返回一个文件字典。EasyIOS会自动进行识别上传
	