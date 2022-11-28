# 1_mentoring.md

# Context

- The person who asked this question has very little experience with Python or college-level mathematics.
- The course will have many opportunities to work with arrays in `NumPy` in the future

# Student question

> I tried to calculate the dot product in Python using `NumPy`, but I got an error. Why is this happening?”
> 

![https://camo.githubusercontent.com/7d3d0732c5096d386bedca4404b144ab15319889d710bf3155aa18d0a0d812d7/68747470733a2f2f742e6779617a6f2e636f6d2f7465616d732f64697665696e746f636f64652f65396533633665393162376434616264393930303161613038343865343035392e706e67](https://camo.githubusercontent.com/7d3d0732c5096d386bedca4404b144ab15319889d710bf3155aa18d0a0d812d7/68747470733a2f2f742e6779617a6f2e636f6d2f7465616d732f64697665696e746f636f64652f65396533633665393162376434616264393930303161613038343865343035392e706e67)

# Response

> I’ll be referring to the student as “you” and myself as “me”
And since he has little experience in Python as well as college-math. I’ll walk through his code with him before arriving at the error

### Code overview

First, let’s look at the matrixes you constructed, both with dimension $(1\times 3)$

$$
a = \begin{bmatrix} -1 & 2 & 3 \end{bmatrix} \text{(1$\times$3)}
\\
b = \begin{bmatrix} 0 & 2 & 1 \end{bmatrix} \text{(1$\times$3)}
$$

*You can verify above dimensions with `a.shape` (`=(1,3)`)*

Now, about the dot product code (aka `np.dot`) (Reference this [documentation](https://numpy.org/doc/stable/reference/generated/numpy.dot.html)), it is specifically stated that:

> 1. If both *a* and *b* are 1-D arrays, it is inner product of vectors (without complex conjugation).
> 2. If both *a* and *b* are 2-D arrays, it is matrix multiplication, but using **`[matmul](https://numpy.org/doc/stable/reference/generated/numpy.matmul.html#numpy.matmul)`** or `a @ b` is preferred.
> 

### Error explanation

Since as we’ve verified above, both `a` and `b` are 2-Dimentional array (matrix), the `np.dot` will follow the second case, which is matrix multiplication. That means:

> For matrix multiplication, the number of columns in the first matrix must be equal to the number of rows in the second matrix. ([Wiki](https://en.wikipedia.org/wiki/Matrix_multiplication))
> 

And in your code, when you multiply `a` (1 x 3) with `b` (1 x 3), the columns in first matrix (3) and rows of the second (1) mismatch, that why we had the error:

```bash
ValueError: shapes (1,3) and (1,3) not aligned: 3 (dim 1) != 1 (dim 0)
```

**************(It’s important to understand error messages so that you can easily debug your code if anything goes wrong)**************

Even the documentation for `np.dot` has this information as quoted below:

> **Raises `ValueError`** If the last dimension of *a* is not the same size as the second-to-last dimension of *b*.
> 

### Fix

So, to fix the error, all we need is to transpose by with `b.T` so that the rows and columns line up for our matrix multiplication

```bash
np.dot(a,b.T) # => array([[7]])
```

Result will be a (1x1) matrix (`[[7]]`)

Or you can just change how you initialized `b` 

```python
b = np.array([
    [0],
    [2],
    [1]])
# OR: b = np.array([[0,2,1]]).T

np.dot(a,b) # => no error
```

## Possible follow up question

> Why can’t the code just work without modifications? Clearly `a` and `b` are just one dimensional vectors that can be dotted together?
> 

### Explain

It is true that from our view, we can treat `a` and `b` as mathematical vector (1-Dimensional array) and execute dot product on them normally.

**BUT**, your code does not reflect that. As we have established earlier, constructing `a` with

```python
a = np.array([[-1,2,3]])
a.shape # => (1,3)
```

Specifically initialized `a` as a 2-Dimentional array (a matrix). That’s why `a.shape` is `(1,3)`

If you want to create `a` as a 1-Dimentional array (a vector). You need to

```python
a = np.array([-1,2,3])
a.shape # => (3,)
```

In this case `a.shape` is `(3,)` indicating it is viewed by `numpy` as a plain 1D array (vector)

### Alternate code

So in the case that you create both `a` and `b` as one dimensional, the below code would be a “possible” alternative

```python
a = np.array([-1,2,3])
b = np.array([0,2,1])

np.dot(a,b) # => 7
```

Do note that since our input for `np.dot` are vectors and not matrixes, the final output of `np.dot(a,b)` is calculated as dot product (as stated in the [documentation](https://numpy.org/doc/stable/reference/generated/numpy.dot.html))

> If both *a* and *b* are 1-D arrays, it is inner product of vectors (without complex conjugation)
> 

and results in a number and not a 1x1 matrix as in the previous version of your code (hence it is only a “possible” alternative)

### Note

If you’re wondering why is there difference between `matrix` (2D array) and `vector` (1D array) in `numpy`, refer to these:

- [https://stackoverflow.com/questions/22053050/difference-between-numpy-array-shape-r-1-and-r](https://stackoverflow.com/questions/22053050/difference-between-numpy-array-shape-r-1-and-r)
- [https://stackoverflow.com/questions/38402227/numpy-why-is-there-a-difference-between-x-1-and-x-dimensionality](https://stackoverflow.com/questions/38402227/numpy-why-is-there-a-difference-between-x-1-and-x-dimensionality)
- [https://stackoverflow.com/questions/16995071/numpy-array-that-is-n-1-and-n](https://stackoverflow.com/questions/16995071/numpy-array-that-is-n-1-and-n)

Simply put, they are only different ways to view the same data from the programming perspective. The main difference between a `(n, )` array and a `(1,n)` array in `numpy` is how we use indexing (indices) to access their inner elements
