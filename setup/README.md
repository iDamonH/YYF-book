YYF 运行环境
============

YYF 会根据不同环境切换运行方式。

开发环境最大程度的方便调试和提高开发效率，和提供简单统一环境。

生产环境，保证安全性和尽可能高的性能(执行效率)。

* [开发环境](develop.md)(甚至你的电脑上不用需要PHP环境)
* [生产环境](yyf-in-server.md)(可一键部署)


基本环境
------

### 1. 必要基础环境
YYF 是基于`YAF`扩展的`PHP`框架，所以这两点是必须的
* 【必需】PHP (版本>=5.3) 
* 【必需】[YAF扩展](http://pecl.php.net/package/yaf)
* 【可选】mcrypt扩展(使用加密相关库需要)
* 【可选】PDO(使用数据库连接需要)
* 【可选】CURL(使用第三方接口需要)


### 2. 数据库和服务器支持

- 数据库支持:(PDO封装,支持大部分关系型数据库)
    * MaraiaDB: 完全封装
    * MySQL：   完全封装
    * SQLite:   常用支持
    * 其他数据库:    未知

- 服务器支持：(无限制)
    * Apache
    * Nginx
    * IIS
    * 其他


### 3. 生产环境服务器部署支持
YYF对库进行轻量级的封装，可以在不同平台和服务器之间平滑迁移，而不必修改代码。

- 云服务器(虚拟机) 完全支持
    * Linux 服务器
    * Windows 服务器

- 云引擎(PAAS)支持
    * SAE(新浪云): 完全支持
    * BAE(百度云): 未测试