#Over The Wire Leviathan Write-Up
This is a write-up for OverTheWire Leviathan.
I am writing this under the assumption that you have
completed the bandit series. This guide shoudl be pretty 
easy to follow along with and build upon the thought process
introduced during Bandit. 

#Level 0 
Alright for this one let's just login to the server and see what's
available to us.
`$ ssh leviathan0@leviathan.labs.overthewire.org -p 2223`
Alright, so now that we are logged in to the first level we can see that
there is a folder named `.backup`. If we hop into that directory, we can 
see that there is a pretty long file named `bookmarks.html`. We can scan
this file for important keywords to us like `password` using `grep` to find
the given regex.
`$ cat bookmarks.html | grep password`
From this we get a nice output that has the password for the next level, so 
we'll grab that and move on.  rioGegei8m

#Level 1
After logging in we should be able to see that there is a file named `check` 
that has the permissions of leviathan2. Here we can use `ltrace` to figure out
exactly how this program works. 
`$ ltrace ./check`
We can see that the program is using a `strcmp` call, which compares two strings.
Using the string that is the program is comparing our input with we are given a 
shell. Performing `$ whoami` tells us that we are leviathan2, so we are now able to
perform actions as if we were leviathan2. Knowing this, we can just print out the 
password for leviathan2, and re-login. 
`$ cat /etc/leviathan_pass/leviathan2`  ougahZi8Ta

#Level 2
On this level the first thing that we should notice is that we are given another file,
which has higher permissions than what we currently have. Create a working directory in
the `/tmp` folder using `$ mkdir`. Once you have a working directory create a small file
in this working directory so we have something to play with while learning how this executable
works. Using `ltrace` we can discover that the executable takes a file as a command line
argument, so we can use the `.bashrc` to see exactly what it does. 
`$ ltrace ./printfile ./.bashrc`
We can see that this executable prints the file that we pass as an argument. Though it likely
won't work we can try to print the password for the next level. 
`$ ./printfile /etc/leviathan_pass/leviathan3`
From the output we are told that we don't have read access to the password file for the next level.
This is of no problem to us, we can find a workaround to print the file that we want. First we shall
see how the executable handles filenames with special characters. We can use `mv` to rename the file
we created in our working directory to a name with spaces. Running the executable on this file, and
analyzing it using `ltrace` we can determine that the executable thinks that we are giving it multiple file
arguments, when we are only giving it one. This is good to know, so now we can try to fool this executable 
into printing our desired password.