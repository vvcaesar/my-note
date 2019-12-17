day6 
---------------
函数和模块的使用    
* 定义函数   
+  函数的参数   
	
#### 用模块管理函数    
* 两个相同的函数名字可以通过

```Python
import module1 as m1   
import module2 as m2   
```  
* if __name__ == '__main__':的使用   
* 变量作用域

day 7
---------
Python 3.6以后，格式化字符串还有更为简洁的书写方式，就是在字符串前加上字母f，我们可以使用下面的语法糖来简化上面的代码。   
```
a, b = 5, 10
print(f'{a} * {b} = {a * b}')
```
输出为：   
```
5 * 10 = 50
```

遍历列表
```
print(list1) # [1, 3, 300, 7, 100]
# 通过循环用下标遍历列表元素
for index in range(len(list1)):
    print(list1[index])
# 通过for循环遍历列表元素
for elem in list1:
    print(elem)
```
列表排序

day8
------
### 面向对象编程基础
定义类
```Python
class Student(object):

    # __init__是一个特殊方法用于在创建对象时进行初始化操作
    # 通过这个方法我们可以为学生对象绑定name和age两个属性
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def study(self, course_name):
        print('%s正在学习%s.' % (self.name, course_name))

    # PEP 8要求标识符的名字用全小写多个单词用下划线连接
    # 但是部分程序员和公司更倾向于使用驼峰命名法(驼峰标识)
    def watch_movie(self):
        if self.age < 18:
            print('%s只能观看《熊出没》.' % self.name)
        else:
            print('%s正在观看岛国爱情大电影.' % self.name)

def main():
    # 创建学生对象并指定姓名和年龄
    stu1 = Student('骆昊', 38)
    # 给对象发study消息
    stu1.study('Python程序设计')
    # 给对象发watch_av消息
    stu1.watch_movie()
    stu2 = Student('王大锤', 15)
    stu2.study('思想品德')
    stu2.watch_movie()


if __name__ == '__main__':
    main()
```

#### 为什么要定义类
定义一个矩形的类，目的是求周长和面积。
* 1、不用init()方法定义类
```Python
class Rectangle():
    def getPeri(self,a,b):
        return (a + b)*2
    def getArea(self,a,b):
        return a*b
	
rect = Rectangle()
print(rect.getPeri(3,4))
print(rect.getArea(3,4))
print(rect.__dict__)
```
得到结果：
```Python
14
12
{}
```  
从上例中可以看到，我们在类中并没有定义init()方法，但是也能够得到类似的要求，结果返回了矩形实例rect的周长及面积。     

但是，我们通过print(rect.dict)来看这个实例的属性，竟然是空的，我定义了一个矩形，按理来说它的属性应该是它的长、宽。但是它竟然没有。这就是没有定义init()的原因了。   

并且，在实例化对象的时候，rect = Rectangle()参数为空，没有指定a、b的值，只有在调用函数的时候才指定了。且类中定义的每个方法的参数都有a、b，这显然浪费感情，在类中直接指定方法就可以了。   

因此吧，需要在类中定义init()方法，方便创建实例的时候，需要给实例绑定上属性，也方便类中的方法（函数）的定义。

* 2、用init()方法定义类
上述同样的例子，采用init()方法定义类，如下：
```Python
class Rectangle():

    def __init__(self,a,b):
        self.a = a
        self.b = b

    def getPeri(self):
        return (self.a + self.b)*2

    def getArea(self):
        return self.a * self.b

rect = Rectangle(3,4)
print(rect.getPeri())
print(rect.getArea())
print(rect.__dict__)
```
得到结果如下：
```Python
14
12
{'a': 3, 'b': 4}
```
定义完init()后，创建的每个实例都有自己的属性，也方便直接调用类中的函数。

day8 面向对象编程
------

#### 属性的分类  

* 1 示例属性：  
    最好在__init__(self,...)中初始化
    内部调用时都需要加上self.
    外部调用时用instancename.propertyname
* 2 类属性：  
  　在__init__()外初始化  
    在内部用classname.类属性名调用  
    外部既可以用classname.类属性名又可以用instancename.类属性名来调用  
* 3 私有属性：  
	1）：单下划线_开头：只是告诉别人这是私有属性，外部依然可以访问更改  
	2）：双下划线__开头：外部不可通过instancename.propertyname来访问或者更改  
		实际将其转化为了_classname__propertyname  
例子：
```Python
class Test:

    def __init__(self, foo):
        self.__foo = foo

    def __bar(self):
        print(self.__foo)
        print('__bar')


def main():
    test = Test('hello')
    # AttributeError: 'Test' object has no attribute '__bar'
    test.__bar()
    # AttributeError: 'Test' object has no attribute '__foo'
    print(test.__foo)


if __name__ == "__main__":
    main()
```
#### @property装饰器
之前我们讨论过Python中属性和方法访问权限的问题，虽然我们不建议将属性设置为私有的，但是如果直接将属性暴露给外界也是有问题的，比如我们没有办法检查赋给属性的值是否有效。我们建议是将属性命名以单下划线开头，通过这种方式来暗示属性是受保护的，不建议外界直接访问，那么如果想访问属性可以通过属性的getter（访问器）和setter（修改器）方法进行对应的操作。如果要做到这点，就可以考虑使用@property包装器来包装getter和setter方法，使得对属性的访问既安全又方便，代码如下所示。
```Python
class Person(object):

    def __init__(self, name, age):
        self._name = name
        self._age = age

    # 访问器 - getter方法
    @property
    def name(self):
        return self._name

    # 访问器 - getter方法
    @property
    def age(self):
        return self._age

    # 修改器 - setter方法
    @age.setter
    def age(self, age):
        self._age = age

    def play(self):
        if self._age <= 16:
            print('%s正在玩飞行棋.' % self._name)
        else:
            print('%s正在玩斗地主.' % self._name)


def main():
    person = Person('王大锤', 12)
    person.play()
    person.age = 22
    person.play()
    # person.name = '白元芳'  # AttributeError: can't set attribute


if __name__ == '__main__':
    main()
```
		name设置为只读，age设置为可读写
