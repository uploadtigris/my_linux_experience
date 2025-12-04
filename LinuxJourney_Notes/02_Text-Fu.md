stdout (Standard Out)

stdin (Standard In)
- > --> replace contents of a file
- >> --> append contents to a file
- cat < peanuts.txt > banana.txt --> cat reads the contents of peanuts.txt to banana.txt

stderr (Standard Err)
- file descriptors
- 0 --> stdin
- 1 --> stdout
- 2 --> stderr
- "ls /fake/directory 2> peanuts.txt" --> sends the error to peanuts.txt
- "ls /fake/directory 2> /dev/null" --> discards any data written to it

pipe and tee
- | --> uses the stdout (left side) as stdin for the command on it's right
- "ls --color=never | tee peanuts.txt" --> reads contents of ls into peanuts.txt, --color=never is required bc my system is adding a bunch of coloring that is not rendering properly
- "ls | tee peanuts.txt" --> same as above. Colored content can be read with "cat peanuts.txt"

env (Environment)
- env --> shows all the environment variables currently set for your session
- echo $HOME --> provides the path to your home directory
- echo $USER --> provides the path to your user directory
- all of this information is store in the shell's environment
- echo $PATH
	- when you type in a command, your system searches through these directories to find the corresponding executable file
- "export TEST=test" --> sets TEST variable to test

- making the environment variable persistent across sessions
	- nano ~/.bashrc
	- enter --> "export TEST=test" --> ctrl+x & yes + ENTER
	- "source ~/.bashrc" --> apply changes immediately

cut
- "cut -c 5 sample.txt" --> prints out the 5th character from each line
- "cut -f 2 sample.txt" --> prints out "field" delimited by tab
- "cut -f 1 -d ";" sample.txt" --> same as above but changes delimitation to ;

paste
- merge lines together in a file. multiple files --> one file

head
- head /var/log/syslog
- head -n 15 /var/log/syslog

tail
- tail /var/log/syslog
- tail -n 20 /var/log/syslog
- "tail -f /var/log/syslog" --> monitor files in real-time

expand and unexpand
- expand
	- turns a single tab into 8 spaces
	- "expand sample.txt > result.txt"
- unexpand
	- "unexpand -a result.txt"
	- -a --> all instances of 8 space, not just those at the beginning

join and split
- join
	- "join file1.txt file2.txt" --> join the items on each line across the files
	- "join -1 2 -2 1 file1.txt file2.txt" 
	- -1 2 --> field 2 of the first file
	- -2 1 --> field 1 of the second file
- split
	- splits files into new files once a 1000 line limit is reached
	- -l --> specify line count
	- -b --> specify file size

sort
- "sort -r file1.txt" --> reverse sort
- "sort -n file1.txt" --> numerical value

tr (Translate)
- echo "hello world" | tr a-z A-Z 
	- piped echo's output to tr
	- tr translated range a-z into A-Z (caps)
- echo "My address is 123 Main Street" | tr -d '0-9'
	- deletes every character in the 0-9 range (inclusive)
- echo "Hello      world,    how   are   you?" | tr -s
	- squeezes the state to only have one space between each word.

uniq (Unique)
- removes duplicate adjacent lines (they HAVE TO BE adjacent)
- -c flag will count the occurrences of each
- -u display only the lines that are not repeated
- -d display only the line that ARE repeated

wc and nl
- wc
	- "wc /etc/passwd"
	- 96     265      5925 /etc/passwd
	- first number --> number of lines
	- second number --> number of words
	- third number --> number of bytes
- 
	- "wc -l /etc/passwd"
	- -l --> shows only the line count
	- -w --> shows only the word count
	- -c --> shows only the byte count
- nl
	- nl file1.txt
	- ^^ numbers the lines of the text

**grep** [*super important! super useful!*]
- -e --> search for the next argument as the pattern
- grep -e "-v" /path/to/some/file.conf
- grep -i somepattern somefile
- -i --> make search case-insensitive
- -c --> count how many lines match your pattern instead of displaying them
- -o --> only see the exact part of the line that matches the pattern
- -f --> when you have multiple patterns to search for, you can list them in a file and use the -f flag to use that file for patterns
- Combining grep with other commands
	- env | grep -i user
		- take the output of env as the input for grep
	- ls /somedir | grep '.txt$' 
		- find all files ending with .txt using regex
