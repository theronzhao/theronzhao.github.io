---
title: property属性
date: 2019-2-01 23:37:41 +0800
categories: [Python,Python进阶]
tags: [Python进阶]
---

## property属性

Python内置的@property装饰器就是负责把一个方法变成属性调用

property属性的定义和调用要注意一下几点：

定义时，在实例方法的基础上添加 @property 装饰器；并且仅有一个self参数

调用时，无需括号

两种方式：

装饰器 即：在方法上应用装饰器

类属性 即：在类中定义值为property对象的类属性

## 装饰器方式

在类的实例方法上应用@property装饰器

```python
class Goods:
    """python3中默认继承object类
        以python2、3执行此程序的结果不同，因为只有在python3中才有@xxx.setter  @xxx.deleter
    """
    @property
    def price(self):
        print('@property')

    @price.setter
    def price(self, value):
        print('@price.setter')

    @price.deleter
    def price(self):
        print('@price.deleter')

obj = Goods()
obj.price          # 自动执行 @property 修饰的 price 方法，并获取方法的返回值
obj.price = 123    # 自动执行 @price.setter 修饰的 price 方法，并将  123 赋值给方法的参数
del obj.price      # 自动执行 @price.deleter 修饰的 price 方法
```

经典类中的属性只有一种访问方式，其对应被 @property 修饰的方法
新式类中的属性有三种访问方式，并分别对应了三个被@property、@方法名.setter、@方法名.deleter修饰的方法

## 类属性方式

创建值为property对象的类属性，当使用类属性的方式创建property属性时，经典类和新式类无区别

property方法中有个四个参数

第一个参数是方法名，调用 对象.属性 时自动触发执行方法

第二个参数是方法名，调用 对象.属性 ＝ XXX 时自动触发执行方法

第三个参数是方法名，调用 del 对象.属性 时自动触发执行方法

第四个参数是字符串，调用 对象.属性.\_\_doc\_\_ ，此参数是该属性的描述信息

```python
#coding=utf-8
class Foo(object):
    def get_bar(self):
        print("getter...")
        return 'laowang'

    def set_bar(self, value): 
        """必须两个参数"""
        print("setter...")
        return 'set value' + value

    def del_bar(self):
        print("deleter...")
        return 'laowang'

    BAR = property(get_bar, set_bar, del_bar, "description...")

obj = Foo()

obj.BAR  # 自动调用第一个参数中定义的方法：get_bar
obj.BAR = "alex"  # 自动调用第二个参数中定义的方法：set_bar方法，并将“alex”当作参数传入
desc = Foo.BAR.__doc__  # 自动获取第四个参数中设置的值：description...
print(desc)
del obj.BAR  # 自动调用第三个参数中定义的方法：del_bar方法
```

property 其实并不是函数，而是一个类。它的实例包含一些魔法方法，而所有的魔法都

是由这些方法完成的。这些魔法方法为 \_\_get\_\_ 、 \_\_set\_\_ 和 \_\_delete\_\_ ，它们一道定义了所谓

的描述符协议。只要对象实现了这些方法中的任何一个，它就是一个描述符。描述符的独特

之处在于其访问方式。例如，读取属性（具体来说，是在实例中访问类中定义的属性）时，如

果它关联的是一个实现了 \_\_get\_\_ 的对象，将不会返回这个对象，而是调用方法 \_\_get\_\_ 并将

其结果返回。