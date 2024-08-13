# Hybrid Search using SQLite

This is a simple python based demonstration of how to perform Hybrid (Vector & Full Text) Search using the [sqlite-vec](https://github.com/asg017/sqlite-vec) and the [full text search](https://www.sqlite.org/fts5.html) implementations.

The demonstration provided uses an in memory SQLite database, however SQLIte also allows for persistence of the database. It is also important to note that as of this time, sqlite-vec currently only supports vector search using full table scans, as oppposed to other techniques such as ANN, although other options are under developments. Regardless, the speed is incredibly impressive, even with large numbers of vectors and has the advantage of higher accuracy over ANN, etc. In addition, sqlite-vec supports pre-filtering of rows to decrease the analysis time.

Althoough this notebook only provides a Python demonstration, other languages are supported.

[Notebook Demo](https://github.com/liamca/sqlite-hybrid-search/blob/main/sqlite-hybrid-search.ipynb)

## Why Hybrid Search?

Hybrid search leverages the strengths of both vector search and keyword search. Vector search excels in identifying information that is conceptually similar to the search query, even when there are no direct keyword matches in the inverted index. On the other hand, keyword or full-text search offers precision and allows for semantic ranking, which enhances the quality of the initial results. Certain situations, such as searching for product codes, specialized jargon, dates, and people's names, may benefit more from keyword search due to its ability to find exact matches.

