---
title: MapReduce
date: 2020-05-06 09:28:19
tags:
	- Hadoop
	- Mapreduce
---

Hadoop MapReduce是一个软件框架，用于轻松编写应用程序，这些应用程序以可靠，容错的方式并行处理大型硬件集群（数千个节点）上的大量数据（多TB数据集）。

MapReduce作业通常将输入数据集分成独立的块，这些任务由Map任务以完全并行的方式处理。
框架对地图的输出进行排序，然后将其输入到reduce任务。
通常，作业的输入和输出都存储在文件系统中。
该框架负责安排任务，监视任务并重新执行失败的任务。

通常，计算节点和存储节点是相同的，即MapReduce框架和Hadoop分布式文件系统（请参阅HDFS体系结构指南）在同一组节点上运行。
此配置使框架可以在已经存在数据的节点上有效地调度任务，从而在整个群集中产生很高的聚合带宽。

MapReduce框架由一个主资源管理器，每个群集节点一个工作器NodeManager和每个应用程序MRAppMaster组成（请参阅YARN体系结构指南）。

至少，应用程序通过适当的接口和/或抽象类的实现来指定输入/输出位置和供应图，并减少功能。
这些以及其他作业参数构成作业配置。

然后，Hadoop作业客户端将作业（jar /可执行文件等）和配置提交给ResourceManager，然后由ResourceManager负责将软件/配置分发给工作人员，安排任务并对其进行监视，为工作提供状态和诊断信息，
客户。

尽管Hadoop框架是用Java实现的，但MapReduce应用程序不必用Java编写。

<!-- more -->

### 包装类

https://zhuanlan.zhihu.com/p/78590948



### 序列化

https://www.zhihu.com/question/47794528

https://www.jianshu.com/p/f62553fd69c2

https://juejin.im/post/5ce3cdc8e51d45777b1a3cdf

序列化：将一个对象用二进制表示

作用：把对象字节序列永久保存在硬盘上；用于网络传输



### 持久化

https://blog.csdn.net/lee576/article/details/1927021



### MapReduce中的Writable

https://acadgild.com/blog/writable-and-writablecomparable-in-hadoop



### 官方WordCount项目阅读

```java
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class WordCount {

    public static class TokenizerMapper
            extends Mapper<Object, Text, Text, IntWritable> {

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
        ) throws IOException, InterruptedException {
            StringTokenizer itr = new StringTokenizer(value.toString());
            while (itr.hasMoreTokens()) {
                word.set(itr.nextToken());
                context.write(word, one);
            }
        }
    }

    public static class IntSumReducer
            extends Reducer<Text, IntWritable, Text, IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
        ) throws IOException, InterruptedException {
            int sum = 0;
            for (IntWritable val : values) {
                sum += val.get();
            }
            result.set(sum);
            context.write(key, result);
        }
    }

    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

过程：

* JobTracker（用于分配和调度任务，一个集群只有一个JobTracker，类似于NameNode的作用）与TaskTracker（在集群的每台的机器上执行MapReduce任务，类似于DataNode）

* 设置job：

  ```java
  Configuration conf = new Configuration();
  Job job = Job.getInstance(conf, "word count");
  ```

* 配置相关内容：

  ```java
  job.setJarByClass(WordCount.class);//设置当前类
  job.setMapperClass(TokenizerMapper.class);//设置充当Mapper的类
  job.setCombinerClass(IntSumReducer.class);//设置充当Combiner的类
  job.setReducerClass(IntSumReducer.class);//设置充当Reducer的类
  job.setOutputKeyClass(Text.class);//设置最后输出的key的类型
  job.setOutputValueClass(IntWritable.class);//设置最后输出的value的类型
  ```

  上述类型是Mapper与Reducer的输出的key  value的类型相同的情况，如果类型不同，需要设置`setMapOutPutKeyClass`与`setMapOutPutValueClass`

  > 参考链接：https://www.cnblogs.com/dtj007/p/5485629.html
  >
  > https://blog.csdn.net/maixia24/article/details/14002235
  >
  > https://blog.csdn.net/qq_16403141/article/details/77598532?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1

* 设置输入输出路径

* mapper类

  ```java
  public static class TokenizerMapper extends Mapper<Object, Text, Text, IntWritable> {
          private final static IntWritable one = new IntWritable(1);
    			//文件数据已经被处理为key，value的类型。key是偏移量，value是文本。<Object, Text, Text, IntWritable>表示输入<Object, Text>, 然后输出<Text, IntWritable>
    			//在MapReduce中的一切数据要序列化, 这里将int类型的1转化为Writable的值。
          private Text word = new Text();
          public void map(Object key, Text value, Context context
          ) throws IOException, InterruptedException {
            //Context是相当于与MapReduce框架进行交互的接口，数据在各个机器之间的传递由框架来完成。
              StringTokenizer itr = new StringTokenizer(value.toString());
            //将输入的value转化为字符串，并且转化为单词列表
              while (itr.hasMoreTokens()) {
                  word.set(itr.nextToken());
                  context.write(word, one);
                //对于每个单词都转化为<word, 1>的键值对
              }
          }
      }
  ```

* reducer类

  ```java
  public static class IntSumReducer
              extends Reducer<Text, IntWritable, Text, IntWritable> {
          private IntWritable result = new IntWritable();
  
          public void reduce(Text key, Iterable<IntWritable> values,
                             Context context
          ) throws IOException, InterruptedException {
            //Iterable<IntWritable>，接受<key,[1,1,1]>中的[1,1,1], [1,1,1]的形成过程由MapReduce框架自己完成了。
              int sum = 0;
              for (IntWritable val : values) {
                  sum += val.get();//计算和
              }
              result.set(sum);
              context.write(key, result);
          }
      }
  ```



### MapReduce程序的基本框架

```java
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.input.FileSplit;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.lib.partition.HashPartitioner;
import org.apache.hadoop.util.GenericOptionsParser;

public class InvertedIndex {
    /**
     * Mapper部分
     **/
    public static class InvertedIndexMapper extends Mapper<Object, Text, Text, IntWritable> {
        private final static IntWritable one = new IntWritable(1);//常量1

        /**
         * 对输入的Text切分为多个word,每个word作为一个key输出
         * 输入：key:当前行偏移位置, value:当前行内容
         * 输出：key:word, value:1
         */
        public void map(Object key, Text value, Context context) throws IOException, InterruptedException {
            
            Text word = new Text();
            StringTokenizer itr = new StringTokenizer(value.toString());
            for (; itr.hasMoreTokens(); ) {
                word.set(itr.nextToken());
                context.write(word, one);//输出<word, 1>
            }
        }
    }

    /**
     * Combiner部分
     **/
    public static class SumCombiner extends Reducer<Text, IntWritable, Text, IntWritable> {
        private IntWritable result = new IntWritable();

        /**
         * 将Mapper输出的中间结果相同key部分的value累加，减少向Reduce节点传输的数据量
         * 输出：key:word, value:累加和
         */
        public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
            int sum = 0;
            for (IntWritable val : values) {
                sum += val.get();
            }
            result.set(sum);
            context.write(key, result);
        }
    }

    /**
     * Partitioner部分
     **/
    public static class NewPartitioner extends HashPartitioner<Text, IntWritable> {
        /**
         * 为了将同一个word的键值对发送到同一个Reduce节点，对key进行临时处理
         */
        public int getPartition(Text key, IntWritable value, int numReduceTasks) {
            String term = key.toString();
            return super.getPartition(new Text(term), value, numReduceTasks);
        }
    }

    /**
     * Reducer部分
     **/
    public static class InvertedIndexReducer extends Reducer<Text, IntWritable, Text, Text> {

        public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
           
        }


    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        Job job = new Job(conf, "name");
        job.setJarByClass(InvertedIndex.class);
        job.setMapperClass(InvertedIndexMapper.class);
        job.setCombinerClass(SumCombiner.class);
        job.setReducerClass(InvertedIndexReducer.class);
        job.setNumReduceTasks(5);//设定使用Reduce节点个数
        job.setPartitionerClass(NewPartitioner.class);
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

> 参考链接：[https://www.polarxiong.com/archives/Hadoop-MapReduce-%E5%B8%A6%E8%AF%8D%E9%A2%91%E5%B1%9E%E6%80%A7%E7%9A%84%E6%96%87%E6%A1%A3%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95.html](https://www.polarxiong.com/archives/Hadoop-MapReduce-带词频属性的文档倒排索引.html)



### 另一个简单的程序

```java
package cn.hadoop.mr.dc;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Partitioner;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class DataCount {

	public static void main(String[] args) throws Exception {
		Configuration conf = new Configuration();
		
		Job job = Job.getInstance(conf);
		
		job.setJarByClass(DataCount.class);
		
		job.setMapperClass(DCMapper.class);
		job.setMapOutputKeyClass(Text.class);
		job.setMapOutputValueClass(DataInfo.class);
		
		job.setReducerClass(DCReducer.class);
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(DataInfo.class);
		
		FileInputFormat.setInputPaths(job, new Path(args[0]));
		
		FileOutputFormat.setOutputPath(job, new Path(args[1]));
		
		job.setPartitionerClass(DCPartitioner.class);
		
		job.setNumReduceTasks(Integer.parseInt(args[2]));
		
		
		job.waitForCompletion(true);

	}

	public static class DCMapper extends Mapper<LongWritable, Text, Text, DataInfo>{
		
		private Text k = new Text();
		/*
		*map一行一行进行处理
		*
		*/
		@Override
		protected void map(LongWritable key, Text value,
				Mapper<LongWritable, Text, Text, DataInfo>.Context context)
				throws IOException, InterruptedException {
			String line = value.toString();
			String[] fields = line.split("\t");
			String tel = fields[1];
			long up = Long.parseLong(fields[8]);
			long down = Long.parseLong(fields[9]);
			DataInfo dataInfo = new DataInfo(tel,up,down);
			k.set(tel);
			context.write(k, dataInfo);

		}
		
	}
	public static class DCReducer extends Reducer<Text, DataInfo, Text, DataInfo>{
		
		@Override
		protected void reduce(Text key, Iterable<DataInfo> values,
				Reducer<Text, DataInfo, Text, DataInfo>.Context context)
				throws IOException, InterruptedException {
			long up_sum = 0;
			long down_sum = 0;
			for(DataInfo d : values){
				up_sum += d.getUpPayLoad();
				down_sum += d.getDownPayLoad();
			}
			DataInfo dataInfo = new DataInfo("",up_sum,down_sum);
			
			context.write(key, dataInfo);
		}
		
	}
	public static class DCPartitioner extends  Partitioner<Text, DataInfo>{
		
		private static Map<String,Integer> provider = new HashMap<String,Integer>();
		
		static{
			provider.put("138", 1);
			provider.put("139", 1);
			provider.put("152", 2);
			provider.put("153", 2);
			provider.put("182", 3);
			provider.put("183", 3);
		}
		@Override
		public int getPartition(Text key, DataInfo value, int numPartitions) {
			
			String tel_sub = key.toString().substring(0,3);
			Integer count = provider.get(tel_sub);
			if(count == null){
				count = 0;
			}
			return count;
		}
		
	}

}
```

```java
package cn.hadoop.mr.dc;

import java.io.DataInput;
import java.io.DataOutput;
import java.io.IOException;

import org.apache.hadoop.io.Writable;

public class DataInfo implements Writable{

	private String tel;
	private long upPayLoad;
	private long downPayLoad;
	private long totalPayLoad;
	
	public DataInfo(){}
	
	public DataInfo(String tel, long upPayLoad, long downPayLoad) {
		this.tel = tel;
		this.upPayLoad = upPayLoad;
		this.downPayLoad = downPayLoad;
		this.totalPayLoad = upPayLoad + downPayLoad;
	}

	@Override
	public void write(DataOutput out) throws IOException {
		out.writeUTF(tel);
		out.writeLong(upPayLoad);
		out.writeLong(downPayLoad);
		out.writeLong(totalPayLoad);
	}

	@Override
	public void readFields(DataInput in) throws IOException {
		this.tel = in.readUTF();
		this.upPayLoad = in.readLong();
		this.downPayLoad = in.readLong();
		this.totalPayLoad = in.readLong();
		
	}

	@Override
	public String toString() {
		return upPayLoad + "\t" + downPayLoad + "\t" + totalPayLoad;
	}

	public String getTel() {
		return tel;
	}

	public void setTel(String tel) {
		this.tel = tel;
	}

	public long getUpPayLoad() {
		return upPayLoad;
	}

	public void setUpPayLoad(long upPayLoad) {
		this.upPayLoad = upPayLoad;
	}

	public long getDownPayLoad() {
		return downPayLoad;
	}

	public void setDownPayLoad(long downPayLoad) {
		this.downPayLoad = downPayLoad;
	}

	public long getTotalPayLoad() {
		return totalPayLoad;
	}

	public void setTotalPayLoad(long totalPayLoad) {
		this.totalPayLoad = totalPayLoad;
	}

}
```

