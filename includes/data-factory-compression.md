### Compression support  
Processing large data sets can cause I/O and network bottlenecks. Therefore, compressed data in stores can not only speed up data transfer across the network and save disk space, but also bring significant performance improvements in processing big data. Currently, compression is supported for file-based data stores such as Azure Blob or On-premises File System.  

> [AZURE.NOTE] Compression settings are not supported for data in the **AvroFormat**, **OrcFormat**, or **ParquetFormat**. 

To specify compression for a dataset, use the **compression** property in the dataset JSON as in the following example:   

	{  
		"name": "AzureBlobDataSet",  
	  	"properties": {  
	    	"availability": {  
	    		"frequency": "Day",  
	    	  	"interval": 1  
	    	},  
	    	"type": "AzureBlob",  
	    	"linkedServiceName": "StorageLinkedService",  
	    	"typeProperties": {  
	    		"fileName": "pagecounts.csv.gz",  
	    	  	"folderPath": "compression/file/",  
	    	  	"compression": {  
	    	    	"type": "GZip",  
	    	    	"level": "Optimal"  
	    	  	}  
    		}  
	  	}  
	}  
 
The **compression** section has two properties:  
  
- **Type:** the compression codec, which can be **GZIP**, **Deflate** or **BZIP2**.  
- **Level:** the compression ratio, which can be **Optimal** or **Fastest**. 
	- **Fastest:** The compression operation should complete as quickly as possible, even if the resulting file is not optimally compressed. 
	- **Optimal**: The compression operation should be optimally compressed, even if the operation takes a longer time to complete. 
	
	For more information, see [Compression Level](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) topic. 

Suppose the above sample dataset is used as the output of a copy activity, the copy activity compresses the output data with GZIP codec using optimal ratio and then write the compressed data into a file named pagecounts.csv.gz in the Azure Blob Storage.   

When you specify compression property in an input dataset JSON, the pipeline can read compressed data from the source and when you specify the property in an output dataset JSON, the copy activity can write compressed data to the destination. Here are a few sample scenarios: 

- Read GZIP compressed data from an Azure blob, decompress it, and write result data to an Azure SQL database. You define the input Azure Blob dataset with the compression JSON property in this case. 
- Read data from a plain-text file from on-premises File System, compress it using GZip format, and write the compressed data to an Azure blob. You define an output Azure Blob dataset with the compression JSON property in this case.  
- Read a GZIP-compressed data from an Azure blob, decompress it, compress it using BZIP2, and write result data to an Azure blob. You define the input Azure Blob dataset with compression type set to GZIP and the output dataset with compression type set to BZIP2 in this case.   
