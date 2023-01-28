#### Commands
`ls`
List directory contents 
`ls -l`
One line per file with additional details
First parameter specifies files or directories to specifically look at

`mkdir`
Make a directory
`rmdir`
remove a directory
`pip3 install ansible==2.10.4 ansible-lint==4.3.7`
Install Ansbile
`pip3 install ansible==2.10.4 ansible-lint==4.3.7 &`
add & to run a command in the background e.g.

#### Files and Directories
`pwd`
prints working directory
`cd directory`
moves into directory 
`cd ..`
jumps up directory
`nano myfile`
opens nano editor on myfile
`cat myfile`
Reads the contents of a file and displays as output
`mv myfile`
delete a file
`rm file`

```
cat > filename<<EOL

some text content
some text content 2
some text content 3

EOL
```

<< symbols are a special code block, allows redirect anything between EOL into a command

pull any thing from a source

`curl address`
address can be localhost or http address
