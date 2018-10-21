# Reverse Engineering and CrackMes 
### 18 October 2018

## Intro
When you compile source code from any _high level_ programming
language, the executable file that is produced usually cannot be 
read in a standard text editor. To read an executable file, you 
need a debugger like _IDA_ to translate the file into a readable
format. 

## The Stack
In Assembler, there exists a stack, whih is pointed to by the
register `ESP`. The stack works in the _First-in Last-out_ order.
This means that the first element or instruction to get added to
the stack will be the last element or instruction to be removed or
executed. This is similar to a stack of cups. The last cup to be added
to the stack of cups is the first cup to be removed. 

Assembler, and other programming languages with a built in stacks, has
two commands related to the stack. These commands are `push` and `pop`,
which add and remove things to and from the stack respectively. This is
especially useful in Assembler, as the stack can be used as a backup for
values when calling functions that modify the value(s) contained in some
register(s). Below are two example programs, showing how the stack can be
useful in Assembler.

_Python_
```python
def foo(a,b):
	return a + b
	
def main():
	foo(2,5)
	
main()
```

_Assembler_
```assembly
main:
	mov eax, 2
	mov ebx, 5
	push ebx
	push eax
	call foo
	
foo:
	mov eax, 
