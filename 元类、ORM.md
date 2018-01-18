# python

### 元类
* 类就是一组用来描述如何生成一个对象的代码段
* 类也是对象，你可以在运行时动态的创建它们，就像其他任何对象一样
* type还有一种完全不同的功能，动态的创建类
```
type可以接收一个类的描述作为参数，然后返回一个类
type(类名，由父类名称组成的元组（针对继承的情况，可以为空)，包含属性的字典（名称和值））
```
* 元类就是用来创建类的"东西" ; 元类就是类的类
* Python中所有的东西，注意我是指所有的东西--都是对象。这包括整数、字符串、函数以及类
* __metaclass__属性
```
Python会通过 __metaclass__创建一个名字为Foo的类（对象）
```
* 元类的主要目的就是为了当创建类时能够自动的改变类

### ORM
* ORM是Python编程语言后端web框架Django的核心思想，"Object Relational Mapping"，即对象-关系映射，简称ORM
```
#ORM实现原理
class ModelMetaclass(type):
    def __new__(cls, name, bases, attrs):
        # name="User"
        # bases=(object,)
        # attrs = {
            #    "uid": ('uid', "int unsigned"),
            #    "name": ('username', "varchar(30)"),
            #    "email": ('email', "varchar(30)"),
            #    "password": ('password', "varchar(30)")
            #    "save": xxxxxx,
            #    "__init__": xxxxxx2
            # }
        mappings = dict()
        # mappings = {
            #    "uid": ('uid', "int unsigned"),
            #    "name": ('username', "varchar(30)"),
            #    "email": ('email', "varchar(30)"),
            #    "password": ('password', "varchar(30)")
            # }
        # 判断是否需要保存
        for k, v in attrs.items():
            # 判断是否是元组类型
            if isinstance(v, tuple):
                print('Found mapping: %s ==> %s' % (k, v))
                mappings[k] = v

        # 删除这些已经在字典中存储的属性
        for k in mappings.keys():
            attrs.pop(k)  # del attrs[k]

        # attrs = {
        #    "save": xxxxxx,
        #    "__init__": xxxxxx2，
        #    "__mappings__": {
        #          "uid": ('uid', "int unsigned"),
        #          "name": ('username', "varchar(30)"),
        #          "email": ('email', "varchar(30)"),
        #          "password": ('password', "varchar(30)")
        #    },
        #    "__table__": "User"
        # }

        # 将之前的uid/name/email/password以及对应的对象引用、类名字
        attrs['__mappings__'] = mappings  # 保存属性和列的映射关系
        attrs['__table__'] = name  # 假设表名和类名一致
        return type.__new__(cls, name, bases, attrs)


class User(metaclass=ModelMetaclass):
    uid = ('uid', "int unsigned")
    name = ('username', "varchar(30)")
    email = ('email', "varchar(30)")
    password = ('password', "varchar(30)")
    # 当指定元类之后，以上的类属性将不在类中，而是在__mappings__属性指定的字典中存储
    # 以上User类中有 
    # __mappings__ = {
    #    "uid": ('uid', "int unsigned")
    #    "name": ('username', "varchar(30)")
    #    "email": ('email', "varchar(30)")
    #    "password": ('password', "varchar(30)")
    # }
    # __table__ = "User"
    def __init__(self, **kwargs):
        for name, value in kwargs.items():
            setattr(self, name, value)

    def save(self):
        fields = []
        args = []
        for k, v in self.__mappings__.items():
            fields.append(v[0])
            args.append(getattr(self, k, None))

        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join([str(i) for i in args]))
        print('SQL: %s' % sql)


u = User(uid=12345, name='Michael', email='test@orm.org', password='my-pwd')
# print(u.__dict__)
u.save()

```
