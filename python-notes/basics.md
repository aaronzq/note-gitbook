---
description: Useful python tricks
---

# Basics

## \*args and \*\*kwargs

These two are used in function definition where the number of variables input by users is unknown. **\*args** for non-keyworded and **\*\*kwargs** for key-worded. _Note: the naming **args** and **kwargs** are not necessary but under convention._

```python
def adder(*args):
    print(args)
    print(type(args))
    return(sum(args))


> adder(1,2,3)
output: 
> (1 2 3)
> <class 'tuple'>
> 6

def adder(**kwargs):
    print(kwargs)
    print(type(kwargs))
    for key,value in kwargs.items():
        print('{}=={}'.format(key,value))
    return(sum(kwargs.values()))

> adder(num1=1,num2=2,num3=3)
output:
> {'num1': 1, 'num2': 2, 'num3': 3}
> <class 'dict'>
> num1==1
> num2==2
> num3==3
> 6
```

## Decorator

[https://wiki.jikexueyuan.com/project/interpy-zh/decorators/your\_first\_decorator.html](https://wiki.jikexueyuan.com/project/interpy-zh/decorators/your\_first\_decorator.html)

todo: write my own version of note after really using the decorator

## Variable scope between different scripts

If you define a variable in one script, we can access it by import this variable from that script. But when we alter the value of that varible, it actually creates a new variable in the current script without changing the original one.

```python
% file one: config.py - create the variable
a = 10
b = {'para1': 20,'para2': 30}
```

```python
% file two: myclass.py - access the variable

from config import a, b

aa = a
bb2 = b['para2']

class Trainer:
    def __init__(self, para1):
        self.para1 = para1
    def func(self):
        print(self.para1, id(self.para1))
        print(aa, id(aa))
        print(bb2, id(bb2))
```

```python
% file three: main.py

from config import a, b
from myclass import Trainer

print(a, id(a))
print(b, id(b))
print(b['para2'], id(b['para2']))

t = Trainer(b['para1'])
t.func()

print('after editting in main.py')
b['para2'] = 40
print(b, id(b))
print(b['para2'], id(b['para2']))

t.func()
```

```bash
$ python main.py

(10, 140577287984896) // a in main.py scope
({'para2': 30, 'para1': 20}, 4315181976)
(30, 140577287984416) // para2 in main.py scope
(20, 140577287984656)
(10, 140577287984896) // a in config.py scope
(30, 140577287984416) // para2 in config.py scope
after editting in main.py
({'para2': 40, 'para1': 20}, 4315181976)
(40, 140577287986152) // para2 in main.py scope after edit
(20, 140577287984656)
(10, 140577287984896) 
(30, 140577287984416) // para2 in edit.py scope after edit
```
