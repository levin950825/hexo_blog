---
title: Install Hadoop on Mac Sierra |Hadoop在Mac上的安装配置
date: 2017-03-29 23:01:14
tags: hadoop
categories: Hadoop
comments:
toc: true
thumbnail:
banner:
---


Mac 系统: macOS Sierra (10.12.1)
Reference links:

- [Hadoop 入门教程](http://hustlijian.github.io/tutorial/2015/06/19/Hadoop%E5%85%A5%E9%97%A8%E4%BD%BF%E7%94%A8.html) (Hadoop 2.7.0, mac)
- [Hadoop集群安装配置教程](http://www.powerxing.com/install-hadoop-cluster/) (Hadoop 2.6.0, Ubuntu/CentOS)

---

### 安装Java SE


#### 检查本机是否有Java和Java版本:

Run `java -version` in your terminal to check which version of Java you have installed.
Mac 下面安装 Java，直接在官网上下载对应的 dmg. 版本选择不宜过高，会遇到不兼容的问题.
#### 配置JAVA_HOME

在`.bashrc` 或`.zshrc` 中加入 `JAVA_HOME` 设置： 

```
sudo vim /etc/profile
```
在最后面加上以下：

```
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk/Contents/Home
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
```
修改 `proflie` 之后需要执行 `source /etc/profile` 命令进行生效 需要注意的是，通过 dmg 文件安装的 jdk 地址是固定的，在`/Library/Java/JavaVirtualMachines/`里面可以找到对应版本的 JDK

### 配置Mac OS ssh

#### 设置Mac允许远程登陆

```bash
$ ssh localhost
206-214:~ PeiYingchi$ ssh localhost ssh: connect to host localhost port 22: Connection refused
```
会有错误提示信息，表示当前用户没有权限。这个多半是系统为安全考虑，默认设置的。更改设置如下：进入system preference --> sharing --> 勾选remote login，并设置allow access for all users。再次输入`ssh localhost`，再输入密码并确认之后，可以看到ssh成功
#### 生成公钥
`ssh-keygen -t rsa` 一路回车，各种提示按默认不要改，等待执行完毕

> `ssh-keygen`表示生成秘钥；-t表示秘钥类型。这个命令在`~/.ssh/`文件夹下创建两个文件`id_rsa`和`id_rsa.pub`，是ssh的一对儿私钥和公钥。接下来，将公钥追加到授权的key中去

然后执行： `ls ~/.ssh` 可以看到两个密钥文件：`id_rsa`（私钥）`id_rsa.pub`（公钥）
#### 设置免密码登陆
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
### 安装Hadoop
```bash
brew install hadoop
```
If saw errors like:

```bash
Could not link:
/usr/local/share/doc/homebrew
Please delete these paths and run brew update.
```
Then run `rm -rf /usr/local/share/doc/homebrew` 安装成功后会出现以下:

```bash
In Hadoops config file:
  /usr/local/Cellar/hadoop/2.7.3/libexec/etc/hadoop/hadoop-env.sh,
  /usr/local/Cellar/hadoop/2.7.3/libexec/etc/hadoop/mapred-env.sh and
  /usr/local/Cellar/hadoop/2.7.3/libexec/etc/hadoop/yarn-env.sh
$JAVA_HOME has been set to be the output of:
  /usr/libexec/java_home
```
### Hadoop设置

#### hadoop-env.sh配置

文件在 `/usr/local/Cellar/hadoop/2.7.3/libexec/etc/hadoop/hadoop-env.sh`
将

```bash
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"
```
修改为

```bash
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm= -Djava.security.krb5.kdc="
```
#### 编辑core-site.xml

```xml /usr/local/Cellar/hadoop/2.7.3/libexec/etc/hadoop/core-site.xml
<configuration>
  <property>
      <name>hadoop.tmp.dir</name>  
      <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
    <description>A base for other temporary directories.</description>
  </property>
  <property>
     <name>fs.default.name</name>                                     
     <value>hdfs://localhost:9000</value>                             
  </property>                                                        
</configuration> 
```
> 注： `fs.default.name` 保存了NameNode的位置，HDFS和MapReduce组件都需要用到它，这就是它出现在`core-site.xml` 文件中而不是 `hdfs-site.xml`文件中的原因
#### 编辑mapred-site.xml, 设置MapReduce使用的框架

可能文件名为 `mapred-site.xml.templete` , 改不改名字都可以。

```xml
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>localhost:9001</value>
  </property>
</configuration>
```
变量`mapred.job.tracker` 保存了JobTracker的位置，因为只有MapReduce组件需要知道这个位置，所以它出现在`mapred-site.xml`文件中。

#### 编辑 hdfs-site.xml
```xml /usr/local/Cellar/hadoop/2.7.3/libexec/etc/hadoop/hdfs-site.xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
</configuration>
```
变量`dfs.replication`指定了每个HDFS数据库的复制次数。 通常为3, 由于我们只有一台主机和一个伪分布式模式的DataNode，将此值修改为1。
#### 编辑 yarn-site.xml
```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```
### 启动Hadoop

#### 格式化 hdfs

`hadoop namenode -format` 这时就可以用`hstart`启动hadoop了。 可以使用 `jps` 命令验证 Hadoop是否在运行。
#### 进入Hadoop主文件夹
```bash
cd /usr/local/Cellar/hadoop/2.7.3/ 
sbin/start-dfs.sh 
sbin/start-yarn.sh
```
为了启动Hadoop的时候避免每次都首先进到安装目录，然后再执行`sbin/start-dfs.sh` 和 `sbin/start-yarn.sh`这么麻烦，所以在编辑 `~/.profiles`文件,加上如下两行

```bash
alias hadoop_start="/usr/local/Cellar/hadoop/2.7.3/sbin/start-dfs.sh;/usr/local/Cellar/hadoop/2.7.3/sbin/start-yarn.sh"
alias hadoop_stop="/usr/local/Cellar/hadoop/2.7.3/sbin/stop-yarn.sh;/usr/local/Cellar/hadoop/2.7.3/sbin/stop-dfs.sh"
```
然后执行 `source ~/.profiles` 更新。 这样就可以用 `hadoop_start` 和 `hadoop_stop` 这两个命令来启动Hadoop了。

> 注意: 以后要使用`hadoop_start`之前还是要运行`source ~/.profiles`才可以哟
### 运行一个Word Count 例子

```bash
hadoop jar /usr/local/Cellar/hadoop/2.7.3/libexec/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar pi 2 5
```
然后可以通过Web端进行监控。
Resource Manager: http://localhost:50070 
JobTracker: http://localhost:8088 
Specific Node Information: http://localhost:8042 
通过他们可以访问 HDFS filesystem, 也可以取得结果输出文件.
#### 建立用户空间
```bash
hadoop fs -ls / # 查看已有user 
hdfs dfs -mkdir /user hdfs dfs -mkdir /user/$(whoami) # 这里是用户
hdfs dfs -put ~/Documents/Hadoop/demo/Ethics.md /user/yingchi # 上传local文件
```
运行`wordcount`,命令如下: 
> 注意：运行实例时，当前目录设置为 `/usr/local/Cellar/hadoop/2.7.0/`

```bash
bin/hadoop jar libexec/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar wordcount /user/yingchi/Ethics.md /user/yingchi/out
```
运行完成后,在`/yingchi`目录下生成名为out的目录,其内容如下所示:

```bash
Found 2 items
-rw-r--r--   1 PeiYingchi supergroup          0 2016-11-13 21:49 /user/yingchi/out/_SUCCESS
-rw-r--r--   1 PeiYingchi supergroup      15719 2016-11-13 21:49 /user/yingchi/out/part-r-00000
```



