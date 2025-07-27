---
layout: post
title: Some log analysis commands
categories: [Linux]
tags: [Linux, Development]
fullview: true
---


1. 
```
grep -H -A 30 "target error message" *.log
```

This command searches all .log files which contains the target message in the current directory.
It prints the filename, the matching line and the 30 lines that follow the matching line.

2. 
```
grep -i -A 30 "target error message" sample.log | less
```

This command locates every line that contains the target message (`-i` here is for ignoring case distinctions) and prints it, along with the 30 lines that immediately follow each matching line. `| less` provides paging functions.

3. 
```
tail -f sample.log | grep -A 30 "target error message"
```

`tail -f` starts continuously outputting new lines as they are written to the specified file.
`grep` then, in real-time, scans these incoming lines and print necessary logs.
This process continues indefinitely until we manually stop the command.

4. 
```
grep -c "target error message" sample.log
```

This command counts the occurrences of the target error message in the sample.log.

5. 
```
grep -C 30 "target error message" sample.log
```

This command prints 30 lines of context both before and after the target message in the sample.log.

6. 
Nowadays we have more useful tools such as ELK (Elasticsearch, Logstash, Kibana) stack.
The configuration is more complicated so I guess I will add those contents in the future :)
