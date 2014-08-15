#一、Model的对象映射操作
Model作为数据核心，功能不强大是不行的！
###Examples

* a.你有可能有这样的 JSON:

		{
		    "name": "Foo",
		    "age": 13
		}
	
* b.或者还有这样的模型类:

		#import "Friend.h"
		@interface Friend : Model
		@property (nonatomic, retain) NSString *name;
		@property (nonatomic, retain) NSNumber *age;
		@end
	
* c.利用Model的对象映射操作，就可以实现一键的转换：

		// Some other code
		NSString *json = /* json form net */;
		Friend *friend = [[Friend alloc] initWithString:json error:&err];
		
		// Log
		friend.name // => Foo
		friend.age // => 13

###还有更酷的！
* a.你有可能还有这样的 JSON:

		{
		    "name": "Foo",
		    "category": {
		        "name": "Bar Category"
		    }
		}

* b.或者还有这样的模型类:

		#import "FriendCategory.h"
		@interface FriendCategory : Model
		@property (nonatomic, retain) NSString *name;
		@end
		
* c.利用Model你可以这样：

		#import "Friend.h"
		#import "FriendCategory.h"
		
		@interface Friend : Model
		@property (nonatomic, retain) NSString *name;
		@property (nonatomic, retain) FriendCategory *category;
		@end
		
* d.同样可以一键转换

		// Code
		NSString *json = /* json form net */;
		Friend *friend = [[Friend alloc] initWithString:json error:&err];
		
		// Log
		friend.name // => Foo
		friend.category // => <FriendCategory>
		friend.category.name // => Bar Category
		
###遇上Array怎么办	?

* a.你有可能还有这样的 JSON:

		// JSON
		{
		    "name": "Foo",
		    "categories": [
		        { "name": "Bar Category 1" },
		        { "name": "Bar Category 2" },
		        { "name": "Bar Category 3" }
		    ]
		}
* b.利用Model你可以这样：

		#import "Friend.h"
		#import "FriendCategory.h"
		
		@protocol FriendCategory
		@end
		
		@interface Friend : Model
		@property (nonatomic, copy) NSString *name;
		@property (nonatomic, retain) NSArray<FriendCategory> *categories;
		@end
		

* c.同样可以一键转换
 
		// Code
		NSString *json = /* json form net */;
		Friend *friend = [[Friend alloc] initWithString:json error:&err];
		
		// Log
		friend.name // => Foo
		friend.categories // => <NSArray>
		[friend.categories count] // => 3
		[friend.categories objectAtIndex:1] // => <ProductCategory>
		[[friend.categories objectAtIndex:1] name] // => Bar Category 2

	
##然后你就可以乱来了。。。

		// JSON
		{
		    "name": "1",
		    "children": [
		        { "name": "1.1" },
		        { "name": "1.2",
		          children: [
		            { "name": "1.2.1",
		              children: [
		                { "name": "1.2.1.1" },
		                { "name": "1.2.1.2" },
		              ]
		            },
		            { "name": "1.2.2" },
		          ]
		        },
		        { "name": "1.3" }
		    ]
		}
##还没完呢！！！

* a.擦，这个json不是驼峰！强迫症！服务器没节操！

		{
		    "first_name": "John",
		    "last_name": "Doe"
		}
		
* b.你可以这样

		// Person.h
		@interface Person : Model
		@property (nonatomic, copy) NSString *firstName;
		@property (nonatomic, copy) NSString *lastName;
		@end
		
		// Person.m
		@implementation Person
		
		+(JSONKeyMapper*)keyMapper
		{
		  return [[JSONKeyMapper alloc] initWithDictionary:@{
		    @"user.first_name": @"firstName",
		    @"user.last_name": @"lastName"
		  }];
		}
		
		@end
		
* c.根本停不下来

		// Code
		NSDictionary *dictionary = /* parse the JSON response to a dictionary */;
		Person *person = [[Person alloc] initWithDictionary:dictionary];
		
		// Log
		person.firstName // => John
		person.lastName // => Doe
		
##服务器有时候给了null

* a.服务器有时候给了null

		{
		  "id": "123",
		  "name": null,
		  "price": 12.95
		}
	或者干脆不给了。
	
		{
		  "id": "123",
		  "price": 12.95
		}
		
* b.你可以这样设置Optional

		@interface ProductModel : JSONModel
		@property (assign, nonatomic) int id;
		@property (strong, nonatomic) NSString<Optional>* name;
		@property (assign, nonatomic) float price;
		@property (strong, nonatomic) NSNumber<Optional>* uuid;
		@end
		
		@implementation ProductModel
		@end
* c.全部设为Optional

		@implementation ProductModel
		+(BOOL)propertyIsOptional:(NSString*)propertyName
		{
		  return YES;
		}
		@end

		
##服务器有时候给了用不到的	

* a.服务器有时候给多了。

		{
		  "id": "123",
		  "a": "123",
		  "b": "123",
		  "c": "123"
		}
		
* b.你可以这样设置Ignore,来减少客户端工作量

		@interface ProductModel : JSONModel
		@property (assign, nonatomic) int id;
		@property (strong, nonatomic) NSString<Ignore>* a;
		@property (strong, nonatomic) NSString<Ignore>* b;
		@property (strong, nonatomic) NSString<Ignore>* c;
		@end
		
		@implementation ProductModel
		@end


##如果你更懒一点.h都不想写。。。。

* 这里有个[懒人工具ModelCoder](https://github.com/zhuchaowe/ModelCoder)

##Using the built-in thin HTTP client

	//add extra headers
	[[JSONHTTPClient requestHeaders] setValue:@"MySecret" forKey:@"AuthorizationToken"];
	
	//make post, get requests
	[JSONHTTPClient postJSONFromURLWithString:@"http://mydomain.com/api"
	                                   params:@{@"postParam1":@"value1"}
	                               completion:^(id json, JSONModelError *err) {
	
	                                   //check err, process json ...
	
	                               }];
                               
###Export model to NSDictionary or to JSON text

	ProductModel* pm = [[ProductModel alloc] initWithString:jsonString error:nil];
	pm.name = @"Changed Name";
	
	//convert to dictionary
	NSDictionary* dict = [pm toDictionary];
	
	//convert to text
	NSString* string = [pm toJSONString];



#二、Model的DataDase数据库操作

##1.创建一个数据库并连接

* (1).在应用的`AppDelegate.h`文件声明一个数据库实例：

		@class MojoDatabase;
		
		@interface MyAppDelegate : NSObject <UIApplicationDelegate> {
		  MojoDatabase *database;
		}
		
		@property (nonatomic, retain) MojoDatabase *database;
		
		@end

* (2).在代理方法`didFinishLaunchingWithOptions`里初始化这个数据库:

		#import "AppDatabase.h"
	
		@implementation MyAppDelegate
		
		-(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
		  // Setup Connection to the database
		  self.database = [[AppDatabase alloc] initWithMigrations];
		}
		@end


	这样就会初始化（如果不存在就会自动创建）一个数据库，如果你想自定义数据库的名字可以在`AppDatabase.m`文件中修改.

##2.创建一个数据模型

* (1).每一个数据模型都需要在数据库里面有对应的数据表，因此你需要在 `AppDatabase`的子类中创建对应的数据表：

		-(void)createFriendTable {
			 [self executeSql:@"CREATE TABLE Friend (primaryKey INTEGER primary key autoincrement, name TEXT, age INTEGER)"];
		}

* (2).你还需要有一个和数据表名称一致的数据模型Model的子类Friend：

		#import "Model.h"
		@interface Friend : Model
		@property (nonatomic, retain) NSString *name;
		@property (nonatomic, retain) NSNumber *age;
		@end
	
* (3).然后你就可以像这样轻松的插入一条数据到Friend数据表了：

		Friend *newFriend = [[Friend alloc] init];
		newFriend.name = @"Norman Rockwell";
		newFriend.age = [NSNumber numerWithInteger:55];
		[newFriend save];  // this will write out the new friend record to the databse
* (4).从Friend表中查询数据，同样也很Easy的啦：

		NSArray *records = [self findByColumn:@"name" value:@"Norman Rockwell"];
		if ( [records count] ) {
		  Friend *normanRockwell = [records objectAtIndex:0];
		}
备注：findByColumn 总是返回一个NSArray对象.
