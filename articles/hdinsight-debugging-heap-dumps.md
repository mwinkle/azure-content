<properties linkid="manage-services-hdinsight-debugging-with-heap-dumps" urlDisplayName="Debugging HDInsight with Heap Dumps" pageTitle="How to Debug HDInsight via Heap Dumps | Azure" metaKeywords="hdinsight, hdinsight development, hadoop development, hdinsight debugging,  hdinsight deployment, development, deployment, tutorial, MapReduce, Java" description="Learn how to collect heap dumps from specific Hadoop services to aid in debugging." services="hdinsight" title="Develop Java MapReduce programs for Hadoop in HDInsight" umbracoNaviHide="0" disqusComments="1" editor="cgronlun" manager="paulettm" authors="nitinme" />

<tags ms.service="hdinsight" ms.workload="big-data" ms.tgt_pltfrm="na" ms.devlang="Java" ms.topic="article" ms.date="10/10/2014" ms.author="nitinme" />


# Collecting Heap Dumps for Debugging and Analysis

Heap dumps can be automatically collected now and placed inside the customer's blob storage account under HDInsightHeapDumps/<service_timestamp>.hprof.  These heap dumps can be large in size so it is advisable to monitor the storage account. The default of this feature is to be off, however it is possible to enable the heap dumps as well as stack traces per subscription. 

The services which can have heap dumps enabled if requested are:
hcatalog: tempelton                         
hive: hiveserver2, metastore, derbyserver
mapreduce: jobhistoryserver
yarn: resourcemanager, nodemanager, timelineserver
hdfs: datanode, secondarynamenode, namenode

In order to turn on heap dumps the user needs to set a configuration for each service, under the corresponding section, that he would like to turn on heap dumps where <service> corresponds to the service he would like to turn on:

1.	"javaargs.<service>. XX:+HeapDumpOnOutOfMemoryError" =
 "-XX:+HeapDumpOnOutOfMemoryError",

2.	"javaargs.<service>.XX:HeapDumpPath" =
"-XX:HeapDumpPath=c:\\Dumps\\<service>_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"  

For example to turn on heap dumps for jobhistoryserver the user would do the following:

**Using powershell sdk:**


	mapredConfigvalues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"  	="-XX:+HeapDumpOnOutOfMemoryError", "javaargs.jobhistoryserver.XX:HeapDumpPath" = 	"-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"  }

**Using c# sdk:**

	clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));
            
	clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));

