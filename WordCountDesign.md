# Word Count Design Approach for list TopN words from books in Digital Library

## Logical archtitecture diagram
<img src="./WordCount.svg">

##
## Design Consideations

1. **What factors do you have to take into consideration? What tradeoffs do you make?**
	- Given large volume need to do parallel processing of word count.
2. How do you handle such a huge volume of data?
	- Partioned the books in multiple blob containers/buckets  
3. What components such a platform would have?
4. How would those components interact with each other to solve your problems?
5. How would the programmer and user interact with it? What kind of abstractions would you build?
6. What kind of SLA guarantees you can provide or not to provide?
7. What enterprise concerns that the platform should address for enterprises like KO to embrace it?
8. How would you make sure your word counts are in fact accurate?
9. Are there any out of the box patterns, utilities or libraries that you will leverage?
