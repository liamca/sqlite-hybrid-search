# Hybrid Search using SQLite

This is a simple python based demonstration of how to perform Hybrid (Vector & Full Text) Search using the [sqlite-vec](https://github.com/asg017/sqlite-vec) and the [full text search](https://www.sqlite.org/fts5.html) implementations.

## Why Hybrid Search?

Hybrid search leverages the strengths of both vector search and keyword search. Vector search excels in identifying information that is conceptually similar to the search query, even when there are no direct keyword matches in the inverted index. On the other hand, keyword or full-text search offers precision and allows for semantic ranking, which enhances the quality of the initial results. Certain situations, such as searching for product codes, specialized jargon, dates, and people's names, may benefit more from keyword search due to its ability to find exact matches.
