# Sed Command in Linux

## Descriptions

UNIX中的SED命令代表Stream Editor，它可以在搜索，查找和替换，插入或删除等文件上执行大量功能。虽然UNIX中的SED命令最常见的是替代或用于查找和更换。通过使用SED，即使在不打开它们的情况下也可以编辑文件，这是更快的方式来查找和更换文件中的内容，而不是在VI编辑器中的第一个打开，然后更改它

- sed是一个强大的文本流编辑器。可以进行插入，删除，搜索和替换（替换）。

- UNIX中的SED命令支持正则表达式，允许它执行复杂的模式匹配。

## Syntax

```shell
    sed OPTIONS... [SCRIPT] [INPUTFILE...] 
```

- Example

```shell
  unix is great os. unix is opensource. unix is free os.
  learn operating system.
  unix linux which one you choose.
  unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
```

1. 替换或替换字符串：SED命令主要用于替换文件中的文本。下面的简单sed命令用文件中的“linux”替换“unix”一词

```shell
sed "s/unix/linux/" sed.txt
```

- Output

```
      linux is great os. unix is opensource. unix is free os.learn operating system.unix linux which one you choose.unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
```

这里“s”指定替换操作。“/”是分隔符。“UNIX”是搜索字符串，“Linux”是替换字符串。默认情况下，SED命令替换每行中的第一次出现的匹配字符串

2. 替换一行中的模式的第n次发生：使用/ 1，/ 2等标志替换一行中的第一个，第二次发生模式。下面的命令用一行中的“Linux”替换了“UNIX”一词的第二个发生。

```shell
   sed "s/unix/linux/2" sed.txt
```

- Output
  
  ```shell
  unix is great os. linux is opensource. unix is free os.learn operating system.unix linux which one you choose.unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
  ```
3. 在一行中替换所有模式的发生：替换标志/ g（全局替换）指定sed命令以替换行中的所有字符串。
   
   ```
   linux is great os. linux is opensource. linux is free os.learn operating system.linux linux which one you choose.linux is easy to learn.linux is a multiuser os.Learn linux .linux is a powerful.
   ```

4. 从第n次替换到一行中的所有出现：使用/ 1，/ 2等和/ g的组合来替换在一行中的第n个发生模式的所有模式。以下SED命令用“Linux”单词替换第三个，第四，第五个......“unix”单词
   
   ```shell
   sed "s/unix/linux/2g" sed.txt
   ```
   
   - Ouput:
     
     ```
     unix is great os. linux is opensource. 
     unix is free os.learn operating system.linux linux which one you choose.linux is easy to learn.linux is a multiuser os.Learn linux .linux is a powerful.
     ```

5. **Parenthesize first character of each word** : 此SED示例在括号中打印每个单词的第一个字符
   
   ```shell
   echo "Welcome To The Geek Stuff" | sed 's/\(\b[A-Z]\)/\(\1\)/g' 
   ```
   
   - Output:
     
     ```
     (W)elcome (T)o (T)he (G)eek (S)tuff
     ```

6. **Replacing string on a specific line number** : 您可以限制SED命令以替换特定行号上的字符串
   
   ```shell
   sed "2 s/unix/linux/" sed.txt  
   ```
   
   - Output:

```
    unix is great os. unix is opensource. 
    linux is free os.learn operating system.unix linux which one you choose.unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
```
