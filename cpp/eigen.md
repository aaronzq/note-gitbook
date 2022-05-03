---
description: Eigen library for linear algebra in c++
---

# Eigen

## Install

[Installation](https://phylogeny.uconn.edu/tutorial-v2/part-1-ide-project-v2/setting-up-the-eigen-library-v2/)

### WINDOWS

1. Download the latest stable release of [Eigen 3](http://eigen.tuxfamily.org)
2. Unpack the downloaded zip or tar.gz file to your _This PC\Documents\libraries_ directory
3. Right-click the project (note: click the project, not the solution) in the Solution Explorer pane of Visual Studio and choose _Properties_ from the popup menu.
4. Expand _VC++ Directories_, then click on _Include Directories_, then click the down-pointing triangle, then click _\<Edit…>_ and add _This PC\Documents\libraries\eigen3_.
5. The Eigen library comprises only header files. Because nothing is compiled, there is no need to specify a library search path and no need to link any libraries.

## Basics

[Referemce](https://eigen.tuxfamily.org/dox/group\_\_QuickRefPage.html)

```cpp
#include <iostream>
#include <Eigen/Eigen>

using namespace Eigen;

int main()
{
    // Constructor
    // Matrix
    Matrix3f m1; // equivalent to        Matrix<float, 3, 3> m1
    MatrixXf m2; // equibalent to        Matrix<float, Dynamic, Dynamic> m2
    MatrixXf m3(4,3); // equibalent to   Matrix<float, 4, 3> m3 
    // Vector
    Vector3f v1; // equivalent to        Matrix<float, 3, 1> v1
    VectorXf v2; // equibalent to        Matrix<float, Dynamic, 1> v2
    Vector3d v3{x, y, z}; // create a vector (x, y, z)

    // Initializer 
    m1 << 1, 2, 3, 
          4, 5, 6,
          7, 8, 9;          
    m1 << Matrix2f::Identity(), Vector2f::Constant(1),
          2, 3, 3;          
    m1 = Matrix3f::Constant(2);
    m1 = (Matrix3f() << 1, 2, 3, 4, 5, 6, 7, 8, 9).finished();

    v1 << 1, 2, 3;
    v1 = {1, 2, 3};
    v1 = Vector3f{1, 2, 3};

    // Coefficience access
    std::cout << m1(1,2) << std::endl;
    std::cout << m1.row(0) << std::endl;
    std::cout << m1.col(2) << std::endl;

    std::cout << v1(1) << std::endl;
    std::cout << v1.x() << std::endl;
    std::cout << v1.y() << std::endl;  // z() and w()


    //Arithmetic

    //matrix/vector products
    std::cout << m1 * v1 << std::endl;

    //dot and cross product
    std::cout << v1.dot(v3) << std::endl; // v1 * v3
    std::cout << v1.cross(v3) << std::endl; // v1 x v3 


}
```

## Solve linear system

$$
Ax = b
$$

Easiest way to solve x is to call

```cpp
x = A.inverse() * b;

// or 

x = A.partialPivLU().solve(b);
```

This assumes A is an **invertible** matrix and it will use **LU decomposition** (`Eigen::PartialPivLU`) to solve the equation. This is helpful when solving simple linear equations.

Check the video below how to do LU decomposition by hand.

[Video 1](https://www.youtube.com/watch?v=MsIvs\_6vC38)

[Video 2](https://www.youtube.com/watch?v=m3EojSAgIao)

Besides the `inverse()` and `partialPivLu()`method, the whole list of decomposition methods (to solve linear system) is at

[Reference](https://eigen.tuxfamily.org/dox/group\_\_TutorialLinearAlgebra.html)

Especially for **least square**, one approach is to solve the normal equation below:

$$
A^TAx=A^Tb
$$

Since A^T\*A is positive semidefinite, we can use `ldlt()` to solve. We can code:

```cpp
MatrixXf A = MatrixXf::Random(3, 2);
VectorXf b = VectorXf::Random(3);
VectorXf x = (A.transpose() * A).ldlt().solve(A.transpose() * b);
```
