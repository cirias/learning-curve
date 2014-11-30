```bash
# http://stackoverflow.com/questions/392022/best-way-to-kill-all-child-processes/15139734#15139734
kill -9 -$(ps -o pgid=$PID` | grep -o '[0-9]*')
```
