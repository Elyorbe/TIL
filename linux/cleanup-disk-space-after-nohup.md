
## Cleanup Disk Space After `nohup` Command

### Problem
When you run a process with in the background `nohup` command you can redirect output of the process to a file or to `/dev/null` to ignore.
Redirecting to a file without any file rotation rules can be very expensive since the output file can grow to a very large size.
Exactly the same thing happened to me. I redirected the output of `nohup` to an output file: `nohup $JAVA_HOME/java -jar $JAR_FILE > nohup.out 2>&1  &`.
After some time one of our servers' disk usage was full. Size of `nohup` output file became too large that ended up eating the whole disk space.
Initially, I thought oh okay let's just remove the output file and the process creates a new one if it needs. 
However, disk space was not freed up even after removing neither a new file was created. Process continued to run without exceptions. Strange. 
Then I found I could have just overwritten the output file to reclaim the space like `cat /dev/null > nohup.out`.
But the problem was the file was already deleted.

## Solution
After some research I found out that when you remove a file, **first it removes the directory entry for this file then releases the space only if no processes
have it open and there are no directory entries(hard links) point to it**. In my case, since the process didn't stop there was still hard link on that file.
Therefore, the process continued to write to the old file that now has become a file descriptor. I needed to overwrite file descriptor.

So this is how solved the problem: 
1. Find the `pid` of the process
```shell
jps | grep myjarfile
```
2. Find file descriptor id(deleted file) that the process holds:
 ```shell
ls -l /proc/79906/fd | grep deleted

l-wx------. 1 centos centos 64 Oct 25 08:41 2 -> /home/centos/service/run.out (deleted)
```
3. Overwrite file descriptor: `79906 = $pid and 2 = $fdid`
```shell
cat /dev/null > /proc/79906/fd/2
```
