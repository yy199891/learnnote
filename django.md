然后从命令行窗口中 进入到 `d:\projects` 目录，执行下面的命令创建项目目录

```py
# 执行命令创建项目目录，并且进入到项目目录
mkdir bysms && cd bysms

# 然后执行命令 创建manage.py 和 项目配置目录 名为 config
django-admin startproject config .
```

```py
python manage.py runserver 0.0.0.0:80
```

这样服务就会被启动。 我们就可以在浏览器访问web服务了。

------



```py
python manage.py startapp sales 
```

这样就会创建一个目录名为 sales， 对应 一个名为 sales 的app，里面包含了如下自动生成的文件。

首先我们需要创建数据库，执行如下命令

------



```py
python manage.py migrate
```

就会在 项目的根目录下面 生成一个配置文件中指定的数据库文件 `db.sqlite3`。

并且 会在其中创建一些表。

------

首先，我们再创建一个名为common的应用目录， 里面存放我们项目需要的一些公共的表的定义。

大家还记得创建应用的命令吗？

对了， 进入项目根目录，执行下面的命令。

```py
python manage.py startapp common 
```

现在Django知道了我们的 common 应用， 我们可以在项目根目录下执行命令

```
d:\projects\bysms>python manage.py makemigrations common
```

得到如下结果

```
Migrations for 'common':
  common\migrations\0001_initial.py
    - Create model Customer
```

这个命令，告诉Django ， 去看看common这个app里面的models.py ，我们已经修改了数据定义， 你现在去产生相应的更新脚本。

执行一下，会发现在 common\migrations 目录下面出现了0001_initial.py, 这个脚本就是相应要进行的数据库操作代码。

随即，执行如下命令

```py
d:\projects\bysms>python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, common, contenttypes, sessions
Running migrations:
  Applying common.0001_initial... OK
```

就真正去数据库创建表了。

------

如果以后我们修改了Models.py 里面的库表的定义，都需要再次运行 

```
python manage.py makemigrations common 
```

和 

```
python manage.py migrate
```

 命令，使数据库同步该修改结果



------

我们可以发现，对数据库的操作是 `通过SQL语句` 进行的。

我们的代码需要先创建一个 **Connection** 对象 ， 然后再通过Connection 对象创建一个**Cursor** 对象。

最后使用Cursor对象的**execute**方法，传入要数据库服务执行的SQL语句。

调用execute执行完SQL语句后，cursor 对象的 **fetchone** 方法是获取一行记录。



**fetchmany**可以获取多行记录，参数为获取记录的条数



**fetchall**可以获取所有记录



凡是执行 `更改` 数据的SQL语句，包括：插入、修改、删除， 后面一定要调用connection的**commit**方法，否则不生效。

如果执行了 INSERT 语句 插入数据到数据库表，并且该表有 AUTO_INCREMENT 字段， cursor对象的**lastrowid** 就会保存自增的字段值

 Python MySQL 语句参数化， 所有占位符写法 都是 `%s` ，不管这个参数是 字符串类型 还是数字类型、日期类型等等。



`ON UPDATE CASCADE` 表示 当我们 `修改` user表里面 的记录的id值的时候， 如果在order表里面，有和这条记录对应的订单记录， 则数据库服务 `自动修改` order表中对应记录的 user_id 值。

UPDATE表示修改记录， CASCADE 表示自动关联修改的意思。

`ON DELETE RESTRICT` 表示 当我们要 `删除` user表里面 的记录的id值的时候， 如果在order表里面，有和这条记录对应的订单记录， 则数据库服务 `禁止该操作` ，也就是返回操作不成功。

DELETE 表示删除记录， RESTRICT 表示禁止的意思。
