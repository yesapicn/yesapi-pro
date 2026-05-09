# 如何编写开发手册

开发手册基于docsify，采用简洁的Markdown格式编写。  

其目录结构如下：  

 + 开发手册入口文件：```./iki/index.html```
 + 封面文件：```./public/_coverpage.md``` 
 + 顶部菜单：```./public/_navbar.md```
 + 左侧菜单配置：```./public/_sidebar.md```
 + 手册首页：```./public/README.md```

更多请继续阅读下一节：[文档编写规范](/guide.md)。

为了更好的访问体验，让用户能实时查看最新更新的文档，推荐增加以下Nginx配置，禁止文档缓存：  
```nginx
    location ~.*\.md$ {
            add_header Cache-Control "no-cache, no-store, must-revalidate";
            add_header Expires 0;
            add_header Pragma no-cache;
    }
```

如果不希望外网可以访问此开发手册，可以在正式环境禁止访问wiki目录，参考Nginx配置：  
```nginx
	# 禁止访问开发手册
	location ^~ /wiki/ {
	  deny all;
	}
```

然后改为访问内网测试环境。  
