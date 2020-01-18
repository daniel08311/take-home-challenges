# Problem 1 - Implementing a Pipe Function

## Fisrt we define pointed_function and pipe_function
* pointed_function takes an int val and return val + 1
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
# val is an int which is not the type another_pointed_function requires, pipe_function should return None
assert(pipe_function(5, another_pointed_function) == 6)
```

    type not matched



    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-8-ead12240a17e> in <module>()
          1 # should raise assertion error
          2 # val is an int which is not the type another_pointed_function requires, pipe_function should return None
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

    testing with value -1487017168, 586831 functions
    testing with value 2693496529, 947693 functions
    testing with value -2553326158, 248183 functions
    testing with value -3979218788, 987666 functions
    testing with value -57060179, 1015368 functions
    testing with value 3651520910, 991652 functions
    testing with value -3729234103, 995530 functions
    testing with value -2464122823, 1005271 functions
    testing with value 656447235, 758925 functions
    testing with value 533682194, 178175 functions
    testing with value -264717116, 844268 functions
    testing with value 1240250745, 361737 functions
    testing with value 3403622094, 597972 functions
    testing with value 347990739, 309815 functions
    testing with value 204102520, 118387 functions
    testing with value 1944457386, 957899 functions
    testing with value 3099451883, 690779 functions
    testing with value 224677826, 406420 functions
    testing with value 2513118019, 565100 functions
    testing with value -1819411429, 621440 functions
    testing with value 2042169542, 510541 functions
    testing with value 1781656454, 703975 functions
    testing with value -2443023453, 468046 functions
    testing with value -1513639713, 552119 functions
    testing with value -4171112937, 70873 functions
    testing with value -2173867874, 254841 functions
    testing with value -1916773955, 1006030 functions
    testing with value 837353881, 893861 functions
    testing with value 3193232788, 655817 functions
    testing with value 1724963963, 479676 functions
    testing with value 2850451056, 720303 functions
    testing with value 1229381414, 516822 functions
    testing with value 537123441, 1040784 functions
    testing with value 756021950, 867658 functions
    testing with value 4186327015, 598393 functions
    testing with value 3947233852, 545627 functions
    testing with value -1916289400, 808258 functions
    testing with value -2609902056, 619770 functions
    testing with value -2737399536, 203270 functions
    testing with value 2826214126, 203019 functions
    testing with value 1204409288, 918956 functions
    testing with value -3393998811, 988490 functions
    testing with value -3385209075, 796388 functions
    testing with value 3319971832, 155341 functions
    testing with value -3269703613, 184135 functions
    testing with value 1187758972, 619168 functions
    testing with value 1251896864, 194716 functions
    testing with value -2942465814, 560011 functions
    testing with value 1357156273, 614156 functions
    testing with value -2183224822, 412663 functions


## Test cases passed withour errors
* I tested around 1000 on my laptop and passed also.
* Note that passing in too many functions into pipe_function results in memory error
