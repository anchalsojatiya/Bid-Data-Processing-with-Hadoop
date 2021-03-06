# Bid-Data-Processing-with-Hadoop
## PART1 - Setup and WordCount
Implementation of MapReduce algorithm to produce count of every word in the files provided in gutenberg folder. The mapper splits the data in files into words and maps every word to the count of 1. We determine the frequency of every word in the 3 files in Gutenberg dataset. We represent the data in the form of key value pair. The mapper maps the keys to existing values. Every word is assigned with the value of 1 by the mapper irrespective of the number of times it occurs. The reducer aggregates the keys of common words and gives the final count of each word. The code implemented in the mapper and reducer is as follows:
### Mapper1:
• Each line is read and for each line the whitespace is removed
• Following that, the stop words and punctuations are removed
• A list of all the words is created and we iterated over list
• Each word with its count (i.e. 1) is printed in the format <word,1>
### Reducer1:
• A blank dictionary is created
• Each line is read and split (" ") space
• Check if word exists as key in dictionary,
If yes, increase count value by 1
if no, create a key with 1 as value
• Print the <key, value>, that gives the <word, total count>
<br>
## PART 2 - N-grams Implementation of MapReduce algorithm to produce modified tri-grams around the key words after replacing the key word with ‘$’ and display the 10 most occurred modified tri-gram in the Gutenberg dataset.
### Mapper2:
• A list of keywords is created, and a blank temporary list is initialized.
• The file in system is read by lines
• The data was converted to lower case
• A temporary list was added to the start of line
• The punctuations were removed
• A list containing all the words in a line was create and we iterated over this list of words in range 0 to last 3rd word of list
• We checked if ith, i+1th or i+2th element is in the list of keywords provided
If yes, then a trigram was created
Replace the word with $
• Print the trigram,1
### Reducer2:
• A blank dictionary was created, and the file was read by line and split by (" ") space
• We check the condition if the first element in split variable is present in dictionary
If yes, increase value of key by 1
If no, add first element as key with value 1
• The dictionary was sorted by value in descending order
• The first 10 key value pairs are chosen as we should display the 10 most occurred modified tri-gram
• The value of the key is printed
<br>
## PART 3 - Inverted Index
Implementation of MapReduce algorithm to produce inverted index for the whole dataset. An inverted index (also referred to as a postings file or inverted file) is a database index storing a mapping from content, such as words or numbers, to its locations in a set of documents in the Gutenberg folder.
### Mapper3:
• Firstly, we get the file path using the “os.environ” command.
• Then we get the name of the files in the path.
• Then we find different files and assign them an id.
• We get an array of all the words in each document.
• Finally, we map the words in the loop.
### Reducer3:
• A blank dictionary was created, and the file was read by line and split by (" \t ") tab.
• Then we find the posting i.e. the inverted index for each word in the document.
• We then save the postings in the dictionary and join it with the words.
• Finally, we print each word, its filename and the posting by using a loop.
<br>
## PART 4 - Relational Join Implementation of relational join on the tables join1 and join2 using MapReduce algorithm to join on the primary key is the ‘Employee ID’.
Modifications made to the dataset:
The files have been converted to csv format. The field ‘Employee ID’ is renamed to EmployeeID.
### Mapper4: • The whitespaces are removed and split to columns using ‘,’ [as csv file].
• A default value of ‘-’ is assigned so that when there is no data, ‘-’ will be seen. When we split the data in table 1, we get ‘-’ for all the columns in table 2.
• Based on the length of the incoming columns, the split may be 2,5 or 6. There is a check on this condition and the columns are assigned based on the number of splits obtained. Here we have taken care of the salary and country fields that may be have multiple splits due to the presence of ‘,’ in the data value.
• The exception of “column split = 4” due to the headers was taken care by the code with else-if statement. • The order in which the data is to be printed is explicitly mentioned.
### Reducer4: • Two dictionaries dict1 and dict2 are initialized and EmployeeeID is the key value • The whitespaces are removed and split ‘\t’. • We check the condition if a value of join2 table is ‘-’, if yes, then consider the column corresponding to key EmployeeID of join1 as the value. If no, the columns of join2 will be the value for the key EmployeeID.
• Iterate over dict1 and assign EmployeeID to the data of dict1.
Iterate over dict2 and if the key is equal to the EmployeeID, assign the columns from the value lists created above.
dict1=> {EmployeeID:Name}
dict2=> {EmployeeID:salary,country,passcode}
• Name is assigned the value of dict1
• Salary is assigned the first field from the value list; country is assigned the second and passcode is assigned the third value from the value list. • The data after joining is printed in the order required.
<br>
## PART 5: K-Nearest Neighbour
Implementation of KNN using MapReduce to return predicted labels for each test record. The files used are - train.csv and test.csv. The train data was normalized and the predictions were checked.
KNN algorithm is a supervised machine learning algorithm that be used to address both classification and regression problems. It assumes that similar things are near to each other. It works based on the concept of similarity and assigns the label based on highest vote for the chosen ‘k’ neighbors.
### Mapper5:
• Read the test.csv file using pandas
• For every line, split the data and store it as a numpy array and datatype is set as float.
• Euclidean distance between every train and test data point is determined using linalg.norm.
• The target labels are stored in a variable.
• Row index, labels and the list of is given as input to the reducer.
### Reducer5:
1. Assuming k = 15.
2. Each line is read and split ("\t ") space and saved as test point index (test_ind) and distance and label(dist_label) and then dist_label is split and stored as label and distance.
3. Check if the test point index is current index and then:
If yes, then the label is appended to the list, and corresponding distance to the distance list
If no:
i. finding the most repeated label among the lowest 15 distances (using np.argsort and then using stats.mode).
ii. Then print the test point index and its corresponding most repeated label.
iii. Then update the current_dist_list and the current test index and current label list with the next point’s values.
Note - Steps 2 and 3 are repeated for all the <key,value> pairs (i.e total test * train points)
