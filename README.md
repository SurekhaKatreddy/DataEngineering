This is the era of data. 
Data has been all over in variety and volume but may not be readily available for consumption.
So the data engineers come to our aid by doing the source to the target mapping to extract the data from the source system, transform it
and send it to the target systems. During this process, data engineers build the ETL/ ELT pipelines.

What is a pipeline?
Set of tasks

# Understand the terminology
ETL - Extract Transform Load
ELT - Extract Load Transform

# ELT vs ETL
Traditionally when handling large scale data we perform ETL i.e Extract Transform and Load
In ETL, we do not have a copy of the source data stored locally.
We try to apply the transformation on the data to make the data compatible to the target format and store it in target system.
There is a chance of network congestion as we are trying to perform transformations on the data store elsewhere.

In ELT, we store a copy of the source data stored locally typically in the data lakes or distributed File systems like HDFS.
We now apply the transformation on local copy of the data thereby avoiding network congestion resulting in high performance. 
Also, when there is any data mismatch during the process of transforming the data, we have a copy of the source data at our expense
to compare and verify at which stage / task of the pipeline caused the data corruption and take corrective action.
