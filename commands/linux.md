# Helpful commands in linux 

knowing how to use the software efficently greatly increases ergonomics of work flow, listed are my most used / most useful 

## Keyboard shortcuts 

`alt + tab` : choose workspace 

## CLI shortcuts
`alt + f`: move one word forward 
`alt + b`: move one word backwards
`ctlr + l`: clear console

## CLI key commands 

- `man` : displays the manual for a specified command 
- `dpkg`: de-packages a deb file, `-i` to install 
- `ctrl + r` : reverse search previous commands 
- `^old^new`: Swapps a letter in the previous command 
- `lpr !$`: the `!$` appends the last word of the last command 
- `apropos keyword`: apropos useful when you dont know the name of the commands you need
- `pwgen`: A utility that generates almost random passwords
- `less`: Displays file one page at a time 
- `hostname`: Displays the host name
- `cp -i source-file destination-file`: Copys a file `-i` asks permission before overwriting a file
- `mv`: Changes file name
- `grep 'word' file`: Searches for a string 
-` head -5 file` && `tails -5 file`: Displays 5 lines from head and tail respectively 
- `diff -u file.1 file.2`: Shows the unified output
-` file file.bz2`: Learn content on any file
- `|`: takes the output of one process and uses it as input in another process
- `unix2dos file`: Utility that converts files to be readable on MAC and Windows
- `cat file | tr -d '\r' > file.txt`: Translates a file and sends the output to file.txt
