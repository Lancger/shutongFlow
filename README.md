
## 一、介绍

  该系统为[loonflow](https://github.com/blackholll/loonflow)第三方调用程序，可以理解为OA或运维工单系统。     
本系统使用Vue.js + Django开发，前端展示由loonflow配置决定，可以说前端是全动态，只需要配置好loonflow即可。如果您有使用上的问题可以加入我    们：QQ(558788490)。

    1.shutongflow 的前端调用了shutongflow的后端， 前端代码里面写死了后端端口
    2.shutongflow 的后端调用loonlflow的后端，shutongflow的后端里面写死了loonflow的地址和端口
    3.celery 针对需要有状态是脚本的工作流，如果都不需要执行脚本  那么可以不启动celery

## 二、部署
### 1、loonflow 部署
```bash
## 安装mysql依赖
yum install mysql-devel -y

#升级pip
wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py
pip3 install --upgrade pip

## 这里使用虚拟环境(loonflow虚拟环境)
cd /usr/local/
python3 -m venv loonflow
cd loonflow
source /usr/local/loonflow/bin/activate

#安装requirements依赖
cd /opt/loonflow
pip3 install -i https://pypi.mirrors.ustc.edu.cn/simple/  -r requirements/dev.txt


##DB配置片段 
#vim /opt/loonflow/settings/dev.py
.....
DATABASES = {
    'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'loonflow',        # 刚刚新建的数据库名称
            'USER': 'root',            # 数据库用户 
            'PASSWORD': '123456',      # 数据库密码
            'HOST': '127.0.0.1',       # 数据库机器IP 
            'PORT': '3306',            # 端口
        }
}

## 安全配置
#vim /opt/loonflow/settings/common.py
ALLOWED_HOSTS 改成如下 ALLOWED_HOSTS = ['*']

## 创建数据库
(loonflow) [root@localhost apps]# mysql -uroot -p123456
> create database loonflownew default charset utf8;
> grant all privileges on loonflownew.* to loonflownew@127.0.0.1 identified by '123456';

## 初始化数据库
(loonflow) [root@localhost loonflow]# python manage.py makemigrations
(loonflow) [root@localhost loonflow]# python manage.py migrate

## 创建超级用户
python manage.py createsuperuser

## 启动服务
python manage.py runserver
python manage.py runserver 192.168.56.138:8000    # 绑定端口启动
python manage.py runserver 0.0.0.0:6060           # 启动loonflow

## 启动之后，就可以访问admin后台，然后随便测试一个接口
http://192.168.56.138:8000/admin
http://192.168.56.138:8000/api/v1.0/workflows

## 数据导入
mysql -uroot -p123456 loonflownew < loonflownew.sql
```
### 2、shutongFlow 部署
```bash
## 这里使用虚拟环境(shutongFlow虚拟环境)
cd /usr/local/
python3 -m venv shutongFlow
cd shutongFlow
source /usr/local/shutongFlow/bin/activate

#安装requirements依赖
cd /opt/shutongFlow/apps
pip3 install -i https://pypi.mirrors.ustc.edu.cn/simple/  -r requirements.txt

## 创建数据库
(shutongFlow) [root@localhost apps]# mysql -uroot -p123456
> create database shutongflow default charset utf8;
> grant all privileges on shutongflow.* to shutongflow@127.0.0.1 identified by '123456';

## 初始化数据库
(shutongFlow) [root@localhost apps]# python manage.py makemigrations
(shutongFlow) [root@localhost apps]# python manage.py migrate

## 创建超级用户
python manage.py createsuperuser

## 导入shutongflow数据（配置数据及用户数据）
mysql -uroot -p123456 shutongflow < shutongflow.sql

## 启动服务
python manage.py runserver   
python manage.py runserver 0.0.0.0:6062           # 启动shutongflow          
```

### 3、前端服务
```
cd /opt/shutongFlow/fronted
(shutongFlow) [root@localhost fronted]# npm install .
(shutongFlow) [root@localhost fronted]# npm run dev

# 如果提示npm包有安全提示可以使用以下命令进行修复
(shutongFlow) [root@localhost fronted]# npm audit fix
(shutongFlow) [root@localhost fronted]# npm audit fix --force
```
## 三、登陆

shutongFlow 中所有普通用户

- 'webb': `asdasd`，    
- 'ops': `asdasd`，   
- 'hr': `asdasd`，   
- 'scm': `asdasd`，   
- 'webb': `asdasd`，   
- 'lilian': `asdasd`，   
- 'david': `asdasd`   

管理员为    
'admin': `yxuqtr`


## 四、展示

> 新建工单页
> ![create-ticket](https://github.com/youshutong2080/shutongFlow/blob/master/docs/images/create-ticket.png)

> 代办列表页
> ![todo-list](https://github.com/youshutong2080/shutongFlow/blob/master/docs/images/todo-list.png)

> 详情页
> ![detail-ticket](https://github.com/youshutong2080/shutongFlow/blob/master/docs/images/detail-ticket.png)


## 进度

#### 完成申请说明富文本编辑器 2018-06-08 23:22:00
- 新增附件和图片上传接口；
- 配置UEditor正常使用；
- 重新优化交互体验；

#### 待办
- 前端展示附件及图片；
- 配置ldap实例同步脚本；

## 提示：
- 建议重新拉取代码，并重新配置数据库及服务；


## 问题

