# Word Count Design Approach for list TopN words from books in the Digital Libraries

## Logical architecture diagram
<img src="./WordCount.svg">

##
## Key Design Considerations

1. **What factors do you have to take into consideration? What tradeoffs do you make?**
	- Maximum storage account capacity
	- Given large volume of data, in order to scale, we will do the parallel processing of word count compute.
	- Compute will be in the batch mode and not in real time.
	- Simplified architecture with minimum platform componets.
2. **How do you handle such a huge volume of data?**
	- Store books retrieved from e digital library in separate buckets/partitions for the distributed processing in parallel. 
        - In this case, suggestion is to bucketize using tile starting with 'A',  'B"...'Z' stored on separate/multiple storage accounts.
	- To contain storage cost, store the data files in cheapest storage, compressed. 
3. **What components such a platform would have?**
	- Assuming Azure Cloud
		- Blob storage with multiple storage accounts - A to Z.
		- HDInsight (Hadoop / MapReduce Framework) - mulitple clusters A to Z for compute for each storage account
		- Function App Service Plan
		- Azure Queue
		- Azure Monitor/Application Insights
		- AzureDevOps/GitHub
4. **How would those components interact with each other to solve your problems?**
	-  Provision the platform components, as the code.
	-  Deploy the applicaton code. 
	-  Wrapper programs (Java based) to invoke various rest API calls to download books from digital library to the blob storage (till storage fills up).
	-  A PowerShell Azure function to invoke 	
	-  A driver program (Java based):
		- Read TopN parameter value from the parameter file
		- Read and cache Stop word list from the stop word list.
		- Read data (books) from the blob storage
		- Filter out the "Stop" word
		- Generate mapping (Token, count)
		- Summarize to produce mapping (Token, count) with intermediate results
	-  A second driver program (java based) to read indeterminate results and produce final results in an output file. 
	-  Deprovision the platform components.	-  
5. **How would the programmer and user interact with it? What kind of abstractions would you build?**
	- Inputs
		- User will input/specify top N word parameter in the file
		- User will input/specify "stop" word to filter in the stop world list file 
		- An output text file will be created listing top N worlds
		- Use will place a trigger file to kick-off or stop the word count process.
	- Output
		- An output text file will be created listing top N worlds
	- Abstractions
		- 
6. **What kind of SLA guarantees you can provide or not to provide?**
	- Since this a batch data processing and not a real time analysis use case - SLA would be in Day(s)
	- For more real time analysis needs, we can use spark (with word count program in Scala) instead of MapReduce to speed up computation but it has cost implications. 
	- Not fit for purpose in this specific use case, but another option is to use hybrid approach also referred as Lambda Architecture.
7. **What enterprise concerns that the platform should address for enterprises like KO to embrace it?**
	- Cost effective to provision 
	- Provisioning and Deprovisioning of platform  components on demand. 
	- Nice to have, integration with Azure AD for the data security
8. **How would you make sure your word counts are in fact accurate?**
	- Test the program with a one or two small files with a short one or two paragraphs and a short list of stop words.
        - Verify the results with manual calculations.
9. **Are there any out of the box patterns, utilities or libraries that you will leverage?**
	- Pattern	
		- MapReduce
	- Libraries
		- java.io.IOException;
		- java.util.StringTokenizer;

		- org.apache.hadoop.conf.Configuration;
		- org.apache.hadoop.fs.Path;
		- org.apache.hadoop.io.IntWritable;
		- org.apache.hadoop.io.Text;
		- org.apache.hadoop.mapreduce.Job;
		- org.apache.hadoop.mapreduce.Mapper;
		- org.apache.hadoop.mapreduce.Reducer;
		- org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
		- org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;	
		- org.apache.hadoop.util.GenericOptionsParser;

