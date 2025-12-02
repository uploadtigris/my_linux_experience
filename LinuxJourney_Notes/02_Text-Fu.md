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

cut

paste

head

tail

expand and unexpand

join and split

sort

tr (Translate)

uniq (Unique)

wc and nl

grep
