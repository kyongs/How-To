### How to cancel git add/commit/push

1. How to cancel `git add`
```shell
$ git reset HEAD [file]
```

2. How to cancel `git commit`
```shell
# 1. cancel commit and reserve the corresponding files to ''staged'' state in working directory
$ git reset --soft HEAD^

# 2. cancel commit and save the corresponding files to ''unstaged'' state in working directory
$ git reset --mixed HEAD^
$ git reset HEAD^

# 3. cancel recent n commits
$ git reset HEAD~n

# 4. cancel commit and ''delete'' the corresponding files to ''unstaged'' state in working directory
$ git reset --hard HEAD^

```


3. How to cancel `git push`
```shell
$ git reset HEAD^
```