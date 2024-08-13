# Hybrid Search using SQLite

This is a simple python based demonstration of how to perform Hybrid (Vector & Full Text) Search using the [sqlite-vec](https://github.com/asg017/sqlite-vec) and the [full text search](https://www.sqlite.org/fts5.html) implementations. Although Hybrid enables search services such as [Azure AI Search](https://azure.microsoft.com/products/ai-services/ai-search) have far greater capabilities, there are times when it is not viable to host content in the cloud which is why SQLite is a perfect choice.

The demonstration provided uses an in memory SQLite database, however SQLIte also allows for persistence of the database. It is also important to note that as of this time, sqlite-vec currently only supports vector search using full table scans, as oppposed to other techniques such as ANN, although other options are under developments. Regardless, the speed is incredibly impressive, even with large numbers of vectors and has the advantage of higher accuracy over ANN, etc. In addition, sqlite-vec supports pre-filtering of rows to decrease the analysis time.

Althoough this notebook only provides a Python demonstration, other languages are supported.

[Notebook Demo](https://github.com/liamca/sqlite-hybrid-search/blob/main/sqlite-hybrid-search.ipynb)

## Why Hybrid Search?

Hybrid search leverages the strengths of both vector search and keyword search. Vector search excels in identifying information that is conceptually similar to the search query, even when there are no direct keyword matches in the inverted index. On the other hand, keyword or full-text search offers precision and allows for semantic ranking, which enhances the quality of the initial results. Certain situations, such as searching for product codes, specialized jargon, dates, and people's names, may benefit more from keyword search due to its ability to find exact matches.

## How Hybrid Search Works

Although there are numerous approaches for taking scores from Vector similarity and BM25 based Full Text search, the approach used in this demonstration uses Reciprocal Rank Fusion (RRF). 
RRF is grounded in the idea of reciprocal rank, which refers to the inverse of the rank of the first relevant document in a set of search results. The technique aims to consider the positions of items in the original rankings and assign greater importance to items that appear higher across multiple lists. This approach enhances the overall quality and reliability of the final ranking, making it more effective for combining multiple ordered search results.

Here is the code used to perform RRF:

```code
def reciprocal_rank_fusion(fts_results, vec_results, k=60):  
    rank_dict = {}  
  
    # Process FTS results  
    for rank, (id,) in enumerate(fts_results):  
        if id not in rank_dict:  
            rank_dict[id] = 0  
        rank_dict[id] += 1 / (k + rank + 1)  
  
    # Process vector results  
    for rank, (rowid, distance) in enumerate(vec_results):  
        if rowid not in rank_dict:  
            rank_dict[rowid] = 0  
        rank_dict[rowid] += 1 / (k + rank + 1)  
  
    # Sort by RRF score  
    sorted_results = sorted(rank_dict.items(), key=lambda x: x[1], reverse=True)  
    return sorted_results
```
