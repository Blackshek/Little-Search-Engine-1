# Little-Search-Engine
Implemeting a simple search engine for text documents using hash tables.

Following is the sequence of method calls that will be performed on a LittleSearchEngine object, to index and search keywords. 
LittleSearchEngine() - Already implemented.
The constructor creates new (empty) keywordsIndex and noiseWords hash tables. The keywordsIndex hash table is the MASTER hash table, which indexes all keywords from all input documents. The noiseWords hash table stores all the noise words. Both of these are fields in the LittleSearchEngine class.

Every key in the keywordsIndex hash table is a keyword. The associated value for a keyword is an array list of (document,frequency) pairs for the documents in which the keyword occurs, arranged in descending order of frequencies. A (document,frequency) pair is held in an Occurrence object. The Occurrence class is defined in the LittleSearchEngine.java file, at the top. In an Occurrence object, the document field is the name of the document, which is basically the file name, e.g. AliceCh1.txt.

void makeIndex(String docsFile, String noiseWordsFile) - Already implemented.
Indexes all the keywords in all the input documents. See the method documentation and body in the LittleSearchEngine.java file for details.

If you want to index the given sample documents, the first parameter would be the file docs.txt and the second parameter would be the noise words file, noisewords.txt

After this method finishes executing, the full index of all keywords found in all input documents will be in the keywordsIndex hash table.

The makeIndex methods calls methods loadKeyWords and mergeKeyWords, both of which you need to implement.

HashMap<String,Occurrence> loadKeyWords(String docFile) - You implement.
This method creates a hash table for all keywords in a single given document. See the method documentation for details.

This method MUST call the getKeyWord method, which you need to implement.

String getKeyWord(String word) - You implement.
Given an input word read from a document, it checks if the word is a keyword, and returns the keyword equivalent if it is.

See the method documentation for details. Here are some examples of input parameter word, and returned value. 
 
Input Parameter	Returned value
distance.	distance (strip off period)
equi-distant	null (not all alphabetic characters)
Rabbit	rabbit (convert to lowercase)
Between	null (noise word)
we're	null (not all alphabetic characters)
World...	world (strip trailing periods)
World?!	world (strip trailing ? and !)
What,ever	null (not all alphabetic characters)
Note that if there is more than one trailing punctuation (as in the "World..." and "World?!" examples above), the method strips all of them. Also, the last example makes it clear that punctuation appearing anywhere but at the end is not stripped, and the word is rejected.
void mergeKeyWords - You implement.
Merges the keywords loaded from a single document (in method loadKeyWords) into the global keywordsIndex hash table.

See the method documentation for details. This method MUST call the insertLastOccurence method, which you need to implement.

ArrayList<Integer> insertLastOccurrence(ArrayList<Occurrence> occs) - You implement.
See the method documentation for details. Note that this method uses binary search on frequency values to do the insertion. The return value is the sequence of mid points encountered during the search, using the regular (not lazy) binary search we covered in class. This return value is not used by the calling method-it is only going to be used for grading this method.

For example, suppose the list had the following frequency values (including the last one, which is to be inserted):

     --------------------
     12  8  7  5  3  2  6
     --------------------
      0  1  2  3  4  5  6
Then, the binary search (on the list excluding the last item) would encounter the following sequence of midpoint indexes:
    2  4  3
Note that if a subarray has an even number of items, then the midpoint is the last item in the first half.
After inserting 6, the input list would be updated to this:

     --------------------
     12  8  7  6  5  3  2
     --------------------
      0  1  2  3  4  5  6
and the sequence 2 4 3 would be returned.
If the new item is a duplicate of something that already exists, it doesn't matter if the new item is placed before or after the existing item.

Note that the items are in DESCENDING order, so the binary search would have to be done accordingly.

ArrayList<String> top5search(String kw1, String kw2) - You implement.
This method computes the search result for the input "kw1 OR kw2", using the keywordsIndex hash table. The result is a list of NAMES of documents (limited to the top 5) in which either of the words "kw1" or "kw2" occurs, arranged in descending order of frequencies. See the method documentation for additional details.

As an example, suppose the search is for "deep or world", in the given test documents, AliceCh1.txt (call it A) and WowCh1.txt (call it W). The word "deep" occurs twice in A and once in W, and the word "world" occurs once in A and 7 times in W:

    deep:  (A,2), (W,1)
    world: (W,7), (A,1)
The result of the search is:
    WowCh1.txt, AliceCh1.txt
in that order. (Recall that the name of a document is the same as the name of the document file.)
NOTE:

If a document occurs in both keywords' match list, consider the one with the higher frequency - do NOT add frequencies.
Return AT MOST 5 non-duplicate entries. This means if there are more than 5 non-duplicate entries, then return the five top frequency entries, but if there are fewer than 5 non-duplicate entries, then return all of them.
If a document in the first match list (for the first keyword) has the same frequency as a document in the second match list (for the second keyword), and both are candidates for inclusion in the output (they are not the same document), then pick the document in the first list before the document in the second list.
