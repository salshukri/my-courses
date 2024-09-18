# User input

The input() function lets the user enter some values. Once Python receives the input, it assigns that input to a variable to make it useful to work with in the program. The following program asks the user to enter some text, then displays that message back to the user:

```python
message = input("What is your name?\n")
print(f"Your name is: {message}!")
```

When you use the input() function, Python interprets everything tas a string. You may use the int() function, which tells Python to treat the input as a numerical value

```python
from datetime import date
today = date.today()
dob = input("Which year you were born?\n")
dob = int(dob)
print(f"You are: {(today.year - dob)} years old!")
```


# Files

## File Handling

Python uses files to quickly analyse lots of data. There are several functions for file handling that allow users to read from, write into, update and delete files.

## Reading from a file
### Reading an entire file
Reading from a file is particularly useful in data analysis applications and data modification. For example, you can read the content of a text file and rewrite the file with formating that allows a browser to display it. To begin, you need a text file to open and then use the built-in function open() to access that file. The syntax for the function is:

```python
f = open("demofile.txt")
print(f.read())
``` 
The open() function returns a file object, which has a read() method for reading the content of the file that is stored where the program that is being executed is stored:

```python
with open("demofile.txt") as file_object:
  contents = file_object.read()
print(contents)
```

Or you may tell Python exactly where the file is on your computer, regardless of where your Python code is store:

```python
with open("C:\\Users\\alshukrishaymaa\\PycharmProjects\\pythonProject\\demofile1.txt") as file_object:
print(file_object.read())
```
By default the read() method returns the whole text, but you can also specify how many characters you want to return

### Reading line by line

You can return one line by using the readline() method:
```python
with open("demofile.txt") as file_object:
    print(file_object.readline())
```

Or you may use a for loop on the file object to examine each line from a file, one at a time:

```python
file_object = open("demofile.txt")
for line in file_object:
   print(line)
```
When you print each line, you will find blank lines between the lines you have in your file. These lines appear becuase an invisible newline character is at the end of each line in the text file and the print function adds its own new line each time you call it. You may use rstrip() each time the print function is called to eliminate the extra lines
```python
filename = 'demofile.txt'
with open(filename) as file_object:
    for line in file_object:
        print(line.rstrip())
```
It is a good practice to always close the file when you are done with it using close()

## Writing to a file

One of the simplest ways to save data is to write it to a file, this way you can always examine the output after a program finish running and you can share the data with others

### Writing to an empty file

To write text to a file, you need to call open() with a second argument telling Python that you want to write to the file. You can open the file in write mode 'w', read mode 'r', append mode 'a', or a mode that allows you to read and write to the file 'r+'. If you omit the mode argument, Python opens the file in read-only mode by default. The open() function automatically creates the file to write to, if you do the write mode 'w' and the file exists, Python will erase the contents of that file before running the file object

```python
filename = 'programming.txt'
with open(filename, 'w') as file_object:
    file_object.write('I love Python')
```
### Writing multiple lines

```python
filename = 'programming.txt'
with open(filename, 'w') as file_object:
    file_object.write('I love Python\n')
    file_object.write('I love programming\n')
```

## Appending to a file

Append mode can be used to add content to a file instead of writing over existing content. Any lines you write to the file will be added at the end of the file. If the file does nto exists, Python will create an empty file 


```python
filename = 'programming.txt'
with open(filename, 'a') as file_object:
    file_object.write('I love Python\n')
    file_object.write('I love programming\n')
```


2024 Summer Python Programming Bootcamp - In-class Exercise  
========

Task 1:
Write a program that prompts the user for their name. The respond should be written to a file called guest.txt.

Task 2:
Write a while loop that prompts users for their name. When they enter their name, add a greeting line recording in a file called guest_book.txt. Make sure each entry appears on a new line in the file. Loop should stops when the name 'quit' is entered
