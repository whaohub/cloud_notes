# shell 脚本for循环遍历目录

```shell
#!/bin/bash

#script to recursively travel a dir of n levels

function run_make()
{
        if [ -f "Makefile" ];then
            echo ""
            echo ===================== FOUND ===================
            pwd;
            echo ===============================================
            make;
        fi
}

function traverse() {
for file in `ls`
do
    if [[ ${file} == "CloudEagleServer" || -f ${file} ]] 
    then
        echo "${file} is a file"
    else 
    cd ${file};make;cd ..;
    fi
done
}

function main() {
    traverse "$1"
}

main "$1"
```
