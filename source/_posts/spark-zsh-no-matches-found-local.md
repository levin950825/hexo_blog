---
title: spark-zsh no matches found local
date: 2017-03-20 18:22:23
tags: spark
categories: Spark
comments:
toc: true
thumbnail:
banner:
---
Solve the 'zsh: no matches found: local[4]' issue when running a spark application.

<!-- more -->
### Issue
When I was trying to submit a Spark streaming application with this script:

```
$ ~/spark/bin/spark-submit \
--class "com.databricks.apps.logs.chapter1.LogAnalyzerStreaming" \
--master local[4] \
target/log-analyzer-1.0.jar \
apache.access.log
```
I came across this error from zsh:

```
$ zsh: no matches found: local[4]
```

### Solution
I replaced `local[4]` with `local` it seemed to work, but according to the usage of master URL:

|command | meaning|
| --- | --- |
| local | run Spark locally with one worker thread (i.e. no parallelism at all) |
| local[K] | run Spark locally with K worker threads (ideally, set K to the number of cores on your machine) |

I should not change the master URL because I wanted it to run in parallelism.

So I changed back to bash and run spark submit script again, interestingly it worked! Obviously it was an issue of zsh.

According this post Zsh says “no matches found” when trying to download video with youtube-dl, it said that zsh only accepts parameters by quoting them:

```
$ ~/spark/bin/spark-submit \
--class "com.databricks.apps.logs.chapter1.LogAnalyzerStreaming" \
--master "local[4]" \
target/log-analyzer-1.0.jar \
apache.access.log
```
It worked!

### Conclusion
In zsh, by default, a failed expansion is an error, but in Bash it isn’t: the failed pattern is just left as an argument exactly as it was written. zsh’s behaviour is safer, in that you can’t write a command that secretly doesn’t do what you meant because a file was missing, but you can change it to have the Bash behaviour if you want:

```
setopt nonomatch
```
This will resolve the original use case. In general it will be better to quote arguments with special characters, though, in order to avoid any mistakes where a file happens to exist with a corresponding name, or doesn’t exist when you thought it did.

The NOMATCH option is on by default, and causes the errors I were seeing. If you disable it with setopt nonomatch then any failed glob expansions will be left intact on the command line, for example:

```
$ echo foo?bar
zsh: no matches found: foo?bar
$ setopt nonomatch
$ echo foo?bar
foo?bar
```


