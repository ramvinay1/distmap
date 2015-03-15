


# Create common group and user account #
Before starting the hadoop setup it is important to create a common user account and a group on all computers.
Create the group _hadoop_ and user _cluster_:

**Step1:** Log in as root.

**Step2:** Create the group hadoop using the _groupadd_ utility followed by the name of the group, in this format:
```
groupadd n hadoop

```

Where n is an unused group ID greater than 100.


**Step3:** Create the user cluster using the _useradd_ utility followed by the group (hadoop) and user name (cluster) in this format:

```
    	useradd -u n -g hadoop cluster
```


**Step4:** Create a password for the user cluster. To do this, use the _passwd_ utility and the following command:
```
passwd cluster
```


# Download Apache Hadoop #
Download Apache Hadoop stable version from http://www.us.apache.org/dist/hadoop/common/stable/ and put into /usr/local/hadoop-1.0.3

The JAVA\_HOME is /usr/lib/jvm/java/jdk1.6.0\_22

The HADOOP\_HOME is /usr/local/hadoop-1.0.3

# Our configuration files #
Here we provide a practical guide and following configuration files to setup a Hadoop cluster from the beginning on Linux and Mac OSX 10.6 or higher computers. The main configuration files can be downloaded from following link:

**/usr/local/hadoop-1.0.3/conf/hadoop-env.sh:** http://distmap.googlecode.com/files/hadoop-env.sh

**/usr/local/hadoop-1.0.3/conf/core-site.xml:** http://distmap.googlecode.com/files/core-site.xml

**/usr/local/hadoop-1.0.3/conf/hdfs-site.xml:** http://distmap.googlecode.com/files/hdfs-site.xml

**/usr/local/hadoop-1.0.3/conf/mapred-site.xml:** http://distmap.googlecode.com/files/mapred-site.xml

**/usr/local/hadoop-1.0.3/fair-scheduler.xml:** http://distmap.googlecode.com/files/fair-scheduler.xml



# 1. Network #
## Edit /etc/hosts on every node ##

If you have, for example, the following nodes:<br><br>
(remember to replace the hostname according to your machine; eg. ubuntu01-01, ubuntu01-02)<br>
<br>
<b>master:</b>
<pre><code>ubuntu01-01<br>
</code></pre>

<b>slaves:</b>
<pre><code>ubuntu01-02<br>
ubuntu01-03<br>
ubuntu01-04<br>
ubuntu01-05<br>
</code></pre>

<h2>Then add the following lines in /etc/hosts on every node:</h2>

<b># /etc/hosts (for master AND slave)</b>

<pre><code>192.168.0.1      ubuntu01-01  <br>
192.168.0.2      ubuntu01-02<br>
192.168.0.3      ubuntu01-03<br>
192.168.0.4      ubuntu01-04<br>
192.168.0.5      ubuntu01-05<br>
</code></pre>

<h1>2. Configure</h1>
<h2>For master:</h2>

edit conf/masters as follow:<br>
<pre><code>ubuntu01-01<br>
</code></pre>

edit conf/slaves as follow:<br>
<pre><code>ubuntu01-01<br>
ubuntu01-02<br>
ubuntu01-03<br>
ubuntu01-04<br>
ubuntu01-05<br>
</code></pre>

<h2>For every node do the following:</h2>

<h3>1) Configure JAVA_HOME</h3>
<pre><code>cd /usr/local/hadoop-1.0.3<br>
gedit conf/hadoop-env.sh<br>
</code></pre>

and change:<br>
<pre><code><br>
# The java implementation to use. Required.<br>
# export JAVA_HOME=/usr/lib/j2sdk1.5-sun<br>
<br>
</code></pre>

to:<br>
<pre><code><br>
# The java implementation to use.  Required.<br>
export JAVA_HOME=/usr/lib/jvm/java/jdk1.6.0_22<br>
<br>
</code></pre>
Save & exit.<br>
<br>
<br>
<h3>2) Create some directories in hadoop home:</h3>
<pre><code>cd /usr/local/hadoop-1.0.3<br>
mkdir tmp<br>
mkdir hdfs<br>
mkdir hdfs/name<br>
mkdir hdfs/data<br>
</code></pre>

<h3>3)  Configurations setup</h3>
Under conf/, edit the following files, (note that "/path/to/your/hadoop" should be replaced with something like "/usr/local/hadoop-1.0.3")<br>
<br>
<b>conf/core-site.xml</b>
<pre><code>&lt;configuration&gt;<br>
    &lt;property&gt;<br>
      &lt;name&gt;fs.default.name&lt;/name&gt;<br>
      &lt;value&gt;hdfs://ubuntu01-01:9000&lt;/value&gt;<br>
    &lt;/property&gt;<br>
    &lt;property&gt;<br>
      &lt;name&gt;hadoop.tmp.dir&lt;/name&gt;<br>
      &lt;value&gt;/usr/local/hadoop-1.0.3/tmp&lt;/value&gt;<br>
    &lt;/property&gt;<br>
  &lt;/configuration&gt;<br>
</code></pre>

<b>conf/hdfs-site.xml</b>
<pre><code>&lt;configuration&gt;<br>
    &lt;property&gt;<br>
      &lt;name&gt;dfs.replication&lt;/name&gt;<br>
      &lt;value&gt;3&lt;/value&gt;<br>
    &lt;/property&gt;<br>
    &lt;property&gt;<br>
      &lt;name&gt;dfs.name.dir&lt;/name&gt;<br>
      &lt;value&gt;/usr/local/hadoop-1.0.3/hdfs/name&lt;/value&gt;<br>
    &lt;/property&gt;<br>
    &lt;property&gt;<br>
      &lt;name&gt;dfs.data.dir&lt;/name&gt;<br>
      &lt;value&gt;/usr/local/hadoop-1.0.3/hdfs/data&lt;/value&gt;<br>
    &lt;/property&gt;<br>
  &lt;/configuration&gt;<br>
</code></pre>

<b>conf/mapred-site.xml</b>

<pre><code>&lt;configuration&gt;<br>
    &lt;property&gt;<br>
      &lt;name&gt;mapred.job.tracker&lt;/name&gt;<br>
      &lt;value&gt;ubuntu01-01:9001&lt;/value&gt;<br>
    &lt;/property&gt;<br>
  &lt;/configuration&gt;<br>
</code></pre>

<h3>4)  Configure passphaseless ssh</h3>
<pre><code>ssh localhost<br>
</code></pre>

You will need a password to log in ssh.<br>
<br>
<pre><code>ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa<br>
cat ~/.ssh/id_rsa.pub &gt;&gt; ~/.ssh/authorized_keys<br>
exit<br>
</code></pre>

Configuration done. Try:<br>
<pre><code>ssh localhost<br>
</code></pre>

You should now be able to log in without password.<br>
<br>
<br>
<h1>3. SSH Access</h1>

The master log in must have password-less log in authorities to all slaves.<br>
<br>
<pre><code>cluster@ubuntu01-01:~$ ssh-copy-id -i $HOME/.ssh/id_rsa.pub cluster@ubuntu01-02<br>
cluster@ubuntu01-01:~$ ssh-copy-id -i $HOME/.ssh/id_rsa.pub cluster@ubuntu01-03<br>
cluster@ubuntu01-01:~$ ssh-copy-id -i $HOME/.ssh/id_rsa.pub cluster@ubuntu01-04<br>
cluster@ubuntu01-01:~$ ssh-copy-id -i $HOME/.ssh/id_rsa.pub cluster@ubuntu01-05<br>
</code></pre>

You will need the corresponding slave's password to running the above commands.<br>
Try:<br>
<br>
<pre><code>cluster@ubuntu01-01:~$ ssh ubuntu01-02<br>
cluster@ubuntu01-01:~$ ssh ubuntu01-03<br>
cluster@ubuntu01-01:~$ ssh ubuntu01-04<br>
cluster@ubuntu01-01:~$ ssh ubuntu01-05<br>
</code></pre>

You should now be able to log in without password.<br>
<br>
<h1>4. First run</h1>
You should format the HDFS (Hadoop Distributed File System).<br>
<br>
Run the following command on the master:<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/hadoop namenode -format<br>
</code></pre>

<h1>5. Start Cluster</h1>

<h2>1) Start HDFS Daemons</h2>
Run the following command on master:<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/start-dfs.sh<br>
</code></pre>

Use the following command on every node to check the status of daemons:<br>
<br>
<pre><code>jps<br>
</code></pre>

Run jps on master, you should see something like this:<br>
<pre><code>8211 DataNode<br>
7803 NameNode<br>
8620 Jps<br>
8354 SecondaryNameNode<br>
</code></pre>

Run jps on the slaves, you should see something like this:<br>
<br>
<pre><code>8211 DataNode<br>
8620 Jps<br>
</code></pre>

<h2>2) Start MapReduce Daemons</h2>
Run the following command on master:<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/start-mapred.sh<br>
</code></pre>

Use the following command on every node to check the status of daemons:<br>
<pre><code>jps<br>
</code></pre>

Run jps on master, you should see something like this:<br>
<pre><code>8211 DataNode<br>
7803 NameNode<br>
8547 TaskTracker<br>
8620 Jps<br>
8422 JobTracker<br>
8354 SecondaryNameNode<br>
</code></pre>

run jps on the slaves, you should see something like this:<br>
<pre><code>8211 DataNode<br>
8547 TaskTracker<br>
8620 Jps<br>
</code></pre>

<h1>6. Hadoop Web Interfaces</h1>
There are some web interfaces that let you know the live progress report for the running Hadoop jobs.<br>
<pre><code>http://localhost:50030/ – web UI for MapReduce job tracker(s)<br>
</code></pre>

<pre><code>http://localhost:50060/ – web UI for task tracker(s)<br>
</code></pre>

<pre><code>http://localhost:50070/ – web UI for HDFS name node(s)<br>
</code></pre>

<h1>7. Run a Map Reduce Job<code>(WordCount)</code></h1>
Create a directory named "input" in HDFS:<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/hadoop dfs -mkdir input<br>
</code></pre>
Copy some text file into input<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/hadoop dfs -put conf/* input<br>
</code></pre>

Run WordCount<br>
<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/hadoop jar hadoop-0.20.2-examples.jar wordcount input output<br>
</code></pre>

Display output:<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/hadoop dfs -cat output/*<br>
</code></pre>


<h1>8. Stop Cluster</h1>

Close MapReduce daemons<br>
<br>
Run on master:<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/stop-mapred.sh<br>
</code></pre>

Close HDFS daemons<br>
<br>
Run on master:<br>
<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/stop-dfs.sh<br>
</code></pre>


<h1>9 Configure Fair scheduler</h1>
Various pools can be created under Hadoop cluster to run multiple jobs in parallel.<br>
Here we have a fair-scheduler implementation xml configuration file <a href='http://distmap.googlecode.com/files/fair-scheduler.xml'>http://distmap.googlecode.com/files/fair-scheduler.xml</a>


<h1>10. Run <code>DistMap</code></h1>

To run <code>DistMap</code>:<br>
<br>
<b>10.1 Start <code>MapReduce</code> daemons</b>

Run on master:<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/start-mapred.sh<br>
</code></pre>


<b>10.2 Start HDFS daemons</b>

Run on master:<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/start-dfs.sh<br>
</code></pre>

<b>10.3 Change HDFS filesystem permission</b>

Run on master:<br>
<br>
<pre><code>/usr/local/hadoop-1.0.3/bin/hadoop dfs -chmod -R 777 /<br>
</code></pre>

<b>10.4 Set permission for tmp folder</b>
<pre><code>/usr/local/hadoop-1.0.3/bin/hadoop dfs -chmod -R 777 /usr/local/hadoop-1.0.3/tmp/<br>
</code></pre>


<b>10.5 Go to <code>DistMap</code> manual page</b> <a href='http://code.google.com/p/distmap/wiki/Manual'>http://code.google.com/p/distmap/wiki/Manual</a><a href='http://code.google.com/p/distmap/wiki/Manual'>http://code.google.com/p/distmap/wiki/Manual</a>