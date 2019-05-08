## Methodology
The method is as follows:
* Select the queries from MSMARCO, which retrieve the maximum number of MARCO passages using Boolean AND. These selected queries will act as seeds for the next step. Each selected query leads to a different conversation sequence (different topic of conversation). For example: Randomly sample 10000 queries, and then retrieve results for these queries using a Boolean AND. Sort the queries based on the number of answer passages it returns.
* Select a query based on the results from the previous step. Lets call this Level 0 query.
* Retrieve top 100 queries based on the Level 0 query. The retrieval is done based on the answer passages. This means that the Level 0 query will be used to get a set of answer passages. Now, each answer passage is linked to a query in the dataset. This becomes the new pool of queries. Finally, the queries corresponding to these answer passages are chosen as seeds for the next step. Lets call these set of queries as Level 1 queries.
* The previous step is performed over all the queries obtained in Level 1. This gives us an overall query pool of 100,000 (maximum).

After the query pool has been formed, the next step is to form a sequence based on this pool. The sequence is formed based on a heuristic which utilises both similarity (on-topic)  and dissimilarity (to prevent duplicates) to the previously selected queries in the sequence. This is outlined as follows:
* Initliase the query sequence and add Level 0 query to it. This demarcates the starting of new query sequence.
* Find the heursitic value for each query in the pool based on the queries present in the existing query sequence.
* Add the query with the maximum heuristic value to the sequence. Repeat the previous step.

The heuristic utilizes a combination of similarity as well as dissimilarity as mentioned previously. Both are calculated using tf-idf. For a query under consideration, its similarity to all the queries already present in the query sequence is measured. The similarity values with each of these queres are added using a discount factor. The discount factor is based upon how high up a particular query (with which simlarity was to be calculated) was in the query sequence. The dissimilarity is calculated in a similar way, just that, for its calculations, it accounts for both the query and its answer passage terms.

The heuristic can be modified to suit ones needs.
