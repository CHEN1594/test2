***第一小节：基础的类和对象***



# 1初始化

```Python
#类的定义
class Washer():
	statement
```

```python
#创建对象
h1 = Washer() 
```

```python
#举个例子
class Washer():
	def wash(self):
    	print('洗！')
  
haier = Washer()

#测试
haier.wash()

#output: 洗！
```



# 2类中的self

* self是指==调用该函数的对象==

* 例子

```python
#定义类
class Washer():
 	def wash(self):
    	print('洗！')
    	print(self)
    
#创建对象
haier = Washer()

#测试
print(haier)
haier.wash()

#output:

# 对象的内存地址
# 洗！
# 对象的内存地址
```



# 3对象属性的访问与添加

* 添加：类外面添加对象属性 `haier1.height = 100`，而类内的添加见下一节魔法方法

* 获取：类外获取，则`haier1.height`, 而如果在类内获取，应该为`self.height`

* 注，类外获取对象属性前提当然是他得添加过这个属性，而对于类内的self.属性，没添加该属性也可以创建对象，但是要调用获取属性那个语句的时候就要前提是添加过

    

# 4魔法方法`__xx__()`



## (1)`__init__`其实类似构造函数

1. 类方法init创建对象时会被自动调用，不需要手动调用
2. init(self)中的self也不用手动写

```python
#一个例子
class Washer():
	#使用魔法方法，在类中添加实例属性
  	def __init__(self):
    	self.height = 100
    	self.width = 80  #添加了实例属性
    
  	#添加了实例方法  
  	def print_info(self):
    	print(f'the height is {self.height}') #使用格式输出，注意f
    	print(f'the width is {self.width}')
    
haier = Washer() #此处自动调用了init
haier.print_info()

#output
#the height is 100
#the width is 80
```

3. 带参数的init		

* 作用是一个类创建多个对象可以灵活设定属性（如不同长宽高的洗衣只因）

* 他会等待创建对象时向里面传入参数

```python
class Washer():
  	def __init__(self,width,height):
    	self.width = width
    	self.height = height
  	......
  
  
haier1 = Washer(100,80)
haier2 = Washer(10,8)
```



## (2)`__str__()`用于解释说明或输出对象状态

* 如果不设置此类，打印对象的时候输出其内存地址，但如果设置了str，再去打印对象的地址，会返回这个方法中return的数据

```python 
class Washer():
 	def __str__(self):
    	return '这是海尔洗衣机说明书'
haier = Washer()
print(haier)
```



## (3)`__del__()`删除时调用

* 即使你不手动删除对象，python会过一会自己删除它，那时也会调用del()

```python
class Washer():
  	def __str__(self):
    	return '这是海尔洗衣机说明书'
    
    def __del__(self):
        print('对象已删除')
        
haier = Washer()
	
```



* 练习：烤地瓜

```python

class Photo():
    def __init__(self):
        self.time = 0
        self.state = '生的'
        self.ingredient = []

    def cook(self,time):
        self.time += time
        if self.time < 3:
            self.state = '生的'
        elif self.time > 3:
            self.state = '熟了'

    def add_ingre(self,str):
        self.ingredient.append(str)

    def __str__(self):
        return f'总共已经烤了{self.time}分钟，现在地瓜的熟度是{self.state},添加的原料是{self.ingredient}'

    def __del__(self):
        print('deleted')


d1 = Photo()
d1.cook(2)
print(d1)

d1.add_ingre('洋葱')
d1.cook(2)
print(d1)

```





***第二小节：继承***

# 1经典类和新式类

* 经典类写法

```
class 类名：
	代码
```

* 新式类

```
class 类名(object)：
	代码
```

* python中所有类默认继承object类(基类，顶级类)，除了object之外所有类都叫派生类

* 子类默认继承父类所有的属性和方法



# 2单继承和多继承

* 单继承是指一个类==只能从一个父类==继承属性和方法。这意味着在单继承中，一个子类最多只能有一个父类，并且只能继承父类的属性和方法。

    多继承是指一个类==可以从多个父类==继承属性和方法。这意味着在多继承中，一个子类可以有多个父类，并且可以继承多个父类的属性和方法。

* 对于多继承，如果各个父类出现同名属性或方法，则默认继承第一个类的

例子：

```python

class A(object):
    def __init__(self):
        self.one = 1

    def func(self):
        print(1.1)

class B():
    def __init__(self):
        self.one = 2

    def func(self):
        print(2.2)

class C(B,A):
    pass


test = C()
print(test.one)
test.func()

#output
#2
#2.2
```



# 3重写

* 如果子类父类有同名属性和方法，子类对象调用时会调用子类中的，这种现象叫重写
* mro小功能可以查看某个类的父类继承关系

```python

class A(object):
    def __init__(self):
        self.one = 1

    def func(self):
        print(1.1)

class B():
    def __init__(self):
        self.one = 2

    def func(self):
        print(2.2)

class C(B,A):
    def __init__(self):
        self.one = 3

    def func(self):
        print(3.3)


test = C()
print(test.one)
test.func()

print(C.__mro__)#可以用来查看一个类（不是对象哦）的继承关系

#output
#3
#3.3
#(<class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)
```



#4重写后试图调用父类方法（难！）

```python
class A(object):
    def __init__(self):
        self.one = 1

    def func(self):
        print(self.one)

class B():
    def __init__(self):
        self.one = 2

    def func(self):
        print(self.one)

class C(B,A):
    def __init__(self):
        self.one = 3
    def func(self):
        self.__init__() #自己初始化的写法用self点调用的init所以括号里当然不应该加self
        print(self.one)

# 子类调用父类的同名方法和属性，把父类的那个再次封装即可
    def do_A_func(self):
        A.__init__(self) #父类初始化方法，由于是用父类进行的调用所以后面要加self
        A.func(self)
    def do_B_func(self):
        B.__init__(self)
        B.func(self)


test = C()

test.func()
test.do_A_func()
test.func()
```

1我没搞明白对于init调用者和后面括号里的参数有什么区别

2我没搞明白这个通过封装金函数而非继承的方法获得另一个类的操作

3这个self究竟是什么，在又有a又有c的情况，self指代哪一个呢



注：实际保留父类被重写的方法时用的是super().方法，所以不用纠结这项



# 5多层继承

* 可以一直继承下去，爷爷类的也能继承，如果父类重写了以父类为准，如果重写了但封装了爷爷的方法，那当然也可以调用

* 如果想super到爷爷类，则在父类那个函数里继续去super就完事了



# 6super()方法调用父类方法

* 想在子类中调用父类方法，可以使用父类点儿，也可以用super

1. 带参数的super

super(当前类名,self).方法

```python
 def do_B_func_s(self):
        super(C, self).__init__()
        super(C, self).func()
```

2. ==无参数的super==

Super().方法

```python
def do_B_func_s(self):
	super().__init__()
	super().func()
```

3. super方法调用顺序遵循mro顺序



# 7私有权限

* 子类默认继承父类的所有属性和方法

```python
class A():
    def __init__(self):
        self.ppublic = 0.001
        self.__pprivate = 1000000

    def func(self):
        print(self.ppublic)
        print(self.__pprivate)

    def __private_func(self):
        print(1)

class B(A):
    pass


test1 = A()
test1.func()
# print(test1.__pprivate)
# test1.__private_func()

test2 = B()
test2.func()#如果继承的方法中用到了私有属性，却没有关系，可以正常用

# print(test2.__pprivate) 无法访问私有的实例属性
# test2.__private_func() 无法调用私有方法
```

* 私有属性不但不能继承给子类，连他自己这个类都调用不了！！



# 8获取和修改私有属性——一般在类里进行

1. 在类中定义get_xx来获取私有属性，定义set_xx来修改私有属性值（约定俗称的习惯）

```python
class A():
    def __init__(self):
        self.ppublic = 0.001
        self.__pprivate = 1000000
        
    def get_pprivate(self):
        return self.__pprivate

    def set_pprivate(self):
        self.__pprivate = 100
```

这样就实现了在其或其子类对象中对私有属性进行修改和访问

而对于私有方法，则可以在类的方法中定义一个公有方法来调用私有方法，这样就可以通过对象访问这个公有方法来间接访问私有方法。（虽然看起来没啥用。。）

注：python的私有属性和方法时可以再类里直接进行调用的，在类外才需要上述这两招



***第三小节：OOP其他***



# 1面向对象三大特性

* 封装：将属性和方法写进类里；私有权限
* 继承：子类父类那些
* 多态：传入不同对象，产生不同结果



# 2多态

* 这玩意的灵魂在于一个父类（比如dog）有多个子类（各种dog），然后你在另一个地方设计函数时要求一个参数是dog，且在该函数里调用dog的方法，然后由于各个dog的的方法不同，就得以实现传入不同对象时达到不同的效果,有点夹心饼干的意思

```python
class Dog():
    def work(self):
        pass

class ArmDog():
    def work(self):
        print('追查罪犯')

class DrugDog():
    def work(self):
        print('查毒品')

class person():
    def work_with_dog(self,dog):
        dog.work()

ad = ArmDog()
dd = DrugDog()

p = person()
p.work_with_dog(ad) #op 追查罪犯
p.work_with_dog(dd) #op 查毒品
```



# 3类属性

* 类属性的归属者是类对象，他被该类所有的对象所共有（而实例属性的归属者是对象）
* 某项数据始终保持一致，就设为类属性

1. 设置和访问

可以使用类对象或者实例对象来访问

```python
class Dog():
    tooth = 10 #设置类属性

dog1 = Dog()

print(Dog.tooth)#类对象访问类属性 op 10
print(dog1.tooth)#实例对象访问类属性 op 10
```

2. 修改

应该通过类对象修改，不可以通过实例对象修改，如果用实例属性修改，会生成一个同名的实例属性

```python
class Dog():
    tooth = 10 #设置类属性

dog1 = Dog()

print(Dog.tooth)#类访问类属性 op 10
print(dog1.tooth)#对象访问类属性 op 10

#通过类修改

Dog.tooth = 8
print(Dog.tooth)#类访问类属性 op 8

#试图实例对象修改
dog1.tooth =9
print(Dog.tooth)# op 8
print(dog1.tooth)# op 9
```

可以发现在用dog1试图修改类属性时，生成了一个普通的实例属性，而类属性值并不变



# 4类方法

* 需要用装饰器@classmethod来标识为类方法
* 类方法第一个参数必须为类对象，一般以`cls`作为第一个参数
* 作用：和私有类属性或者类属性配合操作

```python
class Dog():
    __tooth = 10

    @classmethod
    def get_tooth(cls):
        return cls.__tooth #类属性访问了私有类对象；cls指代该类


dog1 = Dog()
print(dog1.get_tooth()) # op 10
```

这个例子中作用就是访问私有类属性



# 5静态方法

* 需要用装饰器@staticmethod来标识为静态方法
* 不需要传递self/cls
* 作用:不需要参数传递，适合不需要使用实例对象和类对象的情况
* 可以通过类对象调用，也可以通过实例对象调用

```python
class Dog():
    @staticmethod
    def info_print():
        print('dog can wangwangwang!')


dog1 = Dog()

Dog.info_print() #dog can wangwangwang!
dog1.info_print() #dog can wangwangwang!
```

