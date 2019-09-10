Exercise 3
Write a script that allows a user to input a file name. Then, output whether or not the file exists. If the file does exist, output the number of lines in the file as well as the file type. Then, prompt the user to enter a word to search the file for. Ouput how many times the word occurs in the file.
NOTE: You do not need to parse each word in the file. This script can be accomplished using bash commands and conditional statements.
(Useful commands: file, grep, wc)

Attatched is a folder (called Rick_Roll) containing five files full of random words. The word 'ehc' has been inserted into each file. Test your script by locating and searching these files for the word 'ehc'. The occurences of the word 'ehc' is noted below, as well as a sample output.

1-Never.txt: 27 ehc
2-Gonna.txt: 20 ehc
3-Give.txt: 38 ehc
4-You.txt: 6 ehc
5-Up.txt: 21 ehc

Sample Output:
root@kali:~/Documents/Rick_Roll# ./script.sh 
Enter a filename: 1-Never.txt
The file  exists.
Number of lines: 100
1-Never.txt: ASCII text, with very long lines, with CRLF line terminators
Enter the word you want to find: ehc
This file contains ehc 27 times.

