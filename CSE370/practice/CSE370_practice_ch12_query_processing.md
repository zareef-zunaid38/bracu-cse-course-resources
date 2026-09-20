**Question 1:** What occurs during the **Code Generation** step of query processing, and how does it relate to **Query Execution**?

**Question 2:** Why is an SQL query compiled into a relational algebra expression as the first step of query compilation? Identify the formal properties mentioned that enable query performance improvement.

**Question 3:** What role do the leaf nodes play in a standard Query Tree representation? Explain why a tree representation makes it easier for the DBMS to decide how to combine building block codes into a final executable.

**Question 4:** The query processor relies on alternative algorithms for key operations. What are the factors that determine whether an algorithm can be applied to a particular query?

**Question 5:** Explain how building block operations serve as streams or iterators within the concept of query pipelining. How do tuples move between parent and child operations?

**Question 6:** While pipelining reduces buffer requirements, it encounters a strict limitation during Join operations. Describe this limitation and explain under what circumstances materialization can force a DBMS to use disk storage.

**Question 7:** When a DBMS implements an external merge sort, it departs from the traditional recursive method taught in basic computer science. Explain how the size of partitions to be merged is chosen, and how the actual merge step differs from a standard binary merge sort.

**Question 8:** Differentiate between a **brute-force scan** and a **binary search scan** during a table scan operation. Under what exact prerequisite constraint is a Binary search scan possible?

**Question 9:** When a tree-based index evaluates range selection criteria (such as \<, \<=, \>, \>=), how do the dual mechanisms of an **Index seek** and an **Index scan** work together to retrieve data?

**Question 10:** Although multiple advanced options exist for conjunctive (AND) selections (like composite indices or intersecting record pointers), we learned that a DBMS typically implements them as a chain of selections operating on a single attribute. Why is this strategy preferred?

**Question 11:** What are the three fundamental algorithmic paths to execute a standard natural join? Briefly explain the mechanics of the approach that utilizes **hash buckets** to achieve pipelining.

**Question 12:** Describe the special case where an **Index-based join** can be implemented directly, bypassing traditional join algorithms. How are the matching records located and combined using the indexing structure?

**Question 13:** Why does the general implementation strategy for Projection purposely avoid collecting or accumulating a bunch of tuples in a buffer before filtering out attributes?

**Question 14:** Explain the **Split-Apply-Combine** pipeline used for processing aggregate operations. How does the database system optimize the "splitting" stage to prevent accumulating massive data sets in separate buckets?

### **Answers**

**Answer 1:**

* **Code Generation:** This step generates executable code that runs inside an interpreter or directly on hardware against database files. It acts as a way to "glue" or stitch existing implementations of relational algebra building blocks into a larger whole.  
* **Query Execution:** This is the terminal step where the compiled and generated code is actively run by the runtime processor to retrieve and return the results to the user application.

**Answer 2:**

* Relational algebra is a formal mathematical expression constructed in a **recursive way**.  
* Because of this formal structure, there are **formal transformation rules** that can be applied to rewrite and modify the expression to improve performance without changing the underlying meaning of the query.

**Answer 3:**

* **Leaves of the Tree:** The leaf nodes always represent the original database relations (tables).  
* **Combining Logic:** The tree format visually depicts the exact order of operations and demonstrates how intermediate results from earlier operations feed directly as input to later operations. This structural blueprint makes it straightforward to determine how building-block codes should be stitched together to create the final executable.

**Answer 4:**

Whether an algorithm can be applied to a particular query depends on:

1. The existence and nature of database indexes on query-related attributes.  
2. The overall volume and statistical distribution of records in the underlying relations.  
3. The relative fitness of that specific algorithm within the broader combination plan being mapped out by the query optimizer.

**Answer 5:**

* The building-block operations behave as streams/iterators that yield exactly **one tuple/record at a time**.  
* When a child operation returns a single tuple, it is immediately handled by its parent operation. The parent processes it and either passes it higher up the tree or discards it and requests the next single tuple from the child. This enables records to move upward one by one from leaves to root.

**Answer 6:**

* **Join Limitation:** During a join operation, pipelining is broken because all tuples from at least one branch of the join must be fully buffered to match against incoming tuples from the opposing branch.  
* **Disk Materialization:** This buffering process is called materialization. If the data volume in the matching branch is excessively high, the memory buffer overflows, forcing the DBMS to store these intermediate result files directly onto slower physical disks.

**Answer 7:**

* **Partition Size:** Rather than a simple recursive split, the size of sorted partitions is deliberately selected based on the file-system block size or RAM page size (the exact unit of data loaded during a single disk read).  
* **The Merge Step:** The implementation is not a recursive pair-by-pair breakdown. Instead of merging strictly two partitions at a time, the DBMS frequently merges **several sorted partitions simultaneously** to reduce I/O passes.

**Answer 8:**

* **Brute-forced Scan:** This is a basic linear scan that reads all database records sequentially from the beginning to the very end of the file.  
* **Binary Search Scan:** This scans the files using a binary search pattern.  
* **Constraint:** A binary search scan is **only possible** if the actual database records are physically sorted inside the storage files by the specific attribute being searched.

**Answer 9:**

* **Index Seek:** The DBMS uses the tree structure of the index to perform a quick directional search, leading directly to the very first element/leaf node that satisfies the selection criteria.  
* **Index Scan:** Because tree indexes are naturally sorted structures, the system then seamlessly switches to an index scan, sequentially reading the consecutive leaf nodes in order until the range condition becomes false.

**Answer 10:** Implementing conjunctive selections as a linear chain of single-attribute selections is preferred because it **provides significantly more opportunities for query optimization**. The optimizer can easily shuffle, reorder, or push down individual single-attribute selection blocks within the branches of the query tree.

**Answer 11:** In a hash-join pipeline, the tuples of one branch are placed into distinct hash buckets. As tuples from the opposing branch arrive in a streaming/pipelined fashion, the system calculates their hash value and directly checks the matching bucket. If a matching bucket is found, it evaluates only the tuples within that isolated bucket to produce and stream the combined row upward.

**Answer 12:**

* An index-based join occurs when the attributes being joined possess indexes in **both** participating relations.  
* Instead of running traditional join algorithms on retrieved data, the DBMS directly searches the two index structures and computes the **intersection of the indexing node values**.  
* The final combined tuples are then directly generated by looking up the raw data rows via the matching record pointers obtained from that intersection.

**Answer 13:** Collecting or accumulating tuples in intermediate buffers would create unnecessary memory overhead. To keep the operation efficient, Projection operates in an iterative fashion: for every single tuple retrieved from the lower stage of the tree, the unwanted attributes are stripped out **immediately** before the record moves forward.

**Answer 14:**

* **Split-Apply-Combine:** The tuples are split into groups based on the unique values of the grouping attribute, the aggregate function (like SUM or AVG) is applied to each group independently, and the final grouped records are combined for output.  
* **Optimization:** The system avoids accumulating data in physical buckets by processing incoming rows as a pipeline; when a new tuple arrives, it immediately updates a **running group function** (e.g., updating a running total or running counter) inside an active tracking buffer.

