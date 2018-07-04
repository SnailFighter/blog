## Beginner’s Guide

## 入门指导


This guide gives a basic introduction to nginx and describes some simple tasks that can be done with it. It is supposed that nginx is already installed on the reader’s machine. If it is not, see the Installing nginx page. This guide describes how to start and stop nginx, and reload its configuration, explains the structure of the configuration file and describes how to set up nginx to serve out static content, how to configure nginx as a proxy server, and how to connect it with a FastCGI application.

nginx has one master process and several worker processes. The main purpose of the master process is to read and evaluate configuration, and maintain worker processes. Worker processes do actual processing of requests. nginx employs event-based model and OS-dependent mechanisms to efficiently distribute requests among worker processes. The number of worker processes is defined in the configuration file and may be fixed for a given configuration or automatically adjusted to the number of available CPU cores (see worker_processes).

The way nginx and its modules work is determined in the configuration file. By default, the configuration file is named nginx.conf and placed in the directory /usr/local/nginx/conf, /etc/nginx, or /usr/local/etc/nginx.  

该指导简单介绍了ngnix和描述了一些nginx可以做的简单任务。再此假设读者机器上已经安装了nginx。如果没有，请看[安装nginx](http://nginx.org/en/docs/install.html)页面介绍。该指导描述了如何开启，停止nginx，怎么重新加载配置文件，解释了配置文件的结构，介绍了如何建立nginx对外发布静态内容，如何配置nginx来作为代理服务器，怎么通过使用FastCGI应用连接它。  

nginx有一个主进程（下文中统称master进程）和多个工作进程（下文中统称为worker进程）。master进程的目的是读取，评估配置文件，维护worker进程。worker进程通常处理请求。nginx采用基于事件驱动模型和依赖于操作系统的机制来有效地在worker进程中分发请求。worker进程数量定义在配置文件中，可以修改为指定的配置或者自动适配为可用的cup核心数量（[worker进程](http://nginx.org/en/docs/ngx_core_module.html#worker_processes)）  

nginx和它的各个模块的工作方式定义在配置文件中。默认情况下，配置文件命名为nginx.conf，放在/usr/local/nginx/conf，/etc/nginx或者/usr/local/etc/nginx目录下。

#### Starting, Stopping, and Reloading Configuration
#### 开启，停止，重载配置
To start nginx, run the executable file. Once nginx is started, it can be controlled by invoking the executable with the -s parameter. Use the following syntax:

```
nginx -s signal
```
Where signal may be one of the following:
```
stop — fast shutdown
quit — graceful shutdown
reload — reloading the configuration file
reopen — reopening the log files
```
For example, to stop nginx processes with waiting for the worker processes to finish serving current requests, the following command can be executed:
```
nginx -s quit
```
This command should be executed under the same user that started nginx.
Changes made in the configuration file will not be applied until the command to reload configuration is sent to nginx or it is restarted. To reload configuration, execute:
```
nginx -s reload
```  
为了开启nginx，可以运行可执行文件。一旦nginx启动，可以通过使用-s参数调用可执行文件来控制它。使用下面的语法：  
```
nginx -s signal
``` 
信号量可以是下面的任意一种：
```
stop - 快速停止
quit - 优雅地停止
reload - 重载配置文件
reopen - 重新打开日志文件
```

例如，为了停止正在等待worker进程完成当前请求的nginx进程，可以执行以下命令：
```
nginx -s quit
```
此命令应该被开启nginx的用户执行。
在配置文件中的修改不会被应用直到重新载入配置的命令被发送到nginx或者被重启。为了重载配置，执行：
```
nginx -s reload
```


Once the master process receives the signal to reload configuration, it checks the syntax validity of the new configuration file and tries to apply the configuration provided in it. If this is a success, the master process starts new worker processes and sends messages to old worker processes, requesting them to shut down. Otherwise, the master process rolls back the changes and continues to work with the old configuration. Old worker processes, receiving a command to shut down, stop accepting new connections and continue to service current requests until all such requests are serviced. After that, the old worker processes exit.

A signal may also be sent to nginx processes with the help of Unix tools such as the kill utility. In this case a signal is sent directly to a process with a given process ID. The process ID of the nginx master process is written, by default, to the nginx.pid in the directory /usr/local/nginx/logs or /var/run. For example, if the master process ID is 1628, to send the QUIT signal resulting in nginx’s graceful shutdown, execute:
```
kill -s QUIT 1628
```
For getting the list of all running nginx processes, the ps utility may be used, for example, in the following way:
```
ps -ax | grep nginx
```
For more information on sending signals to nginx, see Controlling nginx.

一旦主进程接受到重载配置的信号，它就检查新配置文件的正确性，试着使用里面提供的配置。如果成功，master进程开启新的worker进程并且发送信息给旧的worker进程，请求他们关闭。否则，master进程回滚到配置改变前的状态继续使用旧的配置工作。旧的worker进程接收到一个关闭的命令，则停止接受新的连接并且继续服务当前请求直到所有的请求都被完成。至此，旧worker进程退出。

一个信号也可以被发送到nginx进程在Unix工具的帮助下，例如kill工具。在这个例子中一个信号直接发送到一个指定进程id的进程。nginx主进程的进程id默认被写在nginx.pid文件中，在/usr/local/nginx/logs或者/var/run目录下。例如，如果一个master进程id是1628，发送QUIT信号可以使nginx优雅地关闭。执行：
```
kill -s QUIT 1628
``` 

为了获得所有的正在运行的nginx进程清单，ps工具也许是有用的，例如，下面的方法：
```
ps -ax|grep nginx
```  
更多想nginx发送信号的信息，参考[nginx控制](http://nginx.org/en/docs/control.html)


#### Configuration File’s Structure
#### 配置文件结构
nginx consists of modules which are controlled by directives specified in the configuration file. Directives are divided into simple directives and block directives. A simple directive consists of the name and parameters separated by spaces and ends with a semicolon (;). A block directive has the same structure as a simple directive, but instead of the semicolon it ends with a set of additional instructions surrounded by braces ({ and }). If a block directive can have other directives inside braces, it is called a context (examples: events, http, server, and location).

Directives placed in the configuration file outside of any contexts are considered to be in the main context. The events and http directives reside in the main context, server in http, and location in server.

The rest of a line after the # sign is considered a comment.

nginx由在配置文件中指令控制的模块组成。指令可以分为简单指令和块指令。一个简单的指令包含名称，被空格分割的参数，使用分号（;）结束。块指令有与简单指令相同的结构，但是不同于分号，它使用一组额外的结构包（{}）围着。如果块指令在括号内可以有其他指令，则它可称为上下文（示例：事件、http、服务器和位置）。  

配置文件中的不再任何上下文中的指令被认为是在主上下文中。events和http指令被放置在主上下文中，server在http指令中，而location又在server指令中。

剩下的在#符号后的是注释。

#### Serving Static Content
#### 静态内容服务
An important web server task is serving out files (such as images or static HTML pages). You will implement an example where, depending on the request, files will be served from different local directories: /data/www (which may contain HTML files) and /data/images (containing images). This will require editing of the configuration file and setting up of a server block inside the http block with two location blocks.



First, create the /data/www directory and put an index.html file with any text content into it and create the /data/images directory and place some images in it.

Next, open the configuration file. The default configuration file already includes several examples of the server block, mostly commented out. For now comment out all such blocks and start a new server block:
```
http {
    server {
    }
}
```  

一个重要的web服务器的任务是向外提供文件（例如图片或者HTML页面）。你可以实现一个例子，在这个例子中文件可以根据请求来自不同的本地目录：/data/www (也许包含html文件) ，/data/images (包含图片)。这需要在编辑配置文件，并且在带有两个location块中http块中建立一个服务块。

首先创建一个/data/www 目录，然后放置一个有任意内容的index.html文件，创建/data/images目录然后在其中放一些图片。

接下来，打开配置文件。默认配置文件已经包含了几个服务块案例，大多数被注释掉了。现在注释调所有的这样的块，开始一个新的服务块。
```
http{
    server {
        
    }
    
}
```

Generally, the configuration file may include several server blocks distinguished by ports on which they listen to and by server names. Once nginx decides which server processes a request, it tests the URI specified in the request’s header against the parameters of the location directives defined inside the server block.

Add the following location block to the server block:
```
location / {
    root /data/www;
}
```
通常，配置文件包含几个服务块，它们由它们监听的端口号和服务名称区分起来。一旦nginx决定那个服务处理请求，它测试在请求头中的URI和在服务块中的location指令中的参数。
添加下面的location块到服务块中
```
location / {
    root /data/www;
}
```

This location block specifies the “/” prefix compared with the URI from the request. For matching requests, the URI will be added to the path specified in the root directive, that is, to /data/www, to form the path to the requested file on the local file system. If there are several matching location blocks nginx selects the one with the longest prefix. The location block above provides the shortest prefix, of length one, and so only if all other location blocks fail to provide a match, this block will be used.

Next, add the second location block:



```
location /images/ {
    root /data;
}
```  

location 块指定了“/” 前缀与请求宗的URI比较。为了匹配请求，URI被添加制定的root指令后，就是，/dta/www，从而构成在本地文件系统中的请求文件的路径。如果有几个匹配的位置块，nginx选择前缀最长的一个。上面的位置块提供了长度为1的最短前缀，因此只有当所有其他位置块都不能提供匹配时，才会使用这个块。

接下来，是第二个location块：
```
location /images/ {
    root /data;
}
```  


It will be a match for requests starting with /images/ (location / also matches such requests, but has shorter prefix).

The resulting configuration of the server block should look like this:
```
server {
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
}
```  
它将匹配/images/的请求（/ 也可以匹配这样的请求，但是是比较短的前缀）。

最后服务块配置应该是这样：
```
server {
    location / {
        root /data/www;
    }
    
    location /images/ {
        root /data;
    }
}
```
This is already a working configuration of a server that listens on the standard port 80 and is accessible on the local machine at http://localhost/. In response to requests with URIs starting with /images/, the server will send files from the /data/images directory. For example, in response to the http://localhost/images/example.png request nginx will send the /data/images/example.png file. If such file does not exist, nginx will send a response indicating the 404 error. Requests with URIs not starting with /images/ will be mapped onto the /data/www directory. For example, in response to the http://localhost/some/example.html request nginx will send the /data/www/some/example.html file.

To apply the new configuration, start nginx if it is not yet started or send the reload signal to the nginx’s master process, by executing:
```
nginx -s reload
```  
一个在使用的服务配置监听80端口，通过http://localhost/访问本地。对URIs为/image/请求的响应，服务器将发送来自/data/images目录的文件。例如，对http://localhost/images/example.png的请求的响应，nginx将发送/data/images/example.png文件。如果不存在该文件，nginx将发送指向404错误的响应。不是/images/路径的请求将映射到/data/www目录。例如，http://localhost/some/example.html的请求nginx将发送/data/www/some/example.html文件。  

为了应用新的配置，如果没有开启nginx则启动，或者发送重载信号给nginx的master进程，通过执行：
```
nginx -s reload
```

In case something does not work as expected, you may try to find out the reason in access.log and error.log files in the directory /usr/local/nginx/logs or /var/log/nginx.

有时nginx并没有按照我们的期望工作，这是你应该试着在access.log和error.log中找到原因,在目录/usr/local/nginx/logs 或者 /var/log/nginx。

### Setting Up a Simple Proxy Server
### 建立简单的代理服务器

One of the frequent uses of nginx is setting it up as a proxy server, which means a server that receives requests, passes them to the proxied servers, retrieves responses from them, and sends them to the clients.

We will configure a basic proxy server, which serves requests of images with files from the local directory and sends all other requests to a proxied server. In this example, both servers will be defined on a single nginx instance.

First, define the proxied server by adding one more server block to the nginx’s configuration file with the following contents:
```
server {
    listen 8080;
    root /data/up1;

    location / {
    }
}
```
nginx被频繁使用的之一就是作为代理服务器，接受请求，再发送请求到代理的服务器中。接受响应，再发送给客户端。   

我们将配置一个基本的代理服务器，它使用本地目录中的文件来处理图像请求，并将所有其他请求发送到一个代理服务器。在这个例子中，两个服务江北定义在同一个nginx实例中。  

首先，通过在nginx配置文件中添加多个服务块定义代理服务器，使用下面的内容：
```
server {
    listen 8080;
    root /data/up1;

    location / {
    }
}
```

This will be a simple server that listens on the port 8080 (previously, the listen directive has not been specified since the standard port 80 was used) and maps all requests to the /data/up1 directory on the local file system. Create this directory and put the index.html file into it. Note that the root directive is placed in the server context. Such root directive is used when the location block selected for serving a request does not include own root directive.

Next, use the server configuration from the previous section and modify it to make it a proxy server configuration. In the first location block, put the proxy_pass directive with the protocol, name and port of the proxied server specified in the parameter (in our case, it is http://localhost:8080):
```
server {
    location / {
        proxy_pass http://localhost:8080;
    }

    location /images/ {
        root /data;
    }
}
```  
这是一个简单的监听8080端口的服务（原来的，listen指令没有被指定因为默认使用的是标准的80端口），映射所有的请求到本地文件系统的/data/up1 目录。创建此目录，并放置一个index.html文件。注意root指令被放在了server上下文中。 当为服务请求而选择的位置块不包含自己的根指令时，将使用这种根指令。  
接下来，使用上面的配置，修改它成为一个代理服务器配置。在第一个location块中，放置带有协议、名称和被代理的服务的端口号的proxy_pass指令（在这个例子中是http://localhost:8080）  
```
server {
    location / {
        proxy_pass http://localhost:8080;
    }

    location /images/ {
        root /data;
    }
}
```

We will modify the second location block, which currently maps requests with the /images/ prefix to the files under the /data/images directory, to make it match the requests of images with typical file extensions. The modified location block looks like this:
```
location ~ \.(gif|jpg|png)$ {
    root /data/images;
}
```
我们修改第二个location块，当前使用/images/前缀映射请求到/data/images目录下
这使它匹配那些通用的文件后缀图片请求。location块修改后如下：
```
location ~ \.(gif|jpg|png)$ {
    root /data/images;
}
```

The parameter is a regular expression matching all URIs ending with .gif, .jpg, or .png. A regular expression should be preceded with ~. The corresponding requests will be mapped to the /data/images directory.

When nginx selects a location block to serve a request it first checks location directives that specify prefixes, remembering location with the longest prefix, and then checks regular expressions. If there is a match with a regular expression, nginx picks this location or, otherwise, it picks the one remembered earlier.

The resulting configuration of a proxy server will look like this:
```
server {
    location / {
        proxy_pass http://localhost:8080/;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```  
这个参数是一个正则表达式来匹配所有的以.gif、.jpg、.png结尾的URI。正则表达式前面应该加上~。相应的请求将映射到/data/images目录。  

当nginx选择一个位置块来服务请求时，它首先检查指定前缀的位置指示，记住使用最长前缀的位置，然后检查正则表达式。如果与正则表达式匹配，则nginx选择这个位置，否则选择前面记住的位置。

这个代理服务器配置如下：

```
server {
    location / {
        proxy_pass http://localhost:8080/;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```  

This server will filter requests ending with .gif, .jpg, or .png and map them to the /data/images directory (by adding URI to the root directive’s parameter) and pass all other requests to the proxied server configured above.

To apply new configuration, send the reload signal to nginx as described in the previous sections.

There are many more directives that may be used to further configure a proxy connection.

服务器将过滤以.gif .jpg .png结尾的请求，映射到/data/images目录（通过添加URI到root指令参数），传递所有的其他请求到上面配置的被代理的服务器。

为了使用新的配置，像上面描述的那样发送重载信号给nginx。
 
有更多的指令可以进一步配置代理连接。

#### Setting Up FastCGI Proxying
#### 建立FastCGI 代理
nginx can be used to route requests to FastCGI servers which run applications built with various frameworks and programming languages such as PHP.

The most basic nginx configuration to work with a FastCGI server includes using the fastcgi_pass directive instead of the proxy_pass directive, and fastcgi_param directives to set parameters passed to a FastCGI server. Suppose the FastCGI server is accessible on localhost:9000. Taking the proxy configuration from the previous section as a basis, replace the proxy_pass directive with the fastcgi_pass directive and change the parameter to localhost:9000. In PHP, the SCRIPT_FILENAME parameter is used for determining the script name, and the QUERY_STRING parameter is used to pass request parameters. The resulting configuration would be:


nginx可用于将请求路由到FastCGI服务器，这些服务器运行使用各种框架和编程语言(如PHP)构建的应用程序。

使用FastCGI服务器的最基本的nginx配置包括使用fastcgi_pass指令而不是proxy_pass指令，以及使用fastcgi_param指令设置传递给FastCGI服务器的参数。假设在本地主机上可以访问FastCGI服务器:9000。以前一节中的代理配置为基础，将proxy_pass指令替换为fastcgi_pass指令，并将参数更改为localhost:9000。在PHP中，SCRIPT_FILENAME参数用于确定脚本名称，QUERY_STRING参数用于传递请求参数。产生的配置将是:

```
server {
    location / {
        fastcgi_pass  localhost:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param QUERY_STRING    $query_string;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```
This will set up a server that will route all requests except for requests for static images to the proxied server operating on localhost:9000 through the FastCGI protocol.  

这将设置一个服务器，该服务器将把所有请求(静态图像请求除外)路由到运行在localhost:9000上的代理服务器，通过FastCGI协议。
