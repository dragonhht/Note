---
title: Java中的序列化
date: 2019-04-05 22:38:44
tags: 
categories: 
    - Java
---

**目录 start**
 
1. [序列化](#序列化)
    1. [serialVersionUID](#serialversionuid)
1. [主流编解码框架](#主流编解码框架)
    1. [MessagePack](#messagepack)
    1. [Protobuf](#protobuf)
        1. [proto文件定义](#proto文件定义)
        1. [Linux上安装Protobuf](#linux上安装protobuf)
        1. [Java中的使用](#java中的使用)
    1. [Thrift](#thrift)
        1. [Marshalling](#marshalling)

**目录 end**|_2019-04-19 13:04_| [Kuangcp](https://github.com/Kuangcp/Note) | [yi-yun](https://github.com/yi-yun/Memo)
****************************************
# 序列化
> [码农翻身:序列化： 一个老家伙的咸鱼翻身](https://mp.weixin.qq.com/s?__biz=MzAxOTc0NzExNg==&mid=2665513589&idx=1&sn=d402d623d9121453f1e570395c7f99d7&chksm=80d67a36b7a1f32054d4c779dd26e8f97a075cf4d9ed1281f16d09f1df50a29319cd37520377&scene=21#wechat_redirect) `对象转化为二进制流`

## serialVersionUID
> 简单的说就是类的版本控制, 标明类序列化时的版本, 版本一致表明这两个类定义一致  
> 在进行反序列化时, JVM会把传来的字节流中的serialVersionUID与本地相应实体（类）的serialVersionUID进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常。(InvalidCastException)  
[参考博客](http://swiftlet.net/archives/1268)

- serialVersionUID有两种显示的生成方式： 
    -  一个是默认的1L
    -  一个是根据类名、接口名、成员方法及属性等来生成一个64位的哈希字段

> 当你一个类实现了Serializable接口，如果没有定义serialVersionUID，Eclipse会提供这个提示功能告诉你去定义 。
在Eclipse中点击类中warning的图标一下，Eclipse就会自动给定两种生成的方式。
如果不想定义它，在Eclipse的设置中也可以把它关掉的，设置如下：
Window ==> Preferences ==> Java ==> Compiler ==> Error/Warnings ==>Potential programming problems
将Serializable class without serialVersionUID的warning改成ignore即可。

******************************

# 主流编解码框架
> 因为Java序列化的性能和存储开销都表现不好,而且不能跨语言, 所以一般不使用Java的序列化而是使用以下流行的库

## MessagePack
> [Github:msgpack](https://github.com/msgpack) | [参考博客: MessagePack：一种高效二进制序列化格式](http://hao.jobbole.com/messagepack/)

## Protobuf
> Google开源的库 全称 `Google Protocol Buffers` |  [Github : Protobuf](https://github.com/google/protobuf)

> [参考博客: Protobuf语言指南](http://www.cnblogs.com/dkblog/archive/2012/03/27/2419010.html) `较为详细, 只是版本有点旧`
> [参考博客: 详解如何在NodeJS中使用Google的Protobuf](https://juejin.im/entry/59c1214df265da0658151a2c) | [protocobuf](https://github.com/dcodeIO/protobuf.js)
> [Google 开源技术protobuf ](https://blog.csdn.net/hguisu/article/details/20721109)
> [Google Protocol Buffer 的使用和原理](https://www.ibm.com/developerworks/cn/linux/l-cn-gpb/index.html)

> [参考博客: 数据交换利器 Protobuf 技术浅析](http://blog.jobbole.com/107405/)
> [参考博客: Protobuf3语言指南](https://blog.csdn.net/u011518120/article/details/54604615)

- 他将数据结构以 proto后缀的文件进行描述, 通过代码生成工具, 可以生成对应数据结构的POJO对象和Protobuf相关的方法和属性
    - 特点:
        - 结构化数据存储格式: XML JSON等
        - 高效的编解码性能
        - 语言无关, 平台无关, 扩展性好
        - 官方支持 Java C++ Python三种语言, 并且Js的支持也比较好[](https://github.com/dcodeIO/ProtoBuf.js/)
    - 数据描述文件和代码生成机制优点:
        - 文本化的数据结构描述语言, 可以实现语言和平台无关, 特别适合异构系统间的集成
        - 通过标识字段的顺序, 可以实现协议的前向兼容 _在不同版本的数据结构进程间进行数据传递_
        - 自动代码生成, 不需要手工编写同样数据结构的C++和Java版本;
        - 方便后续的管理和维护,相比于代码, 结构化的文档更容易管理和维护
- 习惯性规则:
    - 命名: `packageName.MessageName.proto`

> 只是编解码的工具, 不支持读半包, 粘包拆包

### proto文件定义

```protobuf
    // 用户数据信息
    message Article {
        required int32 articleId = 1;         // 文章id
        optional string articleExcerpt = 4;    // 文章摘要
        repeated string articlePicture = 5;   // 文章附图
    }
```
> 上面定义了一个消息, 消息具有三个属性, 且行末的注释都会变成Javadoc注释  

1. message 是消息定义的关键字
2. required 表示这个字段是必需的, 必须在序列化的时候被赋值。
3. optional 代表这个字段是可选的，可以为0个或1个但不能大于1个。
4. repeated 则代表此字段可以被重复任意多次包括0次。
5. int32和string是字段的类型。后面是我们定义的字段名。
6. 最后的1，2，3则是代表每个字段的一个唯一的编号标签，在同一个消息里不可以重复。这些编号标签用与在消息二进制格式中标识你的字段，并且消息一旦定义就不能更改。
    - 需要说明的是标签在1到15范围的采用一个字节进行编码。所以通常将标签1到15用于频繁发生的消息字段。编号标签大小的范围是1 到 2的29次幂–1。
    - 此外不能使用protobuf系统预留的编号标签（19000 －19999）。

![数据类型对应表](https://raw.githubusercontent.com/Kuangcp/ImageRepos/master/Learn/java/protobuf/protobuf-type.jpeg)

_复杂类型_  
> 定义了enum枚举类型，嵌套的消息。甚至对原有的消息进行了扩展，也可以对字段设置默认值。添加注释等
```protobuf
    package "com.github.kuangcp";
    message Article {
    required int32 article_id = 1;
    optional string article_excerpt = 2;
    repeated string article_picture = 3;
    optional int32  article_pagecount = 4 [default = 0];
    enum ArticleType {
        NOVEL = 0;
        PROSE = 1;
        PAPER = 2;
        POETRY = 3;
    }
    optional ArticleType article_type = 5 [default = NOVEL];
    message Author {
        required string name = 1; //作者的名字
        optional string phone = 2;
    }
    optional Author author = 6;
    repeated int32 article_numberofwords = 7 [packed=true];
    reserved  9, 10, 12 to 15;
    extensions 100 to 1000;
    }
    extend Article {
    optional int32 followers_count = 101;
    optional int32 likes_count= 102;
    }
    message Other {
        optional string other_info = 1;
        oneof test_oneof {
            string code1 = 2;
            string code2 = 3;
        }
    }
```
> 此外reserved关键字主要用于保留相关编号标签，主要是防止在更新proto文件删除了某些字段，而未来的使用者定义新的字段时重新使用了该编号标签。这会引起一些问题在获取老版本的消息时，譬如数据冲突，隐藏的一些bug等。所以一定要用reserved标记这些编号标签以保证不会被使用

> 当我们需要对消息进行扩展的时候，我们可以用extensions关键字来定义一些编号标签供第三方扩展。这样的好处是不需要修改原来的消息格式。就像上面proto文件，我们用extend关键字来扩展。只要扩展的字段编号标签在extensions定义的范围里。

> 对于基本数值类型，由于历史原因，不能被protobuf更有效的encode。所以在新的代码中使用packed=true可以更加有效率的encode。注意packed只能用于repeated 数值类型的字段。不能用于string类型的字段。

> 在消息Other中我们看到定义了一个oneof关键字。这个关键字作用比较有意思。当你设置了oneof里某个成员值时，它会自动清除掉oneof里的其他成员，也就是说同一时刻oneof里只有一个成员有效。这常用于你有许多optional字段时但同一时刻只能使用其中一个，就可以用oneof来加强这种效果。但需要注意的是oneof里的字段不能用required，optional，repeted关键字

_导入另一个proto定义_  
`import "article.proto";`

- 更新Protobuf文件的要求:
    1. 不能改变已有的任何编号标签。
    2. 只能添加optional和repeated的字段。这样旧代码能够解析新的消息，只是那些新添加的字段会被忽略。但是序列化的时候还是会包含哪些新字段。而新代码无论是旧消息还是新消息都可以解析。
    3. 非required的字段可以被删除，但是编号标签不可以再次被使用，应该把它标记到reserved中去
    4. 非required可以被转换为扩展字段，只要字段类型和编号标签保持一致
    5. 相互兼容的类型，可以从一个类型修改为另一个类型，譬如int32的字段可以修改为int64

***********************
>- 使用上, 因为有多个消息类型, 那么会采用一个数值id作为code, 进行对应 方便沟通

### Linux上安装Protobuf
> [参考博客: linux下Google的Protobuf安装及使用笔记](http://www.cnblogs.com/brainy/archive/2012/05/13/2498671.html) | [参考:proto buffer 安装 及 调用](http://dofound.blog.163.com/blog/static/1711432462013524111644655/)

- `下载二进制(推荐)` [各个版本,平台的 protoc](https://repo1.maven.org/maven2/com/google/protobuf/protoc/)

- `编译安装`
    - [下载2.5](https://github.com/google/protobuf/releases/tag/v2.5.0) 并解压 
    - 进入目录  `./configure` 
    -  `make` `make check` `sudo make install`
    - `protoc --version` 

> 注意: ./configure 时, 默认会安装在/usr/local目录下，可以加`--prefix=/usr`来指定安装到/usr/lib下  
>- 如果不加, 上述参数就要执行 `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib`  
>- 当然,可以将这个环境变量的设置加在 .zshrc 或者 .bashrc 里  
>- 不然就会报错: `protoc: error while loading shared libraries: libprotobuf.so.8: cannot open shared object file: No such file or directory`

### Java中的使用
> [Google Protocol Buffer 的使用和原理](https://www.ibm.com/developerworks/cn/linux/l-cn-gpb/index.html) `C++ 但是原理差不多`  
> [protobuf-gradle-plugin](https://github.com/google/protobuf-gradle-plugin)

`生成Java文件`  
_hi.proto_
```protobuf
package lm;
message helloworld{
    required int32 id = 1;//ID
    required string str = 2;//str
    optional int32 opt = 3;//optional field
}
```
- 据此生成Java文件 `mkdir src && protoc --java_out=./src hi.proto`
_也可以使用该脚本更新协议_
```sh
    # proto文件中明确定义了一样的包结构就可以直接跑脚本
    basePath='minigame/proto/proto'
    targetPath='ssss'
    rm -rf $targetPath \
    && mkdir $targetPath \
    && protoc $basePath/*.proto --java_out=$targetPath \
```

`使用`
```java
    // 实例化一个构建器
    helloworld.Builder msg = helloworld.newBuilder();
    // 填充信息
    msg.setId(12);
```

*********************

## Thrift
> [官网](https://thrift.apache.org/)源于Facebook, 支持多种语言: C++ C# Cocoa Erlang Haskell Java Ocami Perl PHP Python Ruby Smalltalk

- 它支持数据(对象)序列化和多种类型的RPC服务, Thrift适用于静态的数据交换, 需要预先确定好他的数据结构, 当数据结构发生变化时,需要重新编辑IDL文件
### Marshalling
> JBOSS 内部使用的编解码框架
