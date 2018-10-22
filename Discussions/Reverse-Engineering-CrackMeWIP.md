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