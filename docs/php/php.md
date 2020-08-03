
https:https://

www.jianshu.com/p/0a79847c8151

https:https://

hqidi.com/150.html

./configure --prefix=/usr/local/php7.4.8 --enable-opcache --with-config-file-path=/usr/local/php7.4.8/etc --with-curl --enable-fpm --enable-gd --with-iconv --enable-mbstring --with-mysqli=mysqlnd --with-openssl --enable-static --enable-sockets --enable-inline-optimization --with-zlib --disable-ipv6 --disable-fileinfo --disable-debug


./configure --prefix=/usr/local/php7.4.8  --with-config-file-path=/usr/local/php7.4.8/etc  --with-pdo-mysql



./configure --prefix=/usr/local/php7.4.8  --with-config-file-path=/usr/local/php7.4.8/etc  --with-freetype-dir  --with-gd  --with-gettext  --with-iconv-dir  --with-kerberos  --with-libdir=lib64  --with-libxml-dir  --with-mysqli  --with-openssl  --with-pcre-regex  --with-pdo-mysql  --with-pdo-sqlite  --with-pear  --with-png-dir  --with-xmlrpc  --with-xsl  --with-zlib  --enable-fpm  --enable-bcmath  --enable-libxml  --enable-inline-optimization  --enable-mbregex  --enable-mbstring  --enable-opcache  --enable-pcntl  --enable-shmop  --enable-soap  --enable-sockets  --enable-sysvsem  --enable-xml  --enable-zip --with-curl




--prefix=/usr/local/php                      https://

指定 php 安装目录 
--with-apxs2=/usr/local/apache/bin/apxs      https://

整合apache，
                        https://

apxs功能是使用mod_so中的LoadModule指令，
                       https://

加载指定模块到 apache，要求 apache 要打开SO模块
--with-config-file-path=/usr/local/php/etc    https://

指定php.ini位置
--with-MySQL=/usr/local/mysql                 https://

mysql安装目录，对mysql的支持
--with-mysqli=/usr/local/mysql/bin/mysql_config
                      https://

mysqli扩展技术不仅可以调用MySQL的存储过程、处理MySQL事务，
                      https://

而且还可以使访问数据库工作变得更加稳定。
--enable-safe-mode    https://

打开安全模式 
--enable-ftp          https://

打开ftp的支持 
--enable-zip          https://

打开对zip的支持 

--with-bz2            https://

打开对bz2文件的支持 
--with-jpeg-dir       https://

打开对jpeg图片的支持 
--with-png-dir        https://

打开对png图片的支持 
--with-freetype-dir   https://

打开对freetype字体库的支持 
--without-iconv       https://

关闭iconv函数，各种字符集间的转换 
--with-libXML-dir     https://

打开libxml2库的支持 
--with-XMLrpc         https://

打开xml-rpc的c语言 
--with-zlib-dir       https://

打开zlib库的支持 
--with-gd             https://

打开gd库的支持 
--enable-gd-native-ttf https://

支持TrueType字符串函数库 
--with-curl            https://

打开curl浏览工具的支持 
--with-curlwrappers    https://

运用curl工具打开url流 
--with-ttf             https://

打开freetype1.*的支持，可以不加了 
--with-xsl             https://

打开XSLT 文件支持，扩展了libXML2库 ，需要libxslt软件 
--with-gettext         https://

打开gnu 的gettext 支持，编码库用到 
--with-pear            https://

打开pear命令的支持，PHP扩展用的 
--enable-calendar      https://

打开日历扩展功能 
--enable-mbstring      https://

多字节，字符串的支持 
--enable-bcmath        https://

打开图片大小调整,用到zabbix监控的时候用到了这个模块
--enable-sockets       https://

打开 sockets 支持
--enable-exif          https://

图片的元数据支持 
--enable-magic-quotes  https://

魔术引用的支持 
--disable-rpath        https://

关闭额外的运行库文件 
--disable-debug        https://

关闭调试模式 
--with-mime-magic=/usr/share/file/magic.mime  https://

魔术头文件位置


## 查看不带注释命令

```shell
grep -Ev "^$|[#;]" server.conf

```