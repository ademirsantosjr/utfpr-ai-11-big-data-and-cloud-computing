
# Practical Guide: First Steps with HDFS in Google Colab

After the theoretical exploration of Distributed Systems, Apache Hadoop, and HDFS, it's time to put the knowledge into practice. This guide will lead you through setting up a Hadoop environment in "pseudo-distributed" mode and performing basic operations in HDFS, all within a Google Colab notebook.

The pseudo-distributed mode simulates a cluster on a single machine, allowing all Hadoop processes (NameNode, DataNode, etc.) to run locally. It is the ideal environment for learning and experimentation, as the commands used are exactly the same as in a real cluster with multiple machines.

## 1. Environment Setup in Google Colab

The first step is to prepare the Colab virtual machine to run Hadoop.

### 1.1. Download and Unpack Hadoop

Let's download version 3.3.0 of Hadoop directly from the Apache repository.

```bash
# Download the Hadoop file (may take a few minutes)
!wget https://archive.apache.org/dist/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz

# Unpack the downloaded file
!tar -xzvf hadoop-3.3.0.tar.gz

# Copy the files to the recommended directory
!cp -r hadoop-3.3.0/ /usr/local/
```

### 1.2. Configuration of Environment Variables

Hadoop depends on Java and needs the `JAVA_HOME` environment variable to be correctly configured. We will also define `HADOOP_HOME` to facilitate the execution of commands.

```python
import os

# Find the Java installation path
java_home = !readlink -f /usr/bin/java | sed "s:bin/java::"

# Define the environment variables
os.environ["JAVA_HOME"] = java_home[0]
os.environ["HADOOP_HOME"] = "/usr/local/hadoop-3.3.0"
```

With the environment configured, we can now interact with Hadoop.

## 2. Exploring HDFS with the File System (FS) Shell

The `hadoop fs` is the command-line interface (CLI) for interacting with HDFS. It offers a transparent experience, making a distributed file system on thousands of nodes look like a local system.

### 2.1. Viewing Available Commands

To see the list of all possible commands, run:

```bash
!$HADOOP_HOME/bin/hadoop fs
```

You will see a list of familiar operations for those who have used a Linux terminal, such as `ls`, `mkdir`, `cp`, `rm`, among others.

### 2.2. Basic Operations in HDFS

Let's perform some fundamental operations.

*   **List files in the root directory (`/`)**

    Hadoop already comes with a sample data directory.

    ```bash
    !$HADOOP_HOME/bin/hadoop fs -ls /
    ```

*   **Create a new directory**

    Let's create a directory called `my_test`.

    ```bash
    !$HADOOP_HOME/bin/hadoop fs -mkdir /my_test
    ```

*   **Copy a file**

    Let's copy a sample file to our new directory.

    ```bash
    !$HADOOP_HOME/bin/hadoop fs -cp /usr/local/hadoop-3.3.0/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.0.jar /my_test
    ```

*   **View the content of a file**

    The `cat` command displays the full content of a file. For large files, you can use `head` (to see the beginning) or `tail` (to see the end).

    ```bash
    # This command would give an error, as the .jar is not a text file.
    # Example with a text file (if it existed):
    # !$HADOOP_HOME/bin/hadoop fs -cat /path/to/file.txt
    ```

*   **Remove a file and a directory**

    The `rm` command removes files and `rmdir` removes directories (which must be empty).

    ```bash
    # Remove the copied file
    !$HADOOP_HOME/bin/hadoop fs -rm /my_test/hadoop-mapreduce-examples-3.3.0.jar

    # Remove the directory
    !$HADOOP_HOME/bin/hadoop fs -rmdir /my_test
    ```

## 3. Proposed Practical Activity

Now it's your turn to experiment. Using the commands presented, perform the following tasks in HDFS:

1.  **Create a root directory with your name.**
    *   Example: `!$HADOOP_HOME/bin/hadoop fs -mkdir /john`

2.  **Inside it, create another directory with your last name.**
    *   Example: `!$HADOOP_HOME/bin/hadoop fs -mkdir /john/doe`

3.  **Send a sample file to the directory with your name.**
    *   Use the `put` command to send a file from the local Colab machine to HDFS.
    *   First, create a local file: `!echo "File test" > test.txt`
    *   Then, send it to HDFS: `!$HADOOP_HOME/bin/hadoop fs -put test.txt /john`

4.  **List the contents of the directory with your name.**
    *   Example: `!$HADOOP_HOME/bin/hadoop fs -ls /john`

5.  **Delete the directory with your last name.**
    *   Example: `!$HADOOP_HOME/bin/hadoop fs -rmdir /john/doe`

This practical exercise provides a solid foundation for file manipulation in HDFS. In the next stages of study, you will see how MapReduce and Spark applications can be run on the data stored in this distributed file system.
