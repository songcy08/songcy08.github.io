# From Vector to Function — Transformations, Basis, and Kernel Method

Here is only some superficial thought about Fourier transformation (and other similar transformations), space basis, and kernel method. I will continuously update the post for new thoughts.

## 1. What is function
A function is an infinite vector. Every thought begins here. As the following figure shows

<img src="./Signal_Sampling.png" alt="source: http://en.wikipedia.org/wiki/Sampling_(signal_processing)" width="379" height="246" class="center_img" />

For a function defined on the interval $[a,b]$, we take samples by an interval $\Delta x$. If we sample the function $f(x)$ at points $a, x_1,\cdots,x_n,b$, then we can transform the function into a vector $(f(a),f(x_1),\cdots,f(x_n),f(b))^T$. When $\Delta x\rightarrow 0$, the vector should be more and more close to the function and at last, it becomes infinite.

The above analysis assumes $x$ to be a real number. But when $x$ is a vector, it still holds. From now on, we use bold font such as $\mathbf{x}$ to denote normal vector in $\mathcal{R}^n$ space; use $f$ to denote the function itself, namely the infinite vector; use $f(\mathbf{x})$ to denote the evaluation of the function at point $\mathbf{x}$. And the evaluation of a function should be a real number.

## 2. Function inner product
Vectors have inner product. For vector $\mathbf{x}=(x_1,\cdots,x_n)$ and $\mathbf{y}=(y_1, \cdots, y_n)$, the inner product of $\mathbf{x}$ and $\mathbf{y}$ is defined as
$$
	<\mathbf{x},\mathbf{y}>=\sum_{i=1}^n x_i y_i
$$
Similarly, functions have inner product. For two functions $latex f$ and $latex g$ sampling by interval $latex \Delta x$, the inner product may be defined as
$$
	<f,g>=\lim_{\Delta x\rightarrow 0}\sum_{i} f(x_i) g(x_i)
$$
at first thought. However, the above equation doesn't converge when $\Delta x\rightarrow 0$. Something is missing. As $x$ approaches 0, the dimension of $f$ and $g$ grows larger in our denotation.

For a vector, the dimension is discrete. We only have the first, second,... dimension. But we don't have the 0.5, 1.5,... dimension. When we transform a function into a vector, every sample point is located at a discrete dimension. However, this should not be true, since the dimension of each sample point should be stable so that the above equation will converge. Therefore, the dimension is not discrete for functions, but continuous. What we miss is the difference between adjacent dimensions for normalization. Therefore, we can define the inner product of two functions as
$$
	<f,g>=\lim_{\Delta x\rightarrow 0}\sum_{i} f(x_i) g(x_i)\Delta x=\int f(x)g(x)dx
$$

## 3. Basis
What is the meaning of inner product? For two vectors $A$ and $B$, the inner product is the projection of one vector to the other.

<img src="./1000px-Dot_Product.png" alt="" width="250" height="200" class="center_img" />

$$
	<A,B>=|A||B|\cos\theta
$$
Namely
$$
	\mathrm{projection}(A) = \frac{<A,B>}{|B|^2} B
$$
If we regard the length of $B$ as one unit, then the inner product is the coordinate of the projection on $B$. Therefore, given a set of orthogonal basis vector $\{\mathbf{e}_i\}_{i=1}^n$ where $<\mathbf{e}_i,\mathbf{e}_j>=0$ and $|\mathbf{e}_i|=1$. The basis vectors can establish a space by linear operations. Any vector $\mathbf{x}$ in the space can be denoted as
$$
	\mathbf{x}=\sum_{i=1}^n <\mathbf{x},\mathbf{e}_i> \cdot \mathbf{e_i}
$$
More generally, if $|\mathbf{e}_i| \neq 1$, then
$$
	\mathbf{x}=\sum_{i=1}^n \frac{<\mathbf{x},\mathbf{e}_i>}{|\mathbf{e}_i|^2} \mathbf{e}_i=\sum_{i=1}^n \frac{<\mathbf{x},\mathbf{e}_i>}{<\mathbf{e}_i,\mathbf{e}_i>} \mathbf{e}_i
$$
Functions also have basis, which can be orthogonal or non-orthogonal. If we define the set of function basis, any function in the space can also be decomposed into the combination of several basis functions. Function inner product can also be used to calculate the coefficient on any specific basis. However, since the dimension of function is continuous, there may be infinite basis functions.

For a set of function basis $\{h_i\}$, if $<h_i,h_j>=0$ and $|h_i|=1$, any function within the space can be denoted as
$$
	f=\sum <f,h_i> \cdot h_i=\sum_i \int f(x)h_i(x)dx \cdot h_i
$$
If $|h_i| \neq 1$,
$$
	f=\sum_i \frac{<f,h_i>}{<h_i,h_i>} h_i
$$
similar to the vector case.

## 4. Example: Transformations
Let the basis functions $\{h_p\}, (p$ is interger) be
$$
	h_p(x)=e^{i2\pi p x/T}
$$
defined on interval $[0, T]$. Here I use $p$ to index the basis functions in order to distinguish from the imaginary number $i$. These construct a space and any function defined on interval $[0, T]$ is within the space. It can be proven that any two basis function are orthogonal (for complex numbers, the latter term should take conjugate transpose when calculating the inner product).
$$
	<h_p, h_q>=\int_0^T h_p (x) \bar{h_q(x)} dx=\int_0^T e^{i2\pi p x/T} e^{-i2\pi q x/T} dx=0
$$
where $p \neq q$ and $\bar{a+bi}=a-bi$. The "length" of basis is
$$
	|h_p|^2=<h_p,h_p>=T
$$
If a function $latex f$ defined on interval $[0,T]$ is within the space, it can be written as
$$
	f(x)=\sum_p c_p h_p(x)=\sum_p c_p e^{i2\pi px/T}
$$
And the coefficient can be calculated as
$$
	c_p=\frac{<f,h_p>}{|h_p|^2}=\frac{1}{T} \int_0^T f(x)\bar{h_p(x)}dx=\frac{1}{T} \int_0^T f(x) e^{-i2\pi p x} dx
$$
Actually this is the Fourier series. A more thorough discussion about Fourier series can be found [here](http://www.thefouriertransform.com/).

## 5. Kernel Function
Instead of $i$, we use $y$ to index the basis function. Let
$$
	K(x,y)=h_y(x)
$$
The set of basis functions can be denoted as one function with two parameters. In the example, $y$ is discrete. However, this is not the full space. If we let $y$ to be real number, then any functions may be represented.

The function $K(x,y)$ is kernel function. It may be viewed as a cluster of basis functions. If they are orthogonal $<K(\cdot,y_1),K(\cdot,y_2)>=0$ for any $y_1 \neq y_2$, and the squared "length" of basis $|K(\cdot,y)|^2=\int 1 dx$, any function $f$ can be represented as
$$
	f=\int F(y)K(\cdot,y) dy
$$
And the coefficient of a function $f$ on a basis $K(\cdot,y)$ is calculated as
$$
	F(y)=<f,K(\cdot,y)>=\int f(x)K(x,y)dx
$$
Therefore, kernel function is able to transform a function into another space (a function space).

## 6. Example: Transformations
Let the kernel function to be
$$
	K(x,y)=e^{i2\pi xy}
$$
where $i$ is the imaginary number. It is easy to show that the basis functions are mutual orthogonal,
$$
	<K(\cdot,y_1),K(\cdot,y_2)>=\int K(x,y_1) \bar{K(x,y_2)}dx=\int e^{i2\pi xy_1}e^{-i2\pi xy_2}dx=0
$$
when $y_1 \neq y_2$. Then squared "length" of basis is
$$
	<K(\cdot,y),K(\cdot,y)>=\int K(x,y)\bar{K(x,y)} dx=\int 1 dx
$$
This constitutes a full function space. The coefficient is calculated as
$$
	F(y)=<f,K(\cdot ,y)>=\int f(x) \bar{K(x,y)} dx=\int f(x)e^{-i2\pi xy}dx
$$
The original function is represented as
$$
	f(x)=\int F(y) K(x,y) dy=\int F(y) e^{i2\pi xy}dy
$$
Actually this is the Fourier transformation. Other transformations such as Laplace transformation, Z-transformation are similar. Maybe wavelet is also within the framework.

## 7. Something about Kernel Method
For a kernel function $K(x,y)$, if we regard it as a cluster of basis, then we can get a space by the linear combination of the basis. However, the basis may not be orthogonal. This is somewhat like a matrix $P$, if we use the columns of $P$ to generate a space. To construct orthogonal basis for the space, we can use eigen value and eigen vectors of $P$. Similarly, we can use eigenfunctions as the basis for the function space generated by $K(x,y)$. If all the eigen values are positive, then the kernel function is positive. If the inner product is defined as
$$
	<K(\cdot,x), K(\cdot,y)>=K(x,y)
$$
This is the reproducing kernel Hilbert space. It is commonly used to calculate the inner product of two mappings in a high-dimension feature space. When linear model is not enough, we can map the points into feature space and use kernel trick to calculate inner product. Support Vector Machine is an example.