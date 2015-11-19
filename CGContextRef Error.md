
# CGContextRef Error报错


报错例子：
----

> Nov 19 16:49:04 iPhone ImageDemo[22667] <Error>: CGContextSaveGState: invalid context 0x0. This is a serious error. This application, or a library it uses, is using an invalid context  and is thereby contributing to an overall degradation of system stability and reliability. This notice is a courtesy: please fix this problem. It will become a fatal error in an upcoming update.


最终搜索到的**原因**是：
----

文档中提到，系统会维护一个**CGContextRef的栈**。而UIGraphicsGetCurrentContext()会取栈顶的CGContextRef。

正确的做法是，**只在drawRect**里调用UIGraphicsGetCurrentContext()。  

因为在drawRect之前，系统会往栈里面压入一个valid的CGContextRef，
除非自己去维护一个CGContextRef，否则不应该在其他地方取CGContextRef。

所以，在initWithFrame或其他方法中调用UIGraphicsGetCurrentContext()，都会报“invalid context”的error。