# Problem 1 - Implementing a Pipe Function

## Fisrt we define pointed_function and pipe_function
* pointed_function takes an int val and return val + 1
* pipe_function takes a val and an arbitrary number of functions and transform val by those functions 


```python
'''
    The function which will be passed into pipe function.
'''
def pointed_function(val):
    return val + 1


'''
    Pipe function continuously transform val by calling function(val) for every function passed through args.
    Return None if type error occurs.
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
    Another pointed function which might be passed into pipe function
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

# generate random function counts ranging from 0 to 2^20 to be passed into pipe_function
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

    testing with value 126453445, 838933 functions
    testing with value 3552097381, 715128 functions
    testing with value 3157002908, 441291 functions
    testing with value 1418121281, 988080 functions
    testing with value 224453447, 813368 functions
    testing with value -952946194, 537012 functions
    testing with value 2139037987, 943308 functions
    testing with value -3225963461, 453499 functions
    testing with value -1599342218, 854888 functions
    testing with value -1774847758, 958906 functions
    testing with value 2425142407, 346292 functions
    testing with value -3602447569, 335375 functions
    testing with value -453617019, 969865 functions
    testing with value -409538829, 302809 functions
    testing with value -4230366849, 428548 functions
    testing with value 302126716, 718536 functions
    testing with value 1459264484, 7408 functions
    testing with value 1559602848, 422261 functions
    testing with value -2413998361, 23120 functions
    testing with value 488925378, 675118 functions
    testing with value -2854334100, 406206 functions
    testing with value 2061188673, 951002 functions
    testing with value 3393183136, 865951 functions
    testing with value 2435581293, 877498 functions
    testing with value -3384662206, 256649 functions
    testing with value 3950290347, 293668 functions
    testing with value 3026794782, 983125 functions
    testing with value 1866632231, 381830 functions
    testing with value 925771301, 562741 functions
    testing with value 901176383, 490998 functions
    testing with value -3450978800, 482809 functions
    testing with value 3561441880, 44823 functions
    testing with value -3536677507, 902944 functions
    testing with value 1965751287, 333505 functions
    testing with value 2412458972, 559994 functions
    testing with value 2525551795, 11820 functions
    testing with value -2128699853, 327548 functions
    testing with value 3062348068, 546090 functions
    testing with value 3833076686, 843056 functions
    testing with value 2692994041, 809176 functions
    testing with value 3695938609, 776191 functions
    testing with value -777779044, 361666 functions
    testing with value -1343906387, 7960 functions
    testing with value -2791395725, 571103 functions
    testing with value 3558901990, 867906 functions
    testing with value -386323326, 529850 functions
    testing with value 2608842874, 368383 functions
    testing with value -673308514, 548642 functions
    testing with value 2765734590, 159981 functions
    testing with value -2967429951, 586075 functions


## Test cases passed withour errors
* I tested around 1000 on my laptop and passed also.
* Note that passing in too many functions into pipe_function results in memory error (turn down maximum function_counts) 

# Problem 2 - Finding Next Greater Element
* I'll try my best to explain my thoughts. 
* The key points are:
    * Search backwards since we want to find the *smallest* next element, going backwards makes sure that the solution we found are at the smallest possible digit.
    * For a number which is in descending order, e.g 98765, we cannot find a next greater element.
    * We have to find the point at which a number breaks the descending trend.
    * Take 498765 for example, we can see that at the point 4, the descending trend stops.
    * We should now determine what is the smallest number larger than 4 on the right hand side of 9 (including 9) for swapping.
    * Note that we can not consider numbers on the left hand side of 9 since it effects the larger digits.
    * Now that we swapped 4 and 5, the number become 598764
    * We can safely assume that the numbers on the right hand side of 9 is still in descending trend
    * Since it's in descending trend, reversing numbers on the right hand side of 9 returns the smallest number possible, 598764 -> 546789.
    * For a negative number, all remains the same excepts that now we stop when a number breaks the *ascending* trend. 
    * I'll explain more for the negative case during interview.

* The detailed steps goes like this:
    * Consider a number 6876
    * First transform it into an int list [6,8,7,6]
    * Look at the list *backward*
    * Now we are at the rightmost number 6
    * Check if number 6 is larger than it's left neighbor 7
    * Since it's not, we move on and look at 7 with its left neighbor 8
    * Stop at 8 since its left neighbor 6 is smaller than 8
    * Find the smallest number on the right hand side of 8 (including 8) which is larger than 6
    * The number should be 7
    * Swap the leftmost 6 with 7 and get 7866
    * Now reverse the order of numbers on the right hand side of 8 (including 8) 
    * We get 7668 
    * Problem Solved


```python
''' 
    The algorithm takes in a num and a isNegative flag(1 for num > 0, -1 for else) as inputs
    Let n be the digit length of num
    The first for statement will iterate for a maximum of n-1 steps
    The second for statement will only be triggered once and the maximum iterations is n-2 steps
    Combined time complexity should be O(n-1 + n-2) = O(n)
'''
def next_greater_number(num, isNegative=1):
    
    # turn num into string first  
    num_string = str(num)
    
    # reverse num string since we want to search from the last digit
    num_string_reverse = num_string[::-1]
    
    # turn reversed string in to list of int number
    reverse_list = [int(char) for char in num_string_reverse]

    length = len(num_string)
    
    for i in range(length - 1):
        
        # continue searching for i if reverse_list[i] isn't larger than reverse_list[i+1]
        if isNegative*reverse_list[i] <= isNegative*reverse_list[i+1]:
            continue
            
        # the swapping part
        for j in range(i+1):
            if isNegative*reverse_list[j] > isNegative*reverse_list[i+1]:
                reverse_list[j], reverse_list[i+1] = reverse_list[i+1], reverse_list[j]
                result = reverse_list[i+1:length][::-1] + reverse_list[:i+1]
                return isNegative * int("".join([str(digit) for digit in result]))

    return None
```


```python
'''
    Simple run function to run our algorithm.
'''
def run_algo(number):
    if number < 0:
        return(next_greater_number(-number, -1))
    return(next_greater_number(number))
```

## The testing phase

### Test for the basic cases given in the email


```python
assert(run_algo(123) == 132)
assert(run_algo(5566) == 5656)
assert(run_algo(-3310) == -3301)
```

### Test for ascending, descending and flat scenario for positive numbers


```python
assert(run_algo(123456789) == 123456798)
assert(run_algo(987654321) == None)
assert(run_algo(11111111) == None)
```

### Test for ascending, descending and flat scenario for negative numbers


```python
assert(run_algo(-123456789) == None)
assert(run_algo(-987654321) == -987654312)
assert(run_algo(-11111111) == None)
```

### Test for mountain shaped numbers (middle digits larger than digits on two sides)


```python
assert(run_algo(135765431) == 136134557)
assert(run_algo(-135765431) == -135765413)
```

### Test for V shaped numbers (middle digits smaller than digits on two sides)


```python
assert(run_algo(96432455) == 96432545)
assert(run_algo(-96432455) == -96425543)
```

## Short Summary

* We have tested for ascending, descending, flat, V shaped and mountain shaped numbers.
* We can assume that all other shape, e.g W shaped and M shaped, will do fine since they are just a combination of the tested shape.

# This is the end of my answer for the two problems
# Thanks again for putting effort into reading it !
