Update yum and install required libraries

``` sh
# yum upgrade -y
# yum install -y wget
```

Get remi repository

- [English : Repository Configuration - Remi's RPM repository - Blog](https://blog.remirepo.net/pages/Config-en)

``` sh
# wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
# wget https://rpms.remirepo.net/enterprise/remi-release-7.rpm
# rpm -Uvh remi-release-7.rpm epel-release-latest-7.noarch.rpm
```

check current php info via yum

``` sh
# yum info php
```

``` text
>   Loaded plugins: fastestmirror, ovl
>   Loading mirror speeds from cached hostfile
>   epel/x86_64/metalink                                                                                                     | 8.4 kB  00:00:00
>   * base: ty1.mirror.newmediaexpress.com
>   * epel: ftp.riken.jp
>   * extras: ty1.mirror.newmediaexpress.com
>   * remi-safe: ftp.riken.jp
>   * updates: ty1.mirror.newmediaexpress.com
>   epel                                                                                                                     | 4.7 kB  00:00:00
>   remi-safe                                                                                                                | 3.0 kB  00:00:00
>   (1/4): epel/x86_64/group_gz                                                                                              |  95 kB  00:00:00
>   (2/4): epel/x86_64/updateinfo                                                                                            | 1.0 MB  00:00:00
>   (3/4): epel/x86_64/primary_db                                                                                            | 6.8 MB  00:00:01
>   (4/4): remi-safe/primary_db                                                                                              | 1.7 MB  00:00:01
>   Available Packages
>   Name        : php
>   Arch        : x86_64
>   Version     : 5.4.16
>   Release     : 48.el7
>   Size        : 1.4 M
>   Repo        : base/7/x86_64
>   Summary     : PHP scripting language for creating dynamic web sites
>   URL         : http://www.php.net/
>   License     : PHP and Zend and BSD
>   Description : PHP is an HTML-embedded scripting language. PHP attempts to make it
>               : easy for developers to write dynamically generated web pages. PHP also
>               : offers built-in database integration for several commercial and
>               : non-commercial database management systems, so writing a
>               : database-enabled webpage with PHP is fairly simple. The most common
>               : use of PHP coding is probably as a replacement for CGI scripts.
>               :
>               : The php package contains the module (often referred to as mod_php)
>               : which adds support for the PHP language to Apache HTTP Server.
```

check current php-intl info via yum

``` sh
# yum info php-intl
```

``` text
>   Loaded plugins: fastestmirror, ovl
>   Determining fastest mirrors
>   * base: ftp.iij.ad.jp
>   * extras: ftp.iij.ad.jp
>   * updates: ftp.iij.ad.jp
>   base                                                                                                                     | 3.6 kB  00:00:00
>   extras                                                                                                                   | 2.9 kB  00:00:00
>   updates                                                                                                                  | 2.9 kB  00:00:00
>   (1/4): base/7/x86_64/group_gz                                                                                            | 153 kB  00:00:00
>   (2/4): extras/7/x86_64/primary_db                                                                                        | 194 kB  00:00:00
>   (3/4): updates/7/x86_64/primary_db                                                                                       | 2.9 MB  00:00:01
>   (4/4): base/7/x86_64/primary_db                                                                                          | 6.1 MB  00:00:02
>   Available Packages
>   Name        : php-intl
>   Arch        : x86_64
>   Version     : 5.4.16
>   Release     : 48.el7
>   Size        : 98 k
>   Repo        : base/7/x86_64
>   Summary     : Internationalization extension for PHP applications
>   URL         : http://www.php.net/
>   License     : PHP
>   Description : The php-intl package contains a dynamic shared object that will add
>               : support for using the ICU library to PHP.
```

dump content of `remi-php74.repo`

``` sh
# cat /etc/yum.repos.d/remi-php74.repo
```

``` text
>   # This repository only provides PHP 7.4 and its extensions
>   # NOTICE: common dependencies are in "remi-safe"
>
>   [remi-php74]
>   name=Remi's PHP 7.4 RPM repository for Enterprise Linux 7 - $basearch
>   #baseurl=http://rpms.remirepo.net/enterprise/7/php74/$basearch/
>   #mirrorlist=https://rpms.remirepo.net/enterprise/7/php74/httpsmirror
>   mirrorlist=http://cdn.remirepo.net/enterprise/7/php74/mirror
>   enabled=0
>   gpgcheck=1
>   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi
>
>   [remi-php74-debuginfo]
>   name=Remi's PHP 7.4 RPM repository for Enterprise Linux 7 - $basearch - debuginfo
>   baseurl=http://rpms.remirepo.net/enterprise/7/debug-php74/$basearch/
>   enabled=0
>   gpgcheck=1
>   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi
>
>   [remi-php74-test]
>   name=Remi's PHP 7.4 test RPM repository for Enterprise Linux 7 - $basearch
>   #baseurl=http://rpms.remirepo.net/enterprise/7/test74/$basearch/
>   #mirrorlist=https://rpms.remirepo.net/enterprise/7/test74/httpsmirror
>   mirrorlist=http://cdn.remirepo.net/enterprise/7/test74/mirror
>   enabled=0
>   gpgcheck=1
>   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi
>
>   [remi-php74-test-debuginfo]
>   name=Remi's PHP 7.4 test RPM repository for Enterprise Linux 7 - $basearch - debuginfo
>   baseurl=http://rpms.remirepo.net/enterprise/7/debug-test74/$basearch/
>   enabled=0
>   gpgcheck=1
>   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi
```

create backup file for `remi-php74.repo`

``` sh
# cp /etc/yum.repos.d/remi-php74.repo /etc/yum.repos.d/remi-php74.repo.original
```

edit `remi-php74.repo` to enable php74. (then yum recognize php74 as default php package)

``` sh
# vi /etc/yum.repos.d/remi-php74.repo
# diff -u /etc/yum.repos.d/remi-php74.repo.original /etc/yum.repos.d/remi-php74.repo
```

``` diff
--- /etc/yum.repos.d/remi-php74.repo.original	2020-06-27 02:04:09.590090000 +0000
+++ /etc/yum.repos.d/remi-php74.repo	2020-06-27 02:04:40.099475000 +0000
@@ -6,14 +6,14 @@
#baseurl=http://rpms.remirepo.net/enterprise/7/php74/$basearch/
#mirrorlist=https://rpms.remirepo.net/enterprise/7/php74/httpsmirror
mirrorlist=http://cdn.remirepo.net/enterprise/7/php74/mirror
-enabled=0
+enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi

[remi-php74-debuginfo]
name=Remi's PHP 7.4 RPM repository for Enterprise Linux 7 - $basearch - debuginfo
baseurl=http://rpms.remirepo.net/enterprise/7/debug-php74/$basearch/
-enabled=0
+enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi
```

remove backup for `remi-php74.repo`

``` sh
# rm /etc/yum.repos.d/remi-php74.repo.original
```

check current php info via yum

compare `Version` and `Repo` with the one before enabled remi-php74 repository

``` sh
# yum info php
```

``` text
>   Loaded plugins: fastestmirror, ovl
>   Loading mirror speeds from cached hostfile
>   * base: ty1.mirror.newmediaexpress.com
>   * epel: nrt.edge.kernel.org
>   * extras: ty1.mirror.newmediaexpress.com
>   * remi-php74: ftp.riken.jp
>   * remi-safe: ftp.riken.jp
>   * updates: ty1.mirror.newmediaexpress.com
>   Available Packages
>   Name        : php
>   Arch        : x86_64
>   Version     : 7.4.7
>   Release     : 1.el7.remi
>   Size        : 3.4 M
>   Repo        : remi-php74
>   Summary     : PHP scripting language for creating dynamic web sites
>   URL         : http://www.php.net/
>   License     : PHP and Zend and BSD and MIT and ASL 1.0 and NCSA
>   Description : PHP is an HTML-embedded scripting language. PHP attempts to make it
>               : easy for developers to write dynamically generated web pages. PHP also
>               : offers built-in database integration for several commercial and
>               : non-commercial database management systems, so writing a
>               : database-enabled webpage with PHP is fairly simple. The most common
>               : use of PHP coding is probably as a replacement for CGI scripts.
>               :
>               : The php package contains the module (often referred to as mod_php)
>               : which adds support for the PHP language to Apache HTTP Server.
```

check current php info via yum

compare `Version` and `Repo` with the one before enabled remi-php74 repository

``` sh
# yum info php-intl
```

``` text
>   Loaded plugins: fastestmirror, ovl
>   Loading mirror speeds from cached hostfile
>   * base: ty1.mirror.newmediaexpress.com
>   * epel: nrt.edge.kernel.org
>   * extras: ty1.mirror.newmediaexpress.com
>   * remi-php74: ftp.riken.jp
>   * remi-safe: ftp.riken.jp
>   * updates: ty1.mirror.newmediaexpress.com
>   Available Packages
>   Name        : php-intl
>   Arch        : x86_64
>   Version     : 7.4.7
>   Release     : 1.el7.remi
>   Size        : 233 k
>   Repo        : remi-php74
>   Summary     : Internationalization extension for PHP applications
>   URL         : http://www.php.net/
>   License     : PHP
>   Description : The php-intl package contains a dynamic shared object that will add
>               : support for using the ICU library to PHP.
```
