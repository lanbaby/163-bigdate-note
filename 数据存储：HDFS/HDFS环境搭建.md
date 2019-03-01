# HDFS环境搭建


1. 拷贝/home/hadoop/hadoop-2.7.6.tar.gz到自己的目录(以下操作都是在当前用户的家目录下进行操作的!)：
```
cp /home/hadoop/hadoop-2.7.6.tar.gz ~/
```

2. 解压
```
tar xzvf hadoop-2.7.6.tar.gz
```

3. 创建一个指向当前使用的hadoop的软连接，之所以要创建是为了以后在多版本hadoop共存时，方便知道自己使用的是哪个版本的hadoop，不至于混淆
```
ln -s hadoop-2.7.6 hadoop-current
```

4. 添加HADOOP_HOME环境变量，老师讲的是直接将hadoop的目录添加到PATH，这里的方法本质一样，只不过同时配置了HADOOP_HOME环境变量：

```
打开~/.bashrc文件，在文件末尾添加：
# hadoop
export HADOOP_HOME=/mnt/home/1015146591/hadoop-current
export PATH=$HADOOP_HOME/bin:$PATH

```

5. 配置JAVA_HOME环境变量
```
打开~/.bashrc文件，在文件末尾添加：
# JDK
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
export PATH=$JAVA_HOME/bin:$PATH
```
然后 source ~/.bashrc，使配置生效。

6. 配置hadoop-2.7.6/etc/hadoop/core-site.xml
```
<configuration>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://bigdata0:33271</value>
        </property>
</configuration>
```

7. 配置hdfs-site.xml，注意要将其中的端口换成自己的端口（具体可以查看大数据新手村作业评语），另外要在自己目录下创建name和data目录，并在data目录下创建0、1、2这三个目录，配置到xml文件中去。

- 先创建name文件夹：
```
mkdir name
```

- 在创建data文件夹以及其子文件夹：
```
mkdir -p data/{0..2}
```

- 配置hdfs-site.xml(注意要把Namenode和Datanode相关的端口信息改成你自己的端口信息,33271~33275是网易分配给我的,要替换成你们自己的!另外namenode和datanode的dir也换成你们自己的目录!)：
```
<configuration>
   <property>
        <name>dfs.namenode.rpc-address</name>
        <value>bigdata0:33271</value>
    </property>
    <property>
        <name>dfs.namenode.http-address</name>
        <value>bigdata0:33272</value>
    </property>

    <property>
        <name>dfs.datanode.address</name>
        <value>bigdata0:33273</value>
    </property>
    <property>
        <name>dfs.datanode.http.address</name>
        <value>bigdata0:33274</value>
    </property>
    <property>
        <name>dfs.datanode.ipc.address</name>
        <value>bigdata0:33275</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/mnt/home/1015146591/name</value>
    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/mnt/home/1015146591/data/0,/mnt/home/1015146591/data/1,/mnt/home/1015146591/data/2</value>
    </property>
</configuration>
```

8. 初次启动hdfs，格式化：
```
hdfs namenode -format
```

9. 启动namenode
```
hdfs namenode
```

10. 使用浏览器查看hdfs，在浏览器输入10.173.32.5:33272，**注意33272是我配置的自己的端口好，你们根据自己配置的端口号进行替换即可，在dfs-site.xml文件中查看dfs.namenode.http-address字段的值就是**


11. 启动datanode
```
hdfs datanode
```

12. 配置其他远程主机的Hadoop环境，与bigdata0配置流程一样，注意把hdfs-site.xml中的datanode相关字段的bigdata0换成相应的bigdata1、bigdata2、bigdata3、bigdata4即可，namenode相关的value不用改变，因为namenode用的都是bigdata0上的namenode。
