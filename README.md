lua版Minify，合并CSS、JS文件  
=======
1、兼容minify的 f 参数  
2、可以将图片路径，替换为域名的方式   
3、清除CSS的注释、换行等  
4、一键删除服务器的所有缓存文件  
5、错误日志记录

**[安装/配置]**  
一、安装lua posix库:  
```bash
# wget http://git.alpinelinux.org/cgit/luaposix/snapshot/luaposix-5.1.8.tar.bz2
# tar jxvf luaposix-5.1.8.tar.bz2
# cd luaposix-5.1.8
# vi Makefile
  LUAINC=         $(PREFIX)/include/luajit-2.0
# make CC=gcc
# make install CC=gcc
```

二、配置nginx：  
1、将combo目录，放至/usr/local/nginx/conf/目录  
2、修改配置文件：  
```bash
 http {
    location = / {
        set $cache_dir "/dev/shm/combo/";
        set $css_trim "on";
        set $admin_ip "192.168.8.63,192.168.8.181";
        content_by_lua_file /usr/local/nginx/conf/combo/combo.lua;
    }
 }
```
三、访问URL：  
http://x.x.x.x/?f=static/index/header.css,static/index/footer.css

四、常用功能  
1、自定义CSS、JS目录（默认在当前目录）
```bash
set $css_dir "include/css/";
set $js_dir  "include/javascript/";
```
2、多个图片路径替换：  
采用 | 分隔，替换多个路径：  
```bash
set $css_replace "../../../images,http://images.xxx.com|../../../ck,http://images.xxx.com";
```
3、开启清除CSS注释功能：
```bash
set $css_trim "on";
```
4、根据CSS目录，自动替换图片相对路径：
```bash
set $css_path_auto "images/";
```
5、删除服务器的所有缓存文件（慎用）：  
配置$admin_ip：
```bash
set $admin_ip "192.168.8.63,192.168.8.181";  
```
链接增加&c=1即可，页面内容显示sucess即成功：  
http://x.x.x.x/?f=static/index/header.css,static/index/footer.css&c=1

五、感谢：  
感谢Nginx http://nginx.org  
感谢春哥  http://www.weibo.com/agentzh