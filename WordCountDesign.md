# Word Count Design Approach for list TopN words from books in Digital Library

## Logical architecture diagram
<img src="./WordCount.svg">

##
## Key Design Considerations

1. **What factors do you have to take into consideration? What tradeoffs do you make?**
	- Given large volume of data, we need to do the parallel processing of word count compute.
2. **How do you handle such a huge volume of data?**
	-  Store books retrieved from digital library in separate buckets/partitions to for distributed processing in paralle.
         - One approach is to bucketize using tile starting with 'A',  'B"...'Z'  etc. 
3. **What components such a platform would have?**
	- Blob storage
	- Hadoop MapReduce Framework
4. **How would those components interact with each other to solve your problems?**
	-  Wrapper programs (Java based) to invoke various rest API calls to download books from digital library to the blob storage	
	-  A driver program (Java based) which will 
		- Read TopN parameter value from the parameter file
		- Read and cache Stop word list from the stop word list.
		- Read data (books) from the blob storage
		- Filter out the "Stop" word
		- Generate mapping (Token, count)
		- Summarize to produce mapping (Token, count) with intermediate results
	-  A driver program to read indeterminate results and produce final results in an output file. 
5. **How would the programmer and user interact with it? What kind of abstractions would you build?**
	- User will input/specify top N word parameter in the file
	- User will input/specify "stop" word to filter in the stop world list file 
	- An output text file will be created listing top N worlds
6. **What kind of SLA guarantees you can provide or not to provide?**
	- Since this a batch data processing and not a real time analysis use case - SLA would be in Day(s)
	- For more real time analysis needs, we can use spark (with word count program in Scala) instead of MapReduce to speed up computation but it has cost implications. 
	- Not fit for purpose in this specific use case, but another option is to use hybrid approach also referred as Lambda Architecture.
7. **What enterprise concerns that the platform should address for enterprises like KO to embrace it?**
	- Cost effective to provision 
	- Be able to spin-up/shutdown platform on demand. 
	- Nice to have integration with Azure AD for the data security
8. **How would you make sure your word counts are in fact accurate?**
	- Test the program with a one or two small files with a short one or two paragraphs and a short list of stop words.
        - Verify the results with manual calculations.. 
9. **Are there any out of the box patterns, utilities or libraries that you will leverage?**
	-Pattern	
		- MapReduce
	-Libraries
		- org.apache.hadoop.io.*
		- org.apache.hadoop.mapreduce.*
		- java.util.*
		- java.io.*

