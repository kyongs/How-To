### LFS

If you want to upload a large file, you need to download LFS(Large File Storage) to upload file at github.

1. Install lfs
```shell
$ git lfs install
```

2. Track the file extension you want to upload. (For example, if you have super big large text(txt) file you need to upload github, the extension should be `".*txt"`)
```shell
$ git lfs track "*.txt"
```

3. Add gitattributes
```shell
$ git add .gitattributes
``` 

4. Normal add, commit, push to github
