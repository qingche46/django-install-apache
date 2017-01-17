# 1. 安装python和django
### 1.1 ubunt自带python2.7，支持django1.10.4，
### 1.2 安装django用pip安装
#### 1.2.1安装pip

```
sudo apt-get install python-pip
```

#### 1.2.2 利用pip安装django

```
（sudo) pip install Django
```

### 1.3 检查是否安装成功
终端上输入 python ,点击 Enter，进行 python 环境

```
>>> import django
>>> django.VERSION
(1, 10, 4, u'final', 0)
```



# 2. 安装 apache2 和 mod_wsgi
### 安装apache2

```
sudo apt-get install apache2
```


#### Python 2（安装mod_wsgi)

```
sudo apt-get install libapache2-mod-wsgi
```
#### Python 3

```
sudo apt-get install libapache2-mod-wsgi-py3
```



2.1 确认安装apache2版本号

```
 apachectl -v
Server version: Apache/2.4.18 (Ubuntu)
Server built:   2016-07-14T12:32:26
```

## 3.django创建一个新的工程
### 3.1新建一个工程，在/var/www/下创建一个django工程  
输入（testprocject为工程名，可任意改变成你喜欢的）

```
django-admin.py startproject testproject
```
### 3.2新建app

```
python manage.py startapp Myfirstsite

```
在Myfirstsite下创建一个templates文件，在templates的文件夹下创建一个index.html

```
├── db.sqlite3
├── manage.py
├── media
├── Myfirstsite
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   ├── models.py
│   ├── templates
│   ├── tests.py
│   ├── views.py
├── static
│   ├── css
│   ├── fonts
│   ├── image
│   └── js
└── testproject
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    ├── wsgi.py
```


## 4.新建一个网站配置文件

```
sudo vi /etc/apache2/sites-available/testproject.conf
```

```
  
  <VirtualHost *:80>                                                                                                                                                                       
       ServerName www.yourip_OR_domain.com
       ServerAlias otherdomain.com
       Alias /media/ /var/www/testproject/media/
       Alias /static/ /var/www/testproject/static
       /
       <Directory /var/www/testproject/media>
          Require all granted
       </Directory> 
       
       <Directory /var/www/testproject/static>
         Require all granted
       </Directory> 
       
      WSGIScriptAlias / /var/www/testproject/testproject/wsgi.py
      
      <Directory /var/www/testproject/testproject>
        <Files wsgi.py>
         Require all granted
        </Files>     
      </Directory> 
                   
  </VirtualHost>
```
## 4 修改wsgi.py文件

```
vi /var/www/testproject/testproject/wsgi.py
```

```
import os                     
from os.path import join,dirname,abspath
import sys                    
PROJECT_DIR = dirname(dirname(abspath(__file__)))
sys.path.insert(0,PROJECT_DIR)                      
from django.core.wsgi import get_wsgi_application  
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "testproject.settings")
application = get_wsgi_application()
```
在setting.py中加入(这个地方切记加入自己的域名否则会报错）

```
ALLOWED_HOSTS = ['127.0.0.1', 'localhost'，‘yourip_OR_domain’]
```


## 5.激活网站


```
sudo a2ensite testproject 或 sudo a2ensite testproject.conf
```


# 重启apache2命令 
```
sudo service apache2 restart
```







