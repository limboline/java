## 1、JavaSE

#### <span style='color:red'>1）HashMap</span>

Node[] table的初始化长度length(默认值是16)，

Load factor为负载因子(默认值是0.75)，

threshold是HashMap所能容纳的最大数据量的Node(键值对)个数。threshold = length * Load factor。也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数越多。

![HashMap如何确定元素位置](https://img-blog.csdnimg.cn/20181102214046362.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019072810592539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNTc0NTcx,size_16,color_FFFFFF,t_70)

**1.7和1.8的区别**

1. jdk1.7底层采用entry数组+链表的数据结构，而1.8采用node数组+链表/红黑树的数据结构。

2. jdk1.7的HashMap插入新值时使用头插法，1.8使用尾插法。

   使用头插法比较快，但在多线程扩容时会引起**倒序**和**闭环**的问题。所以1.8就采用了尾插法。

3. 扩容后新表中的索引位置计算方式不同，jdk1.7扩容时是将**旧表元素的所有数据重新进行哈希计算**，即hashCode & (length-1)。而1.8中扩容时只需将hashCode和老数组长度做与运算判断是0还是1，是0的话**索引不变**，是1的话索引变为**老索引位置+老数组长度**。

**扩容为什么是2的n次方**

1. 插入新元素确定索引位置的时候是采用key的hashCode和数组长度做与运算，即hashCode&(length-1)。模拟的是取模运算，但速度比取模快很多，要保证这种运算的正确性，必须要求数组的长度是2的n次方。

2. 在扩容时进行新索引位置的计算时也要求数组长度是2的n次方。

#### <span style='color:red'>2）内部类</span>

|                            |            成员内部类            |         静态内部类         |    局部内部类    |    匿名内部类    |
| :------------------------- | :------------------------------: | :------------------------: | :--------------: | :--------------: |
| 外部类访问内部类（静态）   |            不能有静态            |    new Inner().method()    |    不能有静态    |    不能有静态    |
| 外部类访问内部类（非静态） |      需要new一个内部类对象       |       Inner.method()       |     不能访问     |     不能访问     |
| 内部类访问外部类（静态）   |             随意访问             |        只能访问静态        |     随意访问     |     随意访问     |
| 内部类访问外部类（非静态） |             随意访问             |          不能访问          | 不用final了(1.8) | 不用final了(1.8) |
| 外界访问内部类（静态）     |            不能有静态            |    Outer.Inner.method()    |     访问方法     |     访问方法     |
| 外界访问内部类（非静态）   | new Outer().new Inner().method() | new Outer().Inner.method() |     访问方法     |     访问方法     |

#### <span style='color:red'>3）多态、重载、重写</span>

多态（狭义上的）：同一个方法对不同的对象调用行为不同的现象。

重写：同一方法在不同类中的重新实现。

重载：方法名必须是一样的，可以参数类型，参数个数，参数顺序不一致。

#### <span style='color:red'>4）Object的常用方法</span>

1. public boolean **equals**(java.lang.Object) 比较内容

2. public native int **hashCode**() 哈希码

3. public java.lang.String **toString**() 变成字符串

4. public final native java.lang.Class **getClass**() 获取类结构信息

5. protected void **finalize**() throws java.lang.Throwable 垃圾回收前执行的方法

6. protected native Object **clone**() throws java.lang.CloneNotSupportedException 克隆

7. public final void **wait**() throws java.lang.InterruptedException 多线程中等待功能

8. public final native void **notify**() 多线程中唤醒功能

9. public final native void **notifyAll**() 多线程中唤醒所有等待线程的功能

**其中clone方法是浅拷贝，要实现深拷贝需要对该类重写clone方法，首先继承Cloneable接口然后重写clone方法为super.clone()**

但是该方法也不是彻底的深拷贝，彻底的深拷贝不存在，只能对要实现深拷贝的类重写clone方法。

![img](https://img-blog.csdn.net/20140119225541718?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemhhbmdqZ19ibG9n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### <span style='color:red'>5）权限修饰符</span>

|                  |  public  |    protected     |   默认   | private  |
| ---------------- | :------: | :--------------: | :------: | :------: |
| 同一类中成员     | 可以访问 |     可以访问     | 可以访问 | 可以访问 |
| 同一包中其他成员 | 可以访问 |     可以访问     | 可以访问 |          |
| 不同包中的子类   | 可以访问 | 可以访问（继承） |          |          |
| 不同包中非子类   | 可以访问 |                  |          |          |

public   使用对象：最广，类、接口、变量、方法。

protected使用对象：变量、方法 注意：不能修饰类(外部类)。

default  使用对象：类、接口、变量、方法。(即缺省，什么也不写)。

private  使用对象：变量、方法 注意：不能修饰类(外部类)。

abstract使用对象：类、接口、方法。

static  使用对象：类、变量、方法、初始化函数(注意：修饰类时只能修饰 内部类 )。

final   使用对象：类、变量、方法。

transient：告诉编译器，在类对象序列化的时候，此变量不需要持久保存

volatile：指出可能有多个线程修改此变量，要求编译器优化以保证对此变量的修改能够被正确的处理。

native：用该修饰符定义的方法在类中没有实现，而大多数情况下该方法的实现是用C、C++编写的。

synchronized：修饰方法，多线程的支持。

#### <span style='color:red'>6）异常</span>

![img](https://raw.githubusercontent.com/limboline/PicBed/master/img/20210520173254.jpg)

Throwable类有两个直接子类：

（1）**Exception**：出现的问题是可以被捕获的

（2）**Error**：系统错误，通常由JVM处理

Exception分为两类

（3）**Check异常**: **编译器会检查此类异常**，如果程序中出现此类异常，比如说IOException，必须对该异常进行处理，要么使用try-catch捕获，要么使用throws语句抛出，否则编译不通过。

（4）**Runtime异常**：RuntimeException类极其子类表示JVM在运行期间可能出现的错误。**编译器不会检查此类异常**，并且不要求处理异常，比如用空值对象的引用（NullPointerException）、数组下标越界（ArrayIndexOutBoundException）。此类异常属于不可查异常，一般是由程序逻辑错误引起的，在程序中可以选择捕获处理，也可以不处理。

<span style='color:red'>**常见异常**</span>

**java.lang.NullpointerException(空指针异常)**

 **java.lang.ClassNotFoundException（指定的类不存在）**

**java.lang.IndexOutOfBoundsException（数组下标越界异常）**

**java.lang.IllegalArgumentException（方法的参数错误）**

**java.lang.IllegalAccessException（没有访问权限）**

**java.lang.ArithmeticException（数学运算异常）**

**java.lang.ClassCastException（数据类型转换异常）**

**java.lang.FileNotFoundException（文件未找到异常）**

**java.lang.ArrayStoreException（数组存储异常）**

**java.lang.NoSuchMethodException（方法不存在异常）**

**java.lang.EOFException（文件已结束异常）**

**java.lang.InstantiationException（实例化异常）**

**java.lang.InterruptedException（被中止异常）**

**java.lang.CloneNotSupportedException （不支持克隆异常）**

**java.lang.OutOfMemoryException （内存不足错误）**

**java.lang.NoClassDefFoundException （未找到类定义错误）**

#### <span style='color:red'>7）Arrays的常用方法</span>

**List<T> asList(T... a)**：返回由指定数组构成的**大小固定**的列表，该列表不能使用add和remove方法改变长度

**int binarySearch(Object[] a, Object key)**：使用二分查找元素的索引

**T[] copyOfRange(T[] original, int from, int to)**：复制数组，并且指定开始/结束索引

**T[] copyOf(T[] original, int newLength)**：复制数组，并且指定复制长度

**void fill(Object[] a, Object val)**：使用指定元素填充数组

**void sort(Object[] a)**：对数组排序，需要实现数组元素的Comparable接口

**String toString(Object[] a)**：数组转字符串

#### <span style='color:red'>8）Collections的常用方法</span>

Collections.**sort**(list)						 //对集合进行排序

Collections.**shuffle**(list)					//对集合进行随机排序

Collections.**max**(list)						//获取集合最大值

Collections.**binarySearch**(list2, "Thursday")	//查找集合指定元素，返回元素所在索引 ,若不存在，返回该元素最可能存在的位置索引

Collections.**indexOfSubList**(list2, subList)		//查找子串在集合中首次出现的位置

Collections.**replaceAll**(list2, "Sunday", "tttttt") //替换集合中指定的元素，若元素存在返回true，否则返回false

Collections.**reverse**(list2)				//反转集合中的元素的顺序

Collections.**rotate**(list2, 3)				//集合中的元素向后移动k位置，后面的元素出现在集合开始的位置

Collections.**copy**(list2, list3)			//将集合list3中的元素复制到list2中，并覆盖相应索引位置的元素

Collections.**swap**(list2, 0, 3)			//交换集合中指定元素的位置

Collections.**fill**(list2, "替换")			//替换集合中的所有元素，用对象object

Collections.**nCopies**(5, "哈哈")		//生成一个指定大小与内容的集合

#### <span style='color:red'>9）枚举类</span>

**enum定义后在编译后默认继承了java.lang.Enum类，所以不能继承别的类了**，但是可以实现接口，而不是普通的继承Object类。enum声明类继承了Serializable和Comparable两个接口。**且采用enum声明后，该类会被编译器加上final声明（同String），故该类是无法继承的。**枚举类的内部定义的枚举值就是该类的实例（且必须在第一行定义，当类初始化时，这些枚举值会被实例化）。

#### <span style='color:red'>10）反射</span>

反射就是把Java类中的各个成分映射成一个个的Java对象。即在运行状态中，对于任意一个类，都能够知道这个类的所以属性和方法；对于任意一个对象，都能调用它的任意一个方法和属性。这种动态获取信息及动态调用对象方法的功能叫Java的反射机制。

class对象获取的三种方法：**对象.getClass()**，**类.class**，**Class.forName(包名.类名)**

class.getMethods();							// 获取所有公有方法

class.getDeclaredMethods();			// 获取所有私有方法

class.getMethod("show1", String.class);		// 获取名为show1的方法（输入参数为String）

class..getConstructor().newInstance()；		// 获取构造方法

method.invoke(Object, args)			// 执行Object对象的方法，输入参数为args如果为静态方法，Object可以为null

**动态代理就用到了反射机制**

#### <span style='color:red'>11）TCP/IP和UDP</span>

![img](https://img-blog.csdnimg.cn/20190414223004578.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xlaG82NjY=,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190414225550112.gif?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xlaG82NjY=,size_16,color_FFFFFF,t_70)

<span style='color:red'>**应用层**</span>

**运行在TCP协议上的协议**：
**HTTP**（Hypertext Transfer Protocol，超文本传输协议），主要用于普通浏览。
**HTTPS**（Hypertext Transfer Protocol over Secure Socket Layer, or HTTP over SSL，安全超文本传输协议）,HTTP协议的安全版本。
**FTP**（File Transfer Protocol，文件传输协议），由名知义，用于文件传输。
**POP3**（Post Office Protocol, version 3，邮局协议），收邮件用。
**SMTP**（Simple Mail Transfer Protocol，简单邮件传输协议），用来发送电子邮件。
**TELNET**（Teletype over the Network，网络电传），通过一个终端（terminal）登陆到网络。
**SSH**（Secure Shell，用于替代安全性差的TELNET），用于加密安全登陆用。
**运行在UDP协议上的协议**：
**BOOTP**（Boot Protocol，启动协议），应用于无盘设备。
**NTP**（Network Time Protocol，网络时间协议），用于网络同步。
**DHCP**（Dynamic Host Configuration Protocol，动态主机配置协议），动态配置IP地址。
**其他**：
**DNS**（Domain Name Service，域名服务），用于完成地址查找，邮件转发等工作（运行在TCP和UDP协议上）。
**ECHO**（Echo Protocol，回绕协议），用于查错及测量应答时间（运行在TCP和UDP协议上）。
**SNMP**（Simple Network Management Protocol，简单网络管理协议），用于网络信息的收集和网络管理。
**ARP**（Address Resolution Protocol，地址解析协议），用于动态解析以太网硬件的地址。

<span style='color:red'>**传输层**</span>

**UDP**：只提供了基本的错误检测，是一个**无连接**的协议。
特点：把数据打包，数据大小有限制（64k），不建立连接，速度快，但可靠性低。

**TCP**：提供了完善的错误控制和流量控制，能够确保数据正常传输，是一个**面向连接**的协议。
特点：建立连接通道，数据大小无限制速度慢，但是可靠性高。由于传输层涉及的东西比较多，比如端口，Socket等。

![img](https://img-blog.csdnimg.cn/20190414224621538.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xlaG82NjY=,size_16,color_FFFFFF,t_70)

1）：建立连接时，客户端发送SYN包（SYN=i）到服务器，并进入到SYN-SEND状态，等待服务器确认

2）：服务器收到SYN包，必须确认客户的SYN（ack=i+1）,同时自己也发送一个SYN包（SYN=k）,即SYN+ACK包，此时服务器进入SYN-RECV状态

3）：客户端收到服务器的SYN+ACK包，向服务器发送确认报ACK（ack=k+1）,此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手，客户端与服务器开始传送数据。

**有效阻止重复历史连接的初始化**

![img](https://img-blog.csdnimg.cn/20190414224748102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xlaG82NjY=,size_16,color_FFFFFF,t_70)

第一次挥手：Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态。

第二次挥手：Server收到FIN后，发送一个ACK给Client，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号），Server进入CLOSE_WAIT状态。

第三次挥手：Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态。

第四次挥手：Client收到FIN后，Client进入TIME_WAIT状态，接着发送一个ACK给Server，确认序号为收到序号+1，Server进入CLOSED状态，完成四次挥手。

<span style='color:red'>**传输层**</span>

IP地址由两部分组成，即网络地址和主机地址，二者是主从关系：

（1）网络号 net-id，它标志主机（或路由器）所连接到的网络，网络地址表示其属于互联网的哪一个网络

（2）主机号 host-id，它标志该主机（或路由器），主机地址表示其属于该网络中的哪一台主机。

![img](https://img-blog.csdnimg.cn/20190414224020477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xlaG82NjY=,size_16,color_FFFFFF,t_70)

#### <span style='color:red'>12）HTTP和HTTPS</span>

![img](https://pic4.zhimg.com/80/v2-fbef2c48d13068978904f3d1688728ab_720w.jpg)

<img src="https://pic002.cnblogs.com/images/2012/426620/2012072810301161.png" alt="img" style="zoom: 80%;" /><img src="https://images2018.cnblogs.com/blog/867021/201803/867021-20180322001744323-654009411.jpg" alt="img" style="zoom:80%;" />

**状态码**：

1xx：表示服务器已接收了客户端请求，客户端可继续发送请求;

2xx：表示服务器已成功接收到请求并进行处理;

3xx：表示服务器要求客户端重定向;

4xx：表示客户端的请求有非法内容;

5xx：表示服务器未能正常处理客户端的请求而出现意外错误;

**请求头部**：

| Header              | 解释                                                         | 示例                                                    |
| :------------------ | :----------------------------------------------------------- | :------------------------------------------------------ |
| **Accept**          | 指定客户端能够接收的内容类型                                 | Accept: text/plain, text/html                           |
| Accept-Charset      | 浏览器可以接受的字符编码集。                                 | Accept-Charset: iso-8859-5                              |
| **Accept-Encoding** | 指定浏览器可以支持的web服务器返回内容压缩编码类型。          | Accept-Encoding: compress, gzip                         |
| **Accept-Language** | 浏览器可接受的语言                                           | Accept-Language: en,zh                                  |
| Accept-Ranges       | 可以请求网页实体的一个或者多个子范围字段                     | Accept-Ranges: bytes                                    |
| Authorization       | HTTP授权的授权证书                                           | Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==       |
| Cache-Control       | 指定请求和响应遵循的缓存机制                                 | Cache-Control: no-cache                                 |
| **Connection**      | 表示是否需要持久连接。（HTTP 1.1默认进行持久连接）           | Connection: close                                       |
| **Cookie**          | HTTP请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器。 | Cookie: $Version=1; Skin=new;                           |
| Content-Length      | 请求的内容长度                                               | Content-Length: 348                                     |
| Content-Type        | 请求的与实体对应的MIME信息                                   | Content-Type: application/x-www-form-urlencoded         |
| **Date**            | 请求发送的日期和时间                                         | Date: Tue, 15 Nov 2010 08:12:31 GMT                     |
| Expect              | 请求的特定的服务器行为                                       | Expect: 100-continue                                    |
| From                | 发出请求的用户的Email                                        | From: user@email.com                                    |
| **Host**            | 指定请求的服务器的域名和端口号                               | Host: www.zcmhi.com                                     |
| If-Match            | 只有请求内容与实体相匹配才有效                               | If-Match: “737060cd8c284d8af7ad3082f209582d”            |
| If-Modified-Since   | 如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码 | If-Modified-Since: Sat, 29 Oct 2010 19:43:31 GMT        |
| If-None-Match       | 如果内容未改变返回304代码，参数为服务器先前发送的Etag，与服务器回应的Etag比较判断是否改变 | If-None-Match: “737060cd8c284d8af7ad3082f209582d”       |
| If-Range            | 如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为Etag | If-Range: “737060cd8c284d8af7ad3082f209582d”            |
| If-Unmodified-Since | 只在实体在指定时间之后未被修改才请求成功                     | If-Unmodified-Since: Sat, 29 Oct 2010 19:43:31 GMT      |
| Max-Forwards        | 限制信息通过代理和网关传送的时间                             | Max-Forwards: 10                                        |
| Pragma              | 用来包含实现特定的指令                                       | Pragma: no-cache                                        |
| Proxy-Authorization | 连接到代理的授权证书                                         | Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ== |
| Range               | 只请求实体的一部分，指定范围                                 | Range: bytes=500-999                                    |
| Referer             | 先前网页的地址，当前请求网页紧随其后,即来路                  | Referer: http://www.zcmhi.com/archives/71.html          |
| TE                  | 客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息     | TE: trailers,deflate;q=0.5                              |
| Upgrade             | 向服务器指定某种传输协议以便服务器进行转换（如果支持）       | Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11          |
| User-Agent          | User-Agent的内容包含发出请求的用户信息                       | User-Agent: Mozilla/5.0 (Linux; X11)                    |
| Via                 | 通知中间网关或代理服务器地址，通信协议                       | Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)             |
| Warning             | 关于消息实体的警告信息                                       | Warn: 199 Miscellaneous warning                         |

**相应头部**：

| Header               | 解释                                                         | 示例                                                  |
| :------------------- | :----------------------------------------------------------- | :---------------------------------------------------- |
| Accept-Ranges        | 表明服务器是否支持指定范围请求及哪种类型的分段请求           | Accept-Ranges: bytes                                  |
| Age                  | 从原始服务器到代理缓存形成的估算时间（以秒计，非负）         | Age: 12                                               |
| Allow                | 对某网络资源的有效的请求行为，不允许则返回405                | Allow: GET, HEAD                                      |
| **Cache-Control**    | 告诉所有的缓存机制是否可以缓存及哪种类型                     | Cache-Control: no-cache                               |
| **Content-Encoding** | web服务器支持的返回内容压缩编码类型。                        | Content-Encoding: gzip                                |
| Content-Language     | 响应体的语言                                                 | Content-Language: en,zh                               |
| **Content-Length**   | 响应体的长度                                                 | Content-Length: 348                                   |
| Content-Location     | 请求资源可替代的备用的另一地址                               | Content-Location: /index.htm                          |
| Content-MD5          | 返回资源的MD5校验值                                          | Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==                 |
| Content-Range        | 在整个返回体中本部分的字节位置                               | Content-Range: bytes 21010-47021/47022                |
| **Content-Type**     | 返回内容的MIME类型                                           | Content-Type: text/html; charset=utf-8                |
| **Date**             | 原始服务器消息发出的时间                                     | Date: Tue, 15 Nov 2010 08:12:31 GMT                   |
| ETag                 | 请求变量的实体标签的当前值                                   | ETag: “737060cd8c284d8af7ad3082f209582d”              |
| **Expires**          | 响应过期的日期和时间                                         | Expires: Thu, 01 Dec 2010 16:00:00 GMT                |
| Last-Modified        | 请求资源的最后修改时间                                       | Last-Modified: Tue, 15 Nov 2010 12:45:26 GMT          |
| Location             | 用来重定向接收方到非请求URL的位置来完成请求或标识新的资源    | Location: http://www.zcmhi.com/archives/94.html       |
| **Pragma**           | 包括实现特定的指令，它可应用到响应链上的任何接收方           | Pragma: no-cache                                      |
| Proxy-Authenticate   | 它指出认证方案和可应用到代理的该URL上的参数                  | Proxy-Authenticate: Basic                             |
| refresh              | 应用于重定向或一个新的资源被创造，在5秒之后重定向（由网景提出，被大部分浏览器支持） | Refresh: 5; url=http://www.zcmhi.com/archives/94.html |
| Retry-After          | 如果实体暂时不可取，通知客户端在指定时间之后再次尝试         | Retry-After: 120                                      |
| Server               | web服务器软件名称                                            | Server: Apache/1.3.27 (Unix) (Red-Hat/Linux)          |
| Set-Cookie           | 设置Http Cookie                                              | Set-Cookie: UserID=JohnDoe; Max-Age=3600; Version=1   |
| Trailer              | 指出头域在分块传输编码的尾部存在                             | Trailer: Max-Forwards                                 |
| Transfer-Encoding    | 文件传输编码                                                 | Transfer-Encoding:chunked                             |
| **Vary**             | 告诉下游代理是使用缓存响应还是从原始服务器请求               | Vary: *                                               |
| Via                  | 告知代理客户端响应是通过哪里发送的                           | Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)           |
| Warning              | 警告实体可能存在的问题                                       | Warning: 199 Miscellaneous warning                    |
| WWW-Authenticate     | 表明客户端请求实体应该使用的授权方案                         | WWW-Authenticate: Basic                               |

http请求方法

| 序号 | 方法    | 描述                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 1    | GET     | 发送请求来获得服务器上的资源，请求体中不会包含请求数据，请求数据放在协议头中。另外get支持快取、缓存、可保留书签等。幂等 |
| 2    | POST    | 和get一样很常见，向服务器提交资源让服务器处理，比如提交表单、上传文件等，可能导致建立新的资源或者对原有资源的修改。提交的资源放在请求体中。不支持快取。非幂等 |
| 3    | HEAD    | 本质和get一样，但是**响应中没有呈现数据，而是http的头信息**，主要用来检查资源或超链接的有效性或是否可以可达、检查网页是否被串改或更新，获取头信息等，特别适用在有限的速度和带宽下。 |
| 4    | PUT     | 和post类似，html表单不支持，发送资源与服务器，并存储在服务器指定位置，要求客户端事先知道该位置；比如post是在一个集合上（/province），而**put是具体某一个资源上**（/province/123）。所以put是安全的，无论请求多少次，都是在123上更改，而post可能请求几次创建了几次资源。幂等 |
| 5    | DELETE  | 请求服务器删除某资源。和put都具有破坏性，可能被防火墙拦截。如果是https协议，则无需担心。幂等 |
| 6    | CONNECT | HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。就是把服务器作为跳板，去访问其他网页然后把数据返回回来，连接成功后，就可以正常的get、post了。 |
| 7    | OPTIONS | 获取http服务器支持的http请求方法，允许客户端查看服务器的性能，比如ajax跨域时的预检等。 |
| 8    | TRACE   | 回显服务器收到的请求，主要用于测试或诊断。一般禁用，防止被恶意攻击或盗取信息。 |

**基于 请求-响应 的模式**

HTTP协议规定,请求从客户端发出,最后服务器端响应该请求并 返回。换句话说,肯定是先从客户端开始建立通信的,服务器端在没有 接收到请求之前不会发送响应

**无状态保存**

HTTP是一种不保存状态,即无状态(stateless)协议。HTTP协议 自身不对请求和响应之间的通信状态进行保存。也就是说在HTTP这个 级别,协议对于发送过的请求或响应都不做持久化处理。

使用HTTP协议,每当有新的请求发送时,就会有对应的新响应产 生。**协议本身并不保留之前一切的请求或响应报文的信息**。**这是为了更快地处理大量事务,确保协议的可伸缩性**,而特意把HTTP协议设计成 如此简单的。可是,随着Web的不断发展,因无状态而导致业务处理变得棘手 的情况增多了。比如,用户登录到一家购物网站,即使他跳转到该站的 其他页面后,也需要能继续保持登录状态。针对这个实例,网站为了能 够掌握是谁送出的请求,需要保存用户的状态。**HTTP/1.1虽然是无状态协议,但为了实现期望的保持状态功能, 于是引入了Cookie技术**。有了Cookie再用HTTP协议通信,就可以管理状态了。

**无连接**

无连接的含义是限制每次连接只处理一个请求。**服务器处理完客户的请求，并收到客户的应答后，即断开连接**。采用这种方式可以节省传输时间，并且可以提高并发性能，不能和每个用户建立长久的连接，请求一次相应一次，服务端和客户端就中断了。但是无连接有两种方式，早期的http协议是一个请求一个响应之后，直接就断开了，**但是现在的http协议1.1版本不是直接就断开了，而是等几秒钟**，这几秒钟是等什么呢，等着用户有后续的操作，如果用户在这几秒钟之内有新的请求，那么还是通过之前的连接通道来收发消息，如果过了这几秒钟用户没有发送新的请求，那么就会断开连接，这样可以提高效率，减少短时间内建立连接的次数，因为建立连接也是耗时的，默认的好像是3秒，但是这个时间是可以通过咱们后端的代码来调整的，自己网站根据自己网站用户的行为来分析统计出一个最优的等待时间。

#### <span style='color:red'>13）流式编程</span>

**Stream**的操作符大体上分为两种：**中间操作符**和**终止操作符**

中间操作符包含8种(排除了parallel,sequential,这两个操作并不涉及到对数据流的加工操作)：

1. **map**(mapToInt,mapToLong,mapToDouble) 转换操作符，把比如A->B，这里默认提供了转int，long，double的操作符。

2. **flatmap**(flatmapToInt,flatmapToLong,flatmapToDouble) 拍平操作比如把 int[]{2,3,4} 拍平 变成 2，3，4 也就是从原来的一个数据变成了3个数据，这里默认提供了拍平成int,long,double的操作符。

3. **limit** 限流操作，比如数据流中有10个 我只要出前3个就可以使用。
4. **distint** 去重操作，对重复元素去重，底层使用了equals方法。
5. **filter** 过滤操作，把不想要的数据过滤。
6. **peek** 挑出操作，如果想对数据进行某些操作，如：读取、编辑修改等。**（Consumer接口，没有输出，map是Function接口）**
7. **skip** 跳过操作，跳过某些元素。**（前n个元素）**
8. **sorted**(unordered) 排序操作，对元素排序，前提是实现Comparable接口，当然也可以自定义比较器。

终止操作符就是用来对数据进行收集或者消费的，**数据到了终止操作这里就不会向下流动了**，**终止操作符只能使用一次**。

1. **collect** 收集操作，将所有数据收集起来，这个操作非常重要，官方的提供的Collectors 提供了非常多收集器，可以说Stream 的核心在于Collectors。
2. **count** 统计操作，统计最终的数据个数。
3. **findFirst**、**findAny** 查找操作，查找第一个、查找任何一个 返回的类型为Optional。
4. **noneMatch**、**allMatch**、**anyMatch** 匹配操作，数据流中是否存在符合条件的元素 返回值为bool 值。
5. **min**、**max** 最值操作，需要自定义比较器，返回数据流中最大最小的值。
6. **reduce** 规约操作，将整个数据流的值规约为一个值，count、min、max底层就是使用reduce。
7. **forEach**、**forEachOrdered** 遍历操作，这里就是对最终的数据进行消费了。
8. **toArray** 数组操作，将数据流的元素转换成数组。

**foreach不止是stream的操作符也可以用于各种集合，流式编程主要看实现的接口类型，有输出的才会改变流内部的数据**

#### <span style='color:red'>14）JDK8新特性</span>

lambda表达式

方法引用（：：）

函数式接口

默认方法（接口中可以用default 修饰的方法，该方法可以写实现；接口内可以实现静态方法）

stream

新增Optional类

java.time（解决java.util.Date的问题）

#### <span style='color:red'>15）IO的方式</span>

**BIO（同步阻塞IO模型）**：

BIO 就是传统的 java.io 包，它是基于流模型实现的，交互的方式是同步、阻塞方式，也就是说在读入输入流或者输出流时，在读写动作完成之前，线程会一直阻塞在那里，它们之间的调用时可靠的线性顺序。它的有点就是代码比较简单、直观；缺点就是 IO 的效率和扩展性很低，容易成为应用性能瓶颈。

**NIO（同步非阻塞IO模型）**：

NIO 是 Java 1.4 引入的 java.nio 包，提供了 Channel、Selector、Buffer 等新的抽象，可以构建多路复用的、同步非阻塞 IO 程序，同时提供了更接近操作系统底层高性能的数据操作方式。

**AIO（异步非阻塞IO模型）**：

AIO 是 Java 1.7 之后引入的包，是 NIO 的升级版本，提供了异步非堵塞的 IO 操作方式，所以人们叫它 AIO（Asynchronous IO），异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。

**同步异步就是有无返回结果，阻塞非阻塞就是是否等到想要的返回结果**

#### <span style='color:red'>n）杂七杂八的</span>

**StringBuffer和StringBuilder**

**Vector**

**TreeSet**

**字符流、字节流、缓冲流**

**序列化**

**枚举（单例）**

**EnumMap、EnumSet**

**Socket**

**ssl**

## 2、JUC

#### <span style='color:red'>1）什么是JUC</span>

​		JUC就是java.util.concurrent包，俗称java并发包

#### <span style='color:red'>2）线程和进程</span>

​		进程是**操作系统**资源分配的基本单位，而线程是**处理器**任务调度和执行的基本单位。

#### <span style='color:red'>3）并发和并行</span>

​		并发(concurrent)指的是多个程序可以同时运行的现象，更细化的是多进程可以同时运行或者多指令可以同时运行。**当程序中写下多进程或多线程代码时，这意味着的是并发而不是并行**。并发是因为多进程/多线程都是需要去完成的任务，不并行是因为并行与否由操作系统的调度器决定，可能会让多个进程/线程被调度到同一个CPU核心上。只不过调度算法会尽量让不同进程/线程使用不同的CPU核心，所以在实际使用中几乎总是会并行，但却不能以100%的角度去保证会并行。也就是说，**并行与否程序员无法控制，只能让操作系统决定**。

#### <span style='color:red'>4）java实现多线程的三种方式</span>

​		继承 Thread 类，实现 Runnable 接口，使用Callable和Future接口创建线程。

``` java
Callable<Integer> myCallable = new MyCallable();    // 创建MyCallable对象
FutureTask<Integer> ft = new FutureTask<Integer>(myCallable); //使用FutureTask来包装MyCallable对象
Thread thread = new Thread(ft);   //FutureTask对象作为Thread对象的target创建新的线程
thread.start();                      //线程进入到就绪状态
System.out.println("线程返回值：" + ft.get());
//……


class MyCallable implements Callable<Integer> {
    // 与run()方法不同的是，call()方法具有返回值
    @Override
    public Integer call() {
        /*****重写call方法*****/
    }
}
```

​		**注意：**用for循环多次new Thread(同一个实现Runable的类的实例)，就是多次操作该实例，**但是**FutureTask只能执行一次call方法（写死在该类的run方法里面，执行一次call方法后ft状态发生改变）

#### <span style='color:red'>5）java多线程的几种状态</span>

1. **初始(NEW)**：新创建了一个线程对象，但还没有调用start()方法。

2. **运行(RUNNABLE)**：Java线程中将就绪（ready）和运行中（running）两种状态笼统的称为“运行”。

   线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取CPU的使用权，此时处于就绪状态（ready）。就绪状态的线程在获得CPU时间片后变为运行中状态（running）。

3. **阻塞(BLOCKED)**：表示线程阻塞于锁。

4. **等待(WAITING)**：进入该状态的线程需要等待其他线程做出一些特定动作（通知或中断）。

5. **超时等待(TIMED_WAITING)**：该状态不同于WAITING，它可以在指定的时间后自行返回。

6. **终止(TERMINATED)**：表示该线程已经执行完毕。

![线程状态图](https://img-blog.csdnimg.cn/20181120173640764.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BhbmdlMTk5MQ==,size_16,color_FFFFFF,t_70)



#### <span style='color:red'>6）synchronized的用法</span>

1. synchronized**方法**锁定的是当前对象，并且当前对象的其他synchronized方法均被锁定，其他不含synchronized方法不进行锁定
2. synchronized**代码块**锁定的是（）里面的对象（**即使该对象所属实例也被锁，依旧不会互相影响**），对于其他带有相同synchronized对象的代码块进行锁定，其他不带有synchronized的代码不进行锁定（**即使我们操作该对象，但是没有使用synchronized关键字，也不进行锁定**）
3. synchronize静态方法和synchronized非静态方法之间不互斥，锁定的静态方法是类锁，同类之间的静态方法互斥
4. synchronized静态代码块类比非静态代码块和静态方法
5. 对象锁之间互斥、类锁之间互斥（**锁方法和所代码块相同**）

#### <span style='color:red'>7）Synchronized和Lock的区别</span>

1. Synchronized是内置的java关键字，Lock是java的一个类
2. Synchronized无法判断锁的状态，Lock可以判断是否获取到了锁
3. Synchronized会自动释放锁，Lock必须要手动释放锁，**如果不释放锁，死锁**
4. Synchronized线程1（获得锁、阻塞），线程2（等待），Lock锁不一定会等下去
5. Synchronized可重入，不可以中断，非公平，Lock可重入，可以判断锁，可以自己设置公平非公平
6. Synchronized适合锁少量的代码同步问题，Lock适合锁大量的同步代码

#### <span style='color:red'>8）CyclicBarrier和CountDownLatch的区别</span>

![img](https://javadoop.com/blogimages/AbstractQueuedSynchronizer-3/cyclicbarrier-3.png)

1. countDownLatch是一个计数器，线程完成一个记录一个，计数器递减，只能只用一次
2. CyclicBarrier的计数器更像一个阀门，需要所有线程都到达，然后继续执行，计数器递增，提供reset功能，可以多次使用

#### <span style='color:red'>9）semaphore</span>

​		Semaphore也是一个线程同步的辅助类，可以维护当前访问自身的线程个数，并提供了同步机制。使用Semaphore可以控制同时访问资源的线程个数，例如，实现一个文件允许的并发访问数。

Semaphore的主要方法摘要：

　　void acquire()：从此信号量获取一个许可，在提供一个许可前一直将线程阻塞，否则线程被中断。

　　void release()：释放一个许可，将其返回给信号量。

　　int availablePermits()：返回此信号量中当前可用的许可数。

　　boolean hasQueuedThreads()：查询是否有线程正在等待获取。

#### <span style='color:red'>10）读写锁（ReadWriteLock）</span>

读读共享、读写互斥、写读互斥、写写互斥。

**独占锁**：一次只能有一个线程占有。（写锁）

**共享锁**：多个线程可以同时占有。（读锁）

#### <span style='color:red'>11）阻塞队列</span>

BlockingQueue是Queue的一个子接口

| 方式         | 抛出异常 | 有返回值，不抛出异常 | 等待超时            | 阻塞等待(BlockingQueue独有) |
| ------------ | -------- | -------------------- | ------------------- | --------------------------- |
| 添加         | add      | offer                | offer（加超时参数） | put                         |
| 移除         | remove   | poll                 | poll（加超时参数）  | take                        |
| 检查队首元素 | element  | peek                 | ----                | ----                        |

SynchronousQueue是BlockingQueue的实现，相当于只有一个容量的队列（**执行完put后线程阻塞，知道被take**）

#### <span style='color:red'>12）线程池</span>

**注意**：

​		线程池不允许使用Executors去创建，而是通过ThreadPoolExcutor的方式，这样的处理方式能更加明确线程池的运行规则，规避资源耗尽的风险。

​		FixedThreadPool和SingleThreadPool允许的请求队列长度为Integer.MAX_VALUE，可能会堆积大量的请求，从而导致OOM；

​		CacheThreadPool和ScheduledThreadPool允许创建线程数量为Integer.MAX_VALUE，可能会创建大量的线程，从而导致OOM。

**好处**：

1. 降低资源的消耗（**线程复用**）
2. 提高相应的速度（**控制最大并发数**）
3. 方便管理（**管理线程**）

**三大方法**：

```java
Executors.newSingleThreadExecutor();	// 单个线程
Executors.newFixedThreadPool(5);  		// 固定线程数（5）
Executors.newCachedThreadPool();        // 可伸缩
```

**七种参数**：

```java
public ThreadPoolExecutor(int corePoolSize,			// 核心线程池大小
                          int maximumPoolSize,		// 最大核心线程池大小
                          long keepAliveTime,		// 超时了多久，没有人调用就会释放
                          TimeUnit unit,			// 超时单位
                          BlockingQueue<Runnable> workQueue,  // 阻塞队列
                          ThreadFactory threadFactory,		  // 线程工厂，创建线程的，一般不用动的
                          RejectedExecutionHandler handler)	  // 拒绝策略
```

**四种拒绝策略**：

``` java
ThreadPoolExecutor.CallerRunsPolicy()		// 由调用线程处理该任务(例如在main里开线程，main执行)
ThreadPoolExecutor.AbortPolicy()			// 丢弃任务，并抛出RejectedExecutionException异常（默认）
ThreadPoolExecutor.DiscardPolicy ()			// 丢弃任务，但是不抛出异常
ThreadPoolExecutor.DiscardOldestPolicy()	// 丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）
```

**最大线程池数量选择**：

​		根据程序占用资源情况分为**CPU密集型**任务（系统运作大部分的状况是CPU Loading 100%，CPU要读/写I/O(硬盘/内存)，I/O在很短的时间就可以完成，而CPU还有许多运算要处理，CPU Loading很高）、**IO密集型**任务（系统运作，大部分的状况是CPU在等I/O (硬盘/内存) 的读/写操作，此时CPU Loading并不高）。

​		CPU密集型需要选择**逻辑处理器的核数**作为最大线程数【Runtime.getRuntime().availableProcessors()】；

​		IO密集型一般选择**最大IO任务数量的两倍**作为最大线程数。

#### <span style='color:red'>13）四大函数式接口</span>

新时达程序员：**lambda表达式**、**链式编程**、**函数式接口**、**Stream流式计算**

四大函数式接口：**Consumer**、**Function**、**Predicate**、**Supplier**

**Function**：Function<String，String> function = s -> s;（apply方法，输入一个参数，输出一个参数）

**Predicate**：Predicate<String> predicate = String::isEmpty;  （test方法，输入一个参数，输出一个布尔值）

**Consumer**：Consumer<String> consumer = System.out::println;  （accept方法，一个输入参数，没有输出）

**Supplier**：Supplier<Integer> supplier = () -> 1024;  （get方法，没有输入，只有一个输出）

#### <span style='color:red'>14）Forkjoin</span>

ForkJoin是由JDK1.7后提供多线并发处理框架，ForkJoin的框架的基本思想是分而治之。使用ForkJoin将相同的计算任务通过多线程的进行执行。从而能提高数据的计算速度。

<span style='color:red'>**构造函数**</span>

```java
public ForkJoinPool(int parallelism,
                    ForkJoinWorkerThreadFactory factory,
                    UncaughtExceptionHandler handler,
                    boolean asyncMode)
```

- **parallelism**：可并行级别，**默认构造函数中将改值设为CPU的核数**。Fork/Join框架将依据这个并行级别的设定，决定框架内并行执行的线程数量。并行的每一个任务都会有一个线程进行处理，但是千万不要将这个属性理解成Fork/Join框架中最多存在的线程数量，也不要将这个属性和ThreadPoolExecutor线程池中的corePoolSize、maximumPoolSize属性进行比较，因为ForkJoinPool的组织结构和工作方式与后者完全不一样。Fork/Join框架中可存在的线程数量和这个参数值的关系并不是绝对的关联（有依据但并不全由它决定）。
- **factory**：当Fork/Join框架创建一个新的线程时，同样会用到线程创建工厂。只不过这个线程工厂不再需要实现ThreadFactory接口，而是需要实现ForkJoinWorkerThreadFactory接口。后者是一个函数式接口，只需要实现一个名叫newThread的方法。在Fork/Join框架中有一个默认的ForkJoinWorkerThreadFactory接口实现：DefaultForkJoinWorkerThreadFactory。
- **handler**：异常捕获处理器。当执行的任务中出现异常，并从任务中被抛出时，就会被handler捕获。
- **asyncMode**：这个参数也非常重要，从字面意思来看是指的异步模式，它并不是说Fork/Join框架是采用同步模式还是采用异步模式工作。Fork/Join框架中为每一个独立工作的线程准备了对应的待执行任务队列，这个任务队列是使用数组进行组合的双向队列。即是说存在于队列中的待执行任务，**即可以使用先进先出的工作模式，也可以使用后进先出的工作模式。默认为false，即LIFO模式。**

如果你对Fork/Join框架没有特定的执行要求，可以直接使用不带有任何参数的构造函数。也就是说**推荐基于当前操作系统可以使用的CPU内核数作为Fork/Join框架内最大并行任务数量**，这样可以保证CPU在处理并行任务时，尽量少发生任务线程间的运行状态切换。

<span style='color:red'>**Fork方法和Join方法**</span>

Fork/Join框架中提供的fork方法和join方法，可以说是该框架中提供的最重要的两个方法，它们和parallelism“可并行任务数量”配合工作，可以导致拆分的子任务T1.1、T1.2甚至TX在Fork/Join框架中不同的运行效果。

**fork方法用于将新创建的子任务放入当前线程的work queue队列中**，Fork/Join框架将根据当前正在并发执行ForkJoinTask任务的ForkJoinWorkerThread线程状态，决定是让这个任务在队列中等待，还是创建一个新的ForkJoinWorkerThread线程运行它，又或者是唤起其它正在等待任务的ForkJoinWorkerThread线程运行它。

ForkJoinTask任务是一种能在Fork/Join框架中运行的特定任务，也只有这种类型的任务可以在Fork/Join框架中被拆分运行和合并运行。ForkJoinWorkerThread线程是一种在Fork/Join框架中运行的特性线程，它除了具有普通线程的特性外，最主要的特点是**每一个ForkJoinWorkerThread线程都具有一个独立的任务等待队列（work queue）**，这个任务队列用于存储在本线程中被拆分的若干子任务。

**join方法用于**让当前线程阻塞，直到对应的子任务完成运行并返回执行结果。或者，如果这个子任务存在于当前线程的任务等待队列（work queue）中，则**取出这个子任务进行“递归”执行**。其目的是尽快得到当前子任务的运行结果，然后继续执行。

<span style='color:red'>**JoinForkPool的三种提交任务的方法**</span>

**execute**(ForkJoinTask) 异步执行tasks，无返回值

**invoke**(ForkJoinTask) 有Join, tasks会被同步到主进程（同步的直接返回结果）

**submit**(ForkJoinTask) 异步执行，且带Task返回值，可通过task.get 实现同步到主线程

**Java SE中有一些通用的功能，它们已经使用fork/join框架来实现。**

1. 在Java 8的java.util.Arrays中的parallelSort方法采用了fork/join框架，在多处理器系统上，并行排序大量数据比顺序排序更快

2. 在Stream.parallel()中使用并行

#### <span style='color:red'>15）CompletableFuture</span>

​		所谓异步调用其实就是实现一个可**无需等待**被调用函数的返回值而让操作继续运行的方法。在 Java 语言中，简单的讲就是另启一个线程来完成调用中的部分计算，使调用继续运行或返回，而不需要等待计算结果。但调用者仍需要取线程的计算结果。

​		JDK5新增了Future接口，用于描述一个异步计算的结果。虽然 Future 以及相关使用方法提供了异步执行任务的能力，但是对于结果的获取却是很不方便，只能通过阻塞或者轮询的方式得到任务的结果。阻塞的方式显然和我们的异步编程的初衷相违背，轮询的方式又会耗费无谓的 CPU 资源，而且也不能及时地得到计算结果。

​		在Java8中，CompletableFuture提供了非常强大的Future的扩展功能，可以帮助我们简化异步编程的复杂性，并且提供了函数式编程的能力，可以通过回调的方式处理计算结果，也提供了转换和组合 CompletableFuture 的方法。

​		它可能代表一个明确完成的Future，也有可能代表一个完成阶段（ CompletionStage ），它支持在计算完成以后触发一些函数或执行某些动作。

#### <span style='color:red'>16）Volatile</span>

Volatile是java虚拟机提供的轻量级的同步机制，

1. 保证可见性
2. **不保证原子性**
3. 禁止指令重排（内存屏障）

**<span style='color:red'>JMM</span>**：java内存模型，不存在的东西，概念！

1. 在线程解锁前：必须把共享变量**立刻**刷新回主存
2. 在线程加锁前：必须读取主存中的最新值到工作内存中
3. 加锁和解锁是同一把锁

**<span style='color:red'>8种操作</span>**

线程**读取**并**载入**主存数据，线程执行引擎**使用**并**赋值**工作内存，线程**存储**并**写入**主存数据，以及**锁定**和**解锁**

**<span style='color:red'>指令重排</span>**

我们写的程序，计算机并不是按照写的那样去执行。

**源代码** --> **编译器优化的重排** --> **指令并行也可能会重排** --> **内存系统也会重排** --> **执行**

#### <span style='color:red'>17）单例模式</span>

饿汉式：直接创建实例，（浪费内存空间）

懒汉式：需要的时候才创建实例，创建一次后再使用，直接返回即可

``` java
public class LazyMan(){
    private LazyMan(){}
    private volatile static LazyMan LazyMan;
    // 双重检测锁模式，DCL懒汉式（防止在synchronized的时候，其他线程完成了初始化操作）
    public static LazyMan getInstance(){
        if(LazyMan == null){
            synchronized(LazyMan.class){
                if(LazyMan == null){
                    LazyMan = new LazyMan();		// 不是一个原子性操作
                    // 1.分配内存空间
                    // 2.执行构造方法，初始化对象
                    // 3.把这个对象指向这个空间
                    
                    // 如果指令重排后线程A执行顺序为132，当线程B进来时，发现LazyMan已经不为null了，就直接返回LazyMan了，						// 但是此时2还未执行，就会出错。所以要加volatile，保证CPU不能进行指令重排
                }
            }
        }
        return LazyMan;
    }
}
```

可以通过静态内部类创建单例

```java
public class TestInner {
    private TestInner(){}
    public static TestInner getInstance(){
        return InnerClass.testInner;
    }
    public static class InnerClass{
        private static final TestInner testInner = new TestInner();
    }
}
```

​		**单例模式不安全**，可以通过反射破坏，可以通过使用枚举类来实现单例，**枚举是有参构造**（String s，int i）（代码中是无参构造，但是从class反编译发现有参数），枚举不能通过反射破坏，因为在**反射规则中定义**了，只要反射枚举类就会抛出异常

#### <span style='color:red'>18）CAS</span>

​		CAS：CompareAndSet比较当前工作内存中的值和主内存中的值，如果这个值是期望的，那么则执行操作，如果不是就一直循环。CAS是CPU指令级操作，属于原子性操作，提交，检查，更新/返回是**原子操作**。

​		CAS属于**乐观锁**（不上锁，提交时进行检查）、Synchronized属于**悲观锁**（一个上锁，其他挂起）

<span style='color:red'>**问题：**</span>

1. 循环会耗时
2. 一次性只能保证一个共享变量的原子性
3. **ABA问题**（带版本号的原子操作，引入**原子引用**）

#### <span style='color:red'>19）各种锁</span>

**公平锁**：非常公平、不能插队（必须先来后到）

**非公平锁**：非常不公平、可以插队（默认都是非公平锁）

**可重入锁**：也叫递归锁，锁必须配对

**自旋锁**：是指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。

**死锁**：四个必要条件

（1） **互斥条件**：一个资源每次只能被一个进程使用。
（2） **请求与保持条件**：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
（3） **不剥夺条件**：进程已获得的资源，在末使用完之前，不能强行剥夺。
（4） **循环等待条件**：若干进程之间形成一种头尾相接的循环等待资源关系。

**解决方法**：

1. 使用jps定位进程号：<span style='color:red'>**jps -l**</span>（查看当前或者的进程）
2. 使用<span style='color:red'>**jstack + 进程号**</span>查看堆栈信息。

#### <span style='color:red'>n）杂七杂八的</span>

**Condition**

**StampedLock**

**Atomic**

**threadlocal**

## 3、JVM

#### <span style='color:red'>1）JVM结构框架</span>

![image-20210112155234907](https://raw.githubusercontent.com/limboline/PicBed/master/img/20210520173410.png)

![image-20210112160308549](https://raw.githubusercontent.com/limboline/PicBed/master/img/20210520173423.png)

![img](https://img-blog.csdnimg.cn/20190216114129109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NTI0NjYx,size_16,color_FFFFFF,t_70)

#### <span style='color:red'>2）类加载器</span>

1. 虚拟机自带的加载器

2. 启动类（根）加载器

   负责加载JDK中的核心类库，如：rt.jar、resources.jar、charsets.jar等，
   **负责加载存放在%JAVA_HOME%\lib目录中的**，或者通被-Xbootclasspath参数所指定的路径中的，并且被java虚拟机识别的(仅按照文件名识别，如rt.jar，名字不符合的类库，即使放在指定路径中也不会被加载)类库到虚拟机的内存中，启动类加载器无法被java程序直接引用。

3. 扩展类加载器

   由sun.misc.Launcher$ExtClassLoader实现，**负责加载%JAVA_HOME%\lib\ext目录中的**，或者被java.ext.dirs系统变量所指定的路径中的所有类库，开发者可以直接使用扩展类加载器。

4. 应用程序加载器

   由sun.misc.Launcher$AppClassLoader实现，**负责加载用户类路径classpath上所指定的类库**，是类加载器ClassLoader中的getSystemClassLoader()方法的返回值，开发者可以直接使用应用程序类加载器，如果程序中没有自定义过类加载器，该加载器就是程序中默认的类加载器。

双亲委派机制

![image-20210112163754431](https://raw.githubusercontent.com/limboline/PicBed/master/img/20210520173435.png)

#### <span style='color:red'>3）堆内存在不同jdk版本中的分配</span>

![图 25-1  JDK1.6、JDK1.7、JDK1.8，内存模型演变](https://img-blog.csdnimg.cn/img_convert/d8291591919c226d321f30d983823a3a.png)

元空间代替永久代的原因：

**在之前的版本中**，**字符串常量池存在于永久代中**，在大量使用字符串的情况下，**非常容易出现OOM的异常**。此外，JVM加载的class的总数，方法的大小等都很难确定，因此**对永久代大小的指定难以确定**。太小的永久代容易导致**永久代内存溢出**，太大的永久代则容易导致**虚拟机内存紧张**。

元空间(Metaspace)，不再与堆连续，而是**直接存在于本地内存**中，也就是机器的内存。理论上机器内存有多大，元空间就有多大。

#### <span style='color:red'>4）OOM时如何解决问题</span>

1. 尝试扩大堆内存看结果
2. 分析内存，看一下哪个地方出现了问题（专业工具，**内存快照分析工具，MAT，Jprofiler**）

MAT，Jprofiler作用：

1. 分析Dump内存文件，快速定位内存泄漏；
2. 获取队中的数据
3. 获得大的对象

#### <span style='color:red'>5）GC常用算法</span>

主要针对堆内存，堆内存分**新生代（1/3）**和**年老代（2/3）**，新生代又分为三个区域，**Eden（8/10）**、**From Survive（1/10）**、**To Survive（1/10）**。采用**分代收集算法**，分新生代和老年代



首先判断那些对象需要被回收

**引用计数法**和**可达性分析法**

第一步**找出GC Roots**，第二步**从GC Roots开始向下遍历整个对象图**。

在执行GC之前更新OopMap，OopMap用来找GC Roots。

**OopMap**是一种来**记录哪些位置是引用**的数据结构，在类加载完成后就将哪个对象内什么偏移量上是什么数据类型计算出来，这样收集器就可以直接得知这些信息，而不需要遍历寻找GC Roots。

OopMap只在特定位置更新（“安全点”（safepoint）），GC不是在任意位置都可以进入，而只能在safepoint处进入。

**安全点位置**：

1. **循环的末尾** 
2. **方法临返回前 / 调用方法的call指令后** 
3. **可能抛异常的位置**

GC Roots：

1. **虚拟机栈(栈帧中的本地变量表)中引用的对象**。(可以理解为:引用栈帧中的本地变量表的所有对象)
2. **方法区中静态属性引用的对象**(可以理解为:引用方法区该静态属性的所有对象)
3. **方法区中常量引用的对象**(可以理解为:引用方法区中常量的所有对象)
4. **本地方法栈中(Native方法)引用的对象**(可以理解为:引用Native方法的所有对象)



其次进行分代回收

**Minor GC**（复制算法）

JVM 每次只会使用 Eden 和其中的一块 Survivor 区域来为对象服务，所以无论什么时候，总是有一块 Survivor 区域是空闲着的。因此，**一般新生代可用空间=Eden+from**，新生代实际可用的内存空间为 9/10 ( 即90% )的新生代空间。

初级回收将年轻代分为三个区域, 一个新生代 , 2个大小相同的复活代, 应用程序只能使用一个新生代和一个复活代, 当发生初级垃圾回收的时候,gc挂起程序, **然后将新生代和复活代中的存活对象复制到另外一个非活动的复活代中,然后一次性清除新生代和复活代**，**将原来的非复活代标记成为活动复活代**。将在指定次数回收后仍然存在的对象移动到老年代中，初级回收后，得到一个空的可用的新生代。



**Major GC**（标记清除法和标记整理法）

#### <span style='color:red'>6）类的生命周期</span>

![img](https://img2018.cnblogs.com/blog/1595409/201905/1595409-20190521154930506-891623513.png)

**加载**：

**通常我们说类加载指的是类的生命周期中加载、连接、初始化三个阶段。**

在加载阶段，找到需要加载的类并把类的信息加载到jvm的方法区中，然后在堆区中实例化一个java.lang.Class对象，作为方法区中这个类的信息的入口。

**连接**：

这个阶段的主要任务就是做一些加载后的验证工作以及一些初始化前的准备工作，可以细分为三个步骤：验证、准备和解析。

1. **验证：**当一个类被加载之后，必须要**验证一下这个类是否合法**，比如这个类是不是符合字节码的格式、变量与方法是不是有重复、数据类型是不是有效、继承与实现是否合乎标准等等。总之，这个阶段的目的就是**保证加载的类是能够被jvm所运行**。
2. **准备**：准备阶段的工作就是**为类的静态变量分配内存并设为jvm默认的初值**，对于非静态的变量，则不会为它们分配内存。有一点需要注意，这时候，**静态变量的初值为jvm默认的初值，而不是我们在程序中设定的初值**。jvm默认的初值是这样的：
   基本类型（int、long、short、char、byte、boolean、float、double）的默认值为0。
   引用类型的默认值为null。
   常量的默认值为我们程序中设定的值，比如我们在程序中定义final static int a = 100，则准备阶段中a的初值就是100。
3. **解析**：这一阶段的任务就是**把常量池中的符号引用转换为直接引用**。

**初始化**：

 如果一个类被直接引用，就会触发类的初始化。在java中，直接引用的情况有：

- 通过new关键字实例化对象、读取或设置类的静态变量、调用类的静态方法。
- 通过反射方式执行以上三种行为。
- 初始化子类的时候，会触发父类的初始化。
- 作为程序入口直接运行时（也就是直接调用main方法）。

在类的初始化阶段，**只会初始化与类相关的静态赋值语句和静态语句**，也就是有static关键字修饰的信息，而没有static修饰的赋值语句和执行语句在实例化对象的时候才会运行。

除了以上四种情况，其他使用类的方式叫做**被动引用**，而被动引用不会触发类的初始化。

**被动引用**：

- 通过子类引用父类的静态字段，不会导致子类的初始化  
- 通过数组引用来引用类，不会触发此类的初始化 
- 常量在编译阶段会被存入调用类的常量池中，本质上并没有引用到定义常量类类，所以自然不会触发定义常量的类的初始化 

**使用**：

使用阶段包括主动引用和被动引用，主动饮用会引起类的初始化，而被动引用不会引起类的初始化。

**卸载**：

在类使用完之后，如果满足下面的情况，类就会被卸载：

- 该类**所有的实例都已经被回收**，也就是java堆中不存在该类的任何实例。
- 加载**该类的ClassLoader已经被回收**。
- 该类对应的java.lang.Class对象**没有任何地方被引用**，**无法在任何地方通过反射访问该类的方法**。

如果以上三个条件全部满足，jvm就会在方法区垃圾回收的时候对类进行卸载，类的卸载过程其实就是在方法区中清空类信息，java类的整个生命周期就结束了。

#### <span style='color:red'>7）常量池</span>

**类常量池**：

我们写的每一个Java类被编译后，就会形成一份class文件；class文件中除了包含类的版本、字段、方法、接口等描述信息外，还有一项信息就是常量池(constant pooltable)，用于存放编译器生成的各种**字面量**(Literal)和**符号引用**(Symbolic References)；

每个class文件都有一个class常量池。

- 字面量包括：1.**文本字符串** 2.**八种基本类型的值** 3**.被声明为final的常量**等;
- 符号引用包括：1.**类和方法的全限定名** 2.**字段的名称和描述符** 3.**方法的名称和描述符**。

**运行时常量池**：

而当类加载到内存中后，jvm就会将**class常量池中的内容存放到运行时常量池中**，由此可知，运行时常量池也是每个类都有一个。在解析阶段，会**把符号引用替换为直接引用**，除了字符串常亮其他字面量都会停留在运行时常量池。

**字符串常量池**：

在**首次使用某个字符串字面量时**，字符串字面量以真正的String对象的方式存放在 **字符串常量池（String Pool）** 中。

```java
    String s1 = new String("He") + new String("llo");	// 堆上创建"Hello","He","llo"实例,
													 	//	String Pool中创建"He"和"llo"实例,
													 	// 之前"He"和"llo"均在运行时常量池
    s1.intern();					// 将堆上"Hello"的引用存入String Pool
    String s2 = "Hello";			// 获取String Pool中的"Hello"的引用
    System.out.println(s1 == s2);	// true
```
#### <span style='color:red'>8）垃圾收集器</span>

![https://img-blog.csdn.net/20180813194627489?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI5OTgyNTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70](https://img-blog.csdn.net/20180813194627489?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI5OTgyNTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

新生代收集器：Serial、ParNew、Parallel Scavenge；老年代收集器：Serial Old、Parallel Old、CMS；整堆收集器：G1；

**Serial收集器**

是一个单线程的收集器。“单线程”的意义不仅仅是它只会使用一个CPU或一条收集器线程去完成垃圾收集工作，更重要的是它在垃圾收集的时候，必须暂停其他所有工作的线程，直到它收集结束。

**ParNew收集器**

是Serial收集器的**多线程版本**，**并行收集**（非并发，并行指多条垃圾收集线程并行工作，此时用户线程仍处于等待状态）

**Parallel Scavenge收集器（jdk1.8默认）**

是并行的多线程收集器，关注点是**一个可控制的吞吐量**（吞吐量 = 运行用户代码时间 / （运行用户代码时间 + 垃圾收集时间）），而**其他收集器**关注点在尽可能的**缩短垃圾收集时用户线程的停顿时间**。

可以设置的参数有**控制最大垃圾收集停顿时间**、**设置吞吐量的大小**、**可以开启自适应细节参数（新生代大小、Eden与Survive比例、晋升老年代年龄等）**

**Serial Old收集器**

是Seria收集器的老年代版本，他同样是一个单线程收集器，使用" 标记-整理" 算法。

**Parallel Old收集器（jdk1.8默认）**

是Parallel Scavenge收集器的老年代版本，使用多线程和“标记-整理”算法。

**CMS收集器**

是一种**以获取最短回收停顿时间为目标**的收集器。基于“**标记-清除**”算法实现，它的整个运行过程可以分为：**初始登记**（标记一下GC Roots能直接关联到的对象，这个过程速度很快）、**并发标记**（进行GCRoots Tracing的过程）、**重新标记**（修正并发标记期间因用户线程继续运作而导致标记产生变动的那一部分对象的标记记录，速度稍慢）、**并发清除**（清除死亡的对象）4个步骤；其中，初始标记和重新标记仍然需要“Stop The World”。

CMS收集器运行的整个过程中，**最耗费时间**的**并发标记**和**并发清除**过程收集器线程和用户线程是一起工作的，所以**总体来说**，CMS收集器的**内存回收过程是与用户线程一起并发执行**的。

优点是**并发收集**、**低停顿**。

缺点是**并发时占用CPU资源**、**并发清除时可能会继续产生垃圾**、**标记清除会有碎片**。

可以改变的参数有**进行几次Full GC后，进行一次碎片整理**、**设定CMS的线程数量（一般情况约等于可用CPU数量）**

**G1收集器**

与前面的垃圾收集器有很大不同，它把新生代、老年代的划分取消了。取而代之，G1算法将堆**划分为若干个区域**（Region），它仍然属于分代收集器。不过，这些区域的一部分包含新生代，**新生代的垃圾收集依然采用暂停所有应用线程的方式**，将存活对象拷贝到老年代或者Survivor空间。老年代也分成很多区域，**G1收集器通过将对象从一个区域复制到另外一个区域**，完成了清理工作。这就意味着，在正常的处理过程中，G1完成了堆的压缩（至少是部分堆的压缩），这样也就不会有CMS内存碎片问题的存在了。

从**整体**看，是基于**标记-整理**算法；从**局部**（两个Region间）看，是**基于复制**算法。

**G1收集器之所以能建立可预测的停顿时间模型**，是因为它可以**有计划地避免在整个Java堆中进行全区域的垃圾收集**；

G1**跟踪各个Region里面的垃圾堆积的价值大小**（回收所获得的空间大小以及回收所需时间的经验值），**在后台维护一个优先列表**，每次根据允许的收集时间，**优先回收价值最大的Region，这样就保证了在有限的时间内尽可能提高效率。（这也就是Garbage-First名称的来由）**；

这种使用Region**划分内存空间**以及有**优先级的区域回收**方式，保证了G1收集器在**有限的时间**内可以获取**尽可能高的收集效率**。

可以改变的参数有**开始并发标记的Java堆占用率**、**停顿时间（默认200ms）**、**Region大小**

#### <span style='color:red'>9）四种引用</span>

**强引用**：

new对象就是强引用，**永远不会回收**，内存不足，jvm宁愿抛出OutOfMemoryError错误，也不会回收

**软引用**：

SoftReference<String> softRef=new SoftReference<String>(new String("aaa"))；

softRef是强引用，指向new String(“aaa”)的引用是软引用。

如果**内存足够则不会回收软引用指向的对象**，内存不足则会回收该对象。

**可用场景**：**创建缓存的时候**，创建的对象放进缓存中，当内存不足时，JVM就会回收早先创建的对象。

**弱引用**：

WeakReference<String> weakRef=new WeakReference<String>(new String("aaa"))；

softRef是强引用，指向new String(“aaa”)的引用是弱引用。

当进行垃圾回收时**只要被发现**，不管内存够不够，**都会回收该对象**。

**可用场景**：缓存？

**虚引用**：

PhantomReference<String> prf = new PhantomReference<String>(new String("aaa"), new ReferenceQueue<>());

prf 是强引用，指向new String(“aaa”)的引用是虚引用。

**虚引用和没有引用是一样的**，需要和队列(ReferenceQueue)联合使用。**当jvm扫描到虚引用的对象时**，**会先将此对象放入关联的队列中**，因此我们可以通过判断队列中是否存这个对象，来进行回收前的一些处理。

**可用场景**：对象销毁前的一些操作，比如说资源释放等。 Object.finalize() 虽然也可以做这类动作，但是这个方式即不安全又低效

#### <span style='color:red'>10）逃逸分析</span>

如果**一个对象的指针被多个方法或者线程引用**时，那么我们就称这个对象的指针发生了逃逸。

开启逃逸分析后（jdk1.8默认开启），如果一个指针不发生逃逸，则可以

- **栈上分配**，可以降低垃圾收集器运行的频率。

- **同步消除**，如果发现某个对象只能从一个线程可访问，那么在这个对象上的操作可以不需要同步。

- **标量替换**，如果该对象不会被外部访问，并且对象可以被进一步分解时，JVM不会创建该对象，而会将该对象成员变量分解若干个被这个方法使用的成员变量所代替。这些代替的成员变量在栈帧或寄存器上分配空间。

#### <span style='color:red'>n）杂七杂八</span>

**沙箱安全机制**

## 4、操作系统

#### <span style='color:red'>1）进程间的状态</span>

- 就绪：进程已处于准备好运行的状态，即进程已分配到除CPU外的所有必要资源后，只要再获得CPU，便可立即执行
- 执行：进程已经获得CPU，程序正在执行状态
- 阻塞：正在执行的进程由于发生某事件（如I/O请求、申请缓冲区失败等）暂时无法继续执行的状态

![img](https://pic2.zhimg.com/80/v2-f8e903fb92e0458073c84814d5940efd_720w.jpg)

#### <span style='color:red'>2）进程和线程</span>

- **进程**是具有一定独立功能的程序关于某个数据集合上的一次运行活动，**进程是系统进行资源分配和调度的一个独立单位**
- **线程**是进程的一个实体，**是CPU调度和分派的基本单位**，它是比进程更小的能独立运行的基本单位

**进程和线程的关系**

- 一个线程只能属于一个进程，而一个进程可以有多个线程，但至少有一个线程。**线程是操作系统可识别的最小执行和调度单位**
- **资源分配给进程，同一进程的所有线程共享该进程的所有资源**。 **同一进程中的多个线程共享代码段(代码和常量)，数据段(全局变量和静态变量)，扩展段(堆存储)**。但是每个线程拥有自己的栈段，栈段又叫运行时段，用来存放所有局部变量和临时变量
- **处理机分给线程，即真正在处理机上运行的是线程**
- 线程在执行过程中，需要协作同步。**不同进程的线程间要利用消息通信的办法实现同步**

**进程与线程的区别**

- 进程**有自己的独立地址空间，线程没有**
- 进程是**资源分配的最小单位，线程是CPU调度的最小单位**
- 进程和线程通信方式不同(线程之间的通信比较方便。同一进程下的线程共享数据（比如全局变量，静态变量），通过这些数据来通信不仅快捷而且方便，**当然如何处理好这些访问的同步与互斥正是编写多线程程序的难点。**而进程之间的通信只能通过进程通信的方式进行。)
- 进程上下文切换开销大，线程开销小
- 一个进程挂掉了不会影响其他进程，而线程挂掉了会影响其他线程
- 对进程操作一般开销都比较大，对线程开销就小了

**为什么进程上下文切换比线程上下文切换代价高？**

进程切换分两步：

- 切换页目录以使用新的地址空间
- 切换内核栈和硬件上下文

对于linux来说，线程和进程的最大区别就在于地址空间，对于线程切换，第1步是不需要做的，第2是进程和线程切换都要做的

切换的性能消耗：

- 线程上下文切换和进程上下文切换一个最主要的区别是线程的切换虚拟内存空间依然是相同的，但是进程切换是不同的。这两种上下文切换的处理都是通过操作系统内核来完成的。内核的这种切换过程伴随的最显著的性能损耗是将寄存器中的内容切换出
- 另外一个隐藏的损耗是上下文的切换会扰乱处理器的缓存机制。简单的说，一旦去切换上下文，处理器中所有已经缓存的内存地址一瞬间都作废了。还有一个显著的区别是当你改变虚拟内存空间的时候，处理的页表缓冲（processor's Translation Lookaside Buffer (TLB)）或者相当的神马东西会被全部刷新，这将导致内存的访问在一段时间内相当的低效。但是在线程的切换中，不会出现这个问题

#### <span style='color:red'>3）进程同步</span>

进程同步的主要任务：是对多个相关进程在执行次序上进行协调，以使并发执行的诸进程之间能有效地共享资源和相互合作，从而使程序的执行具有可再现性

同步机制遵循的原则：

- 空闲让进；
- 忙则等待（保证对临界区的互斥访问）；
- 有限等待（有限代表有限的时间，避免死等）；
- 让权等待，（当进程不能进入自己的临界区时，应该释放处理机，以免陷入忙等状态）

#### <span style='color:red'>4）进程间通信</span>

进程通信，是指进程之间的信息交换（信息量少则一个状态或数值，多者则是成千上万个字节）。因此，对于用信号量进行的进程间的互斥和同步，由于其所交换的信息量少而被归结为低级通信

所谓高级进程通信指：用户可以利用操作系统所提供的一组通信命令传送大量数据的一种通信方式。操作系统隐藏了进程通信的实现细节。或者说，通信过程对用户是透明的

高级通信机制可归结为三大类：

- 共享存储器系统（存储器中划分的共享存储区）；实际操作中对应的是“剪贴板”（剪贴板实际上是系统维护管理的一块内存区域）的通信方式，比如举例如下：word进程按下ctrl+c，在ppt进程按下ctrl+v，即完成了word进程和ppt进程之间的通信，复制时将数据放入到剪贴板，粘贴时从剪贴板中取出数据，然后显示在ppt窗口上

- 消息传递系统（进程间的数据交换以消息（message）为单位，当今最流行的微内核操作系统中，微内核与服务器之间的通信，无一例外地都采用了消息传递机制。应用举例：邮槽（MailSlot）是基于广播通信体系设计出来的，它采用无连接的不可靠的数据传输。邮槽是一种单向通信机制，创建邮槽的服务器进程读取数据，打开邮槽的客户机进程写入数据

- 管道通信系统（管道即：连接读写进程以实现他们之间通信的共享文件（pipe文件，类似先进先出的队列，由一个进程写，另一进程读））。实际操作中，管道分为：匿名管道、命名管道。匿名管道是一个未命名的、单向管道，通过父进程和一个子进程之间传输数据。匿名管道只能实现本地机器上两个进程之间的通信，而不能实现跨网络的通信。命名管道不仅可以在本机上实现两个进程间的通信，还可以跨网络实现两个进程间的通信

- - **管道**：管道是单向的、先进先出的、无结构的、固定大小的字节流，它把一个进程的标准输出和另一个进程的标准输入连接在一起。写进程在管道的尾端写入数据，读进程在管道的道端读出数据。数据读出后将从管道中移走，其它读进程都不能再读到这些数据。管道提供了简单的流控制机制。进程试图读空管道时，在有数据写入管道前，进程将一直阻塞。同样地，管道已经满时，进程再试图写管道，在其它进程从管道中移走数据之前，写进程将一直阻塞。
  - **信号量**：信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其它进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段
  - **消息队列**：是一个在系统内核中用来保存消 息的队列，它在系统内核中是以消息链表的形式出现的。消息队列克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点
  - **共享内存**：共享内存允许两个或多个进程访问同一个逻辑内存。**这一段内存可以被两个或两个以上的进程映射至自身的地址空间中**，一个进程写入共享内存的信息，可以被其他使用这个共享内存的进程，通过一个简单的内存读取读出，从而实现了进程间的通信**。如果某个进程向共享内存写入数据，所做的改动将立即影响到可以访问同一段共享内存的任何其他进程。**共享内存是最快的IPC方式，它是针对其它进程间通信方式运行效率低而专门设计的。它往往与其它通信机制（如**信号量**）配合使用，来实现进程间的同步和通信
  - **套接字**：套接字也是一种进程间通信机制，与其它通信机制不同的是，它可用于不同机器间的进程通信

#### <span style='color:red'>5）进程的调度</span>

**调度种类**

- **高级调度**：(High-Level Scheduling)又称为作业调度，它决定把后备作业调入内存运行
- **低级调度**：(Low-Level Scheduling)又称为进程调度，它决定把就绪队列的某进程获得CPU
- **中级调度**：(Intermediate-Level Scheduling)又称为在虚拟存储器中引入，在内、外存对换区进行进程对换

**非抢占式调度与抢占式调度**

- **非抢占式**：分派程序一旦把处理机分配给某进程后便让它一直运行下去，直到进程完成或发生进程调度进程调度某事件而阻塞时，才把处理机分配给另一个进程
- **抢占式**：操作系统将正在运行的进程强行暂停，由调度程序将CPU分配给其他就绪进程的调度方式

**调度算法**

**FIFO或First Come, First Served (FCFS)先来先服务**

- 调度的顺序就是任务到达就绪队列的顺序
- 公平、简单(FIFO队列)、非抢占、不适合交互式
- 未考虑任务特性，平均等待时间可以缩短

**Shortest Job First (SJF)**

- 最短的作业(CPU区间长度最小)最先调度
- SJF可以保证最小的平均等待时间

**Shortest Remaining Job First (SRJF)**

- SJF的可抢占版本，比SJF更有优势
- SJF(SRJF): 如何知道下一CPU区间大小？根据历史进行预测: 指数平均法

**优先权调度**

- 每个任务关联一个优先权，调度优先权最高的任务
- 注意：优先权太低的任务一直就绪，得不到运行，出现“饥饿”现象

**Round-Robin(RR)轮转调度算法**

- 设置一个时间片，按时间片来轮转调度（“轮叫”算法）
- 优点: 定时有响应，等待时间较短；缺点: 上下文切换次数较多
- 时间片太大，响应时间太长；吞吐量变小，周转时间变长；当时间片过长时，退化为FCFS

**多级队列调度**

- 按照一定的规则建立多个进程队列
- 不同的队列有固定的优先级（高优先级有抢占权）
- 不同的队列可以给不同的时间片和采用不同的调度方法
- 存在问题1：没法区分I/O bound和CPU bound
- 存在问题2：也存在一定程度的“饥饿”现象

**多级反馈队列**

- 在多级队列的基础上，任务可以在队列之间移动，更细致的区分任务
- 可以根据“享用”CPU时间多少来移动队列，阻止“饥饿”
- 最通用的调度算法，多数OS都使用该方法或其变形，如UNIX、Windows等

#### <span style='color:red'>n）杂七杂八</span>

**死锁**

##  5、Mysql

#### <span style='color:red'>1）不可重复读（第一类丢失更新、第二类丢失更新）、脏读、幻读</span>

1. **第一类丢失更新**(回滚丢失，Lost update) 

​		A事务撤销时，把已经提交的B事务的更新数据覆盖了。这种错误可能造成很严重的问题，通过下面的账户取款转账就可以看出来：

| 时间 | 取款事务A                             | 转账事务B                 |
| ---- | ------------------------------------- | ------------------------- |
| T1   | **开始事务**                          |                           |
| T2   |                                       | **开始事务**              |
| T3   | 查询账户余额为1000元                  |                           |
| T4   |                                       | 查询账户余额为1000元      |
| T5   |                                       | 汇入100元把余额改为1100元 |
| T6   |                                       | **提交事务**              |
| T7   | 取出100元把余额改为900元              |                           |
| T8   | **撤销事务**                          |                           |
| T9   | **余额恢复为1000** **元（丢失更新）** |                           |

 A事务在撤销时，“不小心”将B事务已经转入账户的金额给抹去了。

2. **第二类丢失更新**(覆盖丢失/两次更新问题，Second lost update) 

​		A事务覆盖B事务已经提交的数据，造成B事务所做操作丢失：  

| 时间 | 转账事务A                             | 取款事务B                |
| ---- | ------------------------------------- | ------------------------ |
| T1   |                                       | **开始事务**             |
| T2   | **开始事务**                          |                          |
| T3   |                                       | 查询账户余额为1000元     |
| T4   | 查询账户余额为1000元                  |                          |
| T5   |                                       | 取出100元把余额改为900元 |
| T6   |                                       | **提交事务**             |
| T7   | 汇入100元                             |                          |
| T8   | **提交事务**                          |                          |
| T9   | **把余额改为1100** **元（丢失更新）** |                          |

​		上面的例子里由于支票转账事务覆盖了取款事务对存款余额所做的更新，导致银行最后损失了100元，相反如果转账事务先提交，那么用户账户将损失100元。

3. **脏读**

​		A事务执行过程中，B事务读取了A事务的修改。但是由于某些原因，A事务可能没有完成提交，发生RollBack了操作，则B事务所读取的数据就会是不正确的。这个未提交数据就是脏读（Dirty Read）。

4. **幻读**

​		B事务读取了两次数据，在这两次的读取过程中A事务添加了数据，B事务的这两次读取出来的集合不一样。（与不可重读的区别在于，第二次读到了新插入的数据，即使对全部数据上锁，也锁不住新加入的数据；）

#### <span style='color:red'>2）隔离级别</span>

1）**Read uncommitted 读得到未提交数据**（最低级别）

在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少。读取未提交的数据，也被称之为脏读（Dirty Read）。

2）**Read committed 读已提交数据**（可避免脏读）

这是大多数数据库系统的默认隔离级别（**但不是MySQL默认的、Oracle默认**）。它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这种隔离级别也支持所谓的不可重复读（Nonrepeatable Read），因为同一事务的其他实例在该实例处理其间可能会有新的commit，所以同一select可能返回不同结果。

3）**Repeatable read 可重复读**（避免脏读，不可重复读）（**MySQL默认**）

这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。不过理论上，这会导致另一个棘手的问题：幻读 （Phantom Read）。简单的说，幻读指当用户读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户再读取该范围的数据行时，会发现有新的“幻影” 行。InnoDB和Falcon存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control）机制解决了该问题。

4）**Serializable 串行化**（最高级别，可避免脏读，不可重复读，幻读）

这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。

#### <span style='color:red'>3）mysql自带的数据库</span>

其中information_schema、performance_schema、mysql、sys 都是mysql自动创建的数据库，如下给出这几库的简单信息：

**information_schema**数据库又称为信息架构，数据表保存了MySQL服务器所有数据库的信息。如数据库名，数据库的表，表栏的数据类型与访问权限等。

**performance_schema**数据库主要用于收集数据库服务器性能参数，以便优化mysql数据库性能。

**mysql**数据库是存储着与MySQL运行相关的基本信息等数据管理的数据库。

**sys** 数据库是mysql5.7增加的，通过这个库可以快速的了解系统的元数据信息，这个库可以方便DBA发现数据库的很多信息，提供解决性能瓶颈的信息。

#### <span style='color:red'>4）b树和b+树</span>

1. **b树**：是一种多路搜索树

   a. 每个节点最多有m-1个关键字；

   b. 根节点最少可以只有1个关键字；

   c. 非根节点至少有m/2个关键字；

   d. 每个节点中的关键字都按照从小到大的顺序排列，每个关键字的左子树中的所有关键字都小于它，而右子树中的所有关键字都大于它；

   e. 所有叶子节点都位于同一层，或者说根节点到每个叶子节点的长度都相同；

   f. 每个节点都存有索引和数据，也就是对应的key和value；

   如：（M=3）

![img](https://img-blog.csdnimg.cn/20190626150937968.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhczcyMw==,size_16,color_FFFFFF,t_70)

​		B-树的搜索，从根结点开始，对结点内的关键字（有序）序列进行二分查找，如果命中则结束，否则进入查询关键字所属范围的儿子 		结点；重复，直到所对应的儿子指针为空，或已经是叶子结点；

​		B-树的特性：

​    		1.关键字集合分布在整颗树中；

​    		2.任何一个关键字出现且只出现在一个结点中；

​    		3.搜索有可能在非叶子结点结束；

​    		4.其搜索性能等价于在关键字全集内做一次二分查找；

​    		5.自动层次控制；

​    		由于限制了除根结点以外的非叶子结点，至少含有M/2个儿子，确保了结点的至少利用率，其最低搜索性能为：

![img](https://img-blog.csdnimg.cn/20190626150937763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhczcyMw==,size_16,color_FFFFFF,t_70)

​		其中，M为设定的非叶子结点最多子树个数，N为关键字总数；

​    	所以B-树的性能总是等价于二分查找（与M值无关），也就没有B树平衡的问题；

​    	由于M/2的限制，在**插入结点**时，如果结点已满，需要将结点分裂为两个各占M/2的结点；**删除结点**时，需将两个不足M/2的兄弟结		点合并；



2. **b+树**：B+树是B-树的变体，也是一种多路搜索树

   a. 其定义基本与B-树同，除了：

   b. 非叶子结点的子树指针与关键字个数相同；

   c. 非叶子结点的子树指针P[i]，指向关键字值属于[K[i], K[i+1])的子树（b树是开区间）；

   d. 为所有叶子结点增加一个链指针；

   e. 所有关键字都在叶子结点出现；

    如：（M=3）

![img](https://img-blog.csdnimg.cn/20190626150938306.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhczcyMw==,size_16,color_FFFFFF,t_70)

​		B+的搜索与B-树也基本相同，区别是B+树只有达到叶子结点才命中（B-树可以在非叶子结点命中），其性能也等价于在关键字全集		做一次二分查找；

​    	B+的特性：

​    		1.所有关键字都出现在叶子结点的链表中（稠密索引），且链表中的关键字恰好是有序的；

​		    2.不可能在非叶子结点命中；

​		    3.非叶子结点相当于是叶子结点的索引（稀疏索引），叶子结点相当于是存储（关键字）数据的数据层；

​		    4.更适合文件索引系统；

**mysql选择用b+树做索引不是b树**：

1、 **B+树的磁盘读写代价更低**：B+树的内部节点并没有指向关键字具体信息的指针，因此其内部节点相对B树更小，如果把所有同一内部节点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多，一次性读入内存的需要查找的关键字也就越多，相对IO读写次数就降低了。

2、**B+树的查询效率更加稳定**：由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。

3、**B+树更便于遍历**：由于B+树的数据都存储在叶子结点中，分支结点均为索引，方便扫库，只需要扫一遍叶子结点即可，但是B树因为其分支结点同样存储着数据，我们要找到具体的数据，需要进行一次中序遍历按序来扫，所以B+树更加适合在区间查询的情况，所以通常B+树用于数据库索引。

4、**B+树更适合基于范围的查询**：B树在提高了IO性能的同时并没有解决元素遍历的我效率低下的问题，正是为了解决这个问题，B+树应用而生。B+树只需要去遍历叶子节点就可以实现整棵树的遍历。而且在数据库中基于范围的查询是非常频繁的，而B树不支持这样的操作或者说效率太低。

#### <span style='color:red'>5）哈希和B+树优缺点</span>

简单地说，**哈希索引就是采用一定的哈希算法**，把键值换算成新的哈希值，检索时不需要类似B+树那样从根节点到叶子节点逐级查找，只需一次哈希算法即可立刻定位到相应的位置，速度非常快。

B+树索引和哈希索引的明显区别是：

- **如果是等值查询，那么哈希索引明显有绝对优势**，因为只需要经过一次算法即可找到相应的键值；当然了，这个前提是，键值都是唯一的。如果键值不是唯一的，就需要先找到该键所在位置，然后再根据链表往后扫描，直到找到相应的数据；
- **如果是范围查询检索，这时候哈希索引就毫无用武之地了**，因为原先是有序的键值，经过哈希算法后，有可能变成不连续的了，就没办法再利用索引完成范围查询检索；
- 同理，**哈希索引也没办法利用索引完成排序**，以及like ‘xxx%’ 这样的部分模糊查询（这种部分模糊查询，其实本质上也是范围查询）；
- 优化器无法使用哈希索引来加速 ORDER BY操作。
- **哈希索引也不支持多列联合索引的最左匹配规则**；
- B+树索引的关键字检索效率比较平均，不像B树那样波动幅度大，**在有大量重复键值情况下，哈希索引的效率也是极低的，因为存在所谓的哈希碰撞问题**。

#### <span style='color:red'>6）mysql的索引</span>

MySQL 索引可以分为**普通索引（单列索引）**、 **复合索引（组合索引）**、**唯一索引**、**主键索引**、**全文索引**等

1. **普通索引（单列索引）**：也称为普通索引，单列索引是最基本的索引，它没有任何限制。

2. **复合索引（组合索引）**：是在多个字段上创建的索引。**复合索引遵守“最左前缀”原则**，**即在查询条件中使用了复合索引的第一个字段，索引才会被使用**。因此，在复合索引中索引列的顺序至关重要。

3. **唯一索引**：和普通索引类似，主要的区别在于，**唯一索引限制列的值必须唯一，但允许存在空值（只允许存在一条空值）**。

   如果在已经有数据的表上添加唯一性索引的话：

   a. 如果添加索引的列的值存在两个或者两个以上的**空值**，则不能创建唯一性索引会失败。（一般在创建表的时候，要对自动设置唯一性索引，需要在字段上加上 not null）
   b. 如果添加索引的列的值存在两个或者两个以上的**null值**，还是可以创建唯一性索引，只是后面创建的数据不能再插入null值 ，并且严格意义上此列并不是唯一的，因为存在多个null值。

4. **主键索引**：是一种特殊的唯一索引，一个表只能有一个

5. **主键**，不允许有空值。一般是在建表的时候同时创建主键索引

6. **全文索引**：主要用来查找文本中的关键字，而不是直接与索引中的值相比较。

   在一般情况下，模糊查询都是通过 like 的方式进行查询。但是，对于海量数据，这并不是一个好办法，在 like “value%” 可以使用索引，但是对于 like “%value%” 这样的方式，执行全表查询，这在数据量小的表，不存在性能问题，但是对于海量数据，全表扫描是非常可怕的事情,所以 like 进行模糊匹配性能很差。

   这种情况下，需要考虑使用全文搜索的方式进行优化。全文搜索在 MySQL 中是一个 FULLTEXT 类型索引。**FULLTEXT 索引在 MySQL 5.6 版本之后支持 InnoDB，而之前的版本只支持 MyISAM 表**。

   MySQL 的全文索引最开始仅支持英语，因为英语的词与词之间有空格，使用空格作为分词的分隔符是很方便的。亚洲文字，比如汉语、日语、汉语等，是没有空格的，这就造成了一定的限制。不过 MySQL 5.7.6 开始，引入了一个 **ngram** 全文分析器来解决这个问题，并且对 MyISAM 和 InnoDB 引擎都有效。

   事实上，MyISAM 存储引擎对全文索引的支持有很多的限制，例如表级别锁对性能的影响、数据文件的崩溃、崩溃后的恢复等，这使得 MyISAM 的全文索引对于很多的应用场景并不适合。所以，多数情况下的建议是使用别的解决方案，例如 **Sphinx**、**Lucene** 等等第三方的插件，亦或是使用 InnoDB 存储引擎的全文索引。

**索引的性质**：

1**.索引字段要尽量的小**：我们知道IO次数取决于b+树的高度h，假设当前数据表的数据为N，每个磁盘块的数据项的数量是m，则有h=㏒(m+1)N，当数据量N一定的情况下，m越大，h越小；而m = 磁盘块的大小 / 数据项的大小，磁盘块的大小也就是一个数据页的大小，是固定的，如果数据项占的空间越小，数据项的数量越多，树的高度越低。这就是为什么每个数据项，即索引字段要尽量的小，比如int占4字节，要比bigint8字节少一半。这也是为什么b+树要求把真实的数据放到叶子节点而不是内层节点，一旦放到内层节点，磁盘块的数据项会大幅度下降，导致树增高。当数据项等于1时将会退化成线性表。
2.**索引的最左匹配特性（即从左往右匹配）**：当b+树的数据项是复合的数据结构，比如(name,age,sex)的时候，b+数是按照从左到右的顺序来建立搜索树的，比如当(张三,20,F)这样的数据来检索的时候，b+树会优先比较name来确定下一步的所搜方向，如果name相同再依次比较age和sex，最后得到检索的数据；但当(20,F)这样的没有name的数据来的时候，b+树就不知道下一步该查哪个节点，因为建立搜索树的时候name就是第一个比较因子，必须要先根据name来搜索才能知道下一步去哪里查询。比如当(张三,F)这样的数据来检索时，b+树可以用name来指定搜索方向，但下一个字段age的缺失，所以只能把名字等于张三的数据都找到，然后再匹配性别是F的数据了， 这个是非常重要的性质，即索引的最左匹配特性。

#### <span style='color:red'>7）mysql的存储引擎</span>

1、存储引擎其实就是对于数据库文件的一种存取机制，如何实现存储数据，如何为存储的数据建立索引以及如何更新，查询数据等技术实现的方法。

2、MySQL中的数据用各种不同的技术存储在文件（或内存）中，这些技术中的每一种技术都使用不同的存储机制，索引技巧，锁定水平并且最终提供广泛的不同功能和能力。在MySQL中将这些不同的技术及配套的相关功能称为存储引擎。

**MySQL中常用的几种存储引擎**：**MyISAM**、**InnoDB**、**MEMORY**、**ARCHIVE**

**InnoDB**：支持事务处理，支持外键，支持崩溃修复能力和并发控制。如果需要对事务的完整性要求比较高（比如银行），要求实现并发控制（比如售票），那选择InnoDB有很大的优势。如果需要频繁的更新、删除操作的数据库，也可以选择InnoDB，因为支持事务的提交（commit）和回滚（rollback）。

**MyISAM**：插入数据快，空间和内存使用比较低。如果表主要是用于插入新记录和读出记录，那么选择MyISAM能实现处理高效率。如果应用的完整性、并发性要求比 较低，也可以使用。如果数据表主要用来插入和查询记录，则MyISAM引擎能提供较高的处理效率

**MEMORY**：所有的数据都在内存中，数据的处理速度快，但是安全性不高。如果需要很快的读写速度，对数据的安全性要求较低，可以选择MEMOEY。它对表的大小有要求，不能建立太大的表。所以，这类数据库只使用在相对较小的数据库表。如果只是临时存放数据，数据量不大，并且不需要较高的数据安全性，可以选择将数据保存在内存中的Memory引擎，MySQL中使用该引擎作为临时表，存放查询的中间结果

**Archive**：如果只有INSERT和SELECT操作，可以选择Archive，Archive支持高并发的插入操作，但是本身不是事务安全的。Archive非常适合存储归档数据，如记录日志信息可以使用Archive

注意，同一个数据库也可以使用多种存储引擎的表。如果一个表要求比较高的事务处理，可以选择InnoDB。这个数据库中可以将查询要求比较高的表选择MyISAM存储。如果该数据库需要一个用于查询的临时表，可以选择MEMORY存储引擎。

![image-20210306154156555](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210306154156555.png)

Innodb有frm表结构文件，ibd存表数据和表索引

MYISAM有frm表结构文件，MYD是表数据文件，MYI是表索引文件

#### <span style='color:red'>8）mysql的事务</span>

事务**ACID**(atomicity原子性、consistency一致性、isolation隔离性、durability持久性)
**A**：事务要么全执行，要么全不执行
**C**：事务执行前后，数据完整性一致
**I**：多用户并发访问数据库时，数据库为每个用户创建的事务间相互隔离
**D**：事务一旦被提交，对数据库中数据的改变就是持久的

#### <span style='color:red'>9）mysql的锁</span>

**共享锁（读锁）**：其他事务可以读，但不能写。

**排他锁（写锁）** ：其他事务不能读取，也不能写。

<span style='color:red'>**表级锁、行级锁、页面锁**</span>

**MyISAM** 和 **MEMORY** 存储引擎采用的是**表级锁**（table-level locking）

**BDB** 存储引擎采用的是**页面锁**（page-level locking），但也支持**表级锁**

**InnoDB** 存储引擎既支持**行级锁**（row-level locking），也支持**表级锁**，但默认情况下是采用**行级锁**。

默认情况下，表锁和行锁都是自动获得的， 不需要额外的命令。**但是在有的情况下**， 用户需要明确地进行锁表或者进行事务的控制， 以便确保整个事务的完整性，这样就需要使用事务控制和锁定语句来完成。

- **表级锁**：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低。

  - 这些存储引擎通过总是一次性同时获取所有需要的锁以及总是按相同的顺序获取表锁来避免死锁。
  - 表级锁更适合于以查询为主，并发用户少，只有少量按索引条件更新数据的应用，如Web 应用

- **行级锁**：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。

  - 最大程度的支持并发，同时也带来了最大的锁开销。
  - 在 InnoDB 中，除单个 SQL 组成的事务外，
    锁是逐步获得的，这就决定了在 InnoDB 中发生死锁是可能的。
  - 行级锁只在存储引擎层实现，而Mysql服务器层没有实现。 行级锁更适合于有大量按索引条件并发更新少量不同数据，同时又有并发查询的应用，如一些在线事务处理（OLTP）系统

- **页面锁**：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。

<span style='color:red'>**MyISAM表级锁模式**</span>

- **表共享读锁** （Table Read Lock）：不会阻塞其他用户对同一表的读请求，但会阻塞对同一表的写请求；
- **表独占写锁** （Table Write Lock）：会阻塞其他用户对同一表的读和写操作；

**MyISAM 表的读操作与写操作之间，以及写操作之间是串行的。当一个线程获得对一个表的写锁后， 只有持有锁的线程可以对表进行更新操作。 其他线程的读、 写操作都会等待，直到锁被释放为止。**

**默认情况**下，写**锁比读锁具有更高的优先级**：当一个锁释放时，这个锁会优先给写锁队列中等候的获取锁请求，然后再给读锁队列中等候的获取锁请求。

这也正是 MyISAM 表**不太适合于有大量更新操作和查询操作**应用的原因，因为，大量的更新操作会造成查询操作很难获得读锁，从而可能永远阻塞。同时，一些需要长时间运行的查询操作，也会使写线程“饿死” ，应用中应尽量避免出现长时间运行的查询操作（在可能的情况下可以**通过使用中间表等措施对SQL语句做一定的“分解”** ，使每一步查询都能在较短时间完成，从而减少锁冲突。如果复杂查询不可避免，应尽量安排在数据库空闲时段执行，比如一些定期统计可以安排在夜间执行）。

<span style='color:red'>**MyISAM加表锁方法**</span>

MyISAM 在执行查询语句（SELECT）前，会**自动给涉及的表加读锁**，在执行更新操作（UPDATE、DELETE、INSERT 等）前，会**自动给涉及的表加写锁**，这个过程并不需要用户干预，因此，用户一般不需要直接用 LOCK TABLE 命令给 MyISAM 表显式加锁。

在自动加锁的情况下，**MyISAM 总是一次获得 SQL 语句所需要的全部锁**，这也正是 **MyISAM 表不会出现死锁**（Deadlock Free）的原因。

**MyISAM存储引擎支持并发插入**，以减少给定表的读和写操作之间的争用：

如果一张MyISAM表的数据文件没有洞（在表的中间有删除行），在SELECT语句从表中读取行的同时，一条插入语句在执行的时候可以将新插入的行增加到表的最后。如果有多个INSERT语句，它们会排成队列并依次执行。并发插入的结果可能不会立刻可见。

系统参数concurrent_insert可以用来更改并发插入的处理。

- 当concurrent_insert设置为0时，不允许并发插入。
- 当concurrent_insert设置为1时，如果MyISAM表中没有空洞（即表的中间没有被删除的行），MyISAM允许在一个线程读表的同时，另一个线程从表尾插入记录。**这也是MySQL的默认设置**。
- 当concurrent_insert设置为2时，无论MyISAM表中有没有空洞，都允许在表尾并发插入记录。

<span style='color:red'>**查询表级锁争用情况**</span>

可以通过检查 **table_locks_waited** 和 **table_locks_immediate** 状态变量来分析系统上的表锁的争夺，如果 Table_locks_waited 的值比较高，则说明存在着较严重的表级锁争用情况

<span style='color:red'>**InnoDB锁模式**</span>

InnoDB 实现了以下两种类型的**行锁**：

- **共享锁**（S）：允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁。
- **排他锁**（X）：允许获得排他锁的事务更新数据，阻止其他事务取得相同数据集的共享读锁和排他写锁。

**为了允许行锁和表锁共存，实现多粒度锁机制**，InnoDB 还有两种内部使用的**意向锁**（Intention Locks），这两种意向锁都是**表锁**：

- **意向共享锁**（IS）：事务打算给数据行加行共享锁，事务在给一个数据行加共享锁前必须先取得该表的 IS 锁。
- **意向排他锁**（IX）：事务打算给数据行加行排他锁，事务在给一个数据行加排他锁前必须先取得该表的 IX 锁。

![img](https://pic4.zhimg.com/80/v2-37761612ead11ddc3762a4c20ddab3f3_1440w.jpg)

<span style='color:red'>**InnoDB加锁方法**</span>

- **意向锁是 InnoDB 自动加的**， 不需用户干预。

- 对于 UPDATE、 DELETE 和 INSERT 语句， **InnoDB会自动给涉及数据集加排他锁**（X)；

- 对于普通 SELECT 语句，**InnoDB 不会加任何锁**；**事务可以通过以下语句显式给记录集加共享锁或排他锁**：

- - 共享锁（S）：SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE。 其他 session 仍然可以查询记录，并也可以对该记录加 share mode 的共享锁。但是如果当前事务需要对该记录进行更新操作，则很有可能造成死锁。
  - 排他锁（X)：SELECT * FROM table_name WHERE ... FOR UPDATE。其他 session 可以查询该记录，但是不能对该记录加共享锁或排他锁，而是等待获得锁

- **隐式锁定：**

InnoDB在事务执行过程中，使用两阶段锁协议：

随时都可以执行锁定，InnoDB会根据隔离级别在需要的时候自动加锁；

锁只有在执行commit或者rollback的时候才会释放，并且所有的锁都是在**同一时刻**被释放。

- **显式锁定 ：**

```text
select ... lock in share mode //共享锁 
select ... for update //排他锁 
```

**select for update：**

在执行这个 select 查询语句的时候，会将对应的索引访问条目进行上排他锁（X 锁），也就是说这个语句对应的锁就相当于update带来的效果。

**select XXX for update 的使用场景**：为了让自己查到的数据确保是最新数据，并且查到后的数据只允许自己来修改的时候，需要用到 for update 子句。

**select lock in share mode ：**

in share mode 子句的作用就是将查找到的数据加上一个 share 锁，这个就是表示其他的事务只能对这些数据进行简单的select 操作，并不能够进行 DML 操作。

**select XXX lock in share mode 使用场景**：**为了确保自己查到的数据没有被其他的事务正在修改**，也就是说确保查到的数据是最新的数据，并且不允许其他人来修改数据。但是自己不一定能够修改数据，因为有可能其他的事务也对这些数据 使用了 in share mode 的方式上了 S 锁。

**性能影响：**
select for update 语句，相当于一个 update 语句。在业务繁忙的情况下，**如果事务没有及时的commit或者rollback 可能会造成其他事务长时间的等待**，从而影响数据库的并发使用效率。
select lock in share mode 语句是一个给查找的数据上一个共享锁（S 锁）的功能，它允许其他的事务也对该数据上S锁，但是不能够允许对该数据进行修改。**如果不及时的commit 或者rollback 也可能会造成大量的事务等待**。

**for update 和 lock in share mode 的区别：**

前一个上的是排他锁（X 锁），一旦一个事务获取了这个锁，其他的事务是没法在这些数据上执行 for update ；后一个是共享锁，多个事务可以同时的对相同数据执行 lock in share mode。

<span style='color:red'>**InnoDB 行锁实现方式**</span>

- InnoDB 行锁是**通过给索引上的索引项加锁来实现的**，这一点 MySQL 与 Oracle 不同，后者是通过在数据块中对相应数据行加锁来实现的。InnoDB 这种行锁实现特点意味着：**只有通过索引条件检索数据，InnoDB 才使用行级锁，否则，InnoDB 将使用表锁**！
- 不论是使用主键索引、唯一索引或普通索引，**InnoDB 都会使用行锁来对数据加锁**。
- **只有执行计划真正使用了索引，才能使用行锁**：即便在条件中使用了索引字段，但是否使用索引来检索数据是由 MySQL 通过判断不同执行计划的代价来决定的，如果 MySQL 认为全表扫描效率更高，比如对一些很小的表，它就不会使用索引，这种情况下 InnoDB 将使用表锁，而不是行锁。因此，在分析锁冲突时，别忘了检查 SQL 的执行计划（可以通过 explain 检查 SQL 的执行计划），以确认是否真正使用了索引。
- **由于 MySQL 的行锁是针对索引加的锁**，不是针对记录加的锁，所以虽然多个session是访问不同行的记录， 但是如果是**使用相同的索引键， 是会出现锁冲突的**（后使用这些索引的session需要等待先使用索引的session释放锁后，才能获取锁）。 应用设计的时候要注意这一点。

<span style='color:red'>**InnoDB的间隙锁**</span>

当我们用**范围条件而不是相等条件**检索数据，并请求共享或排他锁时，InnoDB会给符合条件的已有数据记录的索引项加锁；**对于键值在条件范围内但并不存在的记录，叫做“间隙（GAP)”，InnoDB也会对这个“间隙”加锁**，这种锁机制就是所谓的间隙锁（Next-Key锁）。

很显然，在使用范围条件检索并锁定记录时，**InnoDB这种加锁机制会阻塞符合条件范围内键值的并发插入**，这往往会造成严重的锁等待。因此，在实际应用开发中，尤其是并发插入比较多的应用，我们要尽量优化业务逻辑，尽量使用相等条件来访问更新数据，避免使用范围条件。

**InnoDB使用间隙锁的目的：**

1. **防止幻读**，以满足相关隔离级别的要求；
2. 满足恢复和复制的需要：

MySQL 通过 BINLOG 录入执行成功的 INSERT、UPDATE、DELETE 等更新数据的 SQL 语句，并由此实现 MySQL 数据库的恢复和主从复制。MySQL 的恢复机制（复制其实就是在 Slave Mysql 不断做基于 BINLOG 的恢复）有以下特点：

一是 MySQL 的恢复是 SQL 语句级的，也就是重新执行 BINLOG 中的 SQL 语句。

二是 MySQL 的 Binlog 是按照事务提交的先后顺序记录的， 恢复也是按这个顺序进行的。

由此可见，MySQL 的恢复机制要求：在一个事务未提交前，其他并发事务不能插入满足其锁定条件的任何记录，也就是不允许出现幻读。

<span style='color:red'>**获取 InnoDB 行锁争用情况**</span>

可以通过检查 InnoDB_row_lock 状态变量来分析系统上的行锁的争夺情况

```text
mysql> show status like 'innodb_row_lock%'; 
+-------------------------------+-------+ 
| Variable_name | Value | 
+-------------------------------+-------+ 
| InnoDB_row_lock_current_waits | 0 | 
| InnoDB_row_lock_time | 0 | 
| InnoDB_row_lock_time_avg | 0 | 
| InnoDB_row_lock_time_max | 0 | 
| InnoDB_row_lock_waits | 0 | 
+-------------------------------+-------+ 
5 rows in set (0.01 sec)
```

<span style='color:red'>**LOCK TABLES 和 UNLOCK TABLES**</span>

Mysql也支持lock tables和unlock tables，**这都是在服务器层（MySQL Server层）实现的，和存储引擎无关**，它们有自己的用途，并不能替代事务处理。 （**除了禁用了autocommint后可以使用，其他情况不建议使用**）：

- LOCK TABLES 可以锁定用于当前线程的表。如果表被其他线程锁定，则当前线程会等待，直到可以获取所有锁定为止。
- UNLOCK TABLES 可以释放当前线程获得的任何锁定。**当前线程执行另一个 LOCK TABLES 时，或当与服务器的连接被关闭时，所有由当前线程锁定的表被隐含地解锁**

**LOCK TABLES语法：**

- 在用 LOCK TABLES 对 InnoDB 表加锁时要注意，**要将 AUTOCOMMIT 设为 0，否则MySQL 不会给表加锁**；
- 事务结束前，**不要用 UNLOCK TABLES 释放表锁，因为 UNLOCK TABLES会隐含地提交事务**；
- COMMIT 或 ROLLBACK 并不能释放用 LOCK TABLES 加的表级锁，**必须用UNLOCK TABLES 释放表锁**。

正确的方式见如下语句：
例如，如果需要写表 t1 并从表 t2读，可以按如下做：

```text
SET AUTOCOMMIT=0; 
LOCK TABLES t1 WRITE, t2 READ, ...; 
[do something with tables t1 and t2 here]; 
COMMIT; 
UNLOCK TABLES;
```

**使用LOCK TABLES的场景：**

给表显示加表级锁（InnoDB表和MyISAM都可以），一般是为了在一定程度模拟事务操作，实现对某一时间点多个表的一致性读取。（与MyISAM默认的表锁行为类似）

在用 LOCK TABLES 给表显式加表锁时，必须同时取得所有涉及到表的锁，并且 MySQL 不支持锁升级。也就是说，在执行 LOCK TABLES 后，只能访问显式加锁的这些表，不能访问未加锁的表；同时，如果加的是读锁，那么只能执行查询操作，而不能执行更新操作。

其实，在MyISAM自动加锁（表锁）的情况下也大致如此，MyISAM 总是一次获得 SQL 语句所需要的全部锁，这也正是 MyISAM 表不会出现死锁（Deadlock Free）的原因。

**例如**，有一个订单表 orders，其中记录有各订单的总金额 total，同时还有一个 订单明细表 order_detail，其中记录有各订单每一产品的金额小计 subtotal，假设我们需要检 查这两个表的金额合计是否相符，可能就需要执行如下两条 SQL：

```text
Select sum(total) from orders; 
Select sum(subtotal) from order_detail; 
```

**这时，如果不先给两个表加锁，就可能产生错误的结果，因为第一条语句执行过程中，order_detail 表可能已经发生了改变**。因此，正确的方法应该是：

```text
Lock tables orders read local, order_detail read local; 
Select sum(total) from orders; 
Select sum(subtotal) from order_detail; 
Unlock tables;
```

（在 LOCK TABLES 时加了“local”选项，其作用就是允许当你持有表的读锁时，其他用户可以在满足 MyISAM 表并发插入条件的情况下，在表尾并发插入记录（MyISAM 存储引擎支持“并发插入”））

<span style='color:red'>**死锁**</span>

- **死锁产生：**

- - 互斥条件：一个资源每次只能被一个进程使用。
  - 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
  - 不剥夺条件：进程已获得的资源，在末使用完之前，不能强行剥夺。
  - 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系。

- **检测死锁：**数据库系统实现了各种死锁检测和死锁超时的机制。InnoDB存储引擎能检测到死锁的循环依赖并立即返回一个错误。

- **死锁恢复：**死锁发生以后，**只有部分或完全回滚其中一个事务，才能打破死锁**，InnoDB目前处理死锁的方法是，将持有最少行级排他锁的事务进行回滚。所以事务型应用程序在设计时必须考虑如何处理死锁，多数情况下只需要重新执行因死锁回滚的事务即可。

- **外部锁的死锁检测：**发生死锁后，InnoDB 一般都能自动检测到，并使一个事务释放锁并回退，另一个事务获得锁，继续完成事务。但在涉及外部锁，或涉及表锁的情况下，InnoDB 并不能完全自动检测到死锁， 这需要通过设置锁等待超时参数innodb_lock_wait_timeout 来解决

- **死锁影响性能：**死锁会影响性能而不是会产生严重错误，因为InnoDB会自动检测死锁状况并回滚其中一个受影响的事务。**在高并发系统上，当许多线程等待同一个锁时，死锁检测可能导致速度变慢**。 有时当发生死锁时，禁用死锁检测（使用innodb_deadlock_detect配置选项）可能会更有效，这时可以依赖innodb_lock_wait_timeout设置进行事务回滚。

**MyISAM避免死锁：**

- 在自动加锁的情况下，MyISAM 总是一次获得 SQL 语句所需要的全部锁，所以 MyISAM 表不会出现死锁。

**InnoDB避免死锁：**

- 为了在单个InnoDB表上执行多个并发写入操作时避免死锁，**可以在事务开始时通过为预期要修改的每个元祖（行）使用SELECT ... FOR UPDATE语句来获取必要的锁**，即使这些行的更改语句是在之后才执行的。
- 在事务中，**如果要更新记录，应该直接申请足够级别的锁，即排他锁**，而不应先申请共享锁、更新时再申请排他锁，因为这时候当用户再申请排他锁时，其他事务可能又已经获得了相同记录的共享锁，从而造成锁冲突，甚至死锁
- **如果事务需要修改或锁定多个表，则应在每个事务中以相同的顺序使用加锁语句**。 在应用中，如果不同的程序会并发存取多个表，应尽量约定以相同的顺序来访问表，这样可以大大降低产生死锁的机会
- 通过SELECT ... LOCK IN SHARE MODE获取行的读锁后，如果当前事务再需要对该记录进行更新操作，则很有可能造成死锁。
- 改变事务隔离级别

如果出现死锁，可以用 SHOW INNODB STATUS 命令来确定最后一个死锁产生的原因。返回结果中包括死锁相关事务的详细信息，如引发死锁的 SQL 语句，事务已经获得的锁，正在等待什么锁，以及被回滚的事务等。据此可以分析死锁产生的原因和改进措施。

**一些优化锁性能的建议**

- **尽量使用较低的隔离级别**；
- 精心设计索引， 并尽量使用索引访问数据， 使加锁更精确， 从而减少锁冲突的机会
- 选择合理的事务大小，小事务发生锁冲突的几率也更小
- 给记录集显示加锁时，最好一次性请求足够级别的锁。比如要修改数据的话，最好直接申请排他锁，而不是先申请共享锁，修改时再请求排他锁，这样容易产生死锁
- 不同的程序访问一组表时，应尽量约定以相同的顺序访问各表，对一个表而言，尽可能以固定的顺序存取表中的行。这样可以大大减少死锁的机会
- 尽量用相等条件访问数据，这样可以避免间隙锁对并发插入的影响
- 不要申请超过实际需要的锁级别
- 除非必须，查询时不要显示加锁。 MySQL的MVCC可以实现事务中的查询不用加锁，优化事务性能；MVCC只在COMMITTED READ（读提交）和REPEATABLE READ（可重复读）两种隔离级别下工作
- 对于一些特定的事务，可以使用表锁来提高处理速度或减少死锁的可能

#### <span style='color:red'>10）MVCC</span>

MVCC（ Multi-Version Concurrency Control ，多版本并发控制）指的就是在使用**READ COMMITTD**、**REPEATABLE READ**这两种隔离级别的事务在执行普通的SEELCT操作时访问记录的**版本链**的过程。可以使不同事务的读-写、写-读操作并发执行，从而提升系统性能。

<span style='color:red'>**隐式字段**</span>

每行记录除了我们自定义的字段外，还有数据库隐式定义的DB_TRX_ID,DB_ROLL_PTR,DB_ROW_ID等字段

- **DB_TRX_ID**
  6byte，**最近修改(修改/插入)事务ID**：记录创建这条记录/最后一次修改该记录的事务ID
- **DB_ROLL_PTR**
  7byte，**回滚指针**，指向这条记录的上一个版本（存储于rollback segment里）
- **DB_ROW_ID**
  6byte，隐含的自增ID（**隐藏主键**），如果数据表没有主键，InnoDB会自动以DB_ROW_ID产生一个**聚簇索引**
- 实际还有一个删除flag隐藏字段, 既记录被更新或删除并不代表真的删除，而是删除flag变了

![img](https://upload-images.jianshu.io/upload_images/3133209-70cdae4621d5543e.png?imageMogr2/auto-orient/strip|imageView2/2/w/838/format/webp)

<span style='color:red'>**ReadView**</span>

对于使用**READ UNCOMMITTED**隔离级别的事务来说，**直接读取记录的最新版本**就好了，

对于使用**SERIALIZABLE**隔离级别的事务来说，使用**加锁**的方式来访问记录。

对于使用**READ COMMITTED**和**REPEATABLE READ**隔离级别的事务来说，就需要用到我们上边所说的版本链了，核心问题就是：需要判断一下版本链中的哪个版本是当前事务可见的。

**ReadView中主要包含4个比较重要的内容**：

1. **m_ids**：表示在生成ReadView时当前系统中**活跃的读写事务的事务id列表**。
2. **min_trx_id**：表示在生成ReadView时当前系统中活跃的读写事务中**最小的事务id**，也就是m_ids中的最小值。
3. **max_trx_id**：表示生成ReadView时系统中应该分配给**下一个事务的id值**。
4. **creator_trx_id**：表示**生成该ReadView的事务的事务id**。

注意max_trx_id并不是m_ids中的最大值，事务id是递增分配的。比方说现在有id为1， 2， 3这三个事务，之后id为3的事务提交了。那么一个新的读事务在生成ReadView时， m_ids就包括1和2， min_trx_id的值就是1，max_trx_id的值就是4。

有了这个ReadView，这样在访问某条记录时，只需要按照下边的步骤判断记录的某个版本是否可见：

1）如果被访问版本的**trx_id属性值与ReadView中的creator_trx_id值相同**，意味着当前事务在访问它自己修改过的记录，所以该版本可以被当前事务访问。
2）如果被访问版本的**trx_id属性值小于ReadView中的min_trx_id值**，表明生成该版本的事务在当前事务生成ReadView前已经提交，所以该版本可以被当前事务访问。
3）如果被访问版本的**trx_id属性值大于ReadView中的max_trx_id值**，表明生成该版本的事务**在当前事务生成ReadView后才开启**，所以该版本不可以被当前事务访问。
4）如果被访问版本的**trx_id属性值在ReadView的min_trx_id和max_trx_id之间**，那就需要判断一下trx_id属性值是不是在m_ids列表中，如果在，说明创建ReadView时生成该版本的事务还是活跃的，该版本不可以被访问；如果不在，说明创建ReadView时生成该版本的事务已经被提交，该版本可以被访问

READ COMMITTD、 REPEATABLE READ这两个隔离级别的一个很大不同就是：**生成ReadView的时机不同**， READ COMMITTD在每一次进行普通SELECT操作前都会生成一个ReadView，而REPEATABLE READ只在第一次进行普通SELECT操作前生成一个ReadView，之后的查询操作都重复使用这个ReadView就好了

![img](https://upload-images.jianshu.io/upload_images/3133209-1d04f4bede14f4b5.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![img](https://upload-images.jianshu.io/upload_images/3133209-ba10316e166babf6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

#### <span style='color:red'>11）执行顺序</span>

1. from 
2. join 
3. on 
4. where 
5. group by(开始使用select中的别名，后面的语句中都可以使用)
6.  avg,sum.... 
7. having 
8. select 
9. distinct 
10. order by
11. limit 

## 6、redis

#### <span style='color:red'>1）什么是redis</span>

Redis(Remote Dictionary Server) 是一个使用 C 语言编写的，开源的（BSD许可）高性能非关系型（NoSQL）的键值对数据库。

与传统数据库不同的是 Redis 的数据是存在内存中的，所以读写速度非常快，因此 redis 被广泛应用于缓存方向，每秒可以处理超过 10万次读写操作，是已知性能最快的Key-Value DB。另外，**Redis 也经常用来做分布式锁**。除此之外，Redis 支持**事务** 、**持久化**、**LUA脚本**、**LRU驱动事件**、**多种集群方案**。

#### <span style='color:red'>2）redis支持的数据类型</span>

五大常用数据类型：String、Hash、List、Set、Zset

三种特殊数据类型：BitMap（统计在线人数之类）、Geo（地理位置）、HyperLogLog（统计基数）

#### <span style='color:red'>3）redis特性</span>

1. 性能高，读写速度快。
2. 事务——Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。**单个操作是原子性的**。**多个操作**也支持事务，但是**不保证原子性**，即失败了的不影响其他成功的。通过MULTI和EXEC指令包起来。
3. 支持多种数据结构
4. 支持多种编程语言
5. 功能丰富
6. 可持久化
7. 可主从复制
8. 高可用和分布式

#### <span style='color:red'>4）redis为什么快</span>

**基于内存操作**：Redis的所有数据都在内存中，因此所有的运算都是内存级别的，所以它的性能比较高。

**数据结构简单**：Redis的数据结构比较简单，是为Redis专门设计的，而这些简单的数据结构的查找和操作的时间复杂度都是O(1)

**多路复用和非阻塞I/O**：Redis使用I/O多路复用功能来监听多个socket连接客户端，这样就可以使用一个线程来处理多个情况，从而减少线程切换带来的开销，同时也避免了I/O阻塞操作，从而大大地提高了Redis的性能。

**避免上下文切换**：因为是单线程模型，因此就避免了不必要的上下文切换和多线程竞争，这就省去了多线程切换带来的时间和性能上的开销，而且单线程不会导致死锁的问题发生。

![img](https://ss2.baidu.com/6ON1bjeh1BF3odCf/it/u=326543795,2602162783&fm=27&gp=0.jpg)

Redis6.0中新增了**多线程**的功能来**提高I/O的读写性能**，它的主要实现思路是将主线程的I/O读写任务拆分给一组独立的线程去执行，这样就可以使多个socket的读写并行化了，但Redis的命令依旧是由主线程串行执行的。

#### <span style='color:red'>5）redis事务</span>

**Redis的事务本质就是一组命令的集合**。事务支持一次执行多个命令，一个事务中所有命令都会被序列化。**在事务执行过程，会按照顺序串行化执行队列中的命令，其他客户端提交的命令请求不会插入到事务执行命令序列中**。Redis单个命令保证原子性，一个事务里面多个命令就不保证了，即有成功有失败。

Redis的事务只要分为三个阶段：**开始事务**，**命令入队**，**执行事务**。期间可以使用watch指令来对变量进行监控，watch类似乐观锁，如果watch监控的多个KEY中任何KEY的值已经被其他客户端更改，则使用EXEC执行事务时，事务队列将不会被执行，同时返回Nullmulti-bulk应答以通知调用者事务执行失败。

#### <span style='color:red'>6）redis持久化</span>

Redis的所有数据都是保存在内存中，然后**不定期**的通过**异步方式**保存到磁盘上(这称为“**半持久化模式**”)；也可以把**每一次**数据变化都写入到一个append only file(aof)里面(这称为“**全持久化模式**”)。**默认开启RDB，关闭AOF**

由于Redis的数据都存放在内存中，如果没有配置持久化，redis重启后数据就全丢失了，于是需要开启redis的持久化功能，将数据保存到磁盘上，当redis重启后，可以从磁盘中恢复数据。redis提供两种方式进行持久化，一种是**RDB持久化**（**原理是将Reids在内存中的数据库记录定时dump到磁盘上的RDB持久化**），另外一种是**AOF（append only file）持久化**（**原理是将Reids的操作日志以追加的方式写入文件**）。

![img](https://images2017.cnblogs.com/blog/388326/201707/388326-20170726161552843-904424952.png)

![img](https://images2017.cnblogs.com/blog/388326/201707/388326-20170726161604968-371688235.png)

RDB可以用过**手动触发**（**save**（阻塞）和**bgsave**（异步，开子线程））和**自动触发**（写配置文件）的方式触发备份

AOF的触发方式写在配置文件中（**每条**、**每秒**和**从不**）

![img](https://pics5.baidu.com/feed/1c950a7b02087bf43b4490d50ac25f2a11dfcf7e.jpeg?token=22f387ba78130c6115420059481b2393&s=EF48A15796784D8816E1D9EB03007024)

**RDB优点**

1. 一旦采用该方式，那么你的整个Redis数据库将只包含一个文件，这对于文件备份而言是非常完美的。比如，你可能打算每个小时归档一次最近24小时的数据，同时还要每天归档一次最近30天的数据。通过这样的备份策略，**一旦系统出现灾难性故障，我们可以非常容易的进行恢复。**

2. 对于灾难恢复而言，RDB是非常不错的选择。因为我们**可以非常轻松的将一个单独的文件压缩后再转移到其它存储介质上**。
3. **性能最大化**。对于Redis的服务进程而言，在开始持久化时，它**唯一需要做的只是fork出子进程**，之后再由子进程完成这些持久化的工作，这样就**可以极大的避免服务进程执行IO操作**了。
4. 相比于AOF机制，**如果数据集很大，RDB的启动效率会更高**。

**RDB缺点**

1. 如果你想保证数据的高可用性，即最大限度的避免数据丢失，那么RDB将不是一个很好的选择。因为**系统一旦在定时持久化之前出现宕机现象，此前没有来得及写入磁盘的数据都将丢失**。
2. 由于RDB是通过fork子进程来协助完成数据持久化工作的，因此，**如果当数据集较大时，可能会导致整个服务器停止服务几百毫秒，甚至是1秒钟**。

**AOF优点**

1. 该机制可以带来更高的数据安全性，即数据持久性。Redis中提供了3种同步策略，即每秒同步、每修改同步和不同步。**事实上，每秒同步也是异步完成的，其效率也是非常高的**，所差的是一旦系统出现宕机现象，那么这一秒钟之内修改的数据将会丢失。**而每修改同步，我们可以将其视为同步持久化**，即每次发生的数据变化都会被立即记录到磁盘中。可以预见，**这种方式在效率上是最低的**。

2. 由于该机制对日志文件的写入操作采用的是append模式，因此在写入过程中即使出现宕机现象，也不会破坏日志文件中已经存在的内容。然而**如果我们本次操作只是写入了一半数据就出现了系统崩溃问题**，不用担心，在Redis下一次启动之前，**我们可以通过redis-check-aof工具来帮助我们解决数据一致性的问题**。

3. **如果日志过大，Redis可以自动启用rewrite机制**。即Redis以append模式不断的将修改数据写入到老的磁盘文件中，同时Redis还会创建一个新的文件用于记录此期间有哪些修改命令被执行。因此在进行rewrite切换时可以更好的保证数据安全性。

4. **AOF包含一个格式清晰、易于理解的日志文件用于记录所有的修改操作**。事实上，我们也可以通过该文件完成数据的重建。

**AOF缺点**

1. 对于相同数量的数据集而言，AOF文件通常要大于RDB文件。RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快。
2. 根据同步策略的不同，AOF在运行效率上往往会慢于RDB。总之，每秒同步策略的效率是比较高的，同步禁用策略的效率和RDB一样高效。

二者选择的标准，就是看系统是愿意**牺牲一些性能，换取更高的缓存一致性（aof）**，还是愿意**写操作频繁的时候，不启用备份来换取更高的性能，待手动运行save的时候，再做备份（rdb）**。

**RDB持久化配置**

Redis会将数据集的快照dump到dump.rdb文件中。此外，我们也可以通过配置文件来修改Redis服务器dump快照的频率，在打开6379.conf文件之后，我们搜索save，可以看到下面的配置信息：

save 900 1       #在900秒(15分钟)之后，如果至少有1个key发生变化，则dump内存快照。

save 300 10      #在300秒(5分钟)之后，如果至少有10个key发生变化，则dump内存快照。

save 60 10000    #在60秒(1分钟)之后，如果至少有10000个key发生变化，则dump内存快照。

**AOF持久化配置**

在Redis的配置文件中存在三种同步方式，它们分别是：

appendfsync always   #每次有数据修改发生时都会写入AOF文件。

appendfsync everysec #每秒钟同步一次，该策略为AOF的缺省策略。

appendfsync no     #从不同步。高效但是数据不会被持久化。

#### <span style='color:red'>7）redis 6种淘汰策略</span>

**noeviction**: **不删除策略**, 达到最大内存限制时, 如果需要更多内存, 直接返回错误信息。大多数写命令都会导致占用更多的内存(有极少数会例外。

**allkeys-lru:**所有key通用; **优先删除最近最少使用**(less recently used ,LRU) 的 key。

**llkeys-random:**所有key通用; **随机删除**一部分 key。

**volatile-lru:**只限于设置了 expire 的部分; **优先删除最近最少使用**(less recently used ,LRU) 的 key。

**volatile-random**: 只限于设置了 expire 的部分; **随机删除**一部分 key。

**volatile-ttl**: 只限于设置了 expire 的部分; **优先删除剩余时间(time to live,TTL) 短**的key。

#### <span style='color:red'>8）redis 3种删除策略</span>

定时删除：创建一个定时器，当key设置有过期时间，且过期时间到达时，由定时器任务立即执行对键的删除操作

惰性删除：使用数据时,检查数据是否过期，执行get的时候调用

定期删除：redis默认是每隔100ms就随机抽取一些设置了过期时间的key，检查其是否过期，如果过期就删除。注意这里是随机抽取的。

#### <span style='color:red'>9）redis主从复制</span>

**全量同步**和**增量同步**

持久化保证了即使redis服务重启也不会丢失数据，因为redis服务重启后会将硬盘上持久化的数据恢复到内存中，但是**当redis服务器的硬盘损坏**可能会导致数据丢失，如果通过redis的主从复制机制就可以**避免这种单点故障**。

**复制是高可用Redis的基础**，**哨兵**和**集群**都是在复制基础上实现高可用的。复制主要实现了**数据的多机备份**，以及对于读操作的负载均衡和简单的故障恢复。缺陷：**故障恢复无法自动化**；**写操作无法负载均衡**；**存储能力受到单机的限制**。

#### <span style='color:red'>10）redis哨兵模式</span>

Redis主从模式虽然能做到很好的数据备份，但是他并不是高可用的。一旦主服务器点宕机后，只能通过人工去切换主服务器。因此**Redis的哨兵模式也就是为了解决主从模式的高可用方案**。

哨兵模式引入了一个Sentinel系统去监视主服务器及其所属的所有从服务器。一旦发现有主服务器宕机后，会自动选举其中的一个从服务器升级为新主服务器以达到故障转义的目的。

<span style='color:red'>**客户端访问原理**</span>

客户端**遍历所有的 Sentinel 节点**集合,获取一个可用的 Sentinel 节点.

客户端**向可用的 Sentinel 节点发送** get-master-addr-by-name 命令,**获取Redis Master 节点**.

客户端向Redis Master节点发送role或role replication 命令,来**确定其是否是Master节点**,并且能够获取其 slave节点信息.

客户端获取到确定的节点信息后,便可以向Redis发送命令来进行后续操作了

需要注意的是:**客户端是和Sentinel来进行交互的**,通过Sentinel来获取真正的Redis节点信息,然后来操作.实际工作时,**Sentinel 内部维护了一个主题队列,用来保存Redis的节点信息**,并实时更新,客户端订阅了这个主题,然后实时的去获取这个队列的Redis节点信息。



**当有主服务器被判定客观下线后**，Sentinel集群会选举出一个领头Sentinel服务器来对下线的主服务器进行故障转移操作。整个选举其实是基于**RAFT一致性算法**而实现的，大致的思路如下：

- 每个发现主服务器下线的Sentinel都会要求其他Sentinel将自己设置为局部领头Sentinel。
- 接收到的Sentinel可以同意或者拒绝
- 如果有一个Sentinel得到了半数以上Sentinel的支持则在此次选举中成为领头Sentinel。
- 如果给定时间内没有选举出领头Sentinel，那么会再一段时间后重新开始选举，直到选举出领头Sentinel。

**挑选新的主服务器的规**则是：

- 选择健康状态的从节点，排除掉断线的，最近没有回复过 `INFO`命令的从服务器。
- 选择优先级配置高的从服务器
- 选择复制偏移量大的服务器（表示数据最全）

同样的**Sentinel系统**也需要达到高可用，所以**一般也是集群**，互相之间也会监控。

![img](https://pic2.zhimg.com/80/v2-df8c2f72e070ff7c56e6ad9d0d7d2c11_720w.jpg)

#### <span style='color:red'>11）redis Cluster模式</span>

主从和哨兵都还有另外一些问题没有解决，**单个节点的存储能力是有上限**，**访问能力是有上限**的。

Redis Cluster 集群模式具有 **高可用**、**可扩展性**、**分布式**、**容错**等特性。

Redis Cluster的整个数据库将会被分为**16384**个哈希槽，数据库中的每个键都属于这16384个槽中的其中一个，集群中的每个节点可以处0个或者最多16384个槽。

**Sharding流程**

1. 当客户端发起对键值对的操作指令后，将任意分配给其中某个节点
2. 节点计算出该键值所属插槽
3. 判断当前节点是否为该键所属插槽
4. 如果是的话直接执行操作命令
5. 如果不是的话，向客户端返回moved错误，moved错误中将带着正确的节点地址与端口，客户端收到后可以直接转向至正确节点

![img](https://pic3.zhimg.com/80/v2-dc0dd555b761a5815ff45efcfd723ec2_720w.jpg)

**master节点可以做扩充**，数据迁移redis内部自动完成。

当你新增一个master节点，**槽需要重新分配**，**数据也需要重新迁移**，**但是服务不需要下线**。

**如何实现故障转移**

其实**与哨兵模式类似**，Redis的每个节点都会定期向其他节点发送Ping消息，以此来检测对方是否在线。当一个节点检测到另一个节点下线后，会将其设置为疑似下线。如果一个机器中，**有半数以上的节点将某个主节点设为疑Collection线程安全似下线**，则该节点将会被标记为已下线状态，并开始执行故障转移。

1. 通过raft算法**从下线主节点的从节点中选出新的主节点**
2. 被选中的从节点执行 `SLAVEOF no one` 命令，成为新的主节点
3. 新的主节点**撤销掉已下线主节点的槽指派，并将这些槽指给自己**
4. 新的主节点向集群中**广播自己由从节点变为主节点**
5. 新的主节点开始接受和负责自己处理槽的有关命令请求

#### <span style='color:red'>n）杂七杂八</span>

**AOF缓冲区和AOF重写缓冲区**

**AOF重写**

**SDS**

**分布式锁（redission）**

**redis性能优化**

**对比memcahce和MongoDB**

## 7、Spring

#### <span style='color:red'>1）什么是Spring</span>

Spring是一个**轻量级的IoC和AOP容器框架**。是为Java应用程序提供基础性服务的一套框架，目的是用于简化企业应用程序的开发，它使得开发者只需要关心业务需求。主要包括以下七个模块：

Spring Context：提供框架式的Bean访问方式，以及企业级功能（JNDI、定时任务等）；

Spring Core：核心类库，所有功能都依赖于该类库，提供IOC和DI服务；

Spring AOP：AOP服务；

Spring Web：提供了基本的面向Web的综合特性，提供对常见框架如Struts2的支持，Spring能够管理这些框架，将Spring的资源注入给框架，也能在这些框架的前后插入拦截器；

Spring MVC：提供面向Web应用的Model-View-Controller，即MVC实现。

Spring DAO：对JDBC的抽象封装，简化了数据访问异常的处理，并能统一管理JDBC事务；

Spring ORM：对现有的ORM框架的支持；



下图对应的是Spring 4.x的版本，5.x版本中Web模块的Portlet组件已经被废弃

![img](https://img-blog.csdnimg.cn/20201209012554205.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E3NDUyMzM3MDA=,size_16,color_FFFFFF,t_70)

#### <span style='color:red'>2）Spring 的优点</span>

1. spring属于**低侵入式设计**，代码的污染极低；
2. spring的**DI（依赖注入）机制**将对象之间的依赖关系交由框架处理，**减低**组件的**耦合性**；
3. Spring提供了**AOP技术**，支持将一些通用任务，如安全、事务、日志、权限等进行集中式管理，从而提供更好的复用。
4. spring对于主流的应用框架提供了集成支持。

#### <span style='color:red'>3）Spring的IoC理解</span>

1. IOC就是**控制反转**，指**创建对象的控制权转移给Spring框架进行管理**，并由Spring根据配置文件去创建实例和管理各个实例之间的依赖关系，对象与对象之间松散耦合，也利于功能的复用。DI依赖注入，和控制反转是同一个概念的不同角度的描述，即应用程序在运行时依赖IoC容器来动态注入对象需要的外部依赖。
2. 最直观的表达就是，以前创建对象的主动权和时机都是由自己把控的，**IOC让对象的创建不用去new了**，**可以由spring自动生产**，**使用java的反射机制**，根据配置文件在运行时动态的去创建对象以及管理对象，并调用对象的方法的。
3. Spring的IOC有三种注入方式 ：**构造器注入（xml文件constructor）**、**setter方法注入（xml文件ref）**、**根据注解注入**。

#### <span style='color:red'>4）Spring的AOP理解</span>

**OOP面向对象**，允许开发者定义**纵向**的关系，但并不适用于定义**横向**的关系，会导致大量代码的重复，而不利于各个模块的重用。

**AOP**，一般称为**面向切面**，作为**面向对象的一种补充**，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect），**减少**系统中的**重复代码**，**降低**了模块间的**耦合度**，**提高**系统的**可维护性**。可用于权限认证、日志、事务处理。

Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理：

**JDK动态代理**只提供接口的代理，不支持类的代理，要求被代理类实现接口。JDK动态代理的核心是**InvocationHandler**接口和**Proxy**类，在获取代理对象时，使用Proxy类来动态创建目标类的代理类（即最终真正的代理类，这个类继承自Proxy并实现了我们定义的接口），当代理对象调用真实对象的方法时， InvocationHandler 通过invoke()方法反射来调用目标类中的代码，动态地将横切逻辑和业务编织在一起；

如果被代理类**没有实现接口**，那么**Spring AOP会选择使用CGLIB来动态代理目标类**。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法并添加增强代码，从而实现AOP。CGLIB是通过继承的方式做的动态代理，因此**如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的**。

#### <span style='color:red'>5）Spring中bean的生命周期和作用域</span>

Spring Bean的生命周期只有四个阶段：**实例化** Instantiation --> **属性赋值** Populate --> **初始化** Initialization --> **销毁** Destruction

![img](https://img-blog.csdnimg.cn/img_convert/84341632e9df3625a91c3e2a1437ee65.png)

1. **实例化Bean**：

   对于**BeanFactory**容器，当客户向容器请求一个尚未初始化的bean时，或初始化bean的时候需要注入另一个尚未初始化的依赖时，容器就会调用createBean进行实例化。

   对于**ApplicationContext**容器，当容器启动结束后，通过获取BeanDefinition对象中的信息，实例化所有的bean。

2. **设置对象属性**（依赖注入）：实例化后的对象被封装在**BeanWrapper对象**中，紧接着，Spring根据BeanDefinition中的信息 以及 通过BeanWrapper提供的设置属性的接口完成属性设置与依赖注入。

3. **处理Aware接口**：Spring会检测该对象是否实现了xxAware接口，通过Aware类型的接口，可以让我们拿到Spring容器的一些资源：

   ①如果这个Bean实现了**BeanNameAware**接口，会调用它实现的**setBeanName**(String beanId)方法，传入Bean的名字；
   ②如果这个Bean实现了**BeanClassLoaderAware**接口，调用**setBeanClassLoader**()方法，传入ClassLoader对象的实例。
   ②如果这个Bean实现了**BeanFactoryAware**接口，会调用它实现的**setBeanFactory**()方法，传递的是Spring工厂自身。
   ③如果这个Bean实现了**ApplicationContextAware**接口，会调用**setApplicationContext**(ApplicationContext)方法，传入Spring上下文；

4. **BeanPostProcessor前置处理**：如果想对Bean进行一些自定义的前置处理，那么可以让Bean实现了BeanPostProcessor接口，那将会调用postProcessBeforeInitialization(Object obj, String s)方法。

5. **InitializingBean**：如果Bean实现了InitializingBean接口，执行afeterPropertiesSet()方法。

6. **init-method**：如果Bean在Spring配置文件中配置了 init-method 属性，则会自动调用其配置的初始化方法。

7. **BeanPostProcessor后置处理**：如果这个Bean实现了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法；由于这个方法是在Bean初始化结束时调用的，所以可以被应用于内存或缓存技术；

以上几个步骤完成后，Bean就已经被正确创建了，之后就可以使用这个Bean了。

8. **DisposableBean**：当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用其实现的destroy()方法；
9. **destroy-method**：最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。



**作用域：**

（1）**singleton**：默认作用域，单例bean，每个容器中只有一个bean的实例。

（2）**prototype**：为每一个bean请求创建一个实例。

（3）**request**：为每一个request请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收。

（4）**session**：与request范围类似，同一个session会话共享一个实例，不同会话使用不同的实例。

（5）**global-session**：全局作用域，所有会话共享一个实例。如果想要声明让所有会话共享的存储变量的话，那么这全局变量需要存储在global-session中。

#### <span style='color:red'>6）Spring中循环依赖的问题</span>

**原因**是a对象有成员变量b，b对象有成员变量a，实例化a的时候要先实例化b，然后实例b的时候又要实例化a，如此往复……

循环依赖问题在Spring中主要有**三种情况**：

- （1）通过**构造方法进行依赖注入**时产生的循环依赖问题。
- （2）通过**setter方法进行依赖注入**且是在**多例**（原型）模式下产生的循环依赖问题。
- （3）通过**setter方法进行依赖注入**且是在**单例**模式下产生的循环依赖问题。



**在Spring中，只有第（3）种方式的循环依赖问题被解决了**，其他两种方式在遇到循环依赖问题时都会产生异常。其实也很好解释：

- 第（1）种构造方法注入的情况下，在new对象的时候就会堵塞住了，其实也就是”**先有鸡还是先有蛋**“的历史难题。
- 第（2）种setter方法（多例）的情况下，每一次getBean()时，都会产生一个新的Bean，如此反复下去就会有**无穷无尽的Bean产生**了，最终就会导致OOM问题的出现。



**通过使用三级缓存解决该问题**

Spring中有三个缓存，用于存储单例的Bean实例，这**三个缓存是彼此互斥**的，不会针对同一个Bean的实例同时存储。如果调用getBean，则需要从三个缓存中依次获取指定的Bean实例。 读取顺序依次是一级缓存 ==> 二级缓存 ==> 三级缓存。

![img](https://img-blog.csdnimg.cn/img_convert/d9f570aa0dce404d7da35ecf42e9ddb5.png)

**singletonObjects**，**一级缓存**，存储的是所有创建好了的单例Bean

**earlySingletonObjects**，**二级缓存**，完成实例化，但是还未进行属性注入及初始化的对象，

**singletonFactories**，**三级缓存**，提前暴露的一个单例工厂，二级缓存中存储的就是从这个工厂中获取到的对象，

**半成品放在三级缓存中**，**从三级缓存转入二级缓存需要判断是否有代理，如果没有直接放，有则放代理对象**



下图为spring三级缓存解决循环依赖的流程

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5bcb0897632640ffbb4194ba1c449175~tplv-k3u1fbpfcp-watermark.image)



下图为采用二级缓存解决循环依赖的流程

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3a2677f87c904d29ada221b3fe1ea994~tplv-k3u1fbpfcp-watermark.image)



不使用三级缓存就需要在实例化后马上创建代理，**违背了Spring在结合AOP跟Bean的生命周期的设计**。

Spring结合**AOP**跟Bean的生命周期本身就是**通过后置处理器来完成的**，在这个后置处理的方法中对初始化后的Bean完成AOP代理。如果出现了循环依赖，那没有办法，只有给Bean先创建代理，但是没有出现循环依赖的情况下，**设计之初就是让Bean在生命周期的最后一步完成代理而不是在实例化后就立马完成代理**。

#### <span style='color:red'>n）杂七杂八</span>

**spring容器启动流程**

**BeanFactory和ApplicationContext的区别（BeanFactory延迟注入 ApplicationContext加载注入）**

**事务**

## 8、Mybatis

#### <span style='color:red'>1）什么是Mybatis</span>

1. Mybatis是一个**半ORM（对象关系映射）框架**，它内部封装了JDBC，加载驱动、创建连接、创建statement等繁杂的过程，开发者开发时**只需要关注如何编写SQL语句**，可以严格控制sql执行性能，灵活度高。

2. 作为一个半ORM框架，MyBatis 可以使用 **XML** 或**注解**来配置和映射原生信息，将 POJO映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

   <span style='color:red'>称Mybatis是半自动ORM映射工具，是因为在查询关联对象或关联集合对象时，需要手动编写sql来完成。不像Hibernate这种全自动ORM映射工具，Hibernate查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取。</span>

3. 通过xml 文件或注解的方式**将要执行的各种 statement 配置起来**，并通过java对象和 statement中sql的**动态参数**进行映射**生成最终执行的sql语句**，最后**由mybatis框架执行sql**并将结果映射为java对象并返回。（从执行sql到返回result的过程）。

4. 由于MyBatis专注于SQL本身，**灵活度高**，所以比较适合对性能的要求很高，或者**需求变化较多的项目**，如互联网项目。

#### <span style='color:red'>2）mapper.xml，Dao接口</span>

Mapper 接口的工作原理是**JDK动态代理**，Mybatis运行时会使用JDK动态代理为Mapper接口生成代理对象proxy，代理对象会拦截接口方法，根据类的全限定名+方法名，唯一定位到一个MapperStatement并调用执行器执行所代表的sql，然后将sql执行结果返回。

Mapper接口里的方法，是**不能重载**的，因为是使用 全限名+方法名 的保存和寻找策略。

#### <span style='color:red'>3）拦截器</span>

mybatis 拦截器默认可拦截的类型只有四种，即四种接口类型 **Executor（执行器的方法）**、**StatementHandler（sql语法构建的处理）**、**ParameterHandler（参数的处理）**和**ResultSetHandler（结果集的处理）**。

不同拦截器**顺序**：Executor -> ParameterHandler -> StatementHandler -> ResultSetHandler

Mybatis没有任何一个实现拦截器接口的实现类，所以想实现拦截器的功能需要自己写一个实现类。

一共要实现三个方法：

public Object intercept(Invocation invocation)

public Object plugin(Object target)

public` `void` `setProperties(Properties properties)

#### <span style='color:red'>3）分页</span>

|                    |            优点             |                          缺点                           |
| ------------------ | :-------------------------: | :-----------------------------------------------------: |
| 全部查出，部分输出 |          方法简单           |                   数据量大时消耗性能                    |
| sql用limit         |     不需要查询所有数据      | 每次在分页的时候都需要去编写limit语句，很冗余，维护性差 |
| 拦截器（拼接sql）  | 不需要每次都重新编写sql语句 |                        没啥缺点                         |
| RowBounds（插件）  |          实现简单           |                        同第一种                         |
| PageHelper（插件） |          同拦截器           |                        同拦截器                         |

#### <span style='color:red'>4）延迟加载</span>

如果一个对象的成员含有其他对象的集合（表的一对多），当我们对该对象进行查询的时候，首先只查该对象的数据，其他对象的集合（成员）不去查询，当需要该数据的时候再去进行查询，就是延迟加载。

通过xml文件和注解可以得到该查询是否是延迟加载，是延迟加载就创建一个代理对象，当需要查询该代理对象的属性的时候再去数据库中查询加载属性。

#### <span style='color:red'>5）二级缓存</span>

1. 一级缓存: 基于 PerpetualCache 的 HashMap 本地缓存，**其存储作用域为 Session**，当 Session flush 或 close 之后，该 Session 中的所有 Cache 就将清空，**默认打开一级缓存**。
2. 二级缓存与一级缓存其机制相同，默认也是采用 PerpetualCache，HashMap 存储，不同在于**其存储作用域为 Mapper(Namespace)**，并且可自定义存储源，如 Ehcache。**默认不打开二级缓存**，要开启二级缓存，使用二级缓存属性类需要实现Serializable序列化接口(可用来保存对象的状态),可在它的映射文件中配置；
3. 对于缓存数据更新机制，当某一个作用域(一级缓存 Session/二级缓存Namespaces)的进行了C/U/D 操作后，默认该作用域下所有 select 中的缓存将被 clear 掉并重新更新。

<span style='color:red'>**问题**：</span>

在二级缓存中，如果**多个mapper文件中都会对同一张表进行操作**，由于二级缓存的机制，在不同mapper下的查询操作会导致数据的不一致。即便对于每一张表的修改操作都写在同一个mapper下，但是多表查询的时候，一定会出现数据不一致的问题。

#### <span style='color:red'>6）MyBatis和Hibernate</span>

|              |                           Mybatis                            |                          Hibernate                           |
| ------------ | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 开发速度     |          sql语句需要自己手动去完成，过程计较的复杂           |     sql语句已经被封装 ，是直接就可以使用的，加快系统开发     |
| sql优化程度  | Mybatis是手动编写sql，能够避免一些不必要的查询，提高系统性能 |  自动的生成sql，有些语句会比较的繁琐，所以，会消耗一些性能   |
| 对象管理方式 |                  需要自行进行管理，映射关系                  | 它是完整的对象-关系映射的框架，在开发工程当中，只需要去管理对象，对于底层实现不需要进行过多的关注 |
| 缓存机制     | 二级缓存配置都是在**每个具体的表-对象映射中进行详细配置**。除此之外，Mybatis能够在命名空间中共享相同的缓存配置和实例，通过Cache-ref来实现。 | 二级缓存配置在SessionFactory生成的配置文件中进行详细配置。**之后，再在具体的表-对象映射中配置都是那种缓存**。 |



#### <span style='color:red'>n）杂七杂八</span>

**#{}和${}**

**模糊查询**

**关联查询**

**mapper的编写方式**

## 9、SpringBoot

#### <span style='color:red'>1）简介</span>

Spring Boot是一个**简化Spring开发的框架**。**遵循约定大于配置**的原则，去繁就简。我们在使用Spring Boot时**只需要配置相应的Spring Boot**就可以用所有的Spring组件。**简单的说**，**spring boot就是整合了很多优秀的框架**，不用我们自己手动的去写一堆xml配置然后进行配置。**从本质上来说，Spring Boot就是Spring**,它做了那些没有它你也会去做的Spring Bean配置。

<span style='color:red'>**约定**</span>

1. **Maven的目录结构**。默认有resources文件夹,存放资源配置文件。src-main-resources,src-main-java。默认的编译生成的类都在targe文件夹下面
2. spring boot默认的配置文件必须是，也只能是**application.命名的yml文件或者properties文件**，且唯一application.yml中默认属性。
3. 数据库连接信息必须是以spring: datasource: 为前缀；多环境配置。该属性可以根据运行环境自动读取不同的配置文件；端口号、请求路径等

#### <span style='color:red'>2）特点</span>

1. 创建**独立的spring应用**。（之前的Spring必须要依赖tomcat，只是一个整合的作用）
2. 嵌入**Tomcat**, **Jetty** **Undertow**（都是Servlet容器） 而且不需要部署他们。
3. 提供的**“starters”** poms来简化Maven配置（spring-boot-starter-parent）
4. 尽可能自动配置spring应用。
5. 提供生产指标,健壮检查和外部化配置
6. 绝对没有代码生成和XML配置要求

#### <span style='color:red'>3）常见注解（代填）</span>

#### <span style='color:red'>4）杂七杂八</span>

## 10、分布式

#### <span style='color:red'>1）CAP</span>

CAP原则又称CAP定理，指的是在一个分布式系统中，**一致性**（Consistency）、**可用性**（Availability）、**分区容错性**（Partition tolerance）。

1. **一致性**（C）：在分布式系统中的所有数据备份，在同一时刻是否同样的值，即写操作之后的读操作，必须返回该值。（分为弱一致性、强一致性和最终一致性）
2. **可用性**（A）：在集群中一部分节点故障后，集群整体是否还能响应客户端的读写请求。（对数据更新具备高可用性）
3. **分区容错性**（P）：以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在C和A之间做出选择。

**CA**（不允许分区，关系型数据库）、**CP**（需要数据强一致，非关系型数据库）、**AP**（需要高可用，抢票）

#### <span style='color:red'>2）雪崩、击穿、穿透</span>

<span style='color:red'>**雪崩**</span>

缓存雪崩是指在我们设置缓存时**采用了相同的过期时间**，导致缓存在某一时刻同时失效，请求全部转发到DB，DB瞬时压力过重雪崩。

**解决方案**

1. 在原有的失效时间基础上**增加一个随机值**，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。
2. **设置一个队列**，限制同一时刻访问数据库的线程
3. **加互斥锁**，限制同时只能有一个线程访问数据库

<span style='color:red'>**击穿**</span>

极度热点key在失效瞬间，大量请求击穿数据库 

**解决方案**

1. 永不过期（每次访问重置过期时间）
2. 同雪崩

<span style='color:red'>**穿透**</span>

反复访问数据库不存在的数据

**解决方案**

有很多种方法可以有效地解决缓存穿透问题，最常见的则是采用**布隆过滤器**，将所有可能存在的数据哈希到一个足够大的bitmap中，一个一定不存在的数据会被这个bitmap拦截掉，从而避免了对底层存储系统的查询压力。

## 11、SpringCloud

#### <span style='color:red'>1）SpringCloud是什么</span>

spring cloud 是**一系列框架的有序集合**。它利用 spring boot 的开发便利性巧妙地简化了分布式系统基础设施的开发，如**服务发现注册**、**配置中心**、**消息总线**、**负载均衡**、**断路器**、**数据监控**等，都可以用 spring boot 的开发风格做到一键启动和部署。

#### <span style='color:red'>2）常用组件</span>

- **Eureka**：服务注册于发现。
- **Feign**：基于动态代理机制，根据注解和选择的机器，拼接请求 url 地址，发起请求。
- **Ribbon**：实现负载均衡，从一个服务的多台机器中选择一台。
- **Hystrix**：提供线程池，不同的服务走不同的线程池，实现了不同服务调用的隔离，避免了服务雪崩的问题。
- **Zuul**：网关管理，由 Zuul 网关转发请求给对应的服务。
- **Config**：分布式系统的配置管理方案。

#### <span style='color:red'>3）Eureka</span>

![img](https://img-blog.csdnimg.cn/20190427101255808.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3podXlhbmxpbjA5,size_16,color_FFFFFF,t_70)

Eureka的数据存储分了两层：**数据存储层**和**缓存层**。Eureka Client在拉取服务信息时，先从缓存层获取（相当于Redis），如果获取不到，先把数据存储层的数据加载到缓存中（相当于Mysql），再从缓存中获取。**值得注意的是**，**数据存储层**的数据结构是**服务信息**，而**缓存中**保存的是**经过处理加工过的、可以直接传输到Eureka Client的数据结构**。

Eureka这样的数据结构设计是**把内部的数据存储结构与对外的数据结构隔离开**了，就像是我们平时在进行接口设计一样，对外输出的数据结构和数据库中的数据结构往往都是不一样的。



**服务注册Register**

当eureka提供方（provider）向Eureka Server注册时，它**提供自身的元数据**，例如：ip地址，端口，运行状况指示符等（房东在中介登记房屋信息，比如：面积，价格，地段）

**服务续约Renew**

服务提供方（provider）**每隔30s（默认）**发送一次心跳来续约，通过续约告诉Eureka Server **该服务提供方仍然存在**，没有出现问题，正常情况下，如果Eureka Server在**90s内没有收到服务提供方的续约**，它会将实例从注册中心删除（房东定期告诉中介，我的房子还外租（续约），中介就会保留房屋信息）

**服务剔除Eriction**

在默认情况下，当服务提供方连续90s没有向注册中心进行续约，即心跳，注册中心会将该服务从服务注册列表中剔除（房东定期联系中介告诉他房子还续约，如果中介长时间没有收到房东的续约信息，中介会将他的房屋信息下架）

**获取注册列表信息FetchRegistries**

服务消费方从注册中心获取**注册表信息**，并将其缓存到本地，消费方会使用该信息查找其他服务，从而进行远程调用，该注册列表定期**30S更新一次**，每次返回注册列表信息可能与服务消费方缓存信息不同，服务消费方会自动处理，重新获取整个注册表信息，eureka Server和Eureka Client可以使用JSON/XMl格式进行通信，默认情况下Eureka Client使用Json格式来获取注册列表信息（租客去中介获取所有的房屋信息，而且租客为了获取最新的房屋 信息会定期向中介获取并更新本地列表）

**服务下线Cancel**

**服务提供方在程序关闭时向注册中心发送取消请求**，发送后该服务提供方的信息将从注册中心的服务列表中删除（房东告诉中介房子不租了，中介将此房子的信息删除），该下线请求不会自动完成。

**远程调用Remote Call**

当服务消费方从注册中心获取到服务提供方信息后，就可以**通过Http请求调用对应的服务**；服务提供者有多个时，服务消费方会通过Ribbon自动进行负载均衡

**自我保护机制**

默认情况下，如果注册中心在90秒内没有接受到某个微服务实例的心跳，会注销该实例，**但是在微服务架构下服务之间通常都是跨进程调用**，通信往往会面临这各种问题，比如微服务状态不正常，网络分区故障，导致实例被注销。**大量实例被注销，会威胁到整个微服务架构的可用性**，所以eureka就有了**自我保护机制**，Eureka Server 在运行期间会去**统计心跳失败比例在 15 分钟之内是否低于 85%**，如果低于 85%，Eureka Server 即会进入自我保护机制

**Eureka Server 进入自我保护机制，会出现以下几种情况**：

　　1.Eureka不再从注册列表中移除因为长时间没收到心跳而应该过期的服务

　　2.Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上

　　3.当网络稳定时，当前实例新的注册信息会被同步到其他节点上

Eureka 自我保护机制是为了**防止误杀服务而提供的一个机制**。当个别客户端出现心跳失联时，则认为是客户端的问题，剔除掉客户端；当 Eureka 捕获到大量的心跳失败时，则认为可能是网络问题，进入自我保护机制；当客户端心跳恢复时，Eureka 会自动退出自我保护机制。

如果在保护期内刚好这个服务提供者非正常下线了，此时服务消费者就会拿到一个无效的服务实例，即会调用失败。对于这个问题需要服务消费者端要有一些容错机制，如重试，断路器等。

**Eureka**和**Zookeeper**对比

|          |                   Zookeeper                    |                          Eureka                          |
| -------- | :--------------------------------------------: | :------------------------------------------------------: |
| 设计原则 |                       CP                       |                            AP                            |
| 优点     |                   数据强一致                   |                        服务高可用                        |
| 缺点     | 网络分区会影响Leader选举，超过阈值后集群不可用 | 服务节点间数据可能不一致，Client-Server间数据可能不一致  |
| 适用场景 |        单机房集群，对数据一致性要求较高        | 云机房集群，跨越多机房部署，对注册中心服务可用性要求较高 |

#### <span style='color:red'>4）Ribbon</span>

客户端通过Eureka获取提供服务的服务器列表，Ribbon通过负载均衡策略选择合适的服务器。

通过在RestTemplate上加@LoadBalance注解实现

#### <span style='color:red'>5）Feign</span>

Feign是Netflix开发的声明式、模板化的HTTP客户端，其灵感来自Retrofit、JAXRS-2.0以及WebSocket。Feign可帮助我们更加快捷、优雅地调用HTTP API。

在Spring Cloud中，使用Feign非常简单，创建一个接口，并在接口上添加一些注解，代码就完成了。

**Feign其实就是一个封装了HTTPConnection的模块。（默认继承了Ribbon）**

#### <span style='color:red'>6）Hystrix</span>

**雪崩**：

在微服务架构中多层服务之间会相互调用，如果其中**有一层服务故障**了，可能会**导致一层服务或者多层服务故障**，从而**导致整个系统故障**。这种现象被称为服务雪崩效应。

SpringCloud 中的 Hystrix 组件就可以解决此类问题，Hystrix 负责监控服务之间的调用情况，连续多次失败的情况进行熔断保护。保护的方法就是使用 Fallback，当调用的服务出现故障时，就可以使用 Fallback 方法的返回值；Hystrix 间隔时间会再次检查故障的服务，如果故障服务恢复，将继续使用服务。

**熔断**：

当下游的服务因为某种原因突然**变得不可用**或**响应过慢**，上游服务为了保证自己整体服务的可用性，不再继续调用目标服务，直接返回，快速释放资源。如果目标服务情况好转则恢复调用。

**降级**：

当下游的服务因为某种原因**响应过慢**，下游服务主动停掉一些不太重要的业务，释放出服务器资源，增加响应速度！

当下游的服务因为某种原因**不可用**，上游主动调用本地的一些降级逻辑，避免卡顿，迅速返回给用户！

服务降级有很多种降级方式，如**开关降级**、**限流降级**、**熔断降级**。

#### <span style='color:red'>7）Zuul</span>

Zuul是Spring Cloud全家桶中的**微服务API网关**。

所有从设备或网站来的请求都会经过Zuul到达后端的Netflix应用程序。作为一个边界性质的应用程序，Zuul提供了**动态路由**、**监控**、**弹性负载**和**安全功能**。Zuul底层利用各种filter实现如下功能：

- **认证和安全** 识别每个需要认证的资源，拒绝不符合要求的请求。
- **性能监测** 在服务边界追踪并统计数据，提供精确的生产视图。
- **动态路由** 根据需要将请求动态路由到后端集群。
- **压力测试** 逐渐增加对集群的流量以了解其性能。
- **负载卸载** 预先为每种类型的请求分配容量，当请求超过容量时自动丢弃。
- **静态资源处理** 直接在边界返回某些响应。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190601012124618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9mb3JlenAuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70)

在zuul中， 整个请求的过程是这样的，**首先将请求给zuulservlet处理**，zuulservlet中有一个**zuulRunner对象**，该对象中初始化了**RequestContext**：作为存储整个请求的一些数据，并被所有的zuulfilter共享。zuulRunner中还有 **FilterProcessor**，FilterProcessor作为执行所有的zuulfilter的管理器。FilterProcessor从filterloader 中获取zuulfilter，而zuulfilter是被filterFileManager所加载，并支持groovy热加载，采用了轮询的方式热加载。有了这些filter之后，zuulservelet首先执行的Pre类型的过滤器，再执行route类型的过滤器，最后执行的是post类型的过滤器，如果在执行这些过滤器有错误的时候则会执行error类型的过滤器。执行完这些过滤器，最终将请求的结果返回给客户端。

#### <span style='color:red'>n）杂七杂八</span>

**负载均衡算法**

**Config**

## 12、Zookeeper

#### <span style='color:red'>1）Zookeeper是什么</span>

ZooKeeper 是一个**开源**的**分布式协调服务**。它是一个为分布式应用提供一致性服务的软件，分布式应用程序可以基于 Zookeeper 实现诸如**数据发布/订阅**、**负载均衡**、**命名服务**、**分布式协调/通知**、**集群管理**、**Master 选举**、**分布式锁**和**分布式队列**等功能。

- 顺序性：客户端发起的更新请求会按发送顺序被应用到ZooKeeper上。
- 原子性：更新操作要么成功要么失败，不会出现中间状态。
- 可靠性：一个更新操作一旦被接受即不会意外丢失，除非被其他更新操作覆盖。
- 最终一致性：写操作最终（而非立即）会对客户端可见。

#### <span style='color:red'>2）文件系统</span>

Zookeeper 提供一个多层级的节点命名空间（**节点称为 znode**）。与文件系统不同的是，这些节点都可以设置关联的数据，而文件系统中**只有文件节点可以存放数据**而**目录节点不行**。

Zookeeper 为了保证高吞吐和低延迟，在内存中维护了这个**树状的目录结构**，这种特性使得 Zookeeper 不能用于存放大量的数据，每个节点的存放数据**上限为1M**。

#### <span style='color:red'>3）数据同步</span>

Zookeeper 的核心是**原子广播机制**，这个机制保证了各个 server 之间的同步。实现这个机制的协议叫做 **Zab 协议**。Zab 协议有两种模式，它们分别是**恢复模式**和**广播模式**。

<span style='color:red'>**恢复模式**</span>
当**服务启动**或者在**领导者崩溃**后，Zab就进入了恢复模式，当领导者被选举出来，且**半数以上**server 完成了和 leader 的状态同步以后，恢复模式就结束了。状态同步保证了 leader 和 server 具有相同的系统状态。
<span style='color:red'>**广播模式**</span>
一旦 leader 已经和多数的 follower 进行了状态同步后，它就可以开始广播消息了，即进入广播状态。**这时候当一个 server 加入 ZooKeeper 服务中，它会在恢复模式下启动**，发现 leader，并和 leader 进行状态同步。待到同步结束，它也参与消息广播。ZooKeeper 服务一直维持在 Broadcast 状态，直到 leader 崩溃了或者 leader 失去了大部分的 followers 支持。

**过程**：

1. leader接收到消息请求后，将消息赋予一个全局唯一的64位自增id，叫：zxid，通过zxid的大小比较就可以实现因果有序这个特征。
2. eader为每个follower准备了一个FIFO队列（通过TCP协议来实现，以实现全局有序这一个特点）将带有zxid的消息作为一个提案（proposal）分发给所有的 follower。
3. 当follower接收到proposal，先把proposal写到磁盘，写入成功以后再向leader回复一个ack。
4. 当leader接收到合法数量（超过半数节点）的ack后，leader就会向这些follower发送commit命令，同时会在本地执行该消息。
5. 当follower收到消息的commit命令以后，会提交该消息。

#### <span style='color:red'>4）四种类型的数据节点 Znode</span>

1. **PERSISTENT-持久节点**

   除非手动删除，否则节点一直存在于 Zookeeper 上

2. **EPHEMERAL-临时节点**

   临时节点的**生命周期与客户端会话绑定**，一旦客户端会话失效（客户端与zookeeper 连接断开不一定会话失效），那么这个客户端创建的所有临时节点都会被移除。

3. **PERSISTENT_SEQUENTIAL-持久顺序节点**
   基本特性同持久节点，只是增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。

4. **EPHEMERAL_SEQUENTIAL-临时顺序节点**
   基本特性同临时节点，增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。

#### <span style='color:red'>n）杂七杂八</span>

**watcher机制**

**负载均衡**

**分布式锁**

**命名系统**

**队列管理**

## 13、Dubbo

#### <span style='color:red'>1）Dubbo是什么</span>

Dubbo是阿里巴巴开源的基于 Java 的高性能 RPC 分布式服务框架，现已成为 Apache 基金会孵化项目。

![img](https://img-blog.csdn.net/20181002113850939?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21vYWt1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

   

**Provider**: 暴露服务的服务提供方。

**Consumer**: 调用远程服务的服务消费方。

**Registry**: 服务注册与发现的注册中心。

**Monitor**: 统计服务的调用次调和调用时间的监控中心。

**Container**: 服务运行容器。

#### <span style='color:red'>2）Dubbo有哪些协议</span>

- dubbo://（推荐）
- rmi://
- hessian://
- http://
- webservice://
- thrift://
- memcached://
- redis://
- rest://

#### <span style='color:red'>3）Dubbo有哪些集群容错方案</span>

![img](https://img-blog.csdn.net/20181002113930188?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21vYWt1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### <span style='color:red'>4）Dubbo有哪负载均衡策略</span>

![img](https://img-blog.csdn.net/20181002113941952?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21vYWt1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1. **random loadbalance**：默认情况下，dubbo 是 random load balance ，即随机调用实现负载均衡，可以对 provider 不同实例设置不同的权重，会按照权重来负载均衡，权重越大分配流量越高，一般就用这个默认的就可以了。
2. **roundrobin loadbalance**：这个的话默认就是均匀地将流量打到各个机器上去，但是如果各个机器的性能不一样，容易导致性能差的机器负载过高。所以此时需要调整权重，让性能差的机器承载权重小一些，流量少一些。
3. **leastactive loadbalance**：这个就是自动感知一下，如果某个机器性能越差，那么接收的请求越少，越不活跃，此时就会给不活跃的性能差的机器更少的请求
4. **consistanthash loadbalance**：一致性 Hash 算法，相同参数的请求一定分发到一个 provider 上去，provider 挂掉的时候，会基于虚拟节点均匀分配剩余的流量，抖动不会太大。如果你需要的不是随机负载均衡，是要一类请求都到一个节点，那就走这个一致性 Hash 策略。

#### <span style='color:red'>5）Dubbo和SpringCloud</span>

<img src="https://images2018.cnblogs.com/blog/435188/201804/435188-20180412214125747-2064544666.png" alt="img" style="zoom:60%;" />

#### <span style='color:red'>n）杂七杂八</span>

**rpc和rest**

**服务降级（mock）**

**zookeeper宕机**

**直连**

## 14、rabbitMQ

#### <span style='color:red'>1）什么是RabbitMQ，为什么使用RabbitMQ</span>

RabbitMQ是一款开源的，Erlang编写的，基于AMQP协议的，消息中间件；

可以用它来：**解耦**、**异步**、**削峰**。

AMQP（Advanced Message Queuing Protocol，高级消息队列协议）是一个进程间传递**异步消息**的**网络协议**。

![在这里插入图片描述](https://img-blog.csdn.net/20181022113306601?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzY0MTgzMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### <span style='color:red'>2）优缺点</span>

优点：**解耦**、**异步**、**削峰**；

缺点：**降低了系统的稳定性**：本来系统运行好好的，现在你非要加入个消息队列进去，那**消息队列挂了**，你的系统不是呵呵了。因此，系统可用性会降低；

**增加了系统的复杂性**：加入了消息队列，要多考虑很多方面的问题，比如：**一致性问题**、**如何保证消息不被重复消费**、**如何保证消息可靠性传输**等。因此，需要考虑的东西更多，复杂性增大。

#### <span style='color:red'>3）如何保证RabbitMQ不被重复消费</span>

**先说为什么会重复消费**：正常情况下，消费者在消费消息的时候，消费完毕后，会发送一个确认消息给消息队列，消息队列就知道该消息被消费了，就会将该消息从消息队列中删除；

但是**因为网络传输等等故障**，确认信息没有传送到消息队列，导致消息队列不知道自己已经消费过该消息了，再次将消息分发给其他的消费者。

针对以上问题，一个解决思路是：**保证消息的唯一性**，就算是多次传输，不要让消息的多次消费带来影响；**保证消息等幂性**；

比如：在写入消息队列的数据做唯一标示，消费消息时，根据唯一标识判断是否消费过；

#### <span style='color:red'>4）如何保证RabbitMQ消息的可靠传输</span>

消息不可靠的情况可能是消息丢失，劫持等原因；丢失又分为：**生产者丢失消息**、**消息列表丢失消息**、**消费者丢失消息**；

**生产者丢失消息**：从生产者弄丢数据这个角度来看，RabbitMQ提供transaction和confirm模式来确保生产者不丢消息；

**transaction机制就**是说：**发送消息前，开启事务**（channel.txSelect()）,然后发送消息，如果发送过程中出现什么异常，事务就会回滚（channel.txRollback()）,如果发送成功则提交事务（channel.txCommit()）。然而，这种方式有个**缺点：吞吐量下降**；

**confirm模式**用的**居多**：一旦channel进入confirm模式，**所有在该信道上发布的消息都将会被指派一个唯一的ID**（从1开始），**一旦消息被投递到所有匹配的队列之后；rabbitMQ就会发送一个ACK给生产者**（包含消息的唯一ID），这就使得生产者知道消息已经正确到达目的队列了；如果rabbitMQ没能处理该消息，则会发送一个Nack消息给你，你可以进行重试操作。

 

**消息队列丢数据**：**消息持久化**。

处理消息队列丢数据的情况，一般是开启持久化磁盘的配置。

这个持久化配置可以和confirm机制配合使用，你可以在消息持久化磁盘后，再给生产者发送一个Ack信号。

这样，如果消息持久化磁盘之前，rabbitMQ阵亡了，那么生产者收不到Ack信号，生产者会自动重发。

那么如何持久化呢？

这里顺便说一下吧，其实也很容易，就下面两步

1. 将queue的持久化标识durable设置为true,则代表是一个持久的队列
2. 发送消息的时候将deliveryMode=2

这样设置以后，即使rabbitMQ挂了，重启后也能恢复数据

 

**消费者丢失消息**：消费者丢数据一般是因为采用了自动确认消息模式，**改为手动确认消息即可**！

消费者在收到消息之后，处理消息之前，会自动回复RabbitMQ已收到消息；

如果这时处理消息失败，就会丢失该消息；

解决方案：处理消息成功后，手动回复确认消息。

#### <span style='color:red'>5）如何保证RabbitMQ消息的顺序性</span>

单线程消费保证消息的顺序性；对消息进行编号，消费者处理消息是根据编号处理消息；

#### <span style='color:red'>6）各种工作模式</span>

1. **简单模式**：一个生产者，一个消费者。

![img](https://bbsmax.ikafan.com/static/L3Byb3h5L2h0dHBzL3d3dy5yYWJiaXRtcS5jb20vaW1nL3R1dG9yaWFscy9weXRob24tb25lLW92ZXJhbGwucG5n.jpg)

2. **work模式**：一个生产者，多个消费者，每个消费者获取到的消息唯一（消费者彼此竞争成为接收者）。

![img](https://bbsmax.ikafan.com/static/L3Byb3h5L2h0dHBzL3d3dy5yYWJiaXRtcS5jb20vaW1nL3R1dG9yaWFscy9weXRob24tdHdvLnBuZw==.jpg)

3. **订阅模式（fanout）**：一个生产者发送的消息会被多个消费者获取。

![fanout](https://raw.githubusercontent.com/limboline/PicBed/master/img/20210531211011.png)

4. **路由模式（direct）**：发送消息到交换机并且要指定路由key ，消费者将队列绑定到交换机时需要指定路由key。

![img](https://bbsmax.ikafan.com/static/L3Byb3h5L2h0dHBzL3d3dy5yYWJiaXRtcS5jb20vaW1nL3R1dG9yaWFscy9kaXJlY3QtZXhjaGFuZ2UucG5n.jpg)

5. **topic模式**：将路由键和某模式进行匹配，此时队列需要绑定在一个模式上，“#”匹配一个词或多个词，“*”只匹配一个词。

![img](https://bbsmax.ikafan.com/static/L3Byb3h5L2h0dHBzL3d3dy5yYWJiaXRtcS5jb20vaW1nL3R1dG9yaWFscy9weXRob24tZml2ZS5wbmc=.jpg)

## 15、Git

#### <span style='color:red'>1）结构</span>

![img](https://img2018.cnblogs.com/blog/1452443/201908/1452443-20190825154321356-2075865363.png)

**工作区（WORKING DIRECTORY）**: 直接编辑文件的地方，肉眼可见直接操作；

**暂存区（STAGIN AREA）**：数据（快照）暂时存放的地方；

**版本库（GIT DIRECTORT(RESPOSITORY)）：**存放已经提交的数据，push 的时候，就是把这个区的数据 push 到远程git仓库了。

git add就是将**工作区**的修改缓存在**暂存区，**git commit就是将**暂存区**的数据快照提交到**本地库**

#### <span style='color:red'>2）常见指令</span>

git **add** file或者git add .：新增文件的命令

git **commit** –m或者git commit –a：提交文件的命令

git **status** –s：查看工作区状况

git **fetch**/git **merge**或者git **pull**：拉取合并远程分支的操作

git **checkout** **切换分支**（-b 创建并切换分支）或者是**取消当前工作区的修改**，从缓存区**拉取上一个版本**（缓存区没有则从版本库拉取）

git **reset** 回退某一版本



git pull相当于git fetch+git merge，但是最好用后者，因为使用前者将直接从远程版本库拉取文件覆盖掉本地工作区，无法知道两者区别，如果使用后者可以使用diff查看区别

#### <span style='color:red'>n）杂七杂八</span>

**指针之间的关系**

**提交之前没有拉取最新代码，报错**

**拉取代码时，未提交当前修改，报错**

## 16、Maven

#### <span style='color:red'>1）什么是Maven</span>

Maven主要服务于基于java平台的项目构建，依赖管理和项目信息管理。Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。它包含了一个项目对象模型，一组标准集合，一个项目生命周期，一个依赖管理系统和用来运行定义在生命周期阶段中插件目标的逻辑。当使用Maven的时候，你用一个明确定义的项目对象模型来描述你的项目，然后Maven可以应用横切的逻辑，这些逻辑来自于一组共享的（或自定义的）插件。

#### <span style='color:red'>2）为什么选用 Maven 进行构建</span>

- 首先，Maven 是一个优秀的项目构建工具。使用maven，可以很方便的对项目进行分模块构建，这样在开发和测试打包部署时，效率会提高很多。
- 其次，Maven 可以进行依赖的管理。使用 Maven ，可以将不同系统的依赖进行统一管理，并且可以进行依赖之间的传递和继承。

#### <span style='color:red'>3）规约</span>

- `/src/main/java/` ：Java 源码。
- `/src/main/resource` ：Java 配置文件，资源文件。
- `/src/test/java/` ：Java 测试代码。
- `/src/test/resource` ：Java 测试配置文件，资源文件。
- `/target` ：文件编译过程中生成的 `.class` 文件、jar、war 等等。
- `pom.xml` ：配置文件

Maven 要负责项目的自动化构建，以编译为例，Maven 要想自动进行编译，那么它必须知道 Java 的源文件保存在哪里，这样约定之后，不用我们手动指定位置，Maven 能知道位置，从而帮我们完成自动编译。

遵循**“约定>>>配置>>>编码”**。即能进行配置的不要去编码指定，能事先约定规则的不要去进行配置。这样既减轻了劳动力，也能防止出错。

#### <span style='color:red'>4）常用命令</span>

- `mvn archetype：create` ：创建 Maven 项目。
- `mvn compile` ：编译源代码。
- `mvn deploy` ：发布项目。
- `mvn test-compile` ：编译测试源代码。
- `mvn test` ：运行应用程序中的单元测试。
- `mvn site` ：生成项目相关信息的网站。
- `mvn clean` ：清除项目目录中的生成结果。
- `mvn package` ：根据项目生成的 jar/war 等。
- `mvn install` ：在本地 Repository 中安装 jar 。
- `mvn eclipse:eclipse` ：生成 Eclipse 项目文件。
- `mvn jetty:run` 启动 Jetty 服务。
- `mvn tomcat:run` ：启动 Tomcat 服务。
- `mvn clean package -Dmaven.test.skip=true` ：清除以前的包后重新打包，跳过测试类。

#### <span style='color:red'>5）坐标的含义</span>

- **groupId** ：定义当前 Maven 项目隶属的实际项目。首先，Maven 项目和实际项目不一定是一对一的关系。比如 Spring FrameWork 这一实际项目，其对应的 Maven 项目会有很多，如 `spring-core`、`spring-context` 等。这是由于 Maven 中模块的概念，因此，一个实际项目往往会被划分成很多模块。其次，groupId 不应该对应项目隶属的组织或公司。原因很简单，一个组织下会有很多实际项目，如果 groupId 只定义到组织级别，而后面我们会看到，artifactId 只能定义 Maven 项目(模块)，那么实际项目这个层次将难以定义。最后，groupId 的表示方式与 Java 包名的表达方式类似，通常与域名反向一一对应。上例中，groupId 为 `junit` ，是不是感觉很特殊，这样也是可以的，因为全世界就这么个 junit ，它也没有很多分支。

- **artifactId** ：该元素定义当前实际项目中的一个 Maven 项目(模块)。推荐的做法是使用实际项目名称作为 artifactId 的前缀。比如上例中的 junit ，junit 就是实际的项目名称，方便而且直观。在默认情况下，Maven 生成的构件，会以 artifactId 作为文件头。例如 `junit-3.8.1.jar` ，使用实际项目名称作为前缀，就能方便的从本地仓库找到某个项目的构件。

- **version** ：该元素定义了使用构件的版本。如上例中 junit 的版本是 `4.13-BETA` ，你也可以改为 `4.1.2` 表示使用 `4.1.2` 版本的 junit 。

- **packaging** ：定义 Maven 项目打包的方式，使用构件的什么包。打包方式通常与所生成构件的文件扩展名对应。如上例中没有 `packaging` ，则默认为 jar 包，最终的文件名为`junit-4.13-BETA.jar` 。当然，也可以打包成 war 等。

- **classifier** ：该元素用来帮助定义构建输出的一些附件。附属构件与主构件对应。如上例中的主构件为 `junit-4.13-BETA.jar` ，该项目可能还会通过一些插件生成如 `junit-4.13-BETA-javadoc.jar`、`junit-4.13-BETA-sources.jar`，这样附属构件也就拥有了自己唯一的坐标。

- **scope**：依赖项的适用范围。

  `compile` ：默认值，适用于所有阶段（开发、测试、部署、运行），本 jar 会一直存在所有阶段。

  `provided` ：只在开发、测试阶段使用，目的是不让 Servlet 容器和你本地仓库的 jar 包冲突 。如 `servlet.jar` 。

  `runtime` ：只在运行时使用，如 JDBC 驱动，适用运行和测试阶段。

  `test` ：只在测试时使用，用于编译和运行测试代码，不会随项目发布。

  `system` ：类似 `provided` ，需要显式提供包含依赖的 jar 包，Maven 不会在 Repository 中查找它。

  `import` ：用于一个 `<dependencyManagement />` 对另一个 `<dependencyManagement />` 的继承。非常重要，通过它，可以实现类似 [《Maven Spring BOM (bill of materials)》](https://www.cnblogs.com/YLsY/p/5711103.html) 的功能。

#### <span style='color:red'>n）杂七杂八</span>

**maven和Gradle对比**

**依赖机制、依赖原则**

**jar冲突**

**maven插件**

**maven仓库**

## 17、Docker

#### <span style='color:red'>1）什么是Docker</span>

Docker是一个容器化平台，它以容器的形式将您的应用程序及其所有依赖项打包在一起，以确保您的应用程序在任何环境中无缝运行。

#### <span style='color:red'>2）Docker和虚拟机</span>

![img](https://blog.fundebug.com/2017/05/31/docker-and-vm/vm.jpg)

- **基础设施(Infrastructure)**。它可以是你的**个人电脑**，数据中心的**服务器**，或者是**云主机**。
- **主操作系统(Host Operating System)**。你的个人电脑之上，运行的可能是**MacOS**，**Windows**或者某个**Linux**发行版。
- **虚拟机管理系统(Hypervisor)**。利用Hypervisor，可以在**主操作系统**之上运行多个不同的**从操作系统**。类型1的Hypervisor有支持MacOS的**HyperKit**，支持Windows的**Hyper-V**以及支持Linux的**KVM**。类型2的Hypervisor有VirtualBox和VMWare。
- **从操作系统(Guest Operating System)**。假设你需要运行3个相互隔离的应用，则需要使用Hypervisor启动3个**从操作系统**，也就是3个**虚拟机**。这些虚拟机都非常大，也许有700MB，这就意味着它们将占用2.1GB的磁盘空间。更糟糕的是，它们还会消耗很多CPU和内存。
- **各种依赖**。每一个**从操作系统**都需要安装许多依赖。如果你的的应用需要连接PostgreSQL的话，则需要安装**libpq-dev**；如果你使用Ruby的话，应该需要安装gems；如果使用其他编程语言，比如Python或者Node.js，都会需要安装对应的依赖库。
- **应用**。安装依赖之后，就可以在各个**从操作系统**分别运行应用了，这样各个应用就是相互隔离的。

![img](https://blog.fundebug.com/2017/05/31/docker-and-vm/docker.jpg)

- **基础设施(Infrastructure)**。
- **主操作系统(Host Operating System)**。所有主流的Linux发行版都可以运行Docker。对于MacOS和Windows，也有一些办法”运行”Docker。
- **Docker守护进程(Docker Daemon)**。Docker守护进程取代了Hypervisor，它是运行在操作系统之上的后台进程，负责管理Docker容器。
- **各种依赖**。对于Docker，应用的所有依赖都打包在**Docker镜像**中，**Docker容器**是基于**Docker镜像**创建的。
- **应用**。应用的源代码与它的依赖都打包在**Docker镜像**中，不同的应用需要不同的**Docker镜像**。不同的应用运行在不同的**Docker容器**中，它们是相互隔离的。



**Docker守护进程**可以直接与**主操作系统**进行通信，为各个**Docker容器**分配资源；它还可以将容器与**主操作系统**隔离，并将各个容器互相隔离。**虚拟机**启动需要数分钟，而**Docker容器**可以在数毫秒内启动。由于没有臃肿的**从操作系统**，Docker可以节省大量的磁盘空间以及其他系统资源。

<span style='color:red'>**总的来说**</span>，**虚拟机**更擅长于彻底隔离整个运行环境。例如，云服务提供商通常采用虚拟机技术隔离不同的用户。而**Docker**通常用于隔离不同的应用，例如**前端**，**后端**以及**数据库**。

#### <span style='color:red'>3）Docker镜像</span>

Docker镜像是Docker容器的源代码，Docker镜像用于创建容器。使用build命令创建镜像。

#### <span style='color:red'>4）Docker容器</span>

Docker容器包括应用程序及其所有依赖项，作为操作系统的独立进程运行。

Docker容器有四种状态：**运行**、**已暂停**、**重新启动**、**已退出**。

#### <span style='color:red'>5）Dockerfile常用指令</span>

![img](https://img2018.cnblogs.com/blog/22615/201912/22615-20191201204758467-826275333.png)

run是创建镜像的过程中要执行的指令

cmd和entrypoint是执行容器的时候执行的指令

cmd在执行容器的时候加入参数会覆盖，entrypoint不会

#### <span style='color:red'>6）Docker常用指令</span>

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.mamicode.com%2Finfo%2F201809%2F20180930111856338823.png&refer=http%3A%2F%2Fimage.mamicode.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1625121796&t=d6c0b5269de61ad1c163622aaa4a8646)

-it   交互模式

在容器内用ctrl+p+q可以不停止容器推出

docker logs查看日志

docker exec 是进入容器后打开一个新的命令行，所以docker exec需要加/bin/bash，而且进入容器后exit并不会停止该容器

docker attach是进入容器正在执行的命令行，所以并不需要参数/bin/bash

## 18、数据结构

## 19、设计模式

### <span style='color:red'>一）创建型模式</span>

#### <span style='color:red'>1）工厂方法模式</span>

**简单工厂模式**：

创建**抽象产品类** & 定义具体产品的公共接口；

创建**具体产品类**（继承抽象产品类） & 定义生产的具体产品；

创建**工厂类**，通过创建静态方法根据传入不同参数从而创建不同具体产品类的实例；

外界通过调用工厂类的静态方法，**传入不同参数**从而创建不同**具体产品类的实例**

优点：

- 将创建实例的工作与使用实例的工作分开，使用者不必关心类对象如何创建，实现了解耦；
- 把初始化实例时的工作放到工厂里进行，使代码更容易维护。 更符合面向对象的原则 & 面向接口编程，而不是面向实现编程。

缺点：

工厂类集中了所有实例（产品）的创建逻辑，一旦这个工厂不能正常工作，整个系统都会受到影响；

违背“开放 - 关闭原则”，一旦添加新产品就不得不修改工厂类的逻辑，这样就会造成工厂逻辑过于复杂。

简单工厂模式由于使用了静态工厂方法，静态方法不能被继承和重写，会造成工厂角色无法形成基于继承的等级结构。

**工厂方法模式**：

创建**抽象工厂类**，定义具体工厂的公共接口； 

创建**抽象产品类** ，定义具体产品的公共接口； 

创建**具体产品类**（继承抽象产品类） & 定义生产的具体产品； 

创建**具体工厂类**（继承抽象工厂类），定义创建对应具体产品实例的方法； 

外界通过调用具体工厂类的方法，从而创建不同**具体产品类的实例**

**优点**：

- 更符合开-闭原则 

  新增一种产品时，只需要增加相应的具体产品类和相应的工厂子类即可

  <span style='color:red'>简单工厂模式需要修改工厂类的判断逻辑</span>

- 符合单一职责原则 

  每个具体工厂类只负责创建对应的产品

  <span style='color:red'>简单工厂中的工厂类存在复杂的switch逻辑判断</span>

- 不使用静态工厂方法，可以形成基于继承的等级结构。

  <span style='color:red'>简单工厂模式的工厂类使用静态工厂方法</span>

**总结**：工厂模式可以说是简单工厂模式的进一步抽象和拓展，在保留了简单工厂的封装优点的同时，让扩展变得简单，让继承变得可行，增加了多态性的体现。

**缺点**：

- 添加新产品时，除了增加新产品类外，还要提供与之对应的具体工厂类，系统类的个数将成对增加，在一定程度上增加了系统的复杂度；同时，有更多的类需要编译和运行，会给系统带来一些额外的开销；
- 由于考虑到系统的可扩展性，需要引入抽象层，在客户端代码中均使用抽象层进行定义，增加了系统的抽象性和理解难度，且在实现时可能需要用到DOM、反射等技术，增加了系统的实现难度。
- 虽然保证了工厂方法内的对修改关闭，但对于使用工厂方法的类，如果要更换另外一种产品，仍然需要修改实例化的具体工厂类；
- 一个具体工厂只能创建一种具体产品

#### <span style='color:red'>2）抽象工厂模式</span>

创建**抽象工厂类**，定义具体工厂的公共接口；

创建**抽象产品族类** ，定义抽象产品的公共接口；

创建**抽象产品类** （继承抽象产品族类），定义具体产品的公共接口；

创建**具体产品类**（继承抽象产品类） & 定义生产的具体产品；

创建**具体工厂类**（继承抽象工厂类），定义创建对应具体产品实例的方法；

客户端通过实例化具体的工厂类，并调用其创建不同目标产品的方法创建不同具体产品类的实例

**优点**：

- 降低耦合

  抽象工厂模式将具体产品的创建延迟到具体工厂的子类中，这样将对象的创建封装起来，可以减少客户端与具体产品类之间的依赖，从而使系统耦合度低，这样更有利于后期的维护和扩展；

- 更符合开-闭原则

  新增一种产品类时，只需要增加相应的具体产品类和相应的工厂子类即可

  <span style='color:red'>简单工厂模式需要修改工厂类的判断逻辑</span>

- 符合单一职责原则

  每个具体工厂类只负责创建对应的产品

  <span style='color:red'>简单工厂中的工厂类存在复杂的switch逻辑判断</span>

- 不使用静态工厂方法，可以形成基于继承的等级结构。

  <span style='color:red'>简单工厂模式的工厂类使用静态工厂方法</span>

**缺点**：

抽象工厂模式很难支持新种类产品的变化。

这是因为抽象工厂接口中已经确定了可以被创建的产品集合，如果需要添加新产品，此时就必须去修改抽象工厂的接口，这样就涉及到抽象工厂类的以及所有子类的改变，这样也就违背了“开发——封闭”原则。

<span style='color:red'>对于新的产品族符合开-闭原则；对于新的产品种类不符合开-闭原则，这一特性称为开-闭原则的倾斜性。</span>

#### <span style='color:red'>3）单例模式</span>

实现1个类只有1个实例化对象 & 提供一个全局访问点

**优点**：

1. 提供了对唯一实例的受控访问；

2. 由于在系统内存中只存在一个对象，因此可以节约系统资源，对于一些需要频繁创建和销毁的对象单例模式无疑可以提高系统的性能；
3. 可以根据实际情况需要，在单例模式的基础上扩展做出双例模式，多例模式；

**缺点**：

1. 单例类的职责过重，里面的代码可能会过于复杂，在一定程度上违背了“单一职责原则”。
2. 如果实例化的对象长时间不被利用，会被系统认为是垃圾而被回收，这将导致对象状态的丢失。

#### <span style='color:red'>4）建造者模式</span>

指挥者（Director）直接和客户（Client）进行需求沟通；

沟通后指挥者将客户创建产品的需求划分为各个部件的建造请求（Builder）；

将各个部件的建造请求委派到具体的建造者（ConcreteBuilder）；

各个具体建造者负责进行产品部件的构建；

最终构建成具体产品（Product）。

**创建**建造者抽象类；

**创建**具体产品类；

**创建**建造者具体类；

**创建**指挥者具体类；

客户端通过指挥者指挥实例化的建造者创建具体产品。

**优点**：

指挥者（Director）直接和客户（Client）进行需求沟通；

沟通后指挥者将客户创建产品的需求划分为各个部件的建造请求（Builder）；

将各个部件的建造请求委派到具体的建造者（ConcreteBuilder）；

各个具体建造者负责进行产品部件的构建；

最终构建成具体产品（Product）。

**缺点**：

建造者模式所创建的产品一般具有较多的共同点，其组成部分相似；如果产品之间的差异性很大，则不适合使用建造者模式，因此其使用范围受到一定的限制。

如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，导致系统变得很庞大。

**应用场景**：

- 需要生成的产品对象有复杂的内部结构，这些产品对象具备共性；
- 隔离复杂对象的创建和使用，并使得相同的创建过程可以创建不同的产品。

#### <span style='color:red'>5）原型模式</span>

这种模式是实现了一个原型接口，该接口用于创建当前对象的克隆。当直接创建对象的代价比较大时，则采用这种模式。

原型模式的核心思想是，通过拷贝指定的“**原型实例（对象）**”，创建跟该对象一样的新对象。简单理解就是“**克隆指定对象**”。

需要拷贝的原型类必须实现"**java.lang.Cloneable**"接口，然后**重写Object类中的clone方法**，从而才可以实现类的拷贝。

### <span style='color:red'>二）结构型模式</span>

#### <span style='color:red'>6）适配器模式</span>

把一个类的接口变换成客户端所期待的另一种接口，从而使原本接口不匹配而无法一起工作的两个类能够在一起工作。

**创建接口**；**创建源类**；**创建适配器类**（继承源类并实现接口），**实现接口的方法调用源类的方法**

#### <span style='color:red'>7）装饰器模式</span>

动态的给一个对象添加额外的功能，同时又不改变其结构。因此就增加功能来说，装饰模式比子类继承的方式更灵活。

**创建接口**；**创建被装饰对象**（实现接口）；**创建抽象装饰类**（实现接口）；**创建具体装饰类**（继承抽象装饰类），**用被装饰对象初始化具体装饰类**并增强方法

#### <span style='color:red'>8）代理模式</span>

静态代理：由程序员创建或由特定工具自动生成源代码，再对其编译。在程序运行前，代理类的.class文件就已经存在了。

动态代理：在程序运行时，运用反射机制动态创建而成。

静态代理需要：**接口**，**基础实现类**，**代理类**（代理类中含有基础实现类作为成员变量）

动态代理需要：**接口**，**基础实现类**，**处理程序的实现类**（继承**InvocationHandler**接口，实现接口），**代理类**（调用**Proxy**类的方法）

#### <span style='color:red'>9）外观模式</span>

外观模式（Facade Pattern**）隐藏系统的复杂性**，并向客户端提供了一个客户端可以访问系统的接口。它向现有的系统添加一个接口，来隐藏系统的复杂性。

**在外观类中初始化其他类并暴露相关方法，client对于其他类的相应方法的调用可以直接调用外观类的相应方法。**

**就相当于在其他各类和client之间加了一层。**

#### <span style='color:red'>10）桥接模式</span>

桥接模式即**将抽象部分与它的实现部分分离开来**，使他们都可以独立变化。

桥接模式将继承关系转化成关联关系，它降低了类与类之间的耦合度，减少了系统中类的数量，也减少了代码量。

**就是在一个抽象类中定义了一个成员接口，该接口可以有多个实现类，该抽象类可以有多个子类，从而实现了多层继承的结构。**

#### <span style='color:red'>11）组合模式</span>

组合模式（Composite Pattern），又叫部分整体模式，是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。这种类型的设计模式属于结构型模式，它创建了对象组的树形结构。

#### <span style='color:red'>12）享元模式</span>

享元模式的定义提出了两个要求，细粒度和共享对象。因为要求细粒度，所以不可避免地会使对象数量多且性质相近，此时我们就将这些对象的信息分为两个部分：内部状态和外部状态。

- 内部状态指对象共享出来的信息，存储在享元信息内部，并且不回随环境的改变而改变；
- 外部状态指对象得以依赖的一个标记，随环境的改变而改变，不可共享。

比如，连接池中的连接对象，保存在连接对象中的用户名、密码、连接URL等信息，在创建对象的时候就设置好了，不会随环境的改变而改变，这些为内部状态。而当每个连接要被回收利用时，我们需要将它标记为可用状态，这些为外部状态。

享元模式的本质是缓存共享对象，降低内存消耗。

**享元模式是池技术的重要实现方式。**

享元模式的主要角色有如下。

1. **抽象享元角色**（Flyweight）：是所有的具体享元类的基类，为具体享元规范需要实现的公共接口，非享元的外部状态以参数的形式通过方法传入。
2. **具体享元**（Concrete Flyweight）角色：实现抽象享元角色中所规定的接口。
3. **非享元**（Unsharable Flyweight)角色：是不可以共享的外部状态，它以参数的形式注入具体享元的相关方法中。
4. **享元工厂**（Flyweight Factory）角色：负责创建和管理享元角色。当客户对象请求一个享元对象时，享元工厂检査系统中是否存在符合要求的享元对象，如果存在则提供给客户；如果不存在的话，则创建一个新的享元对象。

### <span style='color:red'>三）行为型模式</span>

#### <span style='color:red'>13）策略模式</span>

该模式定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的变化不会影响使用算法的客户。策略模式属于对象行为模式，它通过对算法进行封装，把使用算法的责任和算法的实现分割开来，并委派给不同的对象对这些算法进行管理。

策略模式的主要角色如下。

1. **抽象策略**（Strategy）类：定义了一个公共接口，各种不同的算法以不同的方式实现这个接口，环境角色使用这个接口调用不同的算法，一般使用接口或抽象类实现。
2. **具体策略**（Concrete Strategy）类：实现了抽象策略定义的接口，提供具体的算法实现。
3. **环境**（Context）类：持有一个策略类的引用，最终给客户端调用。

#### <span style='color:red'>14）模板方法模式</span>

定义一个操作中的算法骨架，而将算法的一些步骤延迟到子类中，使得子类可以不改变该算法结构的情况下重定义该算法的某些特定步骤。

**优点**：

1. **它封装了不变部分，扩展可变部分**。它把认为是不变部分的算法封装到父类中实现，而把可变部分算法由子类继承实现，便于子类继续扩展。
2. 它在父类中**提取了公共的部分代码**，便于代码复用。
3. 部分方法是由子类实现的，因此**子类可以通过扩展方式增加相应的功能，符合开闭原则**。

**缺点**：

1. 对每个不同的实现都需要定义一个子类，这导致**类的个数增加**，系统更加庞大，设计也更加抽象，间接地增加了系统实现的复杂度。
2. 父类中的抽象方法由子类实现，**子类执行的结果会影响父类的结果**，这导致一种反向的控制结构，它提高了代码阅读的难度。
3. 由于继承关系自身的缺点，**如果父类添加新的抽象方法，则所有子类都要改一遍**。

#### <span style='color:red'>15）观察者模式</span>

指多个对象间存在一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。这种模式有时又称作发布-订阅模式、模型-视图模式。

观察者模式的主要角色如下。

1. **抽象目标**（Subject）角色：也叫抽象目标类，它提供了一个用于保存观察者对象的聚集类和增加、删除观察者对象的方法，以及通知所有观察者的抽象方法。
2. **具体目标**（Concrete Subject）角色：也叫具体目标类，它实现抽象目标中的通知方法，当具体主题的内部状态发生改变时，通知所有注册过的观察者对象。
3. **抽象观察者**（Observer）角色：它是一个抽象类或接口，它包含了一个更新自己的抽象方法，当接到具体主题的更改通知时被调用。
4. **具体观察者**（Concrete Observer）角色：实现抽象观察者中定义的抽象方法，以便在得到目标的更改通知时更新自身的状态。

#### <span style='color:red'>16）迭代器模式</span>

提供一个对象来顺序访问聚合对象中的一系列数据，而不暴露聚合对象的内部表示。

迭代器模式主要包含以下角色。

1. **抽象聚合**（Aggregate）角色：定义存储、添加、删除聚合对象以及创建迭代器对象的接口。
2. **具体聚合**（ConcreteAggregate）角色：实现抽象聚合类，返回一个具体迭代器的实例。
3. **抽象迭代器**（Iterator）角色：定义访问和遍历聚合元素的接口，通常包含 hasNext()、first()、next() 等方法。
4. **具体迭代器**（Concretelterator）角色：实现抽象迭代器接口中所定义的方法，完成对聚合对象的遍历，记录遍历的当前位置。

#### <span style='color:red'>17）责任链模式</span>

为了避免请求发送者与多个请求处理者耦合在一起，于是将所有请求的处理者通过前一对象记住其下一个对象的引用而连成一条链；当有请求发生时，可将请求沿着这条链传递，直到有对象处理它为止。

在责任链模式中，客户只需要将请求发送到责任链上即可，无须关心请求的处理细节和请求的传递过程，请求会自动进行传递。所以责任链将请求的发送者和请求的处理者解耦了。

职责链模式主要包含以下角色。

1. **抽象处理者**（Handler）角色：定义一个处理请求的接口，包含抽象处理方法和一个后继连接。
2. **具体处理者**（Concrete Handler）角色：实现抽象处理者的处理方法，判断能否处理本次请求，如果可以处理请求则处理，否则将该请求转给它的后继者。
3. **客户类**（Client）角色：创建处理链，并向链头的具体处理者对象提交请求，它不关心处理细节和请求的传递过程。

#### <span style='color:red'>18）命令模式</span>

将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开。这样两者之间通过命令对象进行沟通，这样方便将命令对象进行储存、传递、调用、增加与管理。

命令模式包含以下主要角色。

1. **抽象命令类**（Command）角色：声明执行命令的接口，拥有执行命令的抽象方法 execute()。
2. **具体命令类**（Concrete Command）角色：是抽象命令类的具体实现类，它拥有接收者对象，并通过调用接收者的功能来完成命令要执行的操作。
3. **实现者/接收者**（Receiver）角色：执行命令功能的相关操作，是具体命令对象业务的真正实现者。
4. **调用者/请求者**（Invoker）角色：是请求的发送者，它通常拥有很多的命令对象，并通过访问命令对象来执行相关请求，它不直接访问接收者。

#### <span style='color:red'>19）备忘录模式</span>

在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便以后当需要时能将该对象恢复到原先保存的状态。该模式又叫快照模式。

备忘录模式的主要角色如下。

1. **发起人**（Originator）角色：记录当前时刻的内部状态信息，提供创建备忘录和恢复备忘录数据的功能，实现其他业务功能，它可以访问备忘录里的所有信息。
2. **备忘录**（Memento）角色：负责存储发起人的内部状态，在需要的时候提供这些内部状态给发起人。
3. **管理者**（Caretaker）角色：对备忘录进行管理，提供保存与获取备忘录的功能，但其不能对备忘录的内容进行访问与修改。

#### <span style='color:red'>20）状态模式</span>

对有状态的对象，把复杂的“判断逻辑”提取到不同的状态对象中，允许状态对象在其内部状态发生改变时改变其行为。

状态模式**把受环境改变的对象行为包装在不同的状态对象里**，其意图是让一个对象在其内部状态改变的时候，其行为也随之改变。

状态模式包含以下主要角色。

1. **环境类**（Context）角色：也称为上下文，它定义了客户端需要的接口，内部维护一个当前状态，并负责具体状态的切换。
2. **抽象状态**（State）角色：定义一个接口，用以封装环境对象中的特定状态所对应的行为，可以有一个或多个行为。
3. **具体状态**（Concrete State）角色：实现抽象状态所对应的行为，并且在需要的情况下进行状态切换。

#### <span style='color:red'>21）访问者模式</span>

将作用于某种数据结构中的各元素的操作分离出来封装成独立的类，使其在不改变数据结构的前提下可以添加作用于这些元素的新的操作，为数据结构中的每个元素提供多种访问方式。它将对数据的操作与数据结构进行分离，是行为类模式中最复杂的一种模式。

访问者模式包含以下主要角色。

1. **抽象访问者**（Visitor）角色：定义一个访问具体元素的接口，为每个具体元素类对应一个访问操作 visit() ，该操作中的参数类型标识了被访问的具体元素。
2. **具体访问者**（ConcreteVisitor）角色：实现抽象访问者角色中声明的各个访问操作，确定访问者访问一个元素时该做什么。
3. **抽象元素**（Element）角色：声明一个包含接受操作 accept() 的接口，被接受的访问者对象作为 accept() 方法的参数。
4. **具体元素**（ConcreteElement）角色：实现抽象元素角色提供的 accept() 操作，其方法体通常都是 visitor.visit(this) ，另外具体元素中可能还包含本身业务逻辑的相关操作。
5. **对象结构**（Object Structure）角色：是一个包含元素角色的容器，提供让访问者对象遍历容器中的所有元素的方法，通常由 List、Set、Map 等聚合类实现。

**对于一系列的具体元素（在对象结构中），不同的具体访问者可以对具体元素进行不同的操作。**

#### <span style='color:red'>22）中介者模式</span>

定义一个中介对象来封装一系列对象之间的交互，使原有对象之间的耦合松散，且可以独立地改变它们之间的交互。中介者模式又叫调停模式，它是迪米特法则的典型应用。

中介者模式包含以下主要角色。

1. **抽象中介者**（Mediator）角色：它是中介者的接口，提供了同事对象注册与转发同事对象信息的抽象方法。
2. **具体中介者**（Concrete Mediator）角色：实现中介者接口，定义一个 List 来管理同事对象，协调各个同事角色之间的交互关系，因此它依赖于同事角色。
3. **抽象同事类**（Colleague）角色：定义同事类的接口，保存中介者对象，提供同事对象交互的抽象方法，实现所有相互影响的同事类的公共功能。
4. **具体同事类**（Concrete Colleague）角色：是抽象同事类的实现者，当需要与其他同事对象交互时，由中介者对象负责后续的交互。

#### <span style='color:red'>23）解释器模式</span>

给分析对象定义一个语言，并定义该语言的文法表示，再设计一个解析器来解释语言中的句子。也就是说，用编译语言的方式来分析应用中的实例。这种模式实现了文法表达式处理的接口，该接口解释一个特定的上下文。

解释器模式包含以下主要角色。

1. **抽象表达式**（Abstract Expression）角色：定义解释器的接口，约定解释器的解释操作，主要包含解释方法 interpret()。
2. **终结符表达式**（Terminal Expression）角色：是抽象表达式的子类，用来实现文法中与终结符相关的操作，文法中的每一个终结符都有一个具体终结表达式与之相对应。
3. **非终结符表达式**（Nonterminal Expression）角色：也是抽象表达式的子类，用来实现文法中与非终结符相关的操作，文法中的每条规则都对应于一个非终结符表达式。
4. **环境**（Context）角色：通常包含各个解释器需要的数据或是公共的功能，一般用来传递被所有解释器共享的数据，后面的解释器可以从这里获取这些值。
5. **客户端**（Client）：主要任务是将需要分析的句子或表达式转换成使用解释器对象描述的抽象语法树，然后调用解释器的解释方法，当然也可以通过环境角色间接访问解释器的解释方法。

#### <span style='color:red'>n）杂七杂八</span>

**java七大设计原则**

**工厂模式和建造者模式区别**

**装饰器模式和代理模式区别**

## 20、Nginx

#### <span style='color:red'>1）什么是Nginx</span>

Nginx是一个 轻量级/高性能的**反向代理Web服务器**，他实现非常高效的**反向代理**、**负载平衡**，他可以处理2-3万并发连接数，官方监测能支持5万并发，现在中国使用nginx网站用户有很多，例如：**新浪**、**网易**、 **腾讯**等。

#### <span style='color:red'>2）正向代理和反向代理</span>

1. 正向代理就是一个人发送一个请求**直接**就到达了目标的服务器

   正向代理是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理

2. 反方代理就是请求**统一**被Nginx接收，nginx反向代理服务器接收到之后，按照一定的规则分发给了后端的业务处理服务器进行处理了

   客户端是**无感知代理**的存在，以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端。

#### <span style='color:red'>3）为什么用Nginx</span>

1. 跨平台、配置简单、方向代理、高并发连接：处理2-3万并发连接数，官方监测能支持5万并发，内存消耗小：开启10个nginx才占150M内存 ，nginx处理静态文件好，耗费内存少，

2. 而且Nginx内置的健康检查功能：如果有一个服务器宕机，会做一个健康检查，再发送的请求就不会发送到宕机的服务器了。重新将请求提交到其他的节点上。

3. 使用Nginx的话还能：

   a. 节省宽带：支持GZIP压缩，可以添加浏览器本地缓存
   b. 稳定性高：宕机的概率非常小
   c. 接收用户请求是异步的

#### <span style='color:red'>4）为什么Nginx性能高</span>

因为他的事件处理机制：异步非阻塞事件处理机制：运用了epoll模型，提供了一个队列，排队解决

#### <span style='color:red'>5）优缺点</span>

**优点**：

1. 占内存小，可实现高并发连接，处理响应快
2. 可实现http服务器、虚拟主机、方向代理、负载均衡
3. Nginx配置简单
4. 可以不暴露正式的服务器IP地址

**缺点**：

1. 动态处理差：nginx处理静态文件好,耗费内存少，但是处理动态页面则很鸡肋，现在一般前端用nginx作为反向代理抗住压力，

#### <span style='color:red'>6）应用场景</span>

1. **http服务器**。Nginx是一个http服务可以独立提供http服务。可以做网页静态服务器。
2. **虚拟主机**。可以实现在一台服务器虚拟出多个网站，例如个人网站使用的虚拟机。
3. **反向代理**，**负载均衡**。当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用nginx做反向代理。并且多台服务器可以平均分担负载，不会应为某台服务器负载高宕机而某台服务器闲置的情况。
4. nginz 中也可以**配置安全管理**、比如可以使用Nginx搭建API接口网关,对每个接口服务进行拦截。

#### <span style='color:red'>n）杂七杂八</span>

**cgi和fastcgi**

**限流**（正常限制访问频率、突发限制访问频率、限制并发连接数）

**漏桶流算法和令牌桶算法**

**负载均衡算法**（轮询、权重、ip绑定、fair插件、url_hash插件）

**自身配置**（限制ip、限制浏览器、配置静态资源、配置上游服务器）

**Nginx集群**

## 21、SpringCloud Alibaba

#### <span style='color:red'>1）什么是SpringCloud Alibaba</span>

Spring Cloud Alibaba 是阿里巴巴提供的微服务开发一站式解决方案，是阿里巴巴开源中间件与 Spring Cloud 体系的融合。

同 Spring Cloud 一样，Spring Cloud Alibaba 也是一套微服务解决方案，包含开发分布式应用微服务的必需组件，方便开发者通过 Spring Cloud 编程模型轻松使用这些组件来开发分布式应用服务。

依托 Spring Cloud Alibaba，您只需要添加一些注解和少量配置，就可以将 Spring Cloud 应用接入阿里微服务解决方案，通过阿里中间件来迅速搭建分布式应用系统。

作为 Spring Cloud 体系下的新实现，Spring Cloud Alibaba 跟官方的组件或其它的第三方实现如 Netflix, Consul，Zookeeper 等对比，具备了更多的功能

![img](https://pic4.zhimg.com/80/v2-3a132c503bb7cde19314acc8a4866c4b_720w.jpg)

#### <span style='color:red'>2）开源组件</span>

Nacos：一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。

Sentinel：把流量作为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。

RocketMQ：开源的分布式消息系统，基于高可用分布式集群技术，提供低延时的、高可靠的消息发布与订阅服务。

Dubbo：这个就不用多说了，在国内应用非常广泛的一款高性能 Java RPC 框架。

Seata：阿里巴巴开源产品，一个易于使用的高性能微服务分布式事务解决方案。

Arthas：开源的Java动态追踪工具，基于字节码增强技术，功能非常强大。

![img](https://pic4.zhimg.com/80/v2-46c0b9e0d41c441d222390c79a4cd53b_720w.jpg)

#### <span style='color:red'>n）杂七杂八</span>

**nacos、zookeeper、eureka对比**

**sentinel、hystrix对比**

**gateway、zuul对比**

