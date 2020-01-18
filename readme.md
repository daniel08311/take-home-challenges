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

    type not matched


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

    <ipython-input-8-07622516ddf7> in <module>()
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

## Let's generate random test cases and check if all goes well
* I'll use pointed_function as our testing function
* Generate a random val x and a random number of y pointed_functions
* We can easily check if our pipe_function works well if the return of pipe function equals val + y*1
  * Since we have y pointed_functions and each of them adds 1 to val
  * let a = pipe_function(x, pointed_function, pointed_function, pointed_function .... y pointed_functions in total) 
  * let b = x + y * 1
  * a equals b for every combinations of x, y




```python
import random

# generate 50 test cases
test_sample_count = 50

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
    assert (pipe_function(*args) == (val + 1*function_count))


for val, function_count in zip(vals, function_counts):
    test_pipe_function(val, function_count)
```

    testing with value 623633358, 698135 functions
    testing with value -1597983847, 418287 functions
    testing with value -2878385124, 825604 functions
    testing with value -1611495882, 26685 functions
    testing with value -3079479769, 37926 functions
    testing with value -3832672477, 715285 functions
    testing with value -1931166832, 48664 functions
    testing with value -3713785096, 652057 functions
    testing with value -2046658973, 719168 functions
    testing with value 3969309985, 102809 functions
    testing with value -424417175, 348120 functions
    testing with value 1363437804, 385543 functions
    testing with value 4019401431, 17643 functions
    testing with value -4048488091, 844543 functions
    testing with value 3750048336, 242832 functions
    testing with value -3736889990, 640987 functions
    testing with value -2595198182, 403992 functions
    testing with value -1541607536, 693191 functions
    testing with value 2349772400, 593352 functions
    testing with value -310104137, 337974 functions
    testing with value -1658318705, 77584 functions
    testing with value 2406904483, 11098 functions
    testing with value 2777056533, 561121 functions
    testing with value 3741511574, 970079 functions
    testing with value 2756324031, 469625 functions
    testing with value 1872869435, 675528 functions
    testing with value -4153755873, 629358 functions
    testing with value -3599943383, 908890 functions
    testing with value -3615845288, 564860 functions
    testing with value 224070919, 741701 functions
    testing with value 2481725282, 947238 functions
    testing with value 3505227312, 947950 functions
    testing with value 806279481, 15775 functions
    testing with value 3243937719, 782755 functions
    testing with value -3878248184, 255968 functions
    testing with value 885638340, 116704 functions
    testing with value 933580169, 1008039 functions
    testing with value 40443009, 676291 functions
    testing with value 3350978989, 177754 functions
    testing with value -464772846, 355612 functions
    testing with value -2585168860, 375342 functions
    testing with value -3027969027, 244182 functions
    testing with value 2007946605, 501396 functions
    testing with value -1790859632, 635304 functions
    testing with value 1386701282, 90445 functions
    testing with value -2297988911, 262735 functions
    testing with value -3880272748, 766388 functions
    testing with value -3435621770, 670038 functions
    testing with value -3169637667, 452929 functions
    testing with value -3796042156, 734789 functions


## Test cases passed withour errors
* I tested around 1000 on my laptop and passed also.
* Note that passing in too many functions into pipe_function results in memory error
