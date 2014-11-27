## Install

```bash
$ npm install -g node-inspector
```

## Usage

This command will start `app.js` with node-inspector and auto open the default browser.
Note: Only Chrome and Opera are supported.

```bash
$ node-debug app.js
```

If the program is already running, use this:

```bash
$ kill -s USR1 pid_of_node_process
$ node-inspector
```

## Question

If you use cluster in your code. You should run program with only one worker. Then attach to node-inspector with its pid.

## Reference

[stackoverflow](http://stackoverflow.com/questions/1911015/how-to-debug-node-js-applications/16512303#16512303)
[git issue](https://github.com/node-inspector/node-inspector/issues/130)
[--debug-brk](http://youtrack.jetbrains.com/issue/WEB-1919)
