# Automating workflow 

Ran into a few errors when trying to run `apt-get install curl`, the correct commands to use in docker is `apt-get install -y curl` to ensure that 'yes' is passed as arguments to any prompts 

To overide the prompts in `dpkg` you can use `-o dpkg <pkg name>  "Dpkg::Options::=--force-confdef" -o "Dpkg::Options::=--force-confold"`

# File managment in docker
dpkg is for installing `.deb` files and can not be used with zip folders.
try 

`wget <url>.zip && unzip <tmp.zip> <directory> && rm <tmp.zip>`
