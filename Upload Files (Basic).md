#上传文件

<br>
无论是用NSURLConnection还是NSURLSession，上传小文件（包括不大的Image这种）到服务器，都需要在发出的request中设置特定的Header。并且把要传输的文件数据，放在request的body中，iOS中的数据类型为NSData。
<br>

Request Header里面的设置
----
1. **HTTPMethod必须为`POST`**。因为向服务器发出的请求为传输数据到服务器，其实，那些有数据要传给服务器的request，理论上来说最好都是POST。

		request.HTTPMethod = @"POST"
	
	
2. 添加`Content-Length`字段，值为要传输的body内容的数据长度

		NSString *length = [NSString stringWithFormat:@"%lu", (unsigned long)bodyData.length];
		
		[request setValue:length forHTTPHeaderField:@"Content-Length"];	
3. Header中设置`Content-Type`字段，和普通以json格式call服务器不同，这里用的是`multipart/form-data`,并且需要添加一个分割字符串`boundry`，用来区分body中的块。这个字符串没有硬性规定是什么，只是要和body中使用的`boundry`一致，不然后台就无法对传说过去的数据进行分割。

		NSString *contentType = [NSString stringWithFormat:@"multipart/form-data; boundary=%@", boundry];
		
		[request setValue:contentType forHTTPHeaderField:@"Content-Type"];
<br>

Request Body的设置
----

上传文件的body有一个固定的格式：

1. 第一行：以`--boundy string`为`开头`(boundry为具体的字符串，此处仅用作示意)，表示开始一段需要服务器接受的数据。
2. **第二行**: Content-Disposition: form-data; name=文件的名字; filename=要存储在服务器上的文件名
3. 第三行：空白
4. 第四行：Content-Type: application/octet-stream
5. 第五行：空白
6. 第六行：文件数据，NSData

这样就是一块Complete！最后要加上`结尾`，和`开头`不一样，是多加两个--，`--boundry string--`

如果有多个文件在一个Request中传输，那就在`结尾`之前重复1-6。

比如：

	--TeXtBoundryGa
	Content-Disposition: form-data; name=Test; filename=Test.jpg
		
	Content-Type: application/octet-stream
		
	文件内容
	--TeXtBoundryGa--

<br>多个(未证实😜)
	
	--TeXtBoundryGa
	Content-Disposition: form-data; name=Test1; filename=Test1.jpg
		
	Content-Type: application/octet-stream
		
	文件内容
	--TeXtBoundryGa
	Content-Disposition: form-data; name=Test2; filename=Test2.jpg
		
	Content-Type: application/octet-stream
		
	文件内容
	--TeXtBoundryGa--
	
**NOTE，NSString和NSData，需要用UTF8转一下**

	NSData *data = [str dataUsingEncoding:NSUTF8StringEncoding]; 

	NSString *str= [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
 

NSURLSession实现上传
----
调用NSURLSession的方法：
 		
	- (NSURLSessionUploadTask *)uploadTaskWithRequest:(NSURLRequest *)request fromData:(nullable NSData *)bodyData completionHandler:(void (^)(NSData * __nullable data, NSURLResponse * __nullable response, NSError * __nullable error))completionHandler;
最后一个参数就是完成时候的回调block，包括成功和出现error的情况。 使用的task为NSURLSessionUploadTask，
