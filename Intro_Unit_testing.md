# Introduction to Unit Testing


## Testing

The content on this part will come from the book *Advanced Guide to Python 3 Programming* by John Hunt

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

## PyTest

PyTest ([www.pytest.org](https://docs.pytest.org/en/latest/)) is a testing library for Python very popular among developers. When invoked, it automatically finds tests based on a naming convention and it is very portable. 

You can install *PyTest* in your environment with *pip*

```bash
pip install -U pytest
```

Once it is installed in your environment, you need to create a file pre-fixed with *test_*. 

We need to add a file that contains a function that will perform the test.

> It is necessary that inside your file there is at least one **assert** statement to verify the output.

For instance, if your original file is named *foo.py* and this file contains a function 
that you want to run the test on (say the function is called *fii*), then you 
have to create a separate python test file (e.g., *test_fii.py*)
in which you will have at least one *assert* statement. Then, you can run 
your test as 

```bash
pytest test_fii.py
```

### Parametrized tests.

If you want to run the same tests multiple times with several different input values, then you can use 

```bash
@pytest.mark.parametrize
```

### Assertions and beyond

Pytest has a very rich set of features, for instance you can make assertions about
expected exceptions (e.g., division by zero). 
The applicability of pytest is also to verify consistency among classes, 
and check methods within them. 
However, this goes beyond the aim of this class. 