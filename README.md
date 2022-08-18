# PA 2 - All Caps

## Due 
All files are due on Wednesday, August 24th at 11:59PM.

See "Grading” and “Submission” sections for more details.


## Overview
In this assignment, you will write a program to convert a pre-defined string to upper-case **all in assembly**.

The educational purpose of this programming assignment is to introduce you to writing in ARM assembly.


## Tutorials and readings

Here are a few helpful links in case you need to read up to get familiar with the languages and tools.

- Unix Tutorial http://www.ee.surrey.ac.uk/Teaching/Unix/ 
- Vim Tutorial https://www.openvim.com/ 
- Getting Started-Git Basics http://git-scm.com/book/en/Getting-Started-Git-Basics 
- Git Basics Chapter http://git-scm.com/book/en/Git-Basics 
- Debugging with gdb http://www.delorie.com/gnu/docs/gdb/gdb_toc.html

## Style Guidelines 
We will follow these style guidelines for every assignment in this class. Please familiarize yourself with them. If you have any questions or concerns, you can ask on edstem. We won't grade your code on styles though.
http://cseweb.ucsd.edu/~ricko/CSE30StyleGuidelines.pdf

## Getting Started

### Files

For this assignment **there are no starter files.** You will be writing everything from scratch. 

First create and enter the pa2 working directory.
```
$ mkdir ~/pa2
$ cd ~/pa2
```

Then to create your file you can do the following:

```
$ touch allcaps.s
```

### Compiling

To compile, you can simply just do 

```
$ gcc -g -o allcaps allcaps.s
```

`-g` pretty much means allow debugging and `-o` means name the executable the argument that follows.

### Preparing Git Repository
You are required to use Git with this and ALL future programming assignments. See PA1 for how to set up your repository. **You must make your GitHub repository private if you choose to use GitHub.**


## Output

There is no sample executable for this assignment. All you need to check is the following:
- All lowercase letters are converted to uppercase. 
- All other characters remain the same.
- No characters are deleted. 
- No characters are added. 

#### Warnings
- It is possible to write this assignment as a C program, compile the C file into an assembly file with the `-S` flag, and submit the resulting assembly file into Gradescope. **Do not do this. This will result in a 0 for your correctness grade.** It very is easy to distinguish a compiler-written assembly file from a human-written assembly file. 
- We are not giving you any sample outputs, instead you are provided some example inputs. You are responsible for trying out all functionality of the program; the list of example inputs is not exhaustive or complete. It is important that you fully understand how the program works and you test your final solution thoroughly.

**Strings you might want to test:**
```
Hello World!
HELLO WORLD!
hello world!
1234567890
abcdefghijklmnopqrstuvwxyz
```

Note: You can assume that the string will only contain ASCII characters. 

### Tip

**Exit Codes**

Hopefully you remember how to print out exit codes from CSE 15L. But just as a refresher, you can print exit codes of programs you just executed by using the following command:

```
cs30s222yx@pi-cluster-001:pa2$ echo $?
```

Here's an example:

```
cs30s222yx@pi-cluster-001:pa2$ ./allcaps
cs30s222yx@pi-cluster-001:pa2$ echo $?
0
```

**Hex Mode**

Sometimes it's hard for us to precisely check the output if it involves some special characters like newlines. In this case, you may want to compare the ASCII values. One way to achieve that is to use `hexdump`. With `hexdump`, a newline will show up as its ASCII value `0a` instead. To do so, you can pipe `stdout` from your excutable into `hexdump`:

```
cs30s222yx@pi-cluster-001:pa2$ ./allcaps | hexdump
```

Here's an example using the sample message `Hello World!\n`

```
cs30s222yx@pi-cluster-001:pa2$ ./allcaps | hexdump
00000000  48 45 4c 4c 4f 20 57 4f 52 4c 44 21 0a
0000000d
```


## Detailed overview

For this assignment, there is only one function to write. It is a main function whose function prototype is as follows

```c 
int main(); // (in the file allcaps.s)
```

## Detailed Implementation
Below is the sole module to be written. **Please read the entire writeup before writing any code.**


### allcaps.s
```c 
int main();
```

This is the main driver of your program. Its main task is to convert the string's letter case.

**Detailed overview**

The file should be structured in a way similar to what you learned in class. The following is a very rough outline for how your file should look:

```ARM
@ Describe the hardware to the assembler

@ Declare any external functions

@ Define any constants

@ Start data segment where you will define your string

/* !!!!!!!!!!!!!!!!!!!!!!!!!!!!!! IMPORTANT !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!  */
/* You MUST name the string you will convert as "mesg" (without the quotes) */
/* Also, do NOT have any spaces between "mesg" and ":" when defining your   */ 
/* string. It should look like "mesg:" exactly.                             */

@ Start text segment and begin writing your function

```

**It is extremely important you label your string as `mesg` and not have any spaces between `mesg` and `:` when defining your label. It should look like `mesg:` exactly.** Failure to do so will result in a loss of most of the points you can earn. At submission time on Gradescope, we will tell you if you have done this correctly, so keep an eye out for that.

The string itself can be whatever you like, as we will replace it while grading. This means you should change `mesg` to different strings to test all possible cases.

Now let’s take a closer look at each step of the program. 

1. Go through the string `mesg` letter by letter.

    1. Change the letter case accordingly.
    2. Print the new character using `putchar()`
    
2. Return `EXIT_SUCCESS`.


**Notes:** 
- `EXIT_SUCCESS` is defined to be 0.
- We will be testing that your program exits with the correct exit code. Since there is no checking for errors that will terminate your program early, the only exit code should be `EXIT_SUCCESS`
- Consider which register you want to use to store the information that tells you your position in the string while looping, as data in certain registers may not be available to you after calling `putchar()`.
- You are **NOT** allowed to use any external functions (like `toUpper`) except for `putchar`.
- You should only print out the actual string value. That's said, you should not print out the null terminator. (Remember a string in C is an array of `char`s ended with a null terminator. ) You may use the method described in *Hex Mode* section to help you test your code.

## Grading

#### Style: 0 points (won't be graded)
- See style requirements [here](http://cseweb.ucsd.edu/~ricko/CSE30StyleGuidelines.pdf).
- You are required to style `allcaps.s`.
- Tabs are 8 characters that will count towards the 80-character limit. If you are using tabs, please set your size accordingly so that you will not get points marked off for lines being too long.
- For this assignment, your function header does not require a "Stack variables" section.

#### Correctness: 100 points
- We allocate 1 point towards the correct formating of the string label as an indication of whether or not you have labeled your string correctly. You will be able to see this case upon submission.
- Just like PA1 we will be providing all test cases.
- Correctness will be graded by the following
    -  Correct output.
    -  Correct exit code (just 0).

#### Compile!
- If what you turn in does not compile with the given command, you will receive 0 points for functionality. Do not modify the given command except when specified.


## Submission

**Super important warning:** If your code does not compile or crashes immediately when run, we will not be able to grade it or reward you functionality points.

Log in to gradescope.com using your @ucsd.edu email address, select the CSE 30 course, assignment PA 2, and upload the following files.  Your file names must match the below *exactly* otherwise we will not be able to compile your code.

When submitting, please check that Gradescope shows “compile success” to confirm you code compiles on the autograder.

```
allcaps.s
```

If there is anything in these procedures which needs clarifying, or any mistake in this write-up (it's long, we probably made some), please feel free to ask any tutor, the instructor, or post on Piazza.
