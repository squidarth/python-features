# Intermediate to advanced Python features

### Passing functions as first-class objects

Just as numbers and strings can be passed to functions as arguments, functions themselves can be passed as well!

```python
def double(x):
    return x * 2
    
def copy_list_with_operation(operation, ls):
    new_ls= []
    for item in ls:
        new_ls.append(operation(item))
    return new_ls
    
copy_list_with_operation(double, [1,2,3,4])
```

### Lambda

In the above example, I called `copy_list_with_operation` with the name of the function that we want to apply to every element of the list.  `lambda` is a python keyword that allows you create a function quickly without a name.

```
def copy_list_with_operation(operation, ls):
    new_ls= []
    for item in ls:
        new_ls.append(operation(item))
    return new_ls
    
copy_list_with_operation(lambda x: x * 2, [1,2,3,4])
```


### Decorators

### with/yield

### Generators
