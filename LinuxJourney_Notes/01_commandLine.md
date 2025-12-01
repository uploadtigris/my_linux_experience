
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
- used for renaming files & for moving them
- "mv [SOURCE][DESTINATION]"
- "mv [current file/directory name][desired name]"
- "mv file_1 file_2 /somedirectory" --> move multiple files into a directory

- "mv -t /somedirectory file_1 file_2" --> same as above but describe the directory first
- "mv -i source_file destination_directory" --> -i asks for confirmation before overwriting an existing file
- "mv -b file1 directory_with_file1" --> -b creates a backup of the destination file
- "mv -v file1 file2 /somedirectory" --> -v makes verbose; each action is listed out

mkdir
- "mkdir dir1"
- "mkdir dir1 dir 2"
- "mkdir -p books/author/favorites" --> -p creates **Nested Directories**

rm
- "-f" --> forcefully remove without any prompting
- "-r" --> has rm delete a directory and all of its contents
- "-i" --> prompts for confirmation before deleting each file
- "rmdir" --> remove directory, only if it is completely empty

find
- can find items anywhere in the sub directories below the specified search directory

- "find /home -name puppies.jpg" --> gives the path to the file

- "find /home -type d -name MyFolder"
- "-type f" --> search for regular files
- "d" --> set type to directory

help
- for "built-in" commands --> "help echo"
- for everything else --> ls --help

man
- man pages!
- "man [COMMAND]"
- Great for seeing the syntax and options available

whatis
- enter a command & this will tell you what it does
- this output comes directly from the NAME section of the command's manual page

alias
- create a temporary alias --> "alias ll='ls -la'"
	- this alias will disappears after the current terminal session ends
- To make an alias permanent, you need to add it to the .bashrc file
	- `nano ~/.bashrc`
	- ```alias ll='ls -la' ```
	- ```alias update='sudo apt update && sudo apt upgrade'```
- reloading the configuration file --> ```source ~/.bashrc``` 
- removing an Alias --> ```unalias [alias]```

exit
- ```exit```
- ```logout```

