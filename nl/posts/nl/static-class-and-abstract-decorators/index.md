<!--
.. title: Static, Class and Abstract Decorators
.. slug: static-class-and-abstract-decorators
.. date: 2017-11-07 09:17:04 UTC+01:00
.. tags: python
.. category: python, decorators
.. link: 
.. description:
.. type: text
-->


The first thing that we have to ak ourselves is: What is a Python decorator? Well, in a formal way “A decorator is the name used for a software design pattern. Decorators dynamically alter the functionality of a function, method, or class without having to directly use subclasses or change the source code of the function being decorated”, but my favourite definition is “Factory of functions”. 
The second thing that we need to know is the concept of bound and unbound methods in Python. Methods in Python are objects too, so in runtime the methods are instantiated and they are bound to a class or an instance of a class.

##Static Method

The best way to understand the concept is with the code:

```python
Class Car(object):
    def __init__(self, engine, chassis):
        self.motor = engine
        self.chasis = chasis
    
    def get_total_weight(self, x, y):
        return x + y

    def  weight(self):
        return self.get_total_weigth(self.engine.weight, self.chassis.weight)
```


The question is, is this code working?: The answer is yes, but if you take a look at the get_total_weigth method, the first argument is self and we never use it inside. But what is really happen? Well, remember that the methods are object too, so Python create objects related to the methods and bound them to a Car instance, this is the reason why we pass self like first argument. 

Suppose that car is a Car instance, then:

```python
car.get_total_weight
Out[18]: <bound method Car.get_total_weight of <__main__.Car object at 0x7f72d7dbb750>>
```


This is expensive because every time that we instantiate a Car, Python must create new methods objects,  and  we know that we don’t need the “self” reference. So the solution is to use staticmethod. 

```python
@staticmethod    
def get_total_weight(x, y):
       return x + y


car.get_total_weight
Out[29]: <function __main__.get_total_weight>
```

Now we see that the method is not bound to the instance.

With this approach we have the following advantages:

Python doesn’t have to instantiate a bound-method for each Car object.
We know that the methods don’t  depend on the internal object state
If we have a subclass, we can override the get_total_weight without ovewrite the weight method

One thing that we mention is that a method that is decorated with @staticmethod is callable by a class or by an instance.

REMEMBER: If a method doesn’t use the self reference, it is probably a static method.  

##Class Method

Class methods are methods that are bound to the class instead of the instance of the class, so in the following code we can see that the get_market method receive cls as first argument. 

```python
Class Car(object):
    MARKET = "Onda"    
    
    def __init__(self, engine, chassis):
        self.engine = engine
        self.chassis = chassis
    
    @classmethod
    def car_factory(cls, car_component_list):
        for engine, chassis in car_component_list:
            if engine.consumption < 6:
	         yield cls(engine, chassis)

car_component_list = [(m1, c1), (m2, c2), (m3, c3)]
car_list = []
for car in Car.car_factory(car_component_list):
    if car:
        car_list.append(car)
   
Car.car_factory
Out[42]: <bound method type.car_factory of <class '__main__.Car'>>

Class methods are used by "Factory Methods" when we need to do special operations and then return an instance of the class,  and to call a method that has been decorated with staticmethod.  
```


##Abstract Method

Abstract method are used in inheritance, are defined in a Base Class but are not implemented. See the following example:

```python
Class BaseCar(object):
    MARKET = "Onda"    
   
    def __init__(self, engine, chassis):
        self.engine = engine
        self.chasis = chasis
    
    def turn_on(self):
        raise NotIplementedError
```

Any class that inherited from Car must be implement turn_on method, otherwise an exception will be raised.
One thing that you should know is that if you forget to implement the method in a subclass an error will be raised when you try to call the method. A solution to this problem is to use the ABC module that is Python provide.

```python
import abc

Class BaseCar(object):
    __metaclass__ = ABCMetaclass
    MARKET = "Onda"
   
    def __init__(self, engine, chassis):
        self.engine = engine
        self.chassis = chassis
    
    @abc.abstractmethod
    def turn_on(self):
        """
        This method should be implemented by the subclass
        """
```

Now, if we try to create an instance of BaseCar we have the following error:

```python
c = BaseCar()
TypeError: Can't instantiate abstract class BaseCar with abstract methods turn_on
```


##Conclusion:

If you have a method and inside it you never use the self reference, probably the method should be decorated with @staticmethod.
Remember that @classmethod in general is used by ‘Factory’, and always the first argument must be a reference to the class. We use cls for it. Of course you should use the cls inside the method!!
A static method can be called by any kind of methods.
If you have inheritance and you want to overwrite a method always decorate it with @abc.abstractmethod. It avoid problems. 