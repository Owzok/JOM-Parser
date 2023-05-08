<p align="center"><img src="https://imgur.com/QKXXaZ6.png" width=400></p>

# 💻 JOM Parser  ![a](https://img.shields.io/badge/C%2B%2B-20-blue) ![a](https://img.shields.io/badge/repo%20size-6mb-orange)

> Project 1 with the theme of "File Organization" for Database II (CS2042) - Utec.

## Goal
The goal of our project is to implement and compare two file organization structures for efficient data retrieval and manipulation. Specifically, we will compare the performance of the following structures:  
- [AVL File](https://en.wikipedia.org/wiki/AVL_tree)
- [Extendible-Hashing](https://en.wikipedia.org/wiki/Extendible_hashing)

To evaluate the structures we will implement the following functions:
- **Search**: given a key, we will return the respective records.
- **Range search**: Given a range of keys, return all records whose keys are within the range.
- **Add**: add a new record to the file.
- **Remove**: remove a record from the file.

## 📊 Data
We will use two Kaggle datasets to simulate our SQL parser: [NBA](https://www.kaggle.com/datasets/ethanchen44/nba-playoff-predictions) and [Tornados](https://www.kaggle.com/datasets/danbraswell/us-tornado-dataset-1950-2021). These datasets contain NBA playoff predictions and US tornado data, respectively, we will use a different file organization structure for each dataset and make one file containing the indices for each record for easy access. Both datasets were extracted from Kaggle and contain around 3,000 and 70,000 records, respectively.

## 📝 Expected Results
After performing a comparison of AVL and Extensible-Hashing performance based on the functions implemented using the two datasets, we will measure the following metrics:

- ⏳ Execution time: Time required to execute each function in each structure for each dataset.
- 🛰 Memory usage: Amount of memory required to save each file in each structure.

It is important to highlight that in advance we have knowledge about the strengths and weaknesses of each structure. AVL trees allow searches, range searches, and fast inserts, but their memory usage is high. On the other hand, Extendable-Hashing uses less memory and supports large datasets efficiently, however, it does not support range lookups.

## Técnicas de indexación
### AVL Tree
Unlike Sequential File and ISAM, AVL offers greater efficiency in searching and manipulating individual records, as it is entirely dynamic. This makes it more suitable for applications where frequent and efficient record lookups are needed. For applications that require searches by range of values in the index key it is particularly useful.  
Still, we acknowledge that ISAM can be more efficient in terms of memory usage since it doesn't need to store the entire tree structure like the AVL File does.

### Extensible Hashing
Both Extensible Hashing and the B+ Tree are efficient file organization structures that are widely used today.

In terms of inserting new records, both structures are very efficient, but Extensible Hashing has an advantage in this regard as it is easier to balance and manage. In the B+ Tree, if an insert is made that breaks the tree balancing rule, it may require an expensive and complex reorganization. In other types of extensible hashing, the B+ Tree is more efficient in deleting records, since it deletes the records directly from the leaves of the tree, while in the Extensible Hashing the free cells have to be rearranged. In this case it is not like that, we have done it in a way so that the eliminations are more efficient.

The B+ Tree is more suitable for working with large and sparse data sets, while the Extensible Hashing is more suitable for small and dense data sets. In addition, the B+ Tree is more flexible and can handle a wide range of lookups, such as range lookup and multi-key lookup, while Extensible Hashing can only handle exact key lookups.

Above all, extensible hashing allows us to index for equality, unlike the AVL we are already doing.

To be honest, in the end we chose the hash so as not to code the B+ 😌👍, honesty above all.

## Parser Documentation

📝 Insert all the records from a csv to a table.
```INSERT INTO table FROM file.csv```

🔎 Search records with the key within a range.
```SELECT * FROM table BETWEEN start end```

🔍 Search records with a specific key.
```SELECT * FROM table EQUALS value```

🖍 Delete records that have a specific key.
```DELETE FROM table value```


## Setting
In the "BDII-MINIMAL-PROJECT/datos" folder, add a csv from which you want to extract the data. Then just run the file:
- ./main.exe : Windows
- ./a.out : Mac OS

And the file will be executed, then all that remains is to perform the queries, to copy the database in a structure it is simply to do INSERT INTO table FROM file.csv.

> Before doing that, you must create a **struct** with the records and their fields to later in the ```main.cpp``` replace the ```RegistroNBA``` that comes by default with the created record . To change the **fill factor** of the extensible hashing simply change the const int found in the hash.h and then recompile.

Example: ```g++ -std=c++20 main.cpp```

## Results
<p align="center"><img src="https://imgur.com/TXwXGPF.png" width=400></p>
<p align="center"><img src="https://imgur.com/Kdd2A9g.png" width=400></p>
<p align="center"><img src="https://imgur.com/A8fAH1H.png" width=400></p>
<p align="center"><img src="https://imgur.com/ssntMRr.png" width=400></p>

We see that for the insertions, both the AVL and the Hash take more time. However, the Hash takes longer. We wondered why that would be, since the theoretical complexities said that the insert hash is O(1), and then we determined that the number of buckets online would be about 50 with the largest data count. In addition, the hash implementation performs a search (that is, it performs two "passes" over the file: the search and the insertion itself) to determine uniqueness, unlike the AVL, which simply inserts the value given that it is a multimap. We can see however that hashing is more efficient with less data, when your buckets are not yet full. On the AVl we see that its time grows considerably slower, but it still shows a curved shape suggesting that its behavior for some reason is not completely logarimic as is known from theory (it is probably due to the insertions in the linked lists that make up their nodes).

The AVL as expected is much faster in all range searches, and despite the few data points it seems to show a logarithmic complexity, although with how fast it is it could also be the run-to-run variation.

We cannot conclude much about the search times, since both are relatively short and are subject to sample-to-sample variance. However we can see a tendency for AVL to perform worse when there is less data compared to hash. Again, this must be because of the linkages if it is not the aforementioned sample-to-sample variance.

It is evident, however, that to get all the benefits of AVL, the price is paid in memory: the graph indicates that although both grow in size linearly, the extra size of each AVL node accumulates quickly.
