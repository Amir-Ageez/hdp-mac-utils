# HDP 1.3.0 Mac Utils

A collection of scripts with instructions for installing HDP 1.3.0 (from tarballs) on a Mac.

## Motivation

Provide a working HDP installation on a mac for local use.  Even with the HDP Sandbox, it's sometimes nice to have something on your main OS.  This also proved a valuable exercise to become acquainted with (and still) the various aspects of an HDP manual installation via "tarballs".

## Assumptions
This process assumes you have "[brew](http://mxcl.github.io/homebrew/)" installed.  Before starting, you will need to install "wget" (ie: <code>brew install wget</code>), which we use to pull files down from the web.

You will also need to install "MySQL" via brew (ie: <code>brew install mysql</code>).

Everything will be install AND run from you local user account. 

## Where to Start
The 'do.sh' script will kickoff the installation process.

## Post Installation - House Keeping

### Remove Compression Codecs from core-site.xml

### Environment Variables
> Set the following environment variables:
<pre><code>export HIVE_LOG_DIR=/var/log/hive</code></pre>

### Namenode
> If this is the first-time you've installed HDP, you will need to initialize the Hadoop filesystem with:
	<pre><code>hadoop namenode -format</code></pre>
> If your upgrade to a new HDP version, you may need to update the namenode before starting HDFS.

### Hive
  
### Oozie
> TODO: Get the extjs-2.2 jar and add it to the libs for oozie web.

## Starting Hadoop, etc.. Short-version
### HDFS and MAPRED
<pre><code>/usr/lib/hadoop/bin/start-all.sh</code></pre>
> We've installed a few launch scripts in /usr/bin
### Hive Metastore
<pre><code>start-hive-metastore.sh</code></pre>
> See below for instructions to smoke test hive.

## Starting Hadoop, etc.. Long-version
### HDFS and MAPRED:
<pre><code>cd /usr/lib/hadoop/bin
./start-all.sh</code></pre>
### Hive and Hiveserver2
1. Start Hive Metastore service.
	<pre><code>nohup hive --service metastore>$HIVE_LOG_DIR/hive.out 2>$HIVE_LOG_DIR/hive.log & </code></pre>
2. Smoke Test Hive.
	1. Open Hive command line shell. <pre><code>hive</code></pre>
	2. Run sample commands.
		<pre><code>show databases;
		create table test(col1 int, col2 string);
		show tables;</code></pre> 
3. Start HiveServer2.
	<pre><code>nohup /usr/lib/hive/bin/hiveserver2>$HIVE_LOG_DIR/hiveserver2.out 2>$HIVE_LOG_DIR/hiveserver2.log &</code></pre> 
4. Smoke Test HiveServer2.
	1. Open Beeline command line shell to interact with HiveServer2.
	   <pre><code>beeline</code></pre>
	2. Establish connection to server.
		<pre><code>!connect jdbc:hive2://localhost:10000 $USER password org.apache.hive.jdbc.HiveDriver</code></pre>
    3. Run sample commands.
		<pre><code>show databases;
		create table test2(a int, b string);
		show tables;</code></pre>

## Status of Components install here

### Working
> * HDFS
> * MAPRED
> * Hive
> * Pig

### Todo's
> * Oozie
> * HBase
> * HCat
> * Flume
> * Sqoop

## Scripts

### do.sh [temp_dir]

> This is the main script that will complete the installation and configuration.  
> Once this is complete, everything will be in the correct place and you will only
> need to manually adjust the template configurations for you localhost environment.

### mac_env.sh

> Used to control the parameters used by the rest of the scripts.

### get_artifacts.sh [temp_dir]

> A subscript used to fetch the HDP base artifacts and a few
> other helper file sets used to complete the installation.

### expand_link.sh

> Subscript used to expand and link the artifacts retrieved.

### etc_default.tar.gz

> Contains the defaults (pulled from CentOS install) used by the hadoop scripts
> to properly run the environment.

### hdp_artifacts.txt

> A list of HDP artifacts and links to use for each.

### jdbc_cfg.txt

> A file the contains the location of jdbc drivers and the symlink to create for them.
> These are used by Hive.

### reset.sh [temp_dir]

> Use this to remove the installed HDP libraries and configuration files.