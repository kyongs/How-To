## How to use csope 
Environment: `Ubuntu 18.04.6 LTS`

<br/>

1. Install cscope
```bash
$ sudo apt-get install cscope

```

2. Make cscope file
```
$ find ./ -name '*.[cCsSH]' > file-list
$ cscope -i file_list
(ctrl + d)
```

3. Move to the root directory where cscope.out file exists.
```
: cs find type keyword
ex) cs find 0 keyword
```




