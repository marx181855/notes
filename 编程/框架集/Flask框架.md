# 什么是Flask框架：

   Flask是一个轻量级的可定制框架，使用Python语言编写，较其他同类型框架更为灵活、轻便、安全且容易上手。它可以很好地结合MVC模式进行开发，开发人员分工合作，小型团队在短时间内就可以完成功能丰富的中小型网站或Web服务的实现。另外，Flask还有很强的定制性，用户可以根据自己的需求来添加相应的功能，在保持核心功能简单的同时实现功能的丰富与扩展，其强大的插件库可以让用户实现个性化的网站定制，开发出功能强大的网站。

Flask是目前十分流行的web框架，采用Python编程语言来实现相关功能。它被称为微框架(microframework)，“微”并不是意味着把整个Web应用放入到一个Python文件，微框架中的“微”是指Flask旨在保持代码简洁且易于扩展，Flask框架的主要特征是核心构成比较简单，但具有很强的扩展性和兼容性，程序员可以使用Python语言快速实现一个网站或Web服务。
一般情况下，它不会指定数据库和模板引擎等对象，用户可以根据需要自己选择各种数据库。Flask自身不会提供表单验证功能，在项目实施过程中可以自由配置，从而为应用程序开发提供数据库抽象层基础组件，支持进行表单数据合法性验证、文件上传处理、用户身份认证和数据库集成等功能。

Flask主要包括Werkzeug和Jinja2两个核心函数库，它们分别负责业务处理和安全方面的功能，这些基础函数为web项目开发过程提供了丰富的基础组件。Werkzeug库十分强大，功能比较完善，支持URL路由请求集成，一次可以响应多个用户的访问请求；支持Cookie和会话管理，通过身份缓存数据建立长久连接关系，并提高用户访问速度；支持交互式Javascript调试，提高用户体验；可以处理HTTP基本事务，快速响应客户端推送过来的访问请求。

Jmja2库支持自动HTML转移功能，能够很好控制外部黑客的脚本攻击。系统运行速度很快，页面加载过程会将源码进行编译形成python字节码，从而实现模板的高效运行；模板继承机制可以对模板内容进行修改和维护，为不同需求的用户提供相应的模板。目前Python的web框架有很多。除了Flask，还有django、Web2py等等。其中Diango是目前Python的框架中使用度最高的。但是Django如同java的EJB(EnterpriseJavaBeansJavaEE服务器端组件模型)多被用于大型网站的开发，但对于大多数的小型网站的开发，使用SSH(Struts+Spring+Hibemat的一个JavaEE集成框架)就可以满足，和其他的轻量级框架相比较，Flask框架有很好的扩展性，这是其他Web框架不可替代的。

其他请参考文档 url：http://docs.jinkan.org/docs/flask/