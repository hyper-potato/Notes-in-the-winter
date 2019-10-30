start beeline from command line

```sh
 $ beeline -u jdbc:hive2://
```

We can submit a query directly from the bash shell using

```sh
beeline -u jdbc:hive2:// -e 'your\_sql\_statement(s)'
```