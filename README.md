#PhalApi - 轻量级PHP后台接口开发框架 - V1.1.2


###PhalApi是一个轻量级PHP后台接口开发框架，目的是让接口开发更简单。
```
此框架代码开源、产品开源、思想开源，可用于个人、商业用途等，请放心使用。
如果您决定采用此框架，请邮件（chanzonghuang@gmail.com）知会我一下，以便我们能提供更多帮助，谢谢 ^_^
如有问题，也可以通过国内的通道QQ（376741929）与我联系，或者在Git@OSC上新建Issue。
```
 
在此借一行文字的空间，感谢 **开源中国** 这么好的分享平台，同时也感谢您花费宝贵的时间来阅读此文档，在开源的路上，您每一次真心的关注和肯定都是我们前进的最大动力！谢谢！    
  

#背景
过去十年，是互联网时代；如今的十年，是移动时代。  
  
在iOS、Android、Windows Phone、PC版、Web版等各种终端和各种垂直应用不停更新迭代的大背景下，显然很是需要一组乃至一系列稳定的后台接口支撑。
接口，是如此重要，正如Jaroslav Tulach在《软件框架设计的艺术》一书中说的：
```
API就如同恒星，一旦出现，便与我们永恒共存。
```

所以，这里希望通过提供一个快速可用的后台接口开发框架，可以：

```
一来，支撑轻量级项目后台接口的快速开发；
二来，阐明如何进行接口开发、设计和维护，以很好支持海量访问、大数据、向前向后兼容等；
三来，顺便分享一些好的思想、技巧和有用的工具、最佳实践。
```

如果您有接口项目开发的需要，又刚好需要一个PHP接口框架，欢迎使用！    我们也争取致力于将我们的PhalApi维护成像恒星一样：  
```
不断更新，保持生气；为接口负责，为开源负责！
```


#安装
+ *请从release分支拉取稳定的代码*
+ *推荐在Linux服务器上进行开发*
+ *建议PHP >= 5.3.3*

将代码下载解压到服务器后即可，然后把根目录设置为Public。如nginx下：

```
root   /path/to/PhalApi/Public;
```

为验证是否安装成功，可以访问默认接口服务，如：http://localhost/PhalApi/demo/，正常时会返回类如：
```
{
    "ret": 200,
    "data": {
        "title": "Default Api",
        "content": "PHPer您好，欢迎使用PhalApi！",
        "version": "1.1.0",
        "time": 1422779027
    },
    "msg": ""
}
```
#在线体验
```
//默认的接口服务
http://phalapi.oschina.mopaas.com/Public/demo/

//带参数的示例接口
http://phalapi.oschina.mopaas.com/Public/demo/?service=Default.Index&username=oschina

//故意请求一个非法的服务
http://phalapi.oschina.mopaas.com/Public/demo/?service=Demo.None
{
    "ret": 400,
    "data": [],
    "msg": "非法请求：服务Demo.None不存在"
}
```

#官网
[http://www.phalapi.net](http://www.phalapi.net/)   
![apic](http://webtools.qiniudn.com/phalapi-logo-2-33.png)  
  
#文档
[http://git.oschina.net/dogstar/PhalApi/wikis/home](http://git.oschina.net/dogstar/PhalApi/wikis/home)  

  
#类参考手册
[http://www.phalapi.net/docs/](http://www.phalapi.net/docs/)  


#[酷！]接口参数在线查询
为了方便客户端查看最新的接口参数，特别提供此在线工具，根据接口代码实时生成接口参数报表，完全不需要后台开发编写维护额外的文档。我觉得，这很符合敏捷开发之道。
```
//接口参数在线查询工具链接
http://phalapi.oschina.mopaas.com/Public/demo/checkApiParams.php
```
如：http://phalapi.oschina.mopaas.com/Public/demo/checkApiParams.php ，访问效果如下：

 ![mahua](http://static.oschina.net/uploads/space/2015/0130/190225_8HRX_256338.jpg)
 因此，接口所需要的参数，对于接口开发人员，也只是简单配置一下参数规则，便可以轻松获取。
 
#[赞！]接口单元测试
不能被测试的代码，不是好代码。 
  
在使用此框架进行接口开发时，我们强烈建议使用测试驱动开发，以便于不断积累形成接口测试体系，保证接口向前向后兼容。  
例如对接口 **/?service=User.GetBaseInfo&userId=1** 进行单元测试时，按： **构造-操作-检验（BUILD-OPERATE-CHECK）模式** ，即：  

```
    /**
     * @group testGetBaseInfo
     */ 
    public function testGetBaseInfo()
    {
        $str = 'service=User.GetBaseInfo&userId=1';
        parse_str($str, $params);

        DI()->request = new PhalApi_Request($params);

        $api = new Api_User(); 
        //自己进行初始化
        $api->init();
        $rs = $api->getBaseInfo();

        $this->assertNotEmpty($rs);
        $this->assertArrayHasKey('code', $rs);
        $this->assertArrayHasKey('msg', $rs);
        $this->assertArrayHasKey('info', $rs);

        $this->assertEquals(0, $rs['code']);

        $this->assertEquals('dogstar', $rs['info']['name']);
        $this->assertEquals('oschina', $rs['info']['from']);
    }
```
运行效果：  
 ![运行效果](http://static.oschina.net/uploads/space/2015/0204/234130_GSJ6_256338.png)  
   
对于框架的核心代码，我们也一直坚持着单元测试，其核心框架代码的单元测试覆盖率可高达96%以上。
  
#主要目录结构
```
.
│
├── PhalApi         //PhalApi框架，后期可以整包升级
│
│
├── Public          //对外访问目录，建议隐藏PHP实现
│   └── demo        //Demo服务访问入口
│
│
├── Config          //项目接口公共配置，主要有：app.php, sys.php, dbs.php
├── Data            //项目接口公共数据
├── Language        //项目接口公共翻译
├── Runtime         //项目接口运行文件目录，用于存放日记，可软链到别的区
│
│
└── Demo            //应用接口服务，名称自取，可多组
    ├── Api             //接口响应层
    ├── Domain          //接口领域层
    ├── Model           //接口持久层
    └── Tests           //接口单元测试

```

#加入我们
显然，这只是一个开始，我们要走的路还很长。这些也不是一个人可以完成的。即使可以，也需要很长一段时间。  
  
在一个人还年轻的时候，我觉得，就应该着手致力做一些对社会有意义的事情，因此我选择了开源。  
  
同样，如果能够有机会和你一起为之努力，将会是我的荣幸，也是一段令值得兴奋激动的。SO？如果你对此深感兴趣、有激情和时间，请联系我，邮箱一如既往是：chanzonghuang@gmail.com，或者开源中国站内留言，欢迎加入，谢谢！  
  
除此之外，你也可以通过其他的方式来支持我们。一如：关注和认可，因为在开源的路上，您每一次真心的关注和肯定都是我们前进的最大动力！谢谢！
  
#更新日记
以下更新日记，主要是为了说明，我们一直在努力更新和维护。
###2015-03-27
```
1、代码注释完善与在线类参考手册生成：http://www.phalapi.net/docs/
```
###2015-03-21
```
1、一些bugfixed：规则下标开头大小写问题、文件缓存个数问题预防等
2、扩展类库：七牛云存储接口调用
3、文档整理和编写新的文档
```
###2015-03-15
```
1、增加对RSA加密的支持，以及超长字符串的解决方案
2、文件缓存目录拆分，以支持海量的文件缓存
3、官网再上线：www.phalapi.net
```
###2015-02-26
```
1、对之前的模型代码、查询类、多级缓存等补充完善单元测试
2、WIKI文档补充UML图示
3、Release 1.1.2 发布
```
###2015-02-24
```
1、文件缓存、空缓存及多级缓存的引入
2、表数据入口的Model基类，以供NotORM系列子类使用，关键点为主键映射和分表情况
3、结合多级缓存和广义Model，对高成本的数据获取的应对方案
4、WIKI文档补充
```
###2015-02-15
```
1、年前的更新：三篇文档的编写，到此基础入门只差多级缓存未完成；
```
###2015-02-13
```
1、代码小酌，重构代码、整理注释，让代码更明了，更统一，更达意；
2、添加工具类，其中有：IP地址获取、随机字符串生成；
3、添加扩展类库：微信开发，并编写相关使用文档；
4、添加扩展类库：phprpc协议支持及WIKI编写；
```
###2015-02-11
```
1、官网发布上线：http://112.74.107.125/，域名已申请，待绑定：www.phalapi.net；
```
###2015-02-09
```
1、将原来限制于JSON格式的返回调整成更灵活的组件形式，以便支持JSON、JSONP和测试环境下的格式返回，和扩展项目开发所需要的格式；
2、接口基类的初始化函数PhalApi_Api::initialize()精简名字为init()；
3、全部核心的代码注释中的author追加个人邮箱地址；
4、单元测试完善补充和文档整理；
```
###2015-02-07
```
1、完善接口调试下SQL的输出、示例和单元测试，以及WIKI文档的编写；
2、日记接口文档的编写；
3、合并master到release，并将版本号更新到1.1.1；
```
###2015-02-04
```
1、根据质量分析后Sonar提供的报告，整理代码，包括统一的注释、对齐、代码风格、命名规则等；
2、默认服务的注册，有：DI()->request、DI()->response；
```
###2015-02-02    版本1.1.0    一个全新的开始！
```
此版本在原来的基础上进行了大量的重构和更多的规范统一，主要有：
1、分离框架代码和项目代码，便于以后框架升级；
2、统一全部的入口文件，以便不同的版本、终端、入口和测试环境使用，并隐藏PHP语言实现；
3、框架代码统一从原来的Core_改名为PhalApi_，并且把PhalApi_DI::one()统一为快速函数的写法：DI()；
4、重新界定应用项目的代码目录结构，以包的形式快速开发；
5、全部文档相应更新；
//注意！此版本不兼容旧的写法，如有问题，请与我联系。
```
###2015-02-01
```
1、正常时，ret返回调整为：200，原来为0；
2、异常统一简化为两大类：客户端非法请求400、服务端运行错误500；
3、日记文件写入重构，并将权限更改为777，以便不同场合下日记写入时的permission denied；
4、单元测试整理；
```
###2015-01-31
```
1、参数规则的解析，移除不必要的固定类型，以及addslashes转换，单元测试整理；
2、参数规则文档编写：http://my.oschina.net/u/256338/blog/372947
```
###2015-01-29
```
1、examples代码重新整理，及入门文档同步更新；
2、入口文件的调整；
```

###2015-01-28
```
1、补充入门开发示例的文档，及相关的测试代码和产品代码，主要是examples；
2、提供接口参数在线查询工具；
```
###2015-01-24
```
1、PhalApi开源；
```