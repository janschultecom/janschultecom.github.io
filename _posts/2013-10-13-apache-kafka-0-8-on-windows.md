---
layout: post
title: Apache Kafka 0.8 on Windows
tags: [kafka]
date: 2013-10-13
---
[Apache Kafka](http://kafka.apache.org/ "Apache Kafka") is a scalable, distributed messaging system, which is increasingly getting popular and used by such renowned companies like LinkedIn, Tumblr, Foursquare, Spotify and Netflix [1].

Setting up a Kafka development environment on a Windows machine requires some configuration, so I created this little step-by-step installation tutorial for all the people who want to save themselves from some hours work ;-)

##Setting up the Shell

To run Kafka you need a Linux shell under Windows. How do you do this? With the pretty awesome [Cygwin tool](http://cygwin.com/ "Cygwin").  So I'd like to go through the Cygwin installation, for all of you who have never used this tool beforehand. You can download Cygwin from [here](http://cygwin.com/install.html "Link to cygwin download page"). Most modern PCs have a modern 64-bit processor, so if you are one of these lucky guys, download the 64 bit version, otherwise, go for the 32 bit version. Start the setup-x86_64.exe (respectively the setup-x86.exe) and click on 'Next'. As the download source choose 'Install from Internet' and click 'Next'.

[![Cygwin download source](/images/apacke-kafka-0-8-on-windows/cygwin_download_source.png)](/images/apacke-kafka-0-8-on-windows/cygwin_download_source.png)

Choose the installation directory and the user you want to install it for. If you don't know what to do, my recommendation is to leave the default directory as it is and just install it for yourself.

![Cygwin installation directory](/images/apacke-kafka-0-8-on-windows/cygwin_installation_directory.png)

Select a temporary package where you whant the downloaded temporary files to be stored, e.g. "C:\Users\Jan\Downloads" if your name is also Jan.

[![Cygwin temporary download folder](/images/apacke-kafka-0-8-on-windows/cygwin_temporary_download_folder.png)](/images/apacke-kafka-0-8-on-windows/cygwin_temporary_download_folder.png)

Select your internet connection. If you are at home you'll most probably choose 'Direct connection'. If you are within a university or company network you might need to add  a proxy configuration in order to make the download work (although I guess choosing 'Use Internet Explorer Settings' should be sufficient). 

[![Cygwin connection](/images/apacke-kafka-0-8-on-windows/cygwin_connection.png)](/images/apacke-kafka-0-8-on-windows/cygwin_connection.png)

You then need to choose a site where the packages should be downloaded from. I usually pick a university server which is close to my home location.

![Cygwin download site](/images/apacke-kafka-0-8-on-windows/cygwin_download_site.png)

 Now you will get to a packages selection page where you can select all the useful linux tools and commands that you'd like to have installed. For our use case it is sufficient to leave the default configuration. However, if you are faced with some command not found errors lateron, you might reinstall Cygwin with the necessary packages.

[![Cygwin select packages](/images/apacke-kafka-0-8-on-windows/cygwin_select_packages.png)](/images/apacke-kafka-0-8-on-windows/cygwin_select_packages.png)

After Cygwin has finished downloading the packages, select 'Create icon on Desktop' and finish the installation.

[![Cygwin finish installation](/images/apacke-kafka-0-8-on-windows/cygwin_finish_installation.png)](/images/apacke-kafka-0-8-on-windows/cygwin_finish_installation.png)

Now lets test the Cygwin shell. Click on your Cygwin desktop icon. The shell should open with your home directoy as seen in the screen.

[![Cygwin shell](/images/apacke-kafka-0-8-on-windows/cygwin_shell.png)](/images/apacke-kafka-0-8-on-windows/cygwin_shell.png)

Now you got a full linux shell and do all the funny commands, such like 'ls' (the linux 'dir' equivalent). Go to the root folder by typing 'cd /'. Now enter 'ls' to show a list of the files & dirs at the root level.

[![Cygwin root dir](/images/apacke-kafka-0-8-on-windows/cygwin_root_dir.png)](/images/apacke-kafka-0-8-on-windows/cygwin_root_dir.png)

But you are not interested in the Cygwin root directory, but rather want to dive in to your Windows harddisk. The Windows drives are mounted under 'cygdrive'. So change directory by entering 'cd cygdrive' and list the content with 'ls'. You should now see all your drives. In my case I am on my laptop and have only the c drive. Change to the c drive and list the contents.

[![Cygwin windows c](/images/apacke-kafka-0-8-on-windows/cygdrive_windows_c.png)](/images/apacke-kafka-0-8-on-windows/cygdrive_windows_c.png)

To avoid having to type all this, you can automatically switch to another directory when the shell starts. Go to your Cygwin installation directory and enter the etc directory (in my case: C:\cygwin64\etc)  and open the file bash.bashrc with your favourite text editor. Go to the bottom of the file, add the line 'cd /cygdrive/c/' and save.

[![Cygwin bash.bashrc](/images/apacke-kafka-0-8-on-windows/cygwin_bashrc.png)](/images/apacke-kafka-0-8-on-windows/cygwin_bashrc.png)

Now open another shell by clicking on the Cygwin icon on your desktop. It should open with the c drive. 

[![Cygwin c drive](/images/apacke-kafka-0-8-on-windows/cygwin_c_drive.png)](/images/apacke-kafka-0-8-on-windows/cygwin_c_drive.png)

Ok, now that you have Cygwin up and running, let's unravel the mysteries of Kafka.

##Setting up Kafka

Download the latest 0.8 version of the Kafka binary from the [Kafka download page](http://kafka.apache.org/downloads.html "Kafka download page"). Note. that although the latest release marked as stable is the 0.7.2 release, the kafka guys have already switched the documentation to 0.8 and say that the 0.8 beta is already used in production. Now, extract the .tgz file. Now go to the extracted folder and extract the .tar file.

[![Extracted kafka folder](/images/apacke-kafka-0-8-on-windows/kafka_extracted.png)](/images/apacke-kafka-0-8-on-windows/kafka_extracted.png)

Now, copy the kafka folder and drop it to the c drive (in my case C:\kafka_2.8.0-0.8.0-beta1). I recommend you not to copy it to the 'Program Files' folder, since the space can cause problems with the shell scripts. Now the next step according to the kafka documentation would be to execute './sbt update'. So let's do that. Open the shell, switch to the kafka folder and execute the command.

[![Kafka sbt update](/images/apacke-kafka-0-8-on-windows/kafka_sbt_update.png)](/images/apacke-kafka-0-8-on-windows/kafka_sbt_update.png)

Oh no, we get an error '-bash: ./sbt: No such file or directory'. We are missing the sbt command! So for those of you who didn't know sbt before (like myself), a quick google search reveals that it's the scala build tool we are missing. Download the sbt windows installer from the [sbt download page](http://www.scala-sbt.org/release/docs/Getting-Started/Setup.html# "sbt download page"). Download the .msi installer for windows. Install sbt following the wizard (you can use the standard configuration). Since sbt writes itself to the environment variables, you will have to restart your shell in order for it to find sbt. So close the shell, open it again and go to the kafka folder. Execute 'sbt update'. Sbt will start doing its update work.

[![Kafka sbt update 2](/images/apacke-kafka-0-8-on-windows/kafka_sbt_update_2.png)](/images/apacke-kafka-0-8-on-windows/kafka_sbt_update_2.png)

After some time, the sbt should stop with the line [success] Total time: 2 s, completed 13-Oct-2013 11:21:32 Now, execute the second step from the kafka documentation, enter 'sbt package'. Again it should finish without errors. Finally, type

```sh
sbt assembly-package-dependency
```

Doh! We get an error: [error] Not a valid command: assembly-package-dependency [error] Not a valid project ID: assembly-package-dependency [error] Expected ':' (if selecting a configuration) [error] Not a valid key: assembly-package-dependency (similar: sbt-dependency) [error] assembly-package-dependency [error] ^

[![Kafka sbt dependency error](/images/apacke-kafka-0-8-on-windows/kafka_sbt_dependency_error.png)](/images/apacke-kafka-0-8-on-windows/kafka_sbt_dependency_error.png)

Now thats not good, we get a cryptic error message. Luckily, someone in the [wiki](https://cwiki.apache.org/confluence/display/KAFKA/Kafka+0.8+Quick+Start "Kafka wiki") alrea already found a solution, type 'sbt sbt-dependency'.

[![Kafka sbt dependency](/images/apacke-kafka-0-8-on-windows/kafka_sbt_dependency.png)](/images/apacke-kafka-0-8-on-windows/kafka_sbt_dependency.png)

Ok, so we should be ready to go! The first thing we have to do is to start the zookeeper server, which takes up the coordination among your kafka nodes. Start it with 'bin/zookeeper-server-start.sh config/zookeeper.properties'.

And again, we get an error

```sh
bin/kafka-run-class.sh: line 67: "C:/Program: No such file or directory
```

Again, the great & active kafka community has a solution for us. Go to the bin folder and edit the file 'kafka-run-class.sh'. In line 67 you will find the following:

```sh
$JAVA $KAFKA_OPTS $KAFKA_JMX_OPTS -cp $CLASSPATH "$@"
```

Now change that to

```sh
$JAVA $KAFKA_OPTS $KAFKA_JMX_OPTS -cp `cygpath -wp $CLASSPATH` "$@"
```

(replace $CLASSPATH with `cygpath -wp $CLASSPATH` including the backticks).

Now just before this line add the follow line

```sh
JAVA="java"
```

Now you must make sure that java is set in the PATH variable of your system environment variables. Go to the windows control panel and search for environment.

[![Windows environment variables](/images/apacke-kafka-0-8-on-windows/windows_environment_variables.png)](/images/apacke-kafka-0-8-on-windows/windows_environment_variables.png)

In the System Properties dialog open the Advanced tab and click on environment variables. If you have a PATH variable set, click on edit, otherwise create it. Add the path to your java bin directory. In my case C:\Program Files\Java\jdk1.7.0_21\bin (note, if you have some entry in the PATH variable, you need to separate them with a semicolon). 

[![Windows path variable](/images/apacke-kafka-0-8-on-windows/windows_environment_variable_path.png)](/images/apacke-kafka-0-8-on-windows/windows_environment_variable_path.png)

This will prevent you from errors if you have your java installed in the program files directory like i do.

Now go to the kafka config directory and edit the 'server.properties' file. Change line 51 from

```dosini
log.dir=/tmp/kafka-logs
```

to include double backslashes and pointing to a directory on your harddisk. E.g.

log.dir=c:\\kafka_2.8.0-0.8.0-beta1\\kafka-logs

If your c drive is a ssd, i recommend you to store the logs on your hdd since the logs can grow quite big. Remember to not use any spaces in the path to avoid problems and to use the double backslashes! E.g. d:\\data\\kafka-logs

Do the same for line 113, in my case I changed it to

kafka.csv.metrics.dir=c:\\kafka_2.8.0-0.8.0-beta1\\kafka-metrics

Save the file, edit the log4j.properties file. Change line 23 from

log4j.appender.kafkaAppender.File=server.log

to

log4j.appender.kafkaAppender.File=c:\\kafka_2.8.0-0.8.0-beta1\\logs\\server.log

Do the same for line 29 and 35. Finally, do the same for the zookeeper.properties file. Change line 16 from

dataDir=/tmp/zookeeper

to

dataDir=c:\\kafka_2.8.0-0.8.0-beta1\\dataDir

Now start again the zookeeper by entering

bin/zookeeper-server-start.sh config/zookeeper.properties

Zookeeper should start successfully and the Windows firewall will ask you whether you want to grant it access. Confirm this by clicking on Allow.

Ok, zookeeper is up and running, now let's start kafka. Open a second shell, go to the kafka folder and enter

bin/kafka-server-start.sh config/server.properties

Kafka up and running as well, so lets try it out! Open two more shells and switch to the kafka folder in both shells. In the first shell: Create a topic with the following line

bin/kafka-create-topic.sh --zookeeper localhost:2181 --replica 1 --partition 1 --topic test

This will create the topic 'test' . In the same shell,  enter

bin/kafka-list-topic.sh --zookeeper localhost:2181

Somewhere in the log output, there should appear the following line, indicating that you successfully created the test topic:

topic: test partition: 0 leader: 0 replicas: 0 isr: 0

Now we are going to push some messages to the queue. Start the basic producer by entering

bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

Some log information will be printed and the program does not return the control. So you can now freely enter messages (like 'Hello world!').

[![Kafka hello world](/images/apacke-kafka-0-8-on-windows/kafka_hello_world.png)](/images/apacke-kafka-0-8-on-windows/kafka_hello_world.png)

Go to the second shell and start the consumer by entering

bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning

The consumer will open and at the end of some log messages, it will display the message we just entered in the producer.

[![Kafka hello world consumer](/images/apacke-kafka-0-8-on-windows/kafka_hello_world_listen.png)](/images/apacke-kafka-0-8-on-windows/kafka_hello_world_listen.png)

Thats it, you are now ready to start developing for Kafka on Windows! If you like this article, please share it, thanks! [1] [https://cwiki.apache.org/confluence/display/KAFKA/Powered+By](https://cwiki.apache.org/confluence/display/KAFKA/Powered+By)
