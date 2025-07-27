---
layout: post
title: Some log analysis commands
categories: [Linux]
tags: [Linux, Development]
fullview: true
---

1. This command searches all .log files which contains the target message in the current directory.  
It prints the filename, the matching line and the 30 lines that follow the matching line.

```
grep -H -A 30 "target error message" *.log
```

2. This command locates every line that contains the target message (`-i` here is for ignoring case distinctions) and prints it, along with the 30 lines that immediately follow each matching line.  
`| less` provides paging functions.

```
grep -i -A 30 "target error message" sample.log | less
```

3. `tail -f` starts continuously outputting new lines as they are written to the specified file.  
`grep` then, in real-time, scans these incoming lines and print necessary logs.  
This process continues indefinitely until we manually stop the command.

```
tail -f sample.log | grep -A 30 "target error message"
```

4. This command counts the occurrences of the target error message in the sample.log.

```
grep -c "target error message" sample.log
```

5. This command prints 30 lines of context both before and after the target message in the sample.log.

```
grep -C 30 "target error message" sample.log
```

6. Nowadays we have more useful tools such as ELK (Elasticsearch, Logstash, Kibana) stack.  
The configuration is more complicated so I guess I will add those contents in the future :)
