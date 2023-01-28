`cat readfile > outfile.txt`
reads readfile and outputs to outfile
Using `>>` appends instead
output strings or predefined variables
```
echo "string"
echo "$HOSTNAME"

echo "This is a string" > file.txt`
```
Cut command can be used to specify a delimeter and what fields to retrieve
`cat mypasswd.txt | cut -d ':' -f 1`
retrieves first field

A common usage of pipe | is to search for a given string in command output
`ps -aux | grep bash`

get linux release details
`cat /etc/lsb-release`

