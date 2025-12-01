
# Command Line

pwd
- "/" --> root directory

cd
- "." --> current directory
- ".." --> parent directory
- "~" --> home directory
- "-" --> prevoius directory

ls
- "ls -a" --> view hidden files
- "ls -l" --> get detail information
- "ls -r" --> get items in reverse alphabetical order
- "ls -la" --> hidden files & detailed

touch
- "touch file.txt" --> creates new file

- "ls -l datfile" --> check original timestamp
- "touch datfile" --> update the timestamp
- "ls -l datfile" --> check the new timestamp

- "touch -r file1.txt file2.txt"
- "touch -d "2023-01-01 12:30:00" mysuperduperfile" --> set a specific date

file
- "file file.txt" --> show the type of file

cat
- "cat myfile.txt" --> print the entire contents of file into terminal
- "cat dogfile birdfile" 
- "cat -n file" --> numbers all output lines
- "cat -b file" --> numbers all non-empty output lines

less
- tool to show less text at a time
- use arrow keys
- "g" --> go to the beggining
- "G" --> go to the end
- "h" --> display summary

- "/search_term" --> searches forward for term
- "?search_term" --> searches backward for term
- "n" --> just to the next occurence of the search term
- "N" --> jump to the previous occurence of the search term

- "q" --> quit less viewer

history
- "history"
- up arrow --> bring up previous command
- "!!" --> run previous command

- "Ctrl+R" --> reverse search through recent commands
- "history -c" --> clear the command history
- "history -w linux" --> save the current session's history to your history
- "history -d <offset>" --> offset == entry in history
- "clear" --> clear the current display

cp
- "cp [SOURCE][DESTINATION]"

mv

mkdir

rm

find

help

man

whatis

alias

exit