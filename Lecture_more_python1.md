# Advanced Python

This lecture will be based on the book *Automate the boring stuff with Python* by Al Sweigart

## Exception handling

Real life programs will crash, but having an idea of what causes the problem and determine whether is worth interrupting 
the flow of the program is managed by *exceptions*.

Let's start with a very simple program

```python
def silly_division(denominator):
	return 100/denominator
```
Obviously, *silly_division(0)* will originate a **ZeroDivisionError**

Instead, 

```python
def silly_division(denominator):
	try:
		return 100/denominator
	except ZeroDivisionError:
		print("Error:Invalid Denominator")
```

There are many "exceptions" called by the interpreter.
For instance **KeyboardInterrupt** is the equivalent to Ctrl+C. This combination can be used to break infinite loops 
for the user

```python
except KeyboardInterrupt:
	sys.exit()
```
We can raise exceptions ourselves in the program. For instance when user supplies the wrong type of input

```python
raise Exception('This is an important message for the user')
```
If it is not handled with a try/except this will cause the program to crash

Here is a book program

```python
def boxPrint(symbol, width, height):
    if len(symbol) != 1:
        raise Exception('Symbol must be a single character string.')
    if width <= 2:
        raise Exception('Width must be greater than 2.')
    if height <= 2:
        raise Exception('Height must be greater than 2.')

    print(symbol * width)
    for i in range(height - 2):
        print(symbol + (' ' * (width - 2)) + symbol)
    print(symbol * width)

for sym, w, h in (('*', 4, 4), ('O', 20, 5), ('x', 1, 3), ('ZZ', 3, 3)):
    try:
        boxPrint(sym, w, h)
    except Exception as err:
        print('An exception happened: ' + str(err))
```

## Logging

We have used previously the *print* statement as a way to check our program as it runs. The module **logging** will
help us to properly handle the programs' progress.

```python
import logging
logging.basicConfig(level=logging.DEBU, format='%(asctime)s - %(levelname)s-$(message)s')
``` 
When Python logs s and event, it creates a *LogRecord* object that holds information about that event.

```python
import logging
logging.basicConfig(level=logging.DEBUG, format='%(asctime)s - %(levelname)s - %(message)s')
logging.debug('Start of program')

def factorial(n):
    logging.debug('Start of factorial(%s%%)' % (n))
    total = 1
    for i in range(1, n + 1):
        total *= i
        logging.debug('i is ' + str(i) + ', total is ' + str(total))
    return total
    logging.debug('End of factorial(%s%%)' % (n))

print(factorial(5))
logging.debug('End of program')
```
It prints out

```bash
2021-11-10 12:26:36,302 - DEBUG - Start of program
2021-11-10 12:26:36,302 - DEBUG - Start of factorial(5%)
2021-11-10 12:26:36,302 - DEBUG - i is 1, total is 1
2021-11-10 12:26:36,302 - DEBUG - i is 2, total is 2
2021-11-10 12:26:36,302 - DEBUG - i is 3, total is 6
2021-11-10 12:26:36,303 - DEBUG - i is 4, total is 24
2021-11-10 12:26:36,303 - DEBUG - i is 5, total is 120
120
2021-11-10 12:26:36,303 - DEBUG - End of program
```

Logging levels

| Level     | Logging function    | Description |
| --------|---------|-------|
| DEBUG  | logging.debug()   | The lowest level. Used for small details.   |
| INFO | logging.info() | Used to record information on general events |
| WARNING | logging.warning() | Used to indicate a potential problem |
|ERROR| logging.error() | Used to record an error |
|CRITICAL | logging.critical() | Use to indicate a fatal error|


So, you can use it in your program 

```python
import logging
logging.basicConfig(level=logging.DEBUG, format='%(asctime)s-%(levelname)s -%(message)s')
#logging.dissable(logging.DEBUG)

logging.debug('some details here')
logging.info('This module is working')
logging.warning('A warning is about to be recorded')
logging.error('Ups an error has occured')
logging.critial('The problem is critical')
```
You can also save the logs into a file

```python
logging.basicConfig(filename='myprogramlogs.txt',level=logging.DEBUG, format='%(asctime)s-%(levelname)s -%(message)s')
```

## The time Module

The Unix *epoch* is a time commonly used in programming. It refers to 12 AM on January 1st, 1970 (UTC). 
The **time.time()** function returns the number of seconds since that moment until the time function is invoked. 
This number is called **Epoch timestamp**.

```python
import time
def calcProd()
    product=1
    for i in range(1,100000):
        product=product*i
    return product
start_time=time.time()
prod=calcProd()
end_time=time.time()
print(f"took {end_time-start_time} seconds to calculate the product")
```

### The datetime Module

```python
import datetime
dt=datetime.datetime.now()
print(dt.year)
```

Converting datetime objects to strings

```python
import datetime
nov10th=datetime.datetime(2021,11,10,13,1,0)
print(nov10th.strftime("%y/%B/%d %H:%M:%S"))
```

## Dictionaries Revisited

This part of the lecture is based on the book *Learning Python* by Mark Lutz

Dictionaries are the most flexible build-in data type in Python. 

> Dictionaries are unordered collections, and items are stored and fetched by key, instead of a positional argument.

Some properties of the dictionaries are:

+ Unordered collections of arbitrary objects. Do not assume that the items are in a particular order.
+ Variable length, heterogeneous, and arbitrarily nestable
+ Tables of object references (hash tables). Internally, dictionaries are implemented as has tables. Python employs optimized hashing algorithms to find keys, so retrieval is quick.

```python
D={}
D={'one':1,'two':[1,2]}
D=dict(me='Horacio', class=5003) ## Why this one does not work?
D.keys()
D.values()
D.items()
D[time]='1:00 pm' ## Why I get an error here?
D['my list']=['1','2','3']
del D['my list']
D1={x: x**3 for x in range(2)}
D2={x: (x**3) for x in range(2)}
D3={x: (x**3,) for x in range(2)}
D4={x: [x**3] for x in range(2)}
D5={x: [x**3,] for x in range(2)}
```

If you don't have a key, Python responds with *None*, but if you want to avoid that, you can give a default output (Note. 
It does not change the dictionary).

```python
D1.get(3) ## this key does not exists
print(D1.get(3))
D1.get(3,27)
print(D1.get(3,27))
```

The *update* method is the closest to a concatenation for dictionaries. 

```python
D_1={2:8}
D1.update(D_1)
print(D1)
```
### Using dictionaries for sparse structures

Since list are indexed, we cannot avoid adding extra *stuff* that may or may not be needed. 

A sparse matrix is a typical example of that. The following matrix is the identity of size n, and has a 1 in the upper corner.

![sparse](https://latex.codecogs.com/gif.download?A%3D%5Cleft%28%20%5Cbegin%7Bmatrix%7D%201%20%26%200%20%26%20%5Ccdots%20%26%201%5C%5C%200%20%26%201%20%26%20%5Ccdots%20%26%200%20%5C%5C%20%5Cvdots%20%26%20%5Cvdots%20%26%20%5Cddots%20%26%20%5Cvdots%20%5C%5C%200%20%26%200%20%26%20%5Ccdots%20%26%201%20%5Cend%7Bmatrix%7D%20%5Cright%29)

You can store this matrix as

```python
n=10
matrix_A={}
for i in range(1,n+1):
    matrix_A[(i,i)]=1
matrix_A[(1,n+1)]=1
```

## Revisiting Tuples

What are tuples

+ Ordered collections of *arbitrary objects*
+ They can accessed by offset operations (e.g., slicing or indexing)
+ Immutable sequence
+ Fixed length, heterogenenous and arbitrarily nestable


```python
T=(0,)
T1=(0,(0,),(0,0))
T1[2][1]
T2=('a','b','c','d',1)
T2.index(1)
T3=('a','b','a')
T3.count('a')
```

Other properties

+ concatenation
```python
T4=(1,2)+(3,4) 
```
+ repetition
```python
T5=(1,2,3,4)*3
```
+ slicing
```python
T5[0],T5[2:10]
```

And more about mixing structures

```python
T6=(1,[1,2],3)
T6[1]="hey you" ## what error you are getting?
T6[1][0]="hey you"
```

## Hashing

*Hashing* refers to the mapping of an object to an integer, known as a *hash code*. We refer to an object as *hashable* if it satisfies the 
following properties

+ The object can be compared for equality with other objects via the == operator
+ Whenever two objects compare as equal, they have the same has code.
+ The object hash code does not change during its lifetime.

```python
a="BMIG 5003"
b="BMIG 5003"
c="UAMS"
hash(a),hash(b),hash(c)
```

## Testing

The content on this part will come from the book *Advanced Guide to Python 3 Programmin* by John Hunt

There are at least two ways of thinking about testing:

1. Is the process of executing a program with the intent of finding errors/bugs.
2. It is a process used to establish that software components fulfill the requirements identified for them, that is that they do what they are supposed to do.

These two aspects of testing tend to have been emphasized at different points in the software lifecycle. 
Error testing is an intrinsic part of the development process, and and increasing emphasis is being placed on making testing a central part of software development.

It should be noted that it is extremely difficult -- and in many casess -- impossible to prove that software *works* 
and is completely *error free*. The fact 
that a set of tests find no defects does not prove that the software is error-free. 

> Testing shows the presence, not the absence of bugs. Dijkstra

We have several types of testing:

+ Unit Testing. It is used to verify the behavior of individual components.
+ Integration Testing. It is used to check that individual components are combined together to provide higher-level functional units, that the combination of the units operates appropriately.
+ Regression Testing. It is used to check when new components are added to the system, or existing components are changed and whether the new functionality does not break the existing functionality.

![testing](../../Figures/fig_software_testing.png)

There are several modules that allow you to unit test your code. One of them is pytest. 

## Directory Structure and the **pathlib** module

Recall that in the Unix directory structure a *path* is a combination of directories that lead to a specific file or even directory.

We call an *Absolute Path* if it provides the complete sequence of directories from the root of the filesystem to a specific directory
or file.

In Windows it may look something like *C:\Users\gomezacevedohoracio* whereas in Linux will be something like */home/horacio*.

A *Relative Path* provedes a sequence from the current directory to a particular subdirectory or a file.

The *pathlib* module provides a set of classes representing filesystem paths. A *Path* object contains methods that allow the path 
to a file or directory. 

```python
mypath=Path('.') ## creates a Path object
print(f"Does the current directory exists:{mypath.exists()}")
print(f"Is mypath actually a directory?: {mypath.is_dir()}")
print(f"Is mypath actually a file?: {mypath.is_file()}")
print(f"What is the absolute path?: {mypath.absolute()}")
```

Other methods

|name | function |
|-----|-------------|
| *mkdir()* | create a directory path if it does not exists already |
| *rmdir()* | remove an empty directory |
| *rename(target)*| rename the file or directory |
| *unlink()* | removes the file referenced by the path
| *joinpath(*other)*| appends elements to the path object |
| *cwd()* | returns a new path object representing the current directory |
| *home()* | returns a new path object representing the user's home directory |
|*glob(pattern)*| returns all the files with the given pattern in the current path|

The '/' operator can be used to create a new path object with the name changed. 

```python
mypath=Path.cwd() ## current directory
newdir= mypath / 'test1'
newdir.mkdir()
## you can remove the directory 
# new.dir.rmdir()
newfile= newdir / 'text_1.txt'
newfile.write_text("hello")
newfile.unlink()
```

## Module **argparse**

This section follows the official Python documentation <http://docs.python.org>  

Remember that Python is totally capable from reading from the user. There is a nice module to make a *friendly* user interaction. 
The analog will be the *man* command in Linux or *help* command in Windows. 

Some definitions:

+ Argument. It is a string in the command line passed by the shell to *execl()* or *execv()*. In Python, arguments are elements of 
    *sys.argv[1:]*.
+ Option. It is an argument used to supply extra information to guide or customize the execution of the program. For instance *-v* or *-h*. However, some of these options can include whole words.
+ Option argument. It is an argument that follows an option. For instance *--file myfile.txt*.

Let's see one example 


```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("-v", "--verbose", help="increase output verbosity",
                        action="store_true")
parser.add_argument("-c", "--lectures", type=str, help="provide the name of the lecture")
parser.add_argument("-o", "--other", type=int, default=10)
    args = parser.parse_args()
if args.verbose:
    print("verbosity turned on")
if args.lectures:
    print(f"This is the {args.lectures} ")
    print(f"How many times {args.other}")
```






