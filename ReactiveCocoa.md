
为什么RAC更加适合编写Cocoa App？说这个之前，我们先来看下Web前端编程，因为有些相似之处。目前很火的AngularJS有一个很重要的特性：数据与视图绑定。就是当数据变化时，视图不需要额外的处理，便可正确地呈现最新的数据。而这也是RAC的亮点之一。RAC与Cocoa的编程模式，有点像AngularJS和jQuery。所以要了解RAC，需要先在观念上做调整。

RAC进行数据绑定的例子：

		// When self.username changes, logs the new name to the console.
		//
		// RACObserve(self, username) creates a new RACSignal that sends the current
		// value of self.username, then the new value whenever it changes.
		// -subscribeNext: will execute the block whenever the signal sends a value.
		[RACObserve(self, username) subscribeNext:^(NSString *newName) {
		    NSLog(@"%@", newName);
		}];
当每次self.username改变，都会自动执行subscribeNext，NSLog内容到控制台

##Swift 与 Objective-C 中的使用区别:

设置请看 [RACSwift](https://github.com/zhuchaowe/RACSwift)

`RAC` In `Objective－C`:

	RAC(self.collectionView,page) = RACObserve(self.collectionView,page);

In `Swift`:

	RAC(self.collectionView,"page") <= RACObserve(self.collectionView,"page")
	RACObserve(self.collectionView,"page") => RAC(self.collectionView,"page")
	
___
    
`RACObserve` In `Objective－C`:
	
        [[[RACObserve(self.collectionView, page)
         map:^NSString*(NSNumber* newPage) {
             return @"123";
         }]
         filter:^BOOL(NSString* newPage) {
             return false;
         }]
         subscribeNext:^(NSString* text) {
             NSLog(@"%@",text);
         }];

In `Swift`:

        RACObserve(self.collectionView,"page")
        .mapAs{
            (newpage:NSNumber) -> NSString in
            return "123"
        }
        .filterAs{
            (newpage:NSString) -> Bool in
            return false
        }
        .subscribeNextAs{
            (text:String) -> () in
            println(text)
        }        
        