---
title: "[Unix] – Recursively Listing Files Under a Directory"
date: 2013-07-22T09:19:42+01:00
draft: false
weight: 50
tags: shell, unix, old blog
---

If you prefer a DIY script:

```
#! /bin/bash
 
 
function list {
        if [ -z $1 ]
        then
                echo list starting_dir [exclude_file_or_folder] [action_on_file]
                exit 1
        elif [ "$1" == "-help" ]
        then
                echo list starting_dir [exclude_file_or_folder] [action_on_file]
                echo [exclude_file_or_folder]: use string that works with egrep e.g. ".jar|.log|.bak .jar|.log|.bak"
                echo [action_on_file]: if specified, output listing will not be in ls -l format
                exit 0
        fi
 
        local path=$1
 
        if [ -z $2 ]
        then
                ls -al $path
        else
                ls -al $path | egrep -v $2
        fi
 
        for file in `ls -a $path`
        do
                if [ "$file" != "." ] && [ "$file" != ".." ] && [ -d "$path/$file" ]
                then
                        file=`echo $file|grep -v $2`
                        if [ -z $file ]
                        then
                                echo "skipped" >> /dev/null
                        else
                                echo ============= $path/$file ================
                                list $path/$file $2 $3
                        fi
                fi
        done
}
 
 
list $1 $2 $3
```

But there is a simple alternative

```
find . -type f
```

or simply:

```
find .
```

If we want to skip some file or folder from the recursive listing. For example, I want to exclude “logs” folder from my

```
find . -print -name "logs" -prune
```

Further excluding is also possible. For example, to exclude all .gz file from listing

```
find . -print -name "logs" -prune | grep -v ".gz"
```