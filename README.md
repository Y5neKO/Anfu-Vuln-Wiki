# Anfu-Vlun-Wiki
作为一名安服/渗透仔（悲），难免会经常跟各种报告打交道，在写报告的时候经常花费大量时间在各种繁琐的漏洞描述、危害、修复建议上，每次遇到同样的漏洞又得再重复一遍，非常影响~~**摸鱼**~~效率；因此出现了这个Wiki，用以记录一些经常遇到的漏洞，以及其漏洞描述、漏洞危害、修复建议等。

本Wiki中的各种描述来源均来自以下但不限于：~~个人瞎掰~~、网络文章、AI辅助描述；可能存在误差，因此内容仅供参考，写报告的时候尽量多对一下描述是否符合再写进去。

也欢迎各位师傅提Pull，添加、完善或纠正漏洞描述。

## 漏洞清单

| 漏洞名称          | 漏洞描述                                                     | 漏洞危害                                                     | 修复建议                                                     |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 内网地址泄露      | 网站的内部IP地址，常常被攻击者通过信息收集，得到其内网的IP地址，对于渗透攻击，打下良好基础，如内网Ip地址段，IP路由等等。 |                                                              | 1、	删除js文件中的内网地址。                              |
| 绝对路径泄露      | 应用中泄露出应用在主机中的绝对地址路径，攻击者可通过获取网站物理路径，为下一步攻击做准备。 |                                                              | 1、	媒体链接和超链接采用相对路径的表达方式；     2、	报错信息中不对外输出网站物理路径等敏感信息。 |
| sourceMap源码泄露 | webpack是一个JavaScript应用程序的静态资源打包器。它构建一个依赖关系图，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个bundle。大部分Vue等项目应用会使用webpack进行打包，使用webpack打包应用程序会在网站js同目录下生成  js.map文件。将sourceMap文件暴露给公共网络时。sourceMap文件提供了一种将压缩的、混淆的代码映射回原始源代码的方法，这有助于开发者在生产环境中调试问题。 | 通过这样的源码，可以非常清晰地了解应用的前端业务，包括接口信息，如果前端包含加解密的逻辑的话，这样也非常有利于攻击者进行破解。 | 1、	在scripts/build下的build.js  文件中添加如下配置再重新打包：process.env.GENERATE_SOURCEMAP = 'false';     2、	删除所有.map文件。 |
| 敏感信息泄露      | 由于网站配置原因，存放敏感信息的文件被泄露或由于网站运行出错导致敏感信息泄露。 | 攻击者可通过泄露的数据获取服务器的相关，能配合其他漏洞发动进一步攻击，例如猜解系统账号、造成数据泄露风险。 | 1、删除该示例页面或对页面进行鉴权。                          |
| 链接注入          | 链接注入是将某个URL嵌入到被攻击的网站上，进而修改站点页面。被嵌入的URL包含恶意代码，可能窃取正常用户的用户名、密码，也可能窃取或操纵认证会话，以合法用户的身份执行相关操作。 | [1]获取其他用户Cookie中的敏感数据。  [2]屏蔽页面特定信息。 [3]伪造页面信息。 [4]拒绝服务攻击。 [5]突破外网内网不同安全设置。 | 过滤客户端提交的危险字符，客户端提交方式包含GET、POST、COOKIE、User-Agent、Referer、Accept-Language等，其中危险字符如下：  [1] \| [2] & [3] ; [4] $ [5] % [6] @ [7] ' [8] " [9] [10] () [11] +  [12] CR [13] LF [14] , [15] . [16] script [17] document [18] eval 开发语言的建议：  [1]严格控制输入： Asp:request Php:$_GET、$_POST、$_COOKIE、$_SERVER  Jsp:request.getParameter、request.getCookies  Asp.net:Request.QueryString、Form、Cookies、SeverVaiables  客户端提交的变量一般从以上函数获得，严格限制提交的数据长度、类型、字符集。 [2]严格控制输出：  HtmlEncode：对一段指定的字符串应用HTML编码。 UrlEncode：对一段指定的字符串URL编码。 XmlEncode：将在XML中使用的输入字符串编码。  XmlAttributeEncode：将在XML属性中使用的输入字符串编码。 escape：函数可对字符串进行编码。  decodeURIComponent：返回统一资源标识符的一个已编码组件的非编码形式。 encodeURI：将文本字符串编码为一个有效的统一资源标识符  (URI)。 |
| XSS漏洞           | XSS漏洞（Cross-Site  Scripting）是一种常见的Web应用程序安全漏洞，它允许攻击者在受害者的浏览器中注入恶意脚本。 | 当受害者访问包含恶意脚本的网页时，攻击者就可以利用该漏洞来执行任意的代码，例如窃取用户的敏感信息、修改网页内容或进行其他恶意活动。 | 1、输入验证和过滤：对用户输入的数据进行有效的验证和过滤，以确保不会包含恶意的脚本代码。可以使用正则表达式、编码函数或专门的输入过滤器来实现。      2、输出编码：在将用户输入的数据显示在网页上之前，对其进行适当的输出编码，以防止浏览器将其解析为脚本代码。可以使用HTML实体编码、URL编码或JavaScript编码等方法。      3、设置HTTP头：在网页中设置Content-Security-Policy（CSP）头，限制页面中可以加载和执行的资源，包括脚本文件、样式表和图片等。这可以防止恶意脚本被加载和执行。      4、使用Web应用防火墙（WAF）：部署WAF可以对传入的请求进行检测和过滤，以防止XSS攻击。WAF可以根据已知的攻击模式和签名来识别和阻止恶意请求。      5、设置cookie的HttpOnly属性：将cookie的HttpOnly属性设置为true，可以防止通过JavaScript访问和窃取cookie信息，从而减少XSS攻击的可能性。 |
| swagger-ui泄露    | Swagger  是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web  服务。总体目标是使客户端和文件系统作为服务器以同样的速度来更新。相关的方法，参数和模型紧密集成到服务器端的代码，允许API来始终保持同步。 | Swagger生成的API文档，是直接暴露在相关web路径下的。所有人均可以访问查看。通过这一点即可获取项目上所有的接口信息。那么结合实际业务，例如如果有文件读取相关的接口，可能存在任意文件下载，相关的业务访问可能存在未授权访问等。 | 1、在生产节点禁用Swagger2，在maven中禁用所有关于Swagger包（不建议）      2、结合SpringSecurity/shiro进行认证授权，将Swagger-UI的URLs加入到各自的认证和授权过滤链中，当用户访问Swagger对应的资源时，只有通过认证授权的用户才能进行访问。 |
