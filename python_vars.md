
class TestClass(object):
    #类变量
    val1 = 100

    def __init__(self):
        #成员变量（实例变量）
        self.val2 = 200

    def fcn(self, val=400):
        # 局部变量 
        val3 = 300
        self.val4 = val
        self.val5 = 500

"""
可以发现：python的类变量和C++的静态变量不同，并不是由类的所有对象共享。
类本身拥有自己的类变量（保存在内存），当一个TestClass类的对象被构造时，
会将当前类变量拷贝一份给这个对象，当前类变量的值是多少，这个对象拷贝得到的类
变量的值就是多少；而且，通过对象来修改类变量，并不会影响其他对象的类变量的值
，因为大家都有各自的副本，更不会影响类本身所拥有的那个类变量的值；只有类自己
才能改变类本身拥有的类变量的值。
一个类有一个类变量，初始化一个实例的时候，并没有将类变量添加到实例的命名空间，
如果通过实例查询改类变量，则是去实例的命名空间查询不到，然后去类的命名空间查询。
因此此时通过实例查询改类变量的值则是等于类的变量值，通过类改变了改类变量的值，那么实例的查询到的改类变量的值也就改变了。
但如果一旦通过对象修改了该类变量的值，实际上修改的是实例中拷贝的改类变量的副本，此时会将变量添加到实例的命名空间中，
那么此时不管是对实例变量的查询还是修改，都跟类或者该类的其他对象没有任何关系了。
"""

inst = TestClass()
inst1 = TestClass()
inst2 = TestClass()
print(TestClass.__dict__)  # {'__module__': '__main__', 'val1': 100, '__init__': <function TestClass.__init__ at 0x0000019A9CA12510>, 'fcn': <function TestClass.fcn at 0x0000019A9CA12598>, '__dict__': <attribute '__dict__' of 'TestClass' objects>, '__weakref__': <attribute '__weakref__' of 'TestClass' objects>, '__doc__': None}
print(inst.__dict__)  # {'val2': 200}

print(TestClass.val1)  # 100
print(inst1.val1)  # 100 通过实例查询改类变量，则是去实例的命名空间查询不到，然后去类的命名空间查询。
print(inst1.__dict__)  # {'val2': 200}

inst1.val1 = 1000
print(TestClass.__dict__)  # {'__module__': '__main__', 'val1': 100, '__init__': <function TestClass.__init__ at 0x0000019A9CA12510>, 'fcn': <function TestClass.fcn at 0x0000019A9CA12598>, '__dict__': <attribute '__dict__' of 'TestClass' objects>, '__weakref__': <attribute '__weakref__' of 'TestClass' objects>, '__doc__': None}
print(inst.__dict__)  # {'val2': 200}
print(inst1.__dict__)  # {'val2': 200, 'val1': 1000} 一旦通过对象修改了该类变量的值，实际上修改的是实例中拷贝的改类变量的副本，此时会将变量添加到实例的命名空间中，那么此时不管是对实例变量的查询还是修改，都跟类或者该类的其他对象没有任何关系了。
print(inst1.val1)  # 1000, 修改了实例中类变量的值
print(TestClass.val1)  # 100，但并没有修改类中类变量的值

print(inst2.val1)  # 100，该实例中类变量的值还是实例化该对象时候的变量的值
# inst2.val1=22222
TestClass.val1 = 2000  #, 修改类中类变量的值
print(inst2.val1)  # 2000     该实例中没有重新赋值，跟类变量保存一致

print(TestClass.val1)  # 2000
print(inst1.val1)  # 1000              该实例中被重新赋值后，就跟类变量没关系了

inst3 = TestClass()
print(inst3.val1)  # 2000

下一步
