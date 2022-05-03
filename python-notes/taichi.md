# Taichi

Taichi is a Domain-Specific Language embedded in Python, which is used for Computer Graphics.&#x20;

## At the beginning

```python
import taichi as ti

ti.init(arch = ti.gpu) # initialize taichi

# arch = ti.gpu: choose backend from the priority list:
#                CUDA, OpenGL/Metal, CPU
# arch = ti.opengl
# arch = ti.cpu
```

## Data type

Taichi supports:\
1\. int (ti.i8/i16/i32/i64)\
2\. uint (ti.u8/u16/u32/u64)\
3\. float (**ti.f32**/f64)\
\
Float 32 is commonly used. OpenGL supports i32, f32 and f64.\
Boolean types should be represented using ti.i32

### Tensor: tensors are _global variables_ provided by Taichi.&#x20;

Tensors can be either sparse or dense. An element of a tensor can be either a scalar or a vector/matrix.

```python
a = ti .var(dt=ti .f32 , shape=(42, 63)) 
# A tensor of 42x63 scalars 
b= ti.Vector(3, dt=ti.f32, shape=4) 
# A tensor of 4x 3D vectors 
C= ti.Matrix(2, 2, dt=ti.f32, shape=(3, 5))
# A tensor of 3x5 2x2 matrices
```

If `shape` is `()` (empty tuple), then a 0-D tensor (scalar) is created. And we should access it by passing `None` as index:

```python
loss = ti.var(dt=ti.f32, shape=()) 
# A (0−D) tensor of a single scalar
loss[None] = 3 
print(loss[None]) # 3
```

### How to access components in Tensor?

```python
a[3, 4] = 1
print('a[3, 4] =', a[3, 4])
# "a[3, 4] = 1.000000"

b[2] = [6, 7, 8]
print('b[0] =', b[0][0], b[0][1], b[0][2])
# print(b[0]) is not yet supported

loss[None] = 3
print(loss[None]) 
# 3
```

_**!! All variables should be created and placed before any kernel invocation or any of them accessed from python-scope**_. For example:

```python
x = ti.var(ti.f32)
x[None] = 1 # ERROR: x not placed!
```

```python
x = ti.var(ti.f32, shape=())
@ti.kernel
def func():
    x[None] = 1

func()
y = ti.var(ti.f32, shape=())
# ERROR: cannot create tensor after kernel invocation!
```

```python
x = ti.var(ti.f32, shape=())
x[None] = 1
y = ti.var(ti.f32, shape=())
# ERROR: cannot create tensor after any tensor accesses from the Python-scope!
```

## Kernels and functions

Computations operate inside Taichi's **kernel**. **Kernels** can only be called in Python scope, which means they cannot call each other.

**Kernel** arguments must be _**type-hinted**_. Kernels can have at most 8 parameters, e.g.,

```python
@ti.kernel
def print_xy(x: ti.i32, y: ti.f32):
    print(x + y)
```

A **kernel** can have a **scalar** return value. If a **kernel** has a return value, it must be _**type-hinted**_. The return value will be automatically cast into the hinted type. e.g.,

```python
@ti.kernel
def add_xy(x: ti.f32, y: ti.f32) -> ti.i32:
    return x + y  # same as: ti.cast(x + y, ti.i32)

res = add_xy(2.3, 1.1)
print(res)  # 3, since return type is ti.i32
```

**Functions** are callable only in Taichi-scope (by kernels or other functions). Do not call them in Python-scopes.

```python
@ti.func
def laplacian(t, i, j):
    return inv_dx2 * (
        -4 * p[t, i, j] + p[t, i, j - 1] + p[t, i, j + 1] + p[t, i + 1, j] +
        p[t, i - 1, j])

@ti.kernel
def fdtd(t: ti.i32):
    for i in range(n_grid): # Parallelized
        for j in range(n_grid): # Serial loops in each parallel threads
            laplacian_p = laplacian(t - 2, i, j)
            laplacian_q = laplacian(t - 1, i, j)
            p[t, i, j] = 2 * p[t - 1, i, j] + (
                c * c * dt * dt + c * alpha * dt) * laplacian_q - p[
                           t - 2, i, j] - c * alpha * dt * laplacian_p
```

### For loops

There are 2 types of for-loops:\
1\. Range-for loops\
2\. Struct-for loops

#### Range-for loops

```python
@ti.kernel
def fill():
    for i in range(10):  # Parallelized
        x[i] += i
        
        s = 0
        for j in range(5): # Serialized in each parallel thread
            s += j
            
         y[i] = s 
 
 @ti.kernel
 def fill_3d():
     # Parallelized for all 3 <= i < 8, 1 <= j < 6
     # 0 <= k < 9
     for i, j, k in ti.ndrange((3, 8), (1, 6), 9):
         x[i, j, k] = i + j + k
```

_It is the loop **at the outermost scope** that gets parallelized, not the outermost loop._

```python
@ti.kernel
def foo():
    for i in range(10): # Parallelized :-)
        ...

@ti.kernel
def bar(k: ti.i32):
    if k > 42:
        for i in range(10): # Serial :-(
            ...
```

### Atomic operations

In Taichi, augmented assignments (e.g., `x[i] += 1`) are automatically [atomic](https://en.wikipedia.org/wiki/Fetch-and-add).  When modifying global variables in parallel, make sure you use atomic operations. For example, to sum up all the elements in `x`:

```python
@ti.kernel
def sum():
    for i in x:
        # Approach 1: OK
        total[None] += x[i]

        # Approach 2: OK
        ti.atomic_add(total[None], x[i])

        # Approach 3: Wrong result since the operation is not atomic.
        total[None] = total[None] + x[i]
        # Because the loop is parallized, so global 
        # scalar tensor total will be interupted by
        # other threading  
        
        
        
```

## Taichi-scope and Python-scope

Kernel and function are inside Taichi-scope, compiled by Taichi compiler.\
The others are in Python-scope

## Templates

1. Initialization: ti.init(arch = ti.gpu)
2. Define and assign tensor: ti.var(), ti.Vector(), ti.Matrix()
3. Computation: launch kernels, access tensors in Python-scope
4. Optional: ti.reset() to reset Taichi system: erase cache, variables and kernels.

## &#x20;
