### EC2
Connect remote pc with ssh:

```bash
$ ssh -i mykey.pem username@hostip
```

Move file from local to remote:

```bash
$ scp -i mykey.pem somefile.txt ubuntu@ip-of-ec2:/
```

If you want to move directory, add `-r` before the path, which means recursion.

### Database
#### Postgresql
Install postgresql-client first. On Ubuntu:

```bash
$ sudo apt-get install postgresql-client
```

then connect to the server with the following command.

```bash
$ psql -h sever.domain.name database user
```

Tips:
To analyze sql in postgre, add `EXPLAIN ANALYZE` before sql.

### Unix-like system
Show cpu info:

```bash
$ cat /proc/cpuinfo
```

### Node.js
Get process info of the script:

```bash
$ npm install -g node-tick
$ node -prof app.js
$ node-tick-processor app.log > app.log.log
$ vim app.log.log
```
