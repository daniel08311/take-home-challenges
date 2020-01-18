
# Problem 1 - Implementing a Pipe Function

## Fisrt we define pointed_function and pipe_function
* pointed_function takes a int val and return val + 1
* pipe_function takes a val and an arbitrary number of functions and transform val by those functions 


```python
'''
    function which will be passed into pipe function
'''
def pointed_function(val):
    return val + 1

'''
    pipe function continuously transform val
    by calling function(val) for every function 
    passed through args.
    return None if type error occurs
'''
def pipe_function(val, *args):
    
    functions = args
    
    for function in functions:
        try:
            val = function(val)
        except TypeError:
            print("type not matched")
            return None
    
    return val
```

## Let's test some basic cases


```python
# should pass
assert(pipe_function(5) == 5)
```


```python
# should pass
assert(pipe_function(5, pointed_function) == 6)
```


```python
# should pass
assert(pipe_function(5, pointed_function, pointed_function, pointed_function) == 8)
```


```python
# should raise assertion error
# val is a function which is not the type pointed_function requires, pipe_function should return None
assert(pipe_function(pointed_function, pointed_function, pointed_function) == 8)
```

    type not matched



    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-5-b04e74ea3bce> in <module>()
          1 # should raise assertion error
          2 # val is a function which is not the type pointed_function requires, pipe_function should return None
    ----> 3 assert(pipe_function(pointed_function, pointed_function, pointed_function) == 8)
    

    AssertionError: 



```python
# should pass but print type error warning
assert(pipe_function(pointed_function, pointed_function, pointed_function) == None)
```

## Consider if someone passes another function into our pipe function
* another_pointed_function takes a string val and return val + "1"


```python
''' 
    another pointed function which might be passed into pipe function
''' 
def another_pointed_function(val):
    return val + "1"
```


```python
# should raise assertion error 
# val is a int which is not the type another_pointed_function requires, pipe_function should return None
assert(pipe_function(5, another_pointed_function) == 6)
```

    type not matched



    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-7-07622516ddf7> in <module>()
          1 # should raise assertion error
          2 # val is a int which is not the type another_pointed_function requires, pipe_function should return None
    ----> 3 assert(pipe_function(5, another_pointed_function) == 6)
    

    AssertionError: 



```python
# should pass but print type error warning
assert(pipe_function(5, another_pointed_function) == None)
```

    type not matched



```python
# should pass
assert(pipe_function("a", another_pointed_function) == "a1")
```


```python
# should pass
assert(pipe_function("a", another_pointed_function, another_pointed_function, another_pointed_function) == "a111")
```

## Lets generate random test cases and check if all goes well
* I'll use pointed_function as our testing function
* Generate a random val x and a random number of y pointed_function 
* We can easily check if our pipe_function works well if the return of pipe function equals val + y*1
  * Since we have y pointed_functions and each of them adds 1 to val
  * let a = pipe_function(x, pointed_function, pointed_function, pointed_function .... y pointed_functions in total) 
  * let b = x + y * 1
  * a equals b for every combinations of x, y




```python
import random

# generate 100 test cases
test_sample_count = 100

# generate random vals ranging from -2^32 to 2^32
vals = random.sample(range(-2**32, 2**32), test_sample_count)

# generate random function counts ranging from 0 to 2**20 to be passed into pipe_function
function_counts = random.sample(range(0, 2**20), test_sample_count)
```


```python
# generate args given val and function_count
# pass args into pipe_function and test if result is correct
def test_pipe_function(val, function_count):
    print(f'testing with value {val}, {function_count} functions')
    args = [val] + [pointed_function] * function_count
    assert (pipe_function(*args) == (val + (1)*function_count))


for val, function_count in zip(vals,function_counts):
    test_pipe_function(val, function_count)
```

    testing with value 1268103036, 696342 functions
    testing with value 2824769807, 577457 functions
    testing with value -233961626, 600895 functions
    testing with value 1427236925, 852691 functions
    testing with value -1538965011, 957954 functions
    testing with value -650611847, 878535 functions
    testing with value 2627630170, 21751 functions
    testing with value 1336141115, 105238 functions
    testing with value -3517868752, 798820 functions
    testing with value -898954008, 643873 functions
    testing with value 2222048434, 690879 functions
    testing with value -3995473588, 996475 functions
    testing with value 4033561265, 852752 functions
    testing with value 3959656947, 276947 functions
    testing with value -3423085273, 124016 functions
    testing with value 207086340, 794646 functions
    testing with value -2485190428, 1041163 functions
    testing with value -1759851071, 355796 functions
    testing with value 1427910810, 683154 functions
    testing with value -3242598523, 633507 functions
    testing with value -1574822668, 280001 functions
    testing with value 1495926369, 164098 functions
    testing with value -3004536740, 9922 functions
    testing with value -1926466226, 441133 functions
    testing with value 992725471, 38950 functions
    testing with value 1024572112, 813338 functions
    testing with value 2357702510, 350559 functions
    testing with value 2012527312, 556258 functions
    testing with value 4191040448, 590069 functions
    testing with value -1069751501, 202905 functions
    testing with value 4166453622, 74257 functions
    testing with value 751359778, 876184 functions
    testing with value -3405711760, 10198 functions
    testing with value 3322196821, 313213 functions
    testing with value -1124959687, 189363 functions
    testing with value 2673594331, 50188 functions
    testing with value -2022102402, 440502 functions
    testing with value 3078568817, 333322 functions
    testing with value 1217724715, 1042891 functions
    testing with value -1952643061, 301486 functions
    testing with value -22886516, 962306 functions
    testing with value 1099955529, 940381 functions
    testing with value -2403680309, 522367 functions
    testing with value -3032209548, 178142 functions
    testing with value -3428896246, 301248 functions
    testing with value 2918443741, 490660 functions
    testing with value 391214069, 209068 functions
    testing with value 3443564479, 588031 functions
    testing with value -158309414, 105404 functions
    testing with value -1832956657, 520489 functions
    testing with value -3099352813, 220498 functions
    testing with value -2662789229, 507001 functions
    testing with value 2700972853, 1002174 functions
    testing with value -1219063849, 350992 functions
    testing with value -2421164540, 594249 functions
    testing with value -1544601272, 332263 functions
    testing with value 3239729470, 422216 functions
    testing with value -3536876324, 364188 functions
    testing with value -3490755097, 399000 functions
    testing with value 2819298018, 145829 functions
    testing with value 669125296, 682470 functions
    testing with value -73027878, 94569 functions
    testing with value 139616920, 32246 functions
    testing with value 876206861, 809873 functions
    testing with value -723220194, 15389 functions
    testing with value -1855040762, 502686 functions
    testing with value -4238061743, 600561 functions
    testing with value -2233907594, 18513 functions
    testing with value -3323343810, 1020812 functions
    testing with value -2059176726, 172687 functions
    testing with value 2180505666, 105051 functions
    testing with value -3346297448, 3083 functions
    testing with value -2100369411, 392691 functions
    testing with value 2132940544, 431728 functions
    testing with value -3585714438, 1025830 functions
    testing with value 259374689, 369644 functions
    testing with value 4007556642, 756007 functions
    testing with value -3321613198, 481845 functions
    testing with value 4230512690, 1037638 functions
    testing with value 886176075, 1000359 functions
    testing with value -2434824019, 941595 functions
    testing with value 2241568352, 896653 functions
    testing with value 263341665, 1042004 functions
    testing with value -911043570, 819103 functions
    testing with value 2194568395, 811907 functions
    testing with value 1657392914, 1008526 functions
    testing with value -2086273549, 926628 functions
    testing with value 3158340193, 1004670 functions
    testing with value 716724834, 607322 functions
    testing with value -3851220894, 317581 functions
    testing with value 2353756378, 612190 functions
    testing with value -2766343736, 3934 functions
    testing with value -1342722929, 489683 functions
    testing with value 1235833591, 382499 functions
    testing with value 412489173, 946187 functions
    testing with value -1044576536, 895267 functions
    testing with value -608967789, 1023618 functions
    testing with value 746856813, 931517 functions
    testing with value -159878888, 261586 functions
    testing with value -2611584357, 866955 functions


## Test cases passed withour errors
* note that passing in too many functions into pipe_function results in memory error
