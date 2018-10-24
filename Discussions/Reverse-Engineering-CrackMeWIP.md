# Reverse Engineering and CrackMes 
### 18 October 2018

## Intro
When you compile source code from any _high level_ programming
language, the executable file that is produced usually cannot be 
read in a standard text editor. To read an executable file, you 
need a debugger like _IDA_ to translate the file into a readable
format. 

## The Stack
In Assembly, there exists a stack, whih is pointed to by the
register `ESP`. The stack works in the _First-in Last-out_ order.
This means that the first element or instruction to get added to
the stack will be the last element or instruction to be removed or
executed. This is similar to a stack of cups. The last cup to be added
to the stack of cups is the first cup to be removed. 

Assembly, and other programming languages with a built in stacks, has
two commands related to the stack. These commands are `push` and `pop`,
which add and remove things to and from the stack respectively. This is
especially useful in Assembly, as the stack can be used as a backup for
values when calling functions that modify the value(s) contained in some
register(s). Below are two example programs, showing how the stack can be
useful in Assembly.

_Python_
```python
#Print an array in reverse order
def main():
	myArray = [1,2,3,4,5]
	for x in range(1,6):
		print myArray[-x]
main()
```

_Assembly_
```assembly
.data
myArray BYTE 1h,2h,3h,4h,5h

.code
main:
	mov ecx, 5
	mov esi, OFFSET myArray
myLoop:
	push [esi]
	inc esi
	loop myLoop
Printer:
	mov ecx, 5
myLoop2:
	pop al
	;writeHexByte is not a predefined Assembly function
	call writeHexByte ;Prints hex value of AL to terminal
	loop myLoop2
```

As you can see in the Assembly example, the stack was accessed to save the 
values of `myArray`. This saved us from having to perform a bunch of arithmatic
with the indexing of the elements of the array.

## Reverse Engineering with IDA Debugger
The CrackMe that was used for this meeting can be found [here](https://crackmes.one/crackme/5b8a37a433c5d45fc286ad83).
The password for unzipping this file is _crackmes.one_. After unzipping the file, you should
have a file named **rev50_linux64-bit**. Open this file in _IDA Debugger_ and click *OK*. This
should present us with a nice disassembly graph of the program. If you have access to a Mac/Linux
command line, you can actually run this program and see what is says. Do note, that there isn't
always one correct answer with reverse engineering. The one I am covering in this write-up is not
even the way that Jordan, the EHC President, solves it. Alright, so in _IDA_ we see that the main
window splits into two sides. If we look at the left side and scroll all the way down to the bottom
of the graphs we should see the end of the program. If we look at the graphs that are connected to 
the end of the program we can see that one has comments in _Assembly_ denoted by `;`. This allows us
to see that the flag is found in this portion of the function. We then follow the line that points from
this box to the one above it and analyze it. In this portion, we can see the line `cmp al, 40h ; '@'`.
This line means that there is a comparison of the contents of the register *AL* with _40h_, which is
hexadecimal for the number 64, and also is the ASCII value for "_@_". This could possibly mean that
the password for this program needs to have an "_@_" in it. We then take a similar process for the next
portion of the function. In the next portion, we can find the lines `call _strlen` and `cmp rax, 0Ah`.
These two lines are pretty simple to understand. There is a call to the function _strlen_, which returns
the length of a string, in this case to the register *RAX*. Then, there is a `cmp` call to see if *RAX*
has the value of _0Ah_. In hexadecimal, base 16, the letters _A - F_ symbolizes the numbers 10 - 15 respectively.
This means that the string must be of length 10. So, we now know that there is a string that must be of length
10 and have an _@_ in it. If you had the chance to run the program before opening it in _IDA_, you would know
that the program takes in a string as a parameter, so we can pass in a string of length 10 and has all _@'s_ to
see what we get. Running the program, and passing in a string that meets these qualifications solves this CrackMe.
Now, we are all professional _sreenignE esreveR_. 