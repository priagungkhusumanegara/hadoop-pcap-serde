# hadoop-pcap-serde


This jar can be used to read pcap using hive

1. Copy pcap file into folder pcaps in HDFS
   hadoop fs -mkdir /pcaps
   hadoop fs -put test.pcap /pcaps/
 
2. Enter to hive shell
   hive> ADD JAR hadoop-pcap-serde-0.2-SNAPSHOT-jar-with-dependencies.jar;
   
	 hive> SET hive.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;
   
	 hive> SET mapred.max.split.size=104857600;
   
	 hive> SET net.ripe.hadoop.pcap.io.reader.class=net.ripe.hadoop.pcap.DnsPcapReader;
   
	 hive> CREATE EXTERNAL TABLE pcaps (ts bigint,
                      protocol string,
                      src string,
                      src_port int,
                      dst string,
                      dst_port int,
                      len int,
                      ttl int,
                      dns_queryid int,
                      dns_flags string,
                      dns_opcode string,
                      dns_rcode string,
                      dns_question string,
                      dns_answer array,
                      dns_authority array,
                      dns_additional array)
                      ROW FORMAT SERDE 'net.ripe.hadoop.pcap.serde.PcapDeserializer'
                      STORED AS INPUTFORMAT 'net.ripe.hadoop.pcap.io.PcapInputFormat'
                      OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
                      LOCATION 'hdfs:///pcaps/';
                      
                      
     hive > SELECT * FROM pcaps LIMIT 10;
