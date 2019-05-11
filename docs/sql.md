# SQL

[Overview Source](https://www.red-gate.com/simple-talk/sql/learn-sql-server/sql-server-index-basics/)

## Indexing

Indexing is basically sorting your data on a specified column to improve searching by that column. 

Advantages:

* Improves performance on reading

Disadvantages:

* Decrease performance when writing data because of page splits
* Non-clustered indexes need extra physical memory space

### Clustered index

Clustered index is a physical sorting of the data by the chosen column. Therefore you can only have one clustered index per table. A primary key or unique contraint will by default act as a clustered index if you do not specify a clusterd index.

### Non-clustered index

Non-clustered index is an index that is sorted by the chosen column, and keeps a pointer to the row instead of the actual data. You can define multiple non-clustered indices.

## Pages

[Microsoft Docs Source](https://docs.microsoft.com/en-us/sql/relational-databases/pages-and-extents-architecture-guide?view=sql-server-2017)

![B-Tree](/img/Index_B_Tree.jpg)

`When a query is issued against an indexed column, the query engine starts at the root node and navigates down through the intermediate nodes, with each layer of the intermediate level more granular than the one above. The query engine continues down through the index nodes until it reaches the leaf node. For example, if you’re searching for the value 123 in an indexed column, the query engine would first look in the root level to determine which page to reference in the top intermediate level. In this example, the first page points the values 1-100, and the second page, the values 101-200, so the query engine would go to the second page on that level. The query engine would then determine that it must go to the third page at the next intermediate level. From there, the query engine would navigate to the leaf node for value 123. The leaf node will contain either the entire row of data or a pointer to that row, depending on whether the index is clustered or nonclustered.`

## Page Splits and Fill factor

[Microsoft Docs Source](https://docs.microsoft.com/en-us/sql/relational-databases/indexes/specify-fill-factor-for-an-index?view=sql-server-2017)

Table and index data is stored in 'pages' of 8kb. Fill-factor will define how much extra space you leave open for future expansion of the index. If the pages are filled by 100 (fillfactor 0 or 100), the creation of an extra row, or the update of an existing index with extra data, will (or might) cause the page to split. This will move 50% of the page to a new page to make room for the new data, and will cause a performance hit. Fill-factor is defined between 1-100, being the percentage to fill the page with. The remainder will be left open between the rows.

Considerations:

* Do not use a fillfactor of less then 100 on a identity column index. New data will always be generated at the end of the last page, so leaving extra space will only take more memory without preventing page-splits. This can only be usefull when you can update the existing identity column with extra data.
* A smaller fillfactor will reduce requirements for page splits, but will also decrease read performance and increase memory usage.