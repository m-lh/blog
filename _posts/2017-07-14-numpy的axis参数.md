
# numpy的axis参数理解

numpy中很多操作都有`axis`参数，以前使用的过程中都是现场实验，到底需要设置哪一个轴，今天终于受不了了，下决心搞懂这个问题。

比如一个多维数组，


```python
import numpy as np
a = np.array([[3,2,1],[6,5,4]])
a
```




    array([[3, 2, 1],
           [6, 5, 4]])



求每一行的和，需要


```python
np.sum(a, axis=1) # axis=-1也行
```




    array([ 6, 15])



求每一列的和，需要


```python
np.sum(a, axis=0)
```




    array([9, 7, 5])




```python
np.sum?
```

Docstring:
Sum of array elements over a given axis.

Parameters
----------
axis : None or int or tuple of ints, optional

    Axis or axes along which a sum is performed.  The default,
    axis=None, will sum all of the elements of the input array.  If
    axis is negative it counts from the last to the first axis.

    If axis is a tuple of ints, a sum is performed on all of the axes
    specified in the tuple instead of a single axis or all the axes as
    before.

关键是这一句
> Axis or axes along which a sum is performed.

翻译过来
> 沿着axis指定的轴进行求和(sum)。

这个轴是如何指定的呢？这就是比较重要的地方。



sum时是如何取值的。
- `axis==1`时， 求和的部分是`a[0,:]`和`a[1,:]`；
- `axis==0`时，是`a[:,0]`、`a[:,1]`和`a[:,2]`分别求和。

即axis的值的设置，使得求和变为`array[axis_0, axis_1, axis_2, ...,]`中某一个轴使用`:`代替一样。

下面自己实现一下求和函数，不借助自带的axis参数，实现各个轴的相加


```python
def get_op_target_index(array, axis):
    ind = [range(k) for k in a.shape]
    ind[axis] = [None]
    array_indices = itertools.product(*ind)
    for index in array_indices:
        index = list(index)
        index[axis] = slice(a.shape[axis])
        yield tuple(index)

def mysort(a, axis):
    ind = [range(k) for k in a.shape]
    ind[axis] = [None]
    array_indices = itertools.product(*ind)
    for index in array_indices:
        index = list(index)
        index[axis] = slice(a.shape[axis])
        a[index].sort()
        
def mysum(a, axis):
    ind = [range(k) for k in a.shape]
    ind[axis] = [None]
    array_indices = itertools.product(*ind)
    for index in array_indices:
        index = list(index)
        index[axis] = slice(a.shape[axis])
        a[index] = a[index].sum()
```


```python
print(list(get_op_target_index(a,1)))
```

    [(0, slice(None, 3, None)), (1, slice(None, 3, None))]
    


```python
mysort(a, 1)
a
```




    array([[1, 2, 3],
           [4, 5, 6]])


