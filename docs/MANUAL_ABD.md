# Analyzing Big Data: A Study Manual

*Version 1: aligned with the NOVA IMS "Analysing Big Data — A practical approach" course (2026).*

This manual goes from why Big Data exists to building a Data Lakehouse with Apache Spark, then to streaming and graph analytics; each chapter builds on the previous ones. It draws only on the materials of sessions 1–9: the nine slide decks (#01–#09) and the class notebooks (ABD02–ABD09, plus the two Spark SQL notebooks of session 6). Sessions 11–14 (ML with Spark) have no materials yet and are not covered.

---

## How to use this manual

- **Read in order the first time**: later chapters rely on earlier ones (e.g. Chapter 7's shuffle explains why Chapter 10's joins are expensive).
- **Each chapter:** why it matters, concepts, a lab or worked example, pros and cons, key takeaways. Code is short and only illustrates a concept.
- **Bold sentences are the ones to retain**; skim the bold text and the cards.
- **Sources** close each section in an italic line: "#04 s28" = deck #04, slide 28; "ABD04 nb c42" = notebook cell 42. Slide numbers are the PDF page index, which may differ from the printed footer.
- *Notebook extracts have no outputs;* results are quoted only where slides or notebook text show them.
- **Resolved contradictions:** a `[!flag]` card keeps what the class says and adds a **Resolution (external source)**: the correct answer per official docs or papers (linked), and what to answer in an exam.

### Course facts

- **Lecturer: Rui Rosa** (rrosa@novaims.unl.pt), NOVA IMS. Contact by e-mail or Moodle; syllabus on Moodle. "Collaboration is valued!" The break moves to the end of class unless requested otherwise.
- **Audience:** data engineers, analysts and scientists starting with Spark. **Objectives:**
  1. explain the core concepts of Big Data processing and technologies vs traditional ones;
  2. give a solid understanding of Spark's core capabilities;
  3. explore the main Spark libraries: Spark SQL, Structured Streaming, graph analysis, Spark ML.
- **Programme (14 sessions):** 1 Course Introduction · 2 Databricks Free Edition · 3 Distributed Processing · 4 Spark Basics · 5 Spark SQL · 6 Spark SQL Programming · 7 Data Pipelines and the Data Lakehouse · 8 Spark Streaming · 9 Graph Analysis · 10 Group Project Presentations · 11 Introduction to ML with Spark · 12 Data preparation for ML with Spark · 13 Spark ML Programming · 14 Spark ML Examples.

| Session | Chapters in this manual |
|---|---|
| #01 Course Intro and Big Data Processing | 1, 2, 14, 15 |
| #02 Databricks Free Edition (+ ABD02 notebook) | 4, 5 |
| #03 Distributed Processing (+ ABD03 notebook) | 2, 3, 15, 21 |
| #04 Spark Basics (+ ABD04 notebook) | 6, 7 |
| #05 Spark SQL (+ ABD05 notebook) | 8, 9, 10 |
| #06 Spark SQL Programming (+ SQL1 and SQL2 notebooks) | 11, 12, 13, 20.9 |
| #07 Data Pipelines and the Data Lakehouse (+ ABD07 notebook) | 14, 16 |
| #08 Spark Streaming (+ ABD08 notebook) | 17, 18 |
| #09 Graph Analysis (+ ABD09 notebook) | 19, 20 |

- **Prerequisites:** Python and SQL, Jupyter (or similar). "Basic familiarity with the Linux command line is helpful", as is basic data manipulation and modeling.
- **Software:** **Databricks Free Edition is the main platform** (Chapter 4), **Google Colab the alternative**; streaming (session 8) and GraphFrames (session 9, not in the Free Edition) run in Colab.
- **Assessment:** **group project 20%** (presented 17 and 19 November, class 10) and **final exam 80%**.
- **Project rules (deck #01):** groups of 4–5 (max 8 groups); free choice of topic and public data source(s), simulating a real use case; combine structured and/or unstructured and/or streaming data, using the class concepts; Spark on Databricks Free Edition or Colab; **a working Databricks notebook in HTML is mandatory**, a one-page presentation document optional; submission by 16 November (24:00); 10-minute (max) presentation in class 10.
- **Project instructions (deck #07):**
  - *Objective:* "Create a structured, incremental data analysis process that replicates a real-world situation".
  - *Requirements:* a small Data Lakehouse with an analysis theme and a data source (minimum); Spark on Databricks Free Edition or Colab; an HTML notebook with a header naming the group and its members.
  - *End-to-end process:* read data from a source (ideally more than once) → a layer with the read history, ready for reprocessing → normalize → store normalized, detailed data in a layer modeled for the theme → prepare for optimized analysis → a layer optimized for analysis → analyses: query, dashboard, ML model. Chapter 21 maps each step to chapters.
  - *Conclusion:* about the data, the process and the technology.
- **Project grading grid (20 points = 20% of the course):**

| Points | Criterion |
|---|---|
| 3 | Problem definition and motivation |
| 4 | Big Data technologies use |
| 4 | Notebook overall presentation and structure |
| 3 | Programming standards, style and readability |
| 3 | Execution and output clarity |
| 3 | Conclusion |

- **Bibliography:** *Learning Spark*, 2nd ed. (Damji, Wenig, Das, Lee, 2020) and *Spark: The Definitive Guide* (Chambers, Zaharia, 2018). Readings per session:

| Session | Readings |
|---|---|
| #03 | *Learning Spark* ch. 4, 9; *Definitive Guide* ch. 9; *Delta Lake: The Definitive Guide* ch. 1 |
| #04 | *Definitive Guide* ch. 1, 2, 9; *Learning Spark* ch. 1–2; video "A tale of the three Apache Spark APIs: RDDs, DataFrames & Datasets" |
| #05 | *Definitive Guide* ch. 9–10; *Learning Spark* ch. 4–5, plus end of ch. 3 (Catalyst) |
| #06 | *Definitive Guide* ch. 10; *Learning Spark* ch. 5–6; the *Learning Spark* notebooks on Moodle |
| #07 | *Big Book of Data Warehousing and BI* (Databricks) §3.1; Inmon, *Building the Data Lakehouse*, ch. 1, 6; *Delta Lake: The Definitive Guide* ch. 5; *Learning Spark* ch. 9 |
| #08 | Feick et al. (2018), Fragkoulis et al. (2024), Akidau et al. (2015, Dataflow model), Databricks blog posts |
| #09 | Needham and Hodler, *Graph Algorithms* |

  Every session also asks you to run its class notebook from Moodle.
- **Moodle files for the labs:** purplecow.txt, alice_in_wonderland.txt, Team_Teams.csv, Team_Managers.csv, movielensABD.csv, ABD_Data_24.csv, salesABD_2023.csv, salesABD_2024.csv, SalesABD.csv; notebooks ABD02–ABD09. Recommended workspace folder "ABD26"; schema and volume both "abd" (`/Volumes/workspace/abd/abd`), "just because the exercises notebook use this objects".
- **Not examined.** Slides with the orange **"Out of the program"** badge are kept with a `[!notexam]` card: Databricks widgets (#02 s27) → §5.7; MapReduce phases in detail (#03 s21) → §2.9; initializing a SparkSession yourself (#04 s17) → §6.9; Datasets (#04 s20) → §7.1; creating RDDs with the low-level API (#04 s22–23) → §7.2; DataFrame ↔ RDD conversion (#05 s20–21) → §9.3.

> [!flag] **Presentation length.** Deck #01: "10m (max)". A later lecturer e-mail (group's project notes, external check) says 15 minutes. Check Moodle.

> [!flag] **Submission deadline.** Deck #01: "November 16th (24:00)"; the later e-mail (external check) says 16 November, 23:59. One minute apart.

> [!flag] **Exam dates not fixed.** Deck #01: "TBD ?th January 18:30" and "TBD ?th February 20:30".

> [!flag] **ML in the project, but no ML classes before it — resolved.** Deck #07 asks for an "ML model", but the ML sessions (11–14) come after the presentations (session 10) and sessions 1–9 have no Spark ML.
>
> **Resolution (external source).** An administrative gap. Use `pyspark.ml` as a `Pipeline` (`VectorAssembler` → `LinearRegression` / `RandomForestClassifier` → evaluators); split time-ordered data by date, never `randomSplit`; report a naive baseline. [Spark MLlib guide](https://spark.apache.org/docs/latest/ml-guide.html); [Spark ML Pipelines](https://spark.apache.org/docs/latest/ml-pipeline.html); [scikit-learn: time-series CV](https://scikit-learn.org/stable/modules/cross_validation.html).

> [!flag] **Data requirements stated two ways.** Deck #01: "combine structured and/or unstructured and/or streaming data"; deck #07: "a data source (minimum)" in lakehouse layers.

*Sources: #01 s1, 3–5, 9–14, 41; #02 s10–14, 27, 48; #03 s21, 48–49; #04 s17, 20, 22–23, 52; #05 s20–21, 64; #06 s58; #07 s15, 23–24; #08 s34, 36–39; #09 s37, 54, 56.*

### Legend: the cards used in this manual

| Card | Meaning |
|---|---|
| `[!info]` Info | a core concept or useful explanation |
| `[!warning]` Warning | something easy to get wrong |
| `[!trap]` Watch out | a classic mistake or trap |
| `[!key]` Remember | a compact fact to memorize |
| `[!compare]` A vs B | two class sources that disagree or complement each other |
| `[!notexam]` Out of the program | marked "Out of the program" in class: kept, not evaluated |
| `[!new]` From a specific source | content from one class source, named in the card |
| `[!lab]` Lab | a worked example from a class notebook (named in the card) |
| `[!flag]` Inconsistency | an inconsistency between or within class materials; "(external check)" when the fix needs outside knowledge. Flags titled "— resolved" carry a **Resolution (external source)** from official docs or papers |

### Table of contents

**Part I: Big Data Foundations**
1. The Big Data Challenge
2. Distributed Processing: From Hadoop to Spark
3. Data Formats

**Part II: The Working Environment**
4. Databricks and the Free Edition
5. Notebooks and File Systems

**Part III: Apache Spark Core**
6. Apache Spark: What It Is and How It Runs
7. Data Abstractions, Operations and Lazy Execution

**Part IV: Spark SQL**
8. Spark SQL: Engine, Catalog and Tables
9. DataFrames: Creating, Reading and Writing
10. DataFrame Operations
11. Built-in Functions and UDFs
12. Query Optimization
13. Big Data Algorithms with Spark SQL

**Part V: The Data Lakehouse**
14. Data Architectures: Warehouse, Lake and Lakehouse
15. Delta Lake
16. The Medallion Architecture

**Part VI: Streaming and Graph Analytics**
17. Streaming Data and Streaming Architectures
18. Spark Structured Streaming
19. Graph Concepts
20. Graph Analytics with GraphFrames

**Part VII: Wrap-up**
21. Putting It All Together

**Appendices**
- A. Cheat sheet
- B. Glossary
- C. Inconsistencies and open points

---

# Part I: Big Data Foundations

Why Big Data needed new technology, how distributed processing evolved from Hadoop to Spark, and how data is stored for many machines.

---

# Chapter 1: The Big Data Challenge

Data growth ("market pull") met hardware trends ("technology push"); the answer was many cheap machines instead of one bigger one.

## 1.1 Data processing before Big Data

Around 2000:

- data was stored in an **RDBMS**, good for transactional systems;
- processing was **centralized**: data moved to the processing unit;
- processing was **sequential**;
- growing requirements (CPU, memory, space) meant **growing vertically**: a bigger machine.

Still, very large warehouses existed:

| Source | Record |
|---|---|
| 2000, *Enterprise Systems Journal* | "IBM, EMC, and Oracle Build 82-Terabyte Warehouse" |
| SAP, Guinness World Records | "World's Largest Data Warehouse record – 12.1 PB" (12,100 TB raw, "four times larger than the prior record"), SAP HANA + SAP IQ with HP, Intel, NetApp, Red Hat |

**Data travelled to a central machine, was processed sequentially, and the system grew vertically.** Warehouse architecture: §14.2.

*Sources: #01 s17–18.*

## 1.2 The 3Vs: volume, velocity, variety

The class illustrates "the Big Data challenge" with a META Group note ("3D Data Management", 6 February 2001): data grows along three axes of a cube, **volume, velocity and variety**, each with management solutions:

| Dimension | Solutions (META Group, 2001) |
|---|---|
| **Volume** | tiered storage / hub and spoke; selective retention; statistical sampling; redundancy elimination; offloading "cold" data; outsourcing |
| **Velocity** | operational data stores; data caches; point-to-point routing; balancing data latency with decision cycles |
| **Variety** | inconsistency resolution; XML-based "universal" translation; application-aware EAI adapters; data access middleware and ETLM; distributed query management; metadata management |

Conclusion: "extending data management options enables greater returns on information assets". Summary: **"Data growth (specially after year 2k) started to make an impact on the society being referred as Big Data (volume, variety and velocity)."**

> [!warning] **Partly readable source.** Part of the META Group text is covered by the figure; the table uses only the visible solutions.

*Sources: #01 s19, 43.*

## 1.3 Market pull: data growth and value

The **market pull**: data was growing and worth a lot. Illustrated by *The Economist* (Feb 2010, cover "The data deluge") and McKinsey (May 2011, "Big data: The next frontier for innovation, competition, and productivity"):

| Area | Potential value (McKinsey, 2011) |
|---|---|
| US health care | $300 billion annual value |
| Europe's public sector administration | €250 billion annual value |
| Personal location data, globally | $600 billion annual consumer surplus |
| Retailers | up to 60% increase in operating margins |
| Talent | 140,000–190,000 more deep analytical positions; 1.5 million more data-savvy managers needed in the US |

**Big Data added business value and stressed the existing "monolithic, SMP, relational" technologies.**

*Sources: #01 s20, 43.*

## 1.4 Technology push: parallel and in-memory processing

Two charts, each with a one-line label:

| Chart | What it shows | Label |
|---|---|---|
| "Intel CPU Trends" (1970–2010) | transistors keep growing exponentially; clock speed, power and performance per clock **flatten around 2005** | **Parallel processing** |
| "Historical Cost of Computer Memory and Storage" (1955–2017) | price per MB falls by orders of magnitude | **In-memory processing** |

**CPUs stopped getting faster ~2005, so speed must come from parallelism; cheap memory lets data stay in memory instead of on disk.** Both return in Spark (§2.11, Chapter 6).

> [!warning] **Charts only.** The slide has no text; this reading comes from the charts and their labels.

*Sources: #01 s21.*

## 1.5 Scale up (SMP) vs scale out (MPP)

There are **"two distinct data processing models"**:

- **Scale up = SMP** (symmetric multiprocessing): "adding bigger, more powerful machines". "There is a limit": you "cannot scale up beyond a point"; cost and time rise steeply from terabytes to petabytes.
- **Scale out = MPP** (massive parallel processing): "adding more, smaller and cheaper machines" ("commodity data servers"); cost and time grow much more slowly with data size.

"A decision to either scale up or scale out should be made":

| | Scale up | Scale out |
|---|---|---|
| Cost | Expensive | Cheap |
| Hardware | Specialized | Commodity |
| Fault tolerance | Low | High |
| Licensing | Proprietary | Open source and proprietary options |
| Storage | Terabytes | Petabytes and more |

### How SMP and MPP differ inside

- **SMP: "Processors share resources."** CPUs, each with its own cache, on a shared bus, using shared memory and I/O.
- **MPP: "A single task is processed in parallel."** Nodes linked by a high-speed interconnect, each with its own storage; data partitioned across them (e.g. keys A…F on node 1, G…R on node 2, S…Z on node 3).

#03 defines MPP as **"the coordinated processing of a single, large computational task concurrently across many independent processors or nodes"**, with four characteristics:

| Characteristic | Meaning |
|---|---|
| **Shared-nothing** (strictly) | each node has its own OS, memory and disk |
| **Horizontal data partitioning** (sharding) | data sharded across all nodes |
| **Simultaneous execution** (parallel) | a master breaks one query into fragments, sends them to workers, aggregates results in parallel |
| **Linear scalability** (scale-out) | adding nodes scales capacity linearly |

> [!key] **Scale up = SMP: shared resources, hits a ceiling. Scale out = MPP: shared-nothing, data and query split across nodes, linear scalability.**

*Sources: #01 s26–28; #03 s6.*

## 1.6 Big Data technologies vs traditional DBMS

| Traditional DBMS systems | Big Data technologies |
|---|---|
| Rigid data models | Schema-free data models |
| Weak fault-tolerance architecture | Fault-tolerance architecture |
| Scalability constraints | Highly scalable |
| Expensive to scale | Lower costs |
| Limitation on unstructured data | Handles unstructured and streaming data |
| Proprietary hardware and software | Commodity hardware and open-source software |

*Sources: #01 s25.*

## 1.7 Big Data today: from hype to infrastructure

Recent literature reviews (2024–2026), as summarized on the slide:

| Conclusion | Meaning |
|---|---|
| "Cloud-native distributed processing architectures dominate" | Kubernetes, serverless, distributed storage/processing came from Big Data scalability needs |
| "Real-time analytics is standard" | streaming frameworks (Kafka, Flink, Spark Streaming) changed how IT handles operational data |
| "Big Data is now inseparable from AI" | data pipelines feed ML and generative models |
| "Data governance became critical" | privacy, security and ethics are central IT responsibilities |
| "Big Data is no longer a trend — it is infrastructure" | the hype faded; the technologies became foundational |

Session summary, additionally: MPP and new processing models came from non-traditional vendors; Hadoop handled large structured and unstructured volumes more efficiently; **Hadoop and Spark became the open-source standard Big Data technology** and moved to the enterprise, creating hybrid models, the **Data Lake** and the **Data Lakehouse** (Chapter 14). **"Big Data shifted from a hyped trend to a foundational layer of modern IT."**

*Sources: #01 s36–37, 43.*

### Key takeaways

- Before Big Data: RDBMS, centralized, sequential, vertical growth. Then the 3Vs (META Group, 2001).
- **Market pull** (data growth and value) + **technology push** (parallelism as CPUs plateaued ~2005, in-memory as memory got cheap).
- SMP shares resources and hits a limit; MPP is shared-nothing, sharded, parallel, linearly scalable on commodity hardware.
- Big Data is "no longer a trend — it is infrastructure" (cloud, real-time, AI, governance).

---

# Chapter 2: Distributed Processing: From Hadoop to Spark

The cluster and its layers, running work where the data is, and why MapReduce gave way to Spark.

## 2.1 Hadoop: the starting point

Hadoop, "the starting point for Big Data", grew out of two Google papers:

| Year | Paper | Idea |
|---|---|---|
| 2003 | "The Google File System" (Ghemawat, Gobioff, Leung) | a master keeps the file namespace; chunkservers store data on their Linux file system |
| 2004 | "MapReduce: Simplified Data Processing on Large Clusters" (Dean, Ghemawat) | master and workers; input splits → map → intermediate files on local disks → reduce → output files |

The #04 timeline (MapReduce at Google 2002 → Spark a top-level Apache project 2014) is in §6.1. Hadoop's traits (#03): **framework; distributed storage and computing; commodity hardware and software; open source; scalability; fault tolerance; low cost.**

### The Hadoop cluster and its ecosystem

A cluster of commodity servers with a **Name Node** and several **Data Nodes**, under this stack:

| Layer | Component |
|---|---|
| Distributed storage | **HDFS** |
| Distributed processing | **MapReduce** |
| Metadata services | HCatalog |
| Script | Pig |
| Query | Hive |
| Non-relational database | HBase |
| Other projects | Ambari, Avro, Cassandra, Oozie, Zookeeper, etc. |

Architecture in two phrases: **"distributed processing" and "decoupling of storage and processing".**

### Big Data processing technologies (2010–2015)

Decoupling of storage and processing (§2.3, §3.1); distributed file system (§2.5); data formats (Chapter 3); distributed processing (§2.4–§2.8); in-memory processing (§2.11).

*Sources: #01 s22–24; #03 s3; #04 s3.*

## 2.2 The processing environment: cluster, node, daemon

| Term | Class definition | Detail |
|---|---|---|
| **Cluster** | "group of computers working together" | "Provides data storage, data processing and resource management" |
| **Node** | "an individual computer in the cluster" | master nodes distribute work and data to worker nodes |
| **Daemon** | "a program running on a node" | "Each daemon (runs on a JVM) performs a specific function in the cluster" |

**The master is labelled "Driver node in Spark"** (Chapter 6). ABD04: **"A Spark Cluster consists of a Driver process and multiple Executors (which are individual JVM processes) running across one or more Worker Nodes."**

> [!info] **"Slave" = "worker".** #03 s4 says "worker nodes", s11 says the NodeManager "runs on slave nodes": same role.

*Sources: #03 s4, 51–52; ABD04 nb c2.*

## 2.3 Key concepts of distributed processing

| Concept | What the class says |
|---|---|
| **Decoupling of storage and computing** | "Moving computation is cheaper than moving data" (actually data locality; see the flag) |
| **Parallel processing** | "Workload are processed concurrently across nodes simultaneously" |
| **Distributed storage** | "Working with files larger than an individual processing unit" |
| **Native support for different file formats** (text, Avro, Parquet, etc.) | "Portability across heterogeneous HW and SW platforms" |
| **Native support for different processing engines** | MapReduce, SQL, Spark, etc. |
| **Scalability and elasticity** | "Native support for large data sets and unstructured data" |
| **Fault tolerance** | "Designed for failure" |
| **Optimized for batch processing and analytics** | — |

> [!flag] **Two meanings for "decoupling" — resolved.** #03 s5 explains "Decoupling of Storage and Computing" with "moving computation is cheaper than moving data", which #04 s27 uses for **data locality** (§2.7); elsewhere (#03 s27; #01 s23) decoupling means separate storage and processing layers (§3.1).
>
> **Resolution (external source).** The phrase is the HDFS/MapReduce data-locality principle (code sent to the node holding the block); decoupling is the cloud/lakehouse model (open-format data on object storage, compute clusters scaling independently, as in Databricks). In an exam: the slide pairs them, but the phrase defines data locality. [HDFS Architecture Guide](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html); [Databricks: What is a data lakehouse?](https://docs.databricks.com/aws/en/lakehouse/).

*Sources: #03 s5, 51.*

## 2.4 The three layers of a Big Data architecture

**A Big Data architecture has three components**, drawn as a triangle with two-way arrows (each layer talks to the other two):

| Layer | Role | Examples |
|---|---|---|
| **Processing engine** | runs the computation | Spark, Hive, MapReduce |
| **Resource manager** | allocates cluster resources, schedules jobs | YARN, Mesos |
| **Storage layer** | stores data distributed across the cluster | HDFS, DBFS, Cloud FS |

*Sources: #03 s8, 51.*

## 2.5 The distributed storage layer

"Responsible for the distributed storage of data", using a **distributed file system** (e.g. Google GFS, Cloud Storage). It:

- **sits on top of a native file system** (FAT, NTFS, ext3, ext4, xfs);
- provides **redundant storage** "for massive amounts of data";
- **performs best with large files**;
- has **"write once"** files (no updates);
- is **optimized for reading large streams of files**, not random files.

The diagram: DBFS on the native OS file system on disk storage. **"The Distributed Storage Layer: Chunks data into blocks and distributes them across the cluster when data is stored"** (blocks in action: §2.7).

> [!flag] **DBFS status.** #03 lists DBFS as a storage example (s8–9), while #02 calls DBFS root/mounts "a deprecated pattern" yet its notebook still points to `/dbfs` datasets (marked "Deprecated"). See §4.6, §5.4.

*Sources: #03 s8–9, 52; #02 s30; ABD02 nb c18.*

## 2.6 Resource management with YARN

**YARN ("Yet Another Resource Negotiator") is the Hadoop resource manager, responsible for allocating system resources.** Two functions: **resource manager** and **job scheduler**. Key benefit: it **"allows multiple data processing engines to run on a single cluster"**:

| Workload | Engines |
|---|---|
| Batch processing | Spark, MapReduce |
| Interactive SQL | Hive, Impala |
| Advanced analytics | Spark ML, Mahout |
| Streaming | Storm, Spark Streaming |

Stack: applications (MapReduce, Spark, Tez, …) on compute (YARN) on storage (HDFS, HBase), i.e. §2.4 in Hadoop terms.

### The YARN daemons

| Daemon | Runs on | Does |
|---|---|---|
| **ResourceManager** | a master node | global resource scheduler; arbitrates resources between competing applications |
| **NodeManager** | each worker node | manages its node's resources; talks to the ResourceManager |

**"The Resource Manager schedules jobs and manages resources"**; **"YARN works with the File System to run tasks where the data is stored."**

*Sources: #03 s10–11, 52.*

## 2.7 Running an application on a cluster, and data locality

> [!lab] **The YARN walk-through (#03 s12–19).** Cast: Client, ResourceManager, four NodeManagers, Name Node, file `MyData`.
> 1. `MyData` is split into Block 1 (Node 1) and Block 2 (Node 2); the Name Node knows where.
> 2. Client runs `$ Spark_App MyData`; the ResourceManager launches an **ApplicationMaster** on node 3.
> 3. It asks the Name Node "Where is Mydata?" → "Node 1, Node 2", and requests "1× Node 1", "1× Node 2".
> 4. "Here are your containers": **Executors 1 and 2** start on Nodes 1 and 2; **Tasks 1 and 2** run there, next to the blocks.
> 5. The ApplicationMaster tells the ResourceManager "I'm done!"

**Tasks go to the nodes storing the data: data locality**, #04's **"moving computation (mapper code) close to data rather than moving data towards the computation"**, since "it is not feasible to move large data sets towards computation". Each map job goes to the Data Node holding (or closest to) its block. **Data locality "helps in high throughput and faster execution".**

*Sources: #03 s12–19, 52; #04 s27.*

## 2.8 MapReduce: the programming model

**"MapReduce is a programming model for processing data in a distributed way on many machines (nodes)"**, a **"'batch query processor'"**. In one sentence: **"Map the original data into a collection of key-value pairs and then, Reduce the output with the same key."**

| Step | What it does |
|---|---|
| **Map** (then shuffle & sort) | turns input into key-value pairs; shuffle & sort groups them by key |
| **Reduce** | aggregates the values sharing a key |

```flow
input → map (key-value pairs) → shuffle & sort (group by key) → reduce (aggregate per key) → output
```

- **"Each node processes data stored on that node (when possible)"** (data locality, §2.7).
- "Although the Hadoop framework is implemented in Java, **MapReduce applications need not be written in Java**."

Dean and Ghemawat (2004): "Users specify a **map function** that processes a key/value pair to generate a set of intermediate key/value pairs, and a **reduce function** that merges all intermediate values associated with the same intermediate key."

The class shows a Java **WordCount**: map (split a line, emit `(word, 1)`), reduce (sum per word, emit `(word, sum)`), `main()` running the job. Compared with its Spark versions in §13.3.

*Sources: #03 s20, 22–23; #01 s22; #04 s3.*

## 2.9 MapReduce phases in detail

> [!notexam] **Out of the program** (#03 s21). For understanding only.

- records from the source enter the map function as key-value pairs;
- **map()** "produces one or more intermediate values along with an output key from the input";
- **shuffle & sort**: "all the intermediate values for a given output key are combined together into a sorted list and redistributed across partitions";
- **reduce()** "combines those intermediate values into one or more final values for that same output key".

```flow
text files → splits → map phase → shuffle phase → reduce tasks → output (final key, values)
```

*Sources: #03 s21.*

## 2.10 Why MapReduce was not enough

| Drawback | Detail |
|---|---|
| Complex code maintenance | originally developed in Java |
| No shell | |
| **Disk based** | "data stored (/processing) on disk" |
| Slow data access | many disk reads |
| **No memory operations** | no caching |
| No abstract concepts | for data manipulation and processing |
| **Not optimized for iterative algorithms** | |
| **Limited to batch processing** | |
| **No additional libraries** | ML, Graph, SQL, Streaming, etc. |

The Spark paper (#04): MapReduce's "acyclic data flow model" does not suit apps that "reuse a working set of data across multiple parallel operations" (**iterative ML**, **interactive analysis**); every job or query rereads from disk, with tens of seconds of latency.

**MapReduce keeps everything on disk and runs one batch job at a time, so iterative or interactive work is slow.**

*Sources: #03 s24; #04 s4.*

## 2.11 Apache Spark: next-generation MapReduce

Spark is **"next generation MapReduce"**:

| Hadoop MapReduce | Apache Spark |
|---|---|
| Fast | 100x faster than MapReduce |
| Batch processing | Real-time processing |
| Stores data on disk | **Stores data in memory** |
| Written in Java | Written in Scala |

Strengths (answering §2.10): **speed** (in-memory), **fault tolerance**, **data objects and powerful caching**, **polyglot APIs**, coding and deployment, **libraries** (real-time, graphs, ML), **scalability**. The 2010 paper ("Spark: Cluster Computing with Working Sets", UC Berkeley) introduced **RDDs**: "a read-only collection of objects partitioned across a set of machines that can be rebuilt if a partition is lost" (Chapter 7; architecture in Chapter 6).

> [!flag] **How much faster is Spark? — resolved.** #03 s25: "100x faster than MapReduce", unsourced; the Spark paper on #04 s4: "can outperform Hadoop by **10x** in iterative…" (cut off).
>
> **Resolution (external source).** "100x" is the old home-page claim ("up to 100x faster in memory, or 10x faster on disk"); the 2010 paper measured 10x on iterative ML jobs (the 2012 RDD paper: up to 20x). The gain comes from keeping the working set in memory across iterations, so it is small for single-pass batch. In an exam, state the conditions, never "100x" as a general fact. [Zaharia et al., HotCloud 2010](https://www.usenix.org/legacy/event/hotcloud10/tech/full_papers/Zaharia.pdf); [archived spark.apache.org](https://web.archive.org/web/20160101150833/http://spark.apache.org/).

*Sources: #03 s25; #04 s4.*

### Key takeaways

- Hadoop (from GFS 2003 and MapReduce 2004): HDFS + MapReduce on commodity hardware.
- Three layers: processing engine (Spark, Hive, MapReduce), resource manager (YARN, Mesos), storage (HDFS, DBFS, cloud FS).
- Storage: blocks, write-once, large streams. YARN (ResourceManager + NodeManagers) lets engines share a cluster and **runs tasks where the data is (data locality)**.
- MapReduce: map → shuffle & sort → reduce; disk-based, batch-only, poor at iteration. Spark: in-memory, caching, polyglot APIs, libraries.

---

# Chapter 3: Data Formats

Storage holds the bytes; the format organizes them: text, row-based Avro, columnar Parquet (the base of Part V's table formats).

## 3.1 Why formats matter: decoupling storage from processing

**Data formats provide "decoupling of the storage and the processing layer"**, and native format support gives "portability across heterogeneous HW and SW platforms" (§2.3). Formats are an **"optimization for specific purposes"**: size → storage; performance → workloads; portability → openness.

"Many data formats (evolving)": **Delta, Iceberg, Hudi**; cloud storage: **Microsoft ADLS Gen2, AWS S3, GCS**.

```flow
distributed storage → fast network → data formats (Avro / Parquet / Delta) → processing engine (data catalog) → app queries
```

*Sources: #03 s27; #01 s24.*

## 3.2 Text files

**"The most basic file type."**

| Strengths | Weaknesses |
|---|---|
| read/written by virtually any language | **inefficient at scale** |
| comma- and tab-delimited files work with many applications | numbers stored as strings waste storage |
| **human-readable, "since everything is a string"** | binary data (images) hard to represent |
| useful when debugging | converting to/from native types costs performance |

*Sources: #03 s28.*

## 3.3 Serialization and Avro

### Serialization

**"Serialization is the process of converting data into a sequence of bytes/bits"**, so in-memory data can be saved to disk or sent over the network; **deserialization** reads it back (e.g. Java `Serializable`, Python `pickle`). **"Backwards compatibility and cross-language support can be challenging"**; Avro was developed to address this.

### Avro

**Avro is a row-based storage format that stores data in a compact binary format using serialization:**

- an efficient serialization framework, top-level Apache project, widely supported in Hadoop;
- **JSON defines the schema**, and the **schema metadata is embedded in the file**;
- "compatibility without sacrificing performance": a highly optimized binary encoding and **rules for evolving schemas over time**.

```json
{
  "namespace": "com.datawarehouse.data",
  "type": "record",
  "name": "EmployeeName",
  "fields": [
    { "name": "first", "type": "string" },
    { "name": "last", "type": "string" }
  ]
}
```

Keys: `namespace` = space the object lives in; `type` = JSON field type; `name` = schema name; `fields` = the definition (name + type per field).

### Why use Avro

Also usable outside Hadoop and from many languages; **"ideal for long-term storage of important data"**; the embedded schema means it "will always be readable".

*Sources: #03 s29–32.*

## 3.4 Columnar formats and Parquet

### Columnar formats

**Columnar formats "organize data storage by column, rather than by row"**, so they are **"very efficient when selecting only a small subset of a table's columns"**. Examples: **RCFile**, **ORC** (Optimized Row Columnar), **Parquet** (**"default in Spark"**).

### Parquet

| Property | Detail |
|---|---|
| Origin | developed by **Cloudera and Twitter** |
| Support | Spark, MapReduce, Hive, Pig, Impala and others |
| Schema | **embedded in the file** (like Avro) |
| Optimizations | from **Google's Dremel**: less storage, more performance |
| Best use | **"most efficient when adding many records at once"** (some optimizations rely on repeated patterns) |

> [!compare] **Avro vs Parquet.** Both embed their schema; they differ in layout: Avro row-based (long-term storage, evolution), Parquet columnar (read few columns, write in bulk).

> [!info] **"Default in Spark" vs "default on Databricks".** #03 s33: Parquet is Spark's default; Databricks docs (#03 s44): "Delta Lake is the default format for all operations on Databricks". Different scopes; Delta is built on Parquet (Chapter 15).

*Sources: #03 s33–34, 44.*

## 3.5 Families of formats and the road to table formats

| Family | Examples | Main trait |
|---|---|---|
| **Text** | text, comma- and tab-delimited | human-readable, inefficient at scale |
| **Row-based, binary / serialized** | Avro | compact binary rows, embedded schema |
| **Columnar** | RCFile, ORC, Parquet | efficient when reading few columns |

#07 puts an **open table formats layer** (Delta Lake, Apache Iceberg, Apache Hudi), "for transactional safety with ACID transactions", over the storage formats; #03 defines **"the Delta Lake format: Parquet and columnar-based; ACID"**.

```flow
lake storage (S3, GCS, ADLS, HDFS) → storage formats (Parquet, ORC, JSON, Avro) → open table formats (Delta, Iceberg, Hudi: ACID) → compute engine → analytics
```

**A table format adds a transaction log and ACID guarantees on top of plain files such as Parquet.** Delta: Chapter 15; lakehouse stack: §14.6.

> [!flag] **"Optimized columnar formats" — resolved.** #07 s6 labels "Parquet, ORC, JSON, Avro — Optimized Columnar Formats for Object Storage", but #03 classes Avro as row-based (s30) and JSON-like files as text (s28, 52).
>
> **Resolution (external source).** #03 is right: only Parquet and ORC are columnar; Avro is row-based binary (records in blocks, embedded schema), JSON row-per-record text. The #07 label just means "file formats on object storage". [Apache Parquet](https://parquet.apache.org/); [Apache ORC](https://orc.apache.org/); [Avro specification](https://avro.apache.org/docs/1.12.0/specification/).

*Sources: #03 s52; #07 s6.*

### Key takeaways

- Formats decouple storage from processing and optimize size, performance or portability.
- Text: universal and readable, inefficient at scale (everything is a string).
- Avro: row-based compact binary, embedded JSON schema, schema evolution, "ideal for long-term storage".
- Columnar formats shine when reading few columns; Parquet (Cloudera + Twitter, Dremel) is the "default in Spark".
- Open table formats (Delta, Iceberg, Hudi) add ACID on top of file formats; Delta = Parquet + transaction log (Chapter 15).

---

# Part II: The Working Environment

Part II sets up where every lab runs: Databricks Free Edition, its notebooks and file systems, with Google Colab as the alternative.

---

# Chapter 4: Databricks and the Free Edition

## 4.1 Why Databricks: the company

The course puts a "strong focus and detail on Databricks" for four reasons:

- "It is a leading platform for Big Data processing and a marketing standard for Modern Data Architectures"
- "It is Spark-based and leads the Spark adoption (alongside ASF)"
- "It provides an all-in-one ecosystem for building and governing enterprise data and AI workloads"
- "It provides a free platform for learn and working with Spark"

**Other platforms are Google BigQuery, AWS Redshift, Microsoft Synapse and Snowflake.**

The company: started **late 2013** by **the creators of Spark** (original team from UC Berkeley AMPLab); the **largest code contributor to Apache Spark**; main product the Data Lakehouse Platform; partners in cloud (Azure, AWS, Google Cloud), data integration, governance, orchestration, BI, ML and consulting; "In October 2017 introduced Databricks Delta" (Chapter 15). In one line: **"Databricks is the company from the creators of Spark."**

*Sources: #02 s3–4, 50.*

## 4.2 The Databricks platform and the Lakehouse

A first definition stresses unification: "a cloud-based unified data platform that facilitates data management and collaboration among teams by unifying various data processing and storage systems". Retain the take-away one:

> [!key] **Definition.** "Databricks is a unified, open analytics platform for building, deploying, sharing, and maintaining enterprise-grade data, analytics, and AI solutions at scale."

- **It is based on Apache Spark** for in-memory distributed processing (Chapter 6).
- **It runs on AWS, Azure and Google Cloud**, and "integrates with cloud storage and security in your cloud account, and manages and deploys cloud infrastructure for you".

**It unifies data lakes, data warehouses and analytics tools into one platform, the Data Lakehouse**, "removing the complexity and tool sprawl that many companies struggle with" (see §14.5).

*Sources: #02 s5, 43, 50.*

## 4.3 Databricks Free Edition: scope and limits

**Free Edition is the main platform of the classes.** It was launched on **11 June 2025** at the Data + AI Summit, with a "$100 Million Investment to Develop the Next Generation of Data and AI Talent". The documentation calls it "a no-cost version of Databricks designed for students, educators, hobbyists", "ideal for learning, prototyping, and collaborative exploration", and states: **"Databricks Free Edition contains many of the same features available on the full Databricks platform, but in a serverless-only, quota-limited environment."**

The class calls it "the new (free) learning environment from Databricks (serverless)", the **"old Databricks Community Edition"**. Sign up at databricks.com/learn/free-edition **with a Google, Microsoft or e-mail account**.

Course scope: (1) "basic functionalities and … fundamental concepts"; (2) "Apache Spark and open-source technologies (ex: Delta Lake)"; (3) "Data Engineering for Big Data Processing and Analytics".

> [!warning] **Serverless has consequences.** Some Spark features in the slides do not run on Free Edition: the SparkContext `sc` and many configuration properties (§5.8), and GraphFrames ("not available in Free Edition"), so graph labs run in Colab (§4.8). The only limits stated are "serverless-only, quota-limited"; the "Limitations" icon just links to a web page.

> [!flag] **Free Edition vs Community Edition.** Some materials still show Community Edition ("ABD02 Databricks CE Intro" in #02 s22, 25; "Import … to Databricks CE" in #03 s48), while the current ABD02 notebook says "Free Edition" (c1–2). **In the class materials:** same lineage: #01 s40 calls Free Edition the "old Databricks Community Edition"; follow Free Edition.

*Sources: #01 s6–7, 38–40; #02 s6–9, 44, 46–47; #03 s48; ABD02 nb c1–2.*

## 4.4 Setting up your workspace

Of the left menu (Workspace, Catalog, Compute, Jobs & Pipelines, Marketplace, a SQL section …), **the class highlights Workspace** (your folders and notebooks, Workspace > Users > your user; folders have Import) **and Catalog** (Unity Catalog, §4.7, with catalogs **workspace**, **system** and **samples**).

> [!lab] **Lab: preparing your environment** (#02, done once).
> 1. **Create the ABD26 folder** under Workspace > Users > your user.
> 2. **Import your first notebook** from PC (Moodle) via the folder's three-dot menu. Every session repeats: download from Moodle, import into Databricks.
> 3. **Create the schema `abd`**: Catalog > workspace > Create schema (the catalog then shows `abd`, `default`, `information_schema`).
> 4. **Create the Volume `abd`** inside schema `abd`: Create > Volume.
> 5. **Upload** purplecow.txt and alice_in_wonderland.txt (Moodle Tec Resources) with "Upload to this volume". Path: **`/Volumes/workspace/abd/abd`**.

**Why "abd"?** "creating the schema and volume with the name 'abd' is recommended just because the exercises notebook use this objects"; other names break the notebook paths. ABD03 repeats the purplecow.txt upload and checks it in the UI and programmatically (§5.6).

> [!flag] **ABD26 vs ABD25.** The text says "ABD26"; screenshots show "ABD25" (#02 s10; #03 s48). The screenshot is dated Sep 2025; follow the text (ABD26).

*Sources: #02 s9–15, 21, 48; #03 s48; ABD03 nb c36–37.*

## 4.5 Databricks architecture: control plane and compute planes

| Part | Where it lives | Contains |
|---|---|---|
| **Control plane** | your Databricks account | web application, compute orchestration, Unity Catalog, queries and code |
| **Serverless compute plane** | your Databricks account | serverless compute |
| **Classic compute plane** | your cloud account | classic compute, next to the workspace storage bucket |

Users talk to the control plane, which connects to both compute planes; your own resources connect to the compute planes and workspace storage. **Free Edition uses the serverless compute plane.** The notebooks make it concrete:

- **"Remember that your defaul working path is in the Control Plane. Spark doesn't process or manages data here."** (`%sh pwd`); "This is where your notebooks are stored" (`%sh ls -l`).
- **"Spark processes the data in the Compute Plane (in a cluster or serverless env) and manages the data with Unity Catalog in a Distributed File System."**

ABD03 also checks the Python version: `import sys; print(sys.version)` and `%sh python --version`.

*Sources: #02 s29; ABD02 nb c90–98; ABD03 nb c2–9.*

## 4.6 Account components and DBFS

Account components: **workspace system data** ("notebook revisions, job run details, command results, and Spark logs"); the UC **workspace catalog** ("The workspace storage contains the default workspace catalog"); the UC **system catalog** ("For system information and management"); UC **shared samples** ("Catalog with sample tables"); and **DBFS**.

**DBFS is "a distributed file system in Databricks environments accessible under the dbfs:/ namespace"**; "DBFS root and DBFS mounts (Volumes) are both in the dbfs:/ namespace." **"Storing and accessing data using DBFS root or DBFS mounts is a deprecated pattern and not recommended by Databricks."** Your files go to the UC Volume (§4.4, §4.7).

> [!flag] **Deprecated, but used everywhere.** DBFS and `/databricks-datasets` are "deprecated" and "not visible in the UI", yet course notebooks read sample data from `/databricks-datasets`; ABD02 also speaks of "only 2 distributed file systems units" (databricks-datasets and the abd Volume) though `/Volumes` holds three catalogs (#02 s30, 33, 36–37; ABD02 nb c18, 51, 53). In the course, your own files go to the Volume.

*Sources: #02 s30.*

## 4.7 Unity Catalog: object model and Volumes

**"Every asset you govern in Unity Catalog is modeled as a securable object, an object on which you can grant permissions to users, service principals, or groups."** **Assets follow a three-level namespace `catalog.schema.object`** (e.g. `samples.bakehouse.sales_customers`, §5.2):

```flow
Metastore → Catalog → Schema → Table / View / Volume / Function / Model / Service / Secret
```

Directly under the metastore: service and storage credentials, external locations, external metadata, connections, shares, recipients, providers and clean rooms.

**Tables and Volumes are managed** ("Unity Catalog handles both governance and the underlying file storage lifecycle") **or external** ("Unity Catalog handles governance only"); same distinction for Spark tables in §8.8.

### Volumes

**"Volumes are Unity Catalog objects that enable governance over non-tabular datasets"**: "a logical volume of storage in a cloud object storage location" for "accessing, storing, governing, and organizing files".

> [!key] **Tables vs volumes.** "While tables govern tabular data, volumes govern non-tabular data of any format, including structured, semi-structured, or unstructured." Databricks "recommends using volumes to govern access to all non-tabular data."

Managed Volumes: "For simple Databricks-managed storage". External Volumes: "For adding governance to existing cloud object storage locations". Typical uses: **landing areas** for raw data from external systems (early ETL) and **staging locations** for ingestion (Auto Loader, COPY INTO, CTAS). `/Volumes/workspace/abd/abd` = Volume `abd` in schema `abd` of catalog `workspace`.

> [!info] **UC is the default.** "As of November 8, 2023, workspaces in new accounts are automatically enabled for Unity Catalog".

*Sources: #02 s31–32, 47.*

## 4.8 Google Colab as the alternative

**Google Colab is the alternative to Free Edition**: "a Cloud version of Jupyter Notebook integrated into the Google Cloud environment" (Jupyter: "an open-source web application that allows you to create, run, and share documents with code (e.g., Python), equations, visualizations, and text"). **"To use Colab, you don't need to have basic hardware and software available (it's serverless)"**; it "Makes programming (Python/R) and Data Science accessible to those who do not have an available infrastructure". At colab.research.google.com; used for **Structured Streaming (Chapter 18) and GraphFrames (Chapter 20)**.

### Installing PySpark: old recipe vs 2026 notebooks

A #02 slide shows "Getting Started with PySpark.ipynb" ("PySpark is Python interface for Apache Spark"), installing Java and Spark by hand:

```bash
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!wget -q https://dlcdn.apache.org/spark/spark-3.2.1/spark-3.2.1-bin-hadoop3.2.tgz
!tar xf spark-3.2.1-bin-hadoop3.2.tgz
!pip install -q findspark
!pip install pyspark
```

The 2026 notebooks (ABD08, ABD09) skip this: a commented `#%pip list` ("Check that the software you already have installed"), and only `!pip install graphframes-py==0.12.3` (§20.2), "the latest stable release of GraphFrames for Spark 4.x". The slide's versions (Spark 3.2.1, Hadoop 3.2, openjdk-8) are dated; it itself says "Check this site for the latest download link" (see the flag in §6.5).

### You build the SparkSession yourself

In Databricks `spark` exists (§5.8); **in Colab you create it** (§6.8–6.9):

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("ABD_Streaming_Example").getOrCreate()
```

- **Files live under `/content/...`**, e.g. `!mkdir -p /content/streaming_input`, `!rm -rf /content/streaming_input/*`; mounting Drive (`drive.mount('/content/drive')`) is optional.
- **Spark packages are set when the session is built.** "Unlike standard Python packages installed via `pip`, Spark packages are typically included at the time the SparkSession is initialized": `.config("spark.jars.packages", "io.graphframes:graphframes-spark4_2.13:0.12.3")`.

> [!compare] **A — Databricks Free Edition:** `spark` ready, files in `/Volumes/workspace/abd/abd`, no GraphFrames. **B — Google Colab:** build `spark` with `SparkSession.builder`, files in `/content/...`, packages via `spark.jars.packages`.

*Sources: #01 s8; #02 s39–41; #09 s37, 56; ABD08 nb c3–10; ABD09 nb c4–13.*

## 4.9 Help and documentation

- **docs.databricks.com**, with a **tutorial list** (query/visualize data, create a table, ETL pipelines, train an ML model, query LLMs).
- **Databricks Help Center** (help.databricks.com): Knowledge Base, Spark docs, Community, Training.
- Home-screen courses (Databricks Fundamentals, …) and a YouTube video ("Alex The Analyst - Become a Data Analyst Faster").

*Sources: #02 s9, 15, 20, 44–45, 47.*

### Key takeaways

- **"Databricks is the company from the creators of Spark"** (late 2013, UC Berkeley AMPLab), largest Spark contributor.
- A unified, open analytics platform on Spark and AWS/Azure/GCP, unifying lake + warehouse as the **Data Lakehouse**.
- **Free Edition (11 June 2025, the "old Community Edition"): serverless-only, quota-limited.**
- Setup: ABD26 folder → import notebook → schema `abd` → Volume `abd` → upload to `/Volumes/workspace/abd/abd`.
- Code lives in the **control plane**; Spark processes in the **compute plane**; **UC uses `catalog.schema.object`**, tables govern tabular data, Volumes govern files; DBFS root/mounts are deprecated.
- Colab: serverless alternative for streaming and graphs; you build the SparkSession.

---

# Chapter 5: Notebooks and File Systems

## 5.1 Your first notebook

**"Notebooks are the primary tool for creating data science and machine learning workflows on Databricks"**; they "provide real-time coauthoring in multiple languages, automatic versioning, and built-in data visualizations".

```flow
create the notebook → rename it → Connect to Serverless → run your code → check the language options
```

The first cell is `print("1st ABD Notebook")`. Shortcuts: **Shift+Enter** runs the cell and moves on; Ctrl+Shift+P opens the command palette; Esc H lists all shortcuts. Cell shortcuts: add above (A) / below (B), delete (D,D).

**Comment with Markdown:** start a cell with `%md` (or "+Text"), e.g. `%md # - ABD Databricks Free Edition`.

*Sources: #02 s17–18, 20–22; ABD02 nb c1–3.*

## 5.2 Mixing languages: magic commands

**"You can override the primary language by specifying the language magic command %<language> at the beginning of a cell. The supported magic commands are: %python, %r, %scala, and %sql."** The primary language appears in parentheses next to the notebook name.

```sql
%sql
select * from samples.bakehouse.sales_customers
```

> [!warning] **Languages do not share variables.** "Variables defined in one language (and hence in the REPL for that language) are not available in the REPL of another language. REPLs can share state only through external resources such as files in DBFS or objects in blob storage." To pass data to `%sql`, go through a table, file or view (Chapter 8).

| Magic | Class definition |
|---|---|
| `%sh` | "Allows you to execute shell code in your notebook." `-e` "to fail the cell (and subsequently a job or a run all command) if the shell command has a non-zero exit status." |
| `%fs` | "Allows you to use dbutils filesystem commands." |
| `%md` | "Allows you to include various types of documentation, including text, images, and mathematical formulas and equations." |

`%lsmagic` lists IPython magics (line: `%pip`, `%run`, `%time`; cell: `%%bash`, `%%sh`, `%%writefile`).

*Sources: #02 s23–26; ABD02 nb c88–89.*

## 5.3 Linux shell commands with %sh

> [!lab] **Lab: a tour of the shell** (ABD02, "These are Linux commands (%sh)"). `%sh pwd` (working directory, the control plane of §4.5), `ls -l`, `ps` (processes), `free` (memory), `env`, `python --version`, and `df` ("disk free - print available space on mounted file systems").

**The "Mounted on" column of `df` reveals the three storage locations:**

- **`/dbfs`**: "Databricks Distribute File System with example datasets. Deprecated"
- **`/Volumes`**: "storage units to the saved files in our workspace"
- **`/Workspace`**: "our system admin Workspace for user data"

*Sources: ABD02 nb c4–18; #02 s25.*

## 5.4 Storage locations: /dbfs, /Volumes, /Workspace

### /dbfs and the sample datasets

`/dbfs` holds `databricks-datasets`: `%sh ls /dbfs/databricks-datasets` or `%sh ls /databricks-datasets`; `display(dbutils.fs.ls('/databricks-datasets'))` is "also good … because they are native Databricks commands". **It is "a deprecated dbfs and accessivel only programaticaly"**, yet holds most course sample data (flag in §4.6).

### /Volumes

**"Note that this Volumes are the storage units for the main entries in our UI Catalog"**: `%sh ls /Volumes/samples`, `/Volumes/system`, `/Volumes/workspace`, `/Volumes/workspace/abd/abd`. "we have more usefull data in the Samples Volume", also via the UI.

> [!key] **Volume paths follow `/Volumes/<catalog>/<schema>/<volume>`**: `/Volumes/workspace/abd/abd`.

### /Workspace

Contains `Repos`, `Shared` and `Users`; your folders (e.g. ABD26) are in `/Workspace/Users/'your_user'`.

### The predefined locations at a glance

| Location | Meaning |
|---|---|
| `/Volumes/Workspace` | "The main Catalog of the Workspace" |
| `/Volumes/Workspace/default` | "The default schema" |
| `/Volumes/Workspace/abd` | "The abd schema you created" |
| `/Volumes/samples` | "Shared table samples" |
| `/databricks-datasets` | "dbfs - deprecated and not visible in the UI" |

> [!compare] **A — Shell paths (`%sh`):** mounted folders `/dbfs/databricks-datasets`, `/Volumes/...`, `/Workspace/Users/...`. **B — `%fs` / `dbutils.fs`:** the `dbfs:/` namespace (`dbutils.fs.ls("/")` lists `dbfs:/Volumes/`, `dbfs:/databricks-datasets/`). **The `dbfs:` prefix is "optional and deprecated"**: `%fs ls /Volumes/workspace/abd/abd` = `%fs ls dbfs:/Volumes/workspace/abd/abd`.

*Sources: ABD02 nb c18–36, 48–50, 62–63; #02 s33, 36; ABD03 nb c22, 24, 32–35.*

## 5.5 Distributed file-system commands: %fs and dbutils.fs

### dbutils: the Databricks Utilities

**`dbutils` "make it easy to perform powerful combinations of tasks. You can use the utilities to work with object storage efficiently, to chain and parameterize notebooks, and to work with secrets."** Available in Python, R and Scala. Utilities: credentials, data, **fs**, jobs, library, notebook, secrets, **widgets**, and the Utilities API library.

> [!warning] **Two limits.** "dbutils are not supported outside of notebooks." **"Calling dbutils inside of executors can produce unexpected results."**

```python
dbutils.help()            # all utilities
dbutils.fs.help()         # the file-system utility
dbutils.fs.help('cp')     # one command (also 'ls'; dbutils.notebook.help(), dbutils.widgets.help())
```

### %fs and dbutils.fs do the same job

**`%fs` is a shortcut to the dbutils file-system commands**:

```
%fs ls /
```

```python
display(dbutils.fs.ls("/"))
```

**Wrap `dbutils.fs.ls` in `display()`**: alone it is "too verbose"; `display()` gives a table (path, name, size, modificationTime).

> [!flag] **`%fs` is not a Linux command.** ABD02 labels it "Linux '%fs'" (nb c47–48); #02 s24 defines it as a magic using dbutils fs commands. **In the class materials:** follow the slides: Linux commands are `%sh` (§5.3).

### Exploring the sample datasets

`/databricks-datasets` is "Accessible within the Databricks workspace". Later labs read from:

```
%fs ls /databricks-datasets/definitive-guide/data/flight-data/csv
%fs ls /databricks-datasets/samples/people
%fs ls dbfs:/databricks-datasets/learning-spark-v2/people/people-10m.delta
%fs head /databricks-datasets/sms_spam_collection/README.md
```

**`%fs head` shows the start of a file**, a quick look before reading it with Spark.

*Sources: #02 s34–37; ABD02 nb c48–61, 64–68; ABD03 nb c20–34; ABD04 nb c9–14.*

## 5.6 Managing files: create, move, copy, delete

> [!lab] **Lab: a file's life cycle** (ABD02). Run `%sh ls` on the Volume between steps.

```
%sh touch /Volumes/workspace/abd/abd/Temp01
%sh mv /Volumes/workspace/abd/abd/Temp01 /Volumes/workspace/abd/abd/Temp01_New
%sh rm /Volumes/workspace/abd/abd/Temp01_New
```

"touch is a Linux command to create an empty file or upadate the file timestamp"; with Linux, a move is a rename. Copy: `%fs cp <source> <target>`.

### The dbutils.fs equivalents

| Task | Command |
|---|---|
| Copy | `dbutils.fs.cp("/Volumes/.../purplecow.txt", "/Volumes/.../purplecow1.txt")` |
| Delete a folder and content | `dbutils.fs.rm(path, recurse=True)` or `%fs rm -r <path>` |

Saved DataFrames are folders ("a directory with partitioned files", Chapter 9), so deletion needs the recursive option.

> [!key] **Housekeeping.** Notebooks delete what they create "just for house keeping purposes" / "to have a clean space": files with `dbutils.fs.rm`, tables with `DROP TABLE IF EXISTS`.

*Sources: ABD02 nb c37–47; ABD03 nb c36–39, 49–50, 59–61, 75; ABD04 nb c58–61.*

## 5.7 Databricks widgets

> [!notexam] **Out of the program.** The widgets slide is marked "Out of the program"; kept for completeness.

**"Input widgets allow you to add parameters to your notebooks and dashboards."** The API "consists of calls to create various types of input widgets, remove them, and get bound values". ABD02: "interactive input controls (such as text boxes and drop-down menus) … They act as parameters, allowing you to pass dynamic values into your code without having to hardcode values". Best for "a notebook or dashboard that is re-executed with different parameters" and "Quickly exploring results of a single query with different parameters". See `dbutils.widgets.help()` (or `display(dbutils.widgets)`); ipywidgets on Runtime 11.0+.

*Sources: #02 s27; ABD02 nb c69–71.*

## 5.8 Inspecting the Spark environment on serverless

**In a Databricks notebook the SparkSession already exists as `spark`** (`spark`, `spark.version`; Chapter 6).

> [!warning] **No `sc` on serverless.** Code using `sc` or `spark.sparkContext` (e.g. RDDs) fails on Free Edition: use `spark` (§6.8).

### Reading configuration properties

**"Remember that we are working on a serverless architecture. Many configuration variables and parameters are not available."** **`spark.conf.get` reads a property; `spark.conf.set` changes it** where allowed:

```python
spark.conf.get("spark.sql.shuffle.partitions")
spark.conf.get("spark.sql.session.timeZone")
spark.conf.set("spark.sql.ansi.enabled", "true")
```

Others read: `spark.databricks.clusterUsageTags.clusterId`, `spark.databricks.execution.timeout`, `spark.sql.legacy.timeParserPolicy`.

| Property | Class description |
|---|---|
| `spark.sql.ansi.enabled` | ANSI SQL compatibility |
| `spark.sql.shuffle.partitions`, `spark.sql.files.maxPartitionBytes` | partition settings (defined in §12.5) |
| `spark.sql.adaptive.enabled` | AQE: "# Not available in Serverless" |

Meaning of shuffle partitions and AQE: §12.4–12.5. The extracts have no outputs, so Free Edition values are not recorded.

**Tab-completion:** type `spark.`, `spark.catalog.` or `df0.`, run, and "after the erros just retype the . after spark" to list methods.

*Sources: ABD02 nb c72–74; ABD03 nb c10–19; ABD04 nb c3–8; ABD05 nb c2–7, 9, 54.*

### Key takeaways

- Notebooks: real-time coauthoring, automatic versioning, built-in visualizations; Python, SQL, Scala, R.
- `%python/%r/%scala/%sql` override a cell's language; **each language has its own REPL, so variables are not shared**.
- `%sh` = Linux, `%fs` = dbutils file system, `%md` = documentation.
- **Storage: `/dbfs` (deprecated samples), `/Volumes/<catalog>/<schema>/<volume>` (your files), `/Workspace` (users' folders).**
- `dbutils` only in notebooks; `display(dbutils.fs.ls(...))`; `dbutils.fs.rm(path, recurse=True)`; clean up after each exercise.
- `spark` is ready; serverless has no `sc` and few config properties; widgets are out of the program.

---

# Part III: Apache Spark Core

What Spark is, how an application is split across a cluster, and how data objects, operations and lazy execution fit together.

---

# Chapter 6: Apache Spark: What It Is and How It Runs

Where Spark came from, what it is made of, and which processes and objects take part when you run Spark code.

## 6.1 A brief history: from MapReduce to Spark

The class slide "MapReduce: the starting point" gives this timeline:

```flow
2002 MapReduce @ Google → 2004 MapReduce paper → 2006 Hadoop @ Yahoo! → 2008 Hadoop Summit → 2010 Spark paper → 2014 Apache Spark top-level project
```

The 2010 paper, "Spark: Cluster Computing with Working Sets" (Zaharia, Chowdhury, Franklin, Shenker, Stoica; UC Berkeley), diagnoses the MapReduce limits described in §2.10 (acyclic data flow, rereading from disk for iterative and interactive work).

Spark's answer is the **RDD**: "a read-only collection of objects partitioned across a set of machines that can be rebuilt if a partition is lost" (§7.1). The paper claims Spark "can outperform Hadoop by 10x in iterative..." (cut off on the slide; see §2.11). A 2015 Fast Company article shown in class reports IBM committing "more than 3,500 researchers and developers to the Apache Spark project, along with hundreds of millions of dollars in funding and resources".

*Sources: #04 s3–5.*

## 6.2 What Apache Spark is

> [!key] **Definition.** "Apache Spark is an open-source, distributed computation framework for in-memory parallel data processing across many machines (clusters)". "It's an in-memory, parallel data processing engine with data abstraction concepts and development APIs to allow data workers to execute batch, streaming, SQL, and machine learning workloads".

ASF tagline: **"Unified engine for large-scale data analytics"**; spark.apache.org (#03): "a multi-language engine for executing data engineering, data science, and machine learning on single-node machines or clusters"; notebook: "an open source distributed processing engine used for Big Data workloads". Spark also manages clusters and production-level stability ("not simple at all").

*Sources: #04 s6; #03 s43; ABD04 nb c2.*

## 6.3 Main characteristics and Spark Core

| Characteristic | What it means |
|---|---|
| **Written in Scala** | Scala is a "Functional programming language that runs on a JVM" |
| **Many data abstractions** | for storing (RDD, DataFrame, Dataset…) and processing (map, join…) |
| **API based** | Scala, Java, Python, SQL, R |
| **Unified, evolving platform** | Spark Core manages cluster and basic objects; libraries add Streaming, Graphs, ML… |

**Spark Core is the base layer**: memory management and fault recovery; scheduling, distributing and monitoring jobs; interacting with storage. Libraries on top: Spark SQL (Part IV), Structured Streaming (Ch 18), GraphFrames (Ch 20).

*Sources: #04 s7.*

## 6.4 What Spark is used for

- **Data integration, ETL/ELT:** pipelines that refresh DSS/DW.
- **Interactive analytics / BI:** insight from massive data, ad hoc or in planned dashboards.
- **High-performance computation:** less time for complex algorithms on large data.
- **ML and advanced analytics:** predicting outcomes, detecting fraud, inferring hidden information, deciding.
- **Real-time processing:** continuous, low latency, high reliability.

*Sources: #04 s11.*

## 6.5 Releases and versions

Resources: spark.apache.org, github.com/apache/spark, the latest release notes. **Spark 4.2.0 (14 July 2026) is "your Databricks Free Edition current release".** Homework: "check latest info of Spark 4.2" at spark.apache.org/docs/latest/. Check yours with `spark.version` (§5.8).

> [!flag] **Versions differ across materials.** #04 shows `spark.version` → `'4.0.0'` (Sep 2025 run) while calling 4.2.0 "current"; delta.io: "Delta Lake 4.4.0 on Apache Spark 4.2.0"; #08 quotes the Spark 4.0.1 streaming guide and demos a Spark 2.x notebook; #02 Colab uses 3.2.1; GraphFrames 0.12.3 is "for Spark 4.x". For "current Free Edition release", answer 4.2.0.

*Sources: #04 s8–10, 18, 49, 52; #03 s47; #08 s19, 42–47; #02 s41; ABD09 nb c9.*

## 6.6 Spark application architecture: driver, executors, cluster manager

**A Spark application is a driver process plus a set of executor processes** (diagram from *Learning Spark*, Ch 1):

```flow
Spark Application [ Spark Driver ⊃ SparkSession ] ↔ Cluster Manager ↔ Spark Executors (each with several task slots); driver ↔ executors directly too
```

| Process | What it does |
|---|---|
| **Driver** | "Maintains info about the Spark App and defines the SparkContext / SparkSession"; runs the program's I/O operations; negotiates resources with the resource manager (YARN in Hadoop, §2.6–2.7); analyses, distributes and schedules work on executors |
| **Executors** | run the code given by the driver; report execution state back |

**"A Spark Cluster consists of a Driver process and multiple Executors (which are individual JVM processes) running across one or more Worker Nodes."** Summary slide: a driver that coordinates tasks, executors that run computations in parallel.

*Sources: #04 s13–14, 54; ABD04 nb c2.*

## 6.7 Spark terminology: application, session, job, stage, task

| Term | Class definition |
|---|---|
| **Application** | "a driver process and a set of executors (to run the jobs)"; summary: "set of jobs managed by a driver" |
| **Spark Session** | "an object that provides a point of entry to interact with Spark functionality and programming with its APIs" |
| **Job** | "a sequence of stages (transformations) on data triggered by an action (set of tasks executed as a result of an action)" |
| **Stage** | "a set of tasks in a job that can be executed in parallel"; summary: "a set of tasks in a job (one per partition)" |
| **Task** | "an individual unit of work sent to an executor" |

```flow
application → jobs (one per action) → stages → tasks (one per partition) → executors
```

**A job is triggered by an action (§7.5); one task per partition (§7.4).** The two stage wordings (s15, s55) complement each other. The class diagram shows one job of two stages, each with RDDs and tasks.

*Sources: #04 s15, 55.*

## 6.8 SparkContext and SparkSession

**The driver instantiates the SparkContext or SparkSession: the connection to the cluster and the entry point to Spark.**

- "The SparkContext is the 1st version and the SparkSession was introduced in Spark 2.0 as the new entry point that subsumes SparkContext, SQLContext, StreamingContext, and HiveContext (for backward compatibility, they are preserved)".
- **"In Databricks notebooks the SparkSession is created for you and stored in a variable called spark."** #05: "the Spark session includes a Spark SQL context". In Databricks it is a `pyspark.sql.connect.session.SparkSession`.
- **"Spark Context - sc - is not supported in serverless compute. Use the Spark Session named 'spark' instead".**

Typing `spark.` lists its members (run the cell, then retype the dot after the error):

`catalog` (metadata registry, Ch 8) · `conf` (configuration, §5.8, §12.5) · `createDataFrame(data, schema=None, ...)`, `range(start, end=None, step=1, numPartitions=None)` and `read`, `table(tableName)` (Ch 9) · `readStream`, `streams` (Ch 18) · `sql(sqlQuery)` (Ch 8) · `newSession()`, `stop()`, `sparkContext`, `builder` (session management, §6.9).

> [!flag] **`sc` "not supported", yet used in slides.** ABD04 skips RDD code because `sc` fails on serverless, but slides use `sc` (#04 s22–23; #05 s20 `sc.parallelize`), `spark.sparkContext` (#04 s18; #05 s11; #08 s30 `.stop()`) and `sc.setCheckpointDir` (#09 s47; commented out in ABD09 c64). **Resolution (external source).** Serverless runs on Spark Connect: "Only Spark Connect APIs are supported. Spark RDD APIs are not supported"; no SparkContext, so `sc`/RDD code works only on classic compute — use `spark` and DataFrame equivalents (`spark.createDataFrame` for `sc.parallelize`). [Databricks serverless limitations](https://docs.databricks.com/aws/en/compute/serverless/limitations); [Spark Connect overview](https://spark.apache.org/docs/latest/spark-connect-overview.html).

> [!flag] **Spark SQL's entry point.** #04 s16: SparkSession subsumes SQLContext; #05 s66: Spark SQL "requires SQLContext as entry point or Spark Session"; #09 s40 uses `sqlContext.createDataFrame`. For this course: the SparkSession `spark`.

*Sources: #04 s16, 18; #05 s11, 66; #09 s40; ABD04 nb c3–8.*

## 6.9 Building a SparkSession yourself

> [!notexam] **Out of the program** (#04 s17, "Initializing Spark"). Still needed wherever Spark doesn't create the session, e.g. the Colab notebooks of Ch 18 and 20.

Builder pattern (Spark docs):

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder \
    .appName("Python Spark SQL basic example") \
    .config("spark.some.config.option", "some-value") \
    .getOrCreate()
```

- `appName` names the app; `config` sets a property; `getOrCreate()` returns the session. Short form: `SparkSession.builder.getOrCreate()`.
- Runtime config: `spark.conf.set("spark.notebook.name", "SparkSessionWKSH_1")`; `spark.conf.get(...)` returns it.
- Since Spark 2.0 the session has built-in Hive support (HiveQL, Hive UDFs, Hive tables) without a Hive setup; `spark.conf.get("spark.sql.warehouse.dir")` → `/user/hive/warehouse`.

> [!lab] ABD08: `SparkSession.builder.appName("ABD_Streaming_Example").getOrCreate()`. ABD09 adds `.config("spark.jars.packages", "io.graphframes:graphframes-spark4_2.13:0.12.3")`: GraphFrames "is a separate package … that needs to be explicitly added to your SparkSession", and packages "are typically included at the time the SparkSession is initialized" (Ch 20).

*Sources: #04 s17; ABD08 nb c5; ABD09 nb c10, 12.*

### Key takeaways

- Spark fixes MapReduce's acyclic, disk-based model for iterative/interactive work with the in-memory RDD.
- Open-source, distributed, in-memory, parallel; APIs in Scala, Java, Python, SQL, R; a "unified engine for large-scale data analytics".
- Spark Core: memory and fault recovery, job scheduling/monitoring, storage I/O; libraries add SQL, streaming, graphs, ML.
- Application = driver (holds SparkSession, schedules) + executors (JVMs on workers); jobs (one per action) → stages → tasks (one per partition).
- SparkSession (2.0) subsumes the older contexts; in Databricks it is `spark`; `sc` is unsupported on serverless. Current Free Edition: Spark 4.2.0.

---

# Chapter 7: Data Abstractions, Operations and Lazy Execution

What the work is done on (RDDs and DataFrames, split into partitions) and how it is expressed: transformations build a plan, actions run it.

## 7.1 Three abstractions: RDD, DataFrame, Dataset

**"Spark provides 3 main abstractions to work with data".**

| Abstraction | Class description | Kind |
|---|---|---|
| **RDD** | "Immutable collection of data elements partitioned across the cluster. In-memory resilient distributed abstraction of data"; Scala/Java/Python objects | basic, unstructured |
| **DataFrame** | schema RDD, row; "data organized into named columns - tables" | structured |
| **Dataset** | extends the DataFrame API with a type-safe, object-oriented interface | structured, typed |

The RDD paper ("Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing", Zaharia et al., NSDI '12): RDDs let programmers "perform in-memory computations on large clusters in a fault-tolerant manner", motivated by iterative algorithms and interactive data mining, where in-memory data can give an order-of-magnitude speed-up. **An RDD provides "a restricted form of shared memory, based on coarse-grained transformations rather than fine-grained updates to shared state".** #05: the DataFrame is the "main abstraction in Spark SQL (like RDDs in core Spark)", built on RDDs of Row objects, "like Pandas in Python" (Ch 8).

> [!notexam] **Datasets are "Out of the program"** (#04 s20). Yet later decks use them in definitions: "DataFrames and Datasets can represent static, bounded data, as well as streaming, unbounded data" (#08 s17, 21); a Spark job is "a chain of a Data Object (RDD/Dataset) dependencies" in a DAG (#09 s11).

*Sources: #04 s20–21, 54; #05 s13; #08 s17, 21; #09 s11.*

## 7.2 Creating RDDs with the low-level API

> [!notexam] **Out of the program, and not runnable on Free Edition** (#04 s22–23). ABD04: "We'll not run RDD code because RDDs creation is linked to sparkContext and this object in not suported on serverless clusters" (§6.8); its RDD cells are commented out.

```python
rdd = sc.parallelize(["Lisboa", "Porto", "Faro"])
rdd.collect()            # ['Lisboa', 'Porto', 'Faro']   (1 Spark Job)
mydata = sc.textFile("dbfs:/FileStore/tables/purplecow.txt")
mydata.map(lambda line: line.upper()).collect()
# ['I NEVER SAW A PURPLE COW.', 'I NEVER HOPE TO SEE ONE.', ...]
```

`parallelize` builds an RDD from a list; `textFile` from a file, one element per line (DataFrame equivalent: `spark.read.text()`). `map(lambda …)` applies an anonymous function to each element — what the summary means by **"Spark uses functional programming"**: functions as parameters, anonymous functions like `lambda`.

*Sources: #04 s22–23; ABD04 nb c15–17.*

## 7.3 A first look at DataFrames

**"A DataFrame is a data structure that organizes data into a 2-dimensional object - table - of rows and columns, much like a spreadsheet"**, "a flexible and intuitive way of storing and working with data". Created through the SparkSession, so they run on serverless.

```python
df = spark.createDataFrame([("James", 20), ("Anna", 31), ("Michael", 30),
                            ("Charles", 35), ("Brooke", 25)], ["name", "age"])
textFile = spark.read.text("/databricks-datasets/samples/docs/README.md")   # [value: string]
diamonds = spark.read.csv('/databricks-datasets/Rdatasets/data-001/csv/ggplot2/diamonds.csv', header=True)
```

- `createDataFrame` takes a list of tuples (rows) and column names (or `spark.createDataFrame(data, columns)`).
- **`spark.read.text` gives one column named `value`**, one row per line (ABD04 reads `/Volumes/workspace/abd/abd/purplecow.txt`; ABD02 read `alice_in_wonderland.txt` and a README). `spark.read.csv(..., header=True)` reads columnar data.
- **"Remember that a DataFrame is made of row objects"**: `textFile.take(5)` → `[Row(value='Welcome to the Spark documentation!'), Row(value=''), ...]`.

Inspection calls: `show(n)` (text table), `take(n)` (first n Rows; `take(1)` = `first()`), `collect()` (all Rows), `display(df)` (Databricks graphic table), `count()`, `type(df)`/`schema`/`dtypes`/`printSchema()`. Full detail in §9.2, §9.4, §9.6.

*Sources: #04 s24–25; ABD04 nb c18–41; ABD02 nb c75–87.*

## 7.4 Partitions and parallelism

**"In order to allow every executor to perform work in parallel, Spark breaks up the data into chunks, called partitions".**

- **"A partition is a logical chunk of data. It's a collection of rows on a node in the cluster".**
- **"Partitions are basic units of parallelism in Apache Spark"**; an RDD is a collection of partitions.
- By default Spark creates as many partitions as cluster cores, or as many as already saved on the file system.
- One task per partition, run in parallel; more parallelism = more speed, and it can be controlled.
- **Operations depending on several partitions (join, group by) run in separate stages** (§7.6).
- When possible, tasks run on the workers where the data is in memory (#04 s32; locality §2.7).

*Sources: #04 s26, 32, 55.*

## 7.5 Transformations and actions

**"Transformations are the ones that produce new DF. Actions are the ones that trigger computation and return results".**

| | Transformations | Actions |
|---|---|---|
| What | "Define a new DF based on the current one" | "Return values" and start the job |
| Result | a new, **immutable** DataFrame | a value to the user, or data written to storage |
| Examples | `select`, `filter`, `where`, `withColumn`, `withColumnRenamed`, `groupBy`, `join`, `drop`, `dropDuplicates` | `count`, `collect`, `show`, `take(n)`, `first`, `write`; also `top(n)`, `takeOrdered(n, function)` |
| When | lazily (§7.7) | immediately |

Notebook: "Other actions like save() write the DataFrame to distributed storage (like HDFS, DBFS or S3)". #05: **queries chain like transformations; "DataFrames are immutable"**.

::: example
*Example: select and where on diamonds (ABD04).*

```python
d_good = diamonds.select('cut', 'price').where("price >= 200 and price < 400")
d_good.show(5)
```

`d_good` is a new DataFrame `[cut: string, price: string]`; nothing is computed until `show(5)` runs the job (Ideal 326, Premium 326, Good 327, Premium 334, Good 335). `d_good.write.mode("overwrite").parquet(...)` is also an action (§9.7).
:::

> [!flag] **Lists mix RDD and DataFrame APIs.** s28 says transformations "Define a new DF", yet s29 lists `groupByKey()` (an RDD op); s28 says actions "Return values", yet s29–30 list `write.save(file)` and `saveAsTextFile(file)`, which write instead. **Resolution (external source).** `groupByKey` is a wide pair-RDD transformation (DataFrame equivalent `groupBy(...).agg(...)`); `saveAsTextFile` and `write.save()` are actions because they trigger execution — an action returns a result *or* writes data, as the notebook says. [Spark RDD Programming Guide](https://spark.apache.org/docs/latest/rdd-programming-guide.html).

*Sources: #04 s28–31, 33; #05 s42; ABD04 nb c42–58; ABD05 nb c50.*

## 7.6 Narrow vs wide transformations and the shuffle

| | Narrow | Wide |
|---|---|---|
| Diagram | 1 → 1 | 1 → N |
| Rule | each input partition feeds one output partition; **preserves partitioning** | each input partition feeds several; **shuffles** |
| Examples | `where` (#04); `.map()`, `.filter()` (#06) | `group by` (#04); `join`, `groupBy`, `sort`, `distinct`, `dropDuplicates`, `repartition` (#06) |
| Runs | "entirely in-memory and locally on individual partitions" | same-key data gathered from across the cluster, over the network |

**The shuffle makes wide transformations expensive: "the Shuffle is an expensive operation since it involves disk I/O, data serialization, and network I/O"** (Spark docs). Spark generates map tasks to organize the data and reduce tasks to aggregate it (the Ch 2 pattern); #06: network movement "introduces a massive overhead". Cheaper shuffles: §12.1.

*Sources: #04 s32; #06 s4.*

## 7.7 Lazy execution

**"Operations in Spark are lazy."** They only create an **"execution plan"**; **"The transformations are only computed when an action requires a result to be returned to the driver program".** Notebook: "Transformations contribute to a query plan, but nothing is executed until an action is called". **Benefit: "Spark can optimize the entire data flow from end to end"** (Catalyst, Ch 8).

::: example
*Example: the purple cow (#04 s35–41). Banner: "Data is not processed until an action is performed".*

```python
mydata = spark.read.text("purplecow.txt")
DF_Up  = mydata.select(upper('text'))
DF_F   = DF_Up.filter(col("text").startswith("I"))
DF_F.show()
```

Lines 1–3 define `mydata`, `DF_Up`, `DF_F`: the slides show them empty. Only `show()` materializes all three: 4 lines read, upper-cased, filtered to 3 ("I'VE NEVER SEEN...", "I NEVER HOPE TO SEE ONE;", "I'D RATHER SEE THAN BE ONE.").
:::

> [!trap] **Defining a DataFrame is not computing it.**

*Sources: #04 s34–41; ABD04 nb c43.*

## 7.8 Lineage and the DAG

```flow
Create → RDD1 → RDD2 → RDD3 (transformations) → Action → Result
```

> [!key] **Definitions.** "The Lineage is in a graph (DAG) that represents the dependencies (sequence) of the data objects (DataFrames) in a job". "Lineage: set of pointers stored in a graph (DAG) to the sequence of the RDDs in a job".

**"Spark keeps track of each DF lineage to provide fault tolerance"** — a lost partition can be rebuilt (Spark paper). The graph is a **DAG (Directed Acyclic Graph)**, shown two ways:

- **Spark UI** "DAG Visualization" per job, operations grouped by stage (e.g. Job 1, Stage 14: `textFile → textFile → PythonRDD collect`).
- **Generic DAG:** Stage 0 (`textFile → map`) and Stage 1 (`parallelize`) feed Stage 2 (`join → mapPartitions → filter → saveAsTextFile`); the `join` is wide (§7.6).

`diamonds.explain()` ("Check the lineage of the Dataframe", ABD04) prints the plan; reading plans: §12.5. #09 reuses the DAG as a graph example: vertices = data objects, edges = operations (§19.3).

*Sources: #04 s42–43, 55; ABD04 nb c39; #09 s11.*

### Key takeaways

- 3 abstractions: RDD (immutable, partitioned, resilient, unstructured), DataFrame (named columns + schema, Row objects), Dataset (typed; out of the program).
- RDDs need `sc`, so not on serverless; a partition is the unit of parallelism, one task each.
- Transformations return a new immutable DataFrame; actions return results or write data and trigger a job.
- Narrow (1 → 1) stays in a partition; wide (1 → N) shuffles (disk, serialization, network) and starts a new stage.
- Lazy: transformations build a plan, the first action runs it, so Spark optimizes end to end.
- The lineage DAG gives fault tolerance; see it in the Spark UI or with `explain()`. "Spark uses functional programming" (`lambda`).

---

# Part IV: Spark SQL

Part IV covers Spark's most used library: the Spark SQL engine, creating, reading, writing and transforming DataFrames, the built-in functions, query optimization, and classic big data algorithms written with it.

---

# Chapter 8: Spark SQL: Engine, Catalog and Tables

This chapter covers the module that owns DataFrames: its engine, the Catalog that tracks databases, tables and views, and why a DataFrame is not a table.

## 8.1 What Spark SQL is and its components

Spark SQL is the **"Spark module for structured data processing"**: it works with "table like" data objects and brings an SQL engine and syntax. It is **built on top of Spark Core** and uses DataFrames: data in columns with a schema, so data plus metadata.

| Component | What it is |
|---|---|
| DataFrame API | a library for data as tables; DataFrames of Rows and Columns |
| SQL Engine (interpreter and optimizer) | Catalyst optimizer + Tungsten memory management (§8.2, §8.3) |
| SQL command line interface | listed, not developed |
| Apache Hive (HiveQL) integration, ODBC/JDBC drivers | listed, not developed (JDBC reading: §9.5) |

The deck shows the 2015 SIGMOD paper *Spark SQL: Relational Data Processing in Spark* (Armbrust, Xin, Zaharia et al.): "a new module in Apache Spark that integrates relational processing with Spark's functional programming API". **It adds a tighter relational/procedural integration through a declarative DataFrame API, and a highly extensible optimizer, Catalyst.** A relational approach alone is not enough: users want ETL to/from semi-structured or unstructured sources and advanced analytics (ML, graphs).

> [!info] **Pandas API on Spark.** Available from Spark 3.2 (Databricks Runtime 10.0; older runtimes use Koalas). Pandas does not scale to big data; this API fills the gap, e.g. plotting directly from a PySpark DataFrame.

> [!flag] **Hive-era wording vs Unity Catalog.** Deck #05 uses Hive names ("Integration with Apache Hive", s3; "SparkSession - hive", s12; "Creating a DataFrame from a Table in Hive", s27); deck #02 (s31) describes Unity Catalog's `catalog.schema.object` namespace, which the labs use (`workspace.abd`, `/Volumes/workspace/abd/abd`).

*Sources: #05 s3–4, 7–9, 61, 66; #02 s31.*

## 8.2 The Catalyst optimizer

Catalyst is **"Spark's query optimization engine"**. SQL and DataFrame code reach it as the same kind of plan, turned into executable code in four phases:

```flow
SQL AST / DataFrame → Analysis → Logical Optimization → Physical Planning → Code Generation → RDDs
```

| Phase | Input | Output |
|---|---|---|
| 1. Analysis | **unresolved logical plan**, resolved with the **Catalog** | logical plan |
| 2. Logical optimization | logical plan | optimized logical plan |
| 3. Physical planning | optimized logical plan | several physical plans; a **cost model** selects one |
| 4. Code generation | selected physical plan | RDDs |

**The Catalog is used in phase 1** (names resolved against registered metadata, §8.4); **the cost model in phase 3**. The output is RDDs; DataFrames are built on RDDs of Row objects (§8.6).

*Sources: #05 s5.*

## 8.3 The Tungsten execution engine

Tungsten is **"Spark (in-memory) execution engine"**.

- **Memory management and bytecode generation:** "employs binary data storage to represent data more compactly than traditional Java objects" (less memory overhead); **dynamically generates native bytecode at runtime**, tailored to the transformations and actions, reducing interpretation overhead.
- **Query optimization and code generation:** **works together with Catalyst** to improve performance on modern compilers and CPUs; applies **predicate pushdown and constant folding**.

> [!compare] **Catalyst vs Tungsten.** Catalyst decides *what plan* to run; Tungsten decides *how data is held and executed* (binary storage, runtime bytecode). Together they are Spark SQL's "SQL Engine".

*Sources: #05 s6.*

## 8.4 The SparkSession and the Spark Catalog

The entry point is the SparkSession `spark` (§6.8); **"the Spark session includes a Spark SQL context"**. Its members include `catalog`, `conf`, `createDataFrame`, `range`, `read`, `readStream`, `sql`, `table`, `stop`.

**The Spark Catalog is "a central registry for storing and managing metadata about your data assets, like databases, tables, views, and functions"**, reached through `spark.catalog` (ABD05: "Through the SparkSession.catalog field, you can access all the Catalog metadata information about your tables, database, UDFs etc.").

| Catalog method | What it does |
|---|---|
| `listDatabases()`, `currentDatabase()` | lists schemas; shows the one in use |
| `listTables(dbName=None)` | lists tables and views |
| `listColumns(tableName)`, `listFunctions()` | columns of a table; functions |
| `dropTempView`, `dropGlobalTempView` | removes a view (§8.7) |
| `cacheTable`, `isCached`, `clearCache` | table caching |
| `createTable`, `createExternalTable` | creates tables |

```python
display(spark.catalog.listDatabases())
spark.sql('show databases').display()    # same, programmatic SQL
```

**The same metadata is readable three ways:** `spark.catalog` methods, `spark.sql("show ...")`, or a `%sql` cell (§8.9).

*Sources: #05 s11–12; ABD05 nb c8–15, 22.*

## 8.5 Databases (schemas): create, use, drop

**Database and schema are synonyms** in Spark SQL; both syntaxes work.

```python
spark.sql('create schema IF NOT EXISTS work2026')    # or: create database ...
spark.sql("USE schema work2026")
spark.sql("USE schema default")
spark.sql('drop schema work2026')
```

| Command | Effect |
|---|---|
| `show databases` / `show schemas` | lists schemas |
| `CREATE SCHEMA \| DATABASE IF NOT EXISTS name` | creates only if absent |
| `USE schema name` | sets the current schema |
| `DROP SCHEMA name` | deletes it |
| `DESCRIBE DATABASE EXTENDED name` | details, including location |

ABD05 switches back to `default` before dropping `work2026`. The Lakehouse lab (ABD07, Ch 16) uses the full Unity Catalog name and a comment, `CREATE SCHEMA IF NOT EXISTS workspace.DL COMMENT '...'`, checked with `DESCRIBE SCHEMA EXTENDED`, and resets each run with `DROP SCHEMA IF EXISTS workspace.DL CASCADE`.

> [!key] **CASCADE vs RESTRICT.** `CASCADE` drops the schema and everything in it; `RESTRICT` drops it only if empty.

*Sources: ABD05 nb c10–21, 30; ABD07 nb c10, 12–13.*

## 8.6 DataFrames vs tables; DDL vs DML

**"A Table is not the same thing as a Dataframe."** **"A Dataframe is data in memory. A Table is data on disk with its definition registered on the Catalog."**

| | DataFrame | Table |
|---|---|---|
| Definition | "Main abstraction in Spark SQL (like RDDs in core Spark)", 2-D labeled structure with a schema | "Structured collection of data stored persistently in a catalog, complete with a name, schema, metadata and data" |
| Lives | in memory, your session | on disk, registered in the catalog |
| Built from | base RDDs of Row objects | data files + catalog metadata |
| Usable by | your session | other sessions, users and tools on the same catalog |
| Analogy | "like Pandas in Python" | a database table |

After reading a CSV into `dep_delays`, `listTables()` shows **"there is no dep_delays on the Spark Catalog"** until it is saved as a table (§8.8).

| | DDL: Data Definition (and Management) Language | DML: Data (Retrieval and) Manipulation Language |
|---|---|---|
| Acts on | structure (metadata) | data |
| Examples | `CREATE`, `ALTER`, `DROP TABLE`, `DROP VIEW` | `INSERT`, `UPDATE`, `DELETE`, `MERGE` |

```sql
%sql
CREATE TABLE IF NOT EXISTS default.people_age (p_id INT, p_name STRING, p_age INT);   -- DDL
INSERT INTO default.people_age VALUES (10,'Charles',31),(10,'Sofia',36),(20,'Mary',28); -- DML
DROP TABLE IF EXISTS default.people_age;                                               -- DDL
```

*Sources: #05 s13–14; ABD05 nb c23–29, 43.*

## 8.7 Registering DataFrames as views: temporary and global

**"DataFrames are (in-memory) objects from your spark session."** To share them or query them with SQL, **register** them as a view or table.

| Method | Lifetime | Query as |
|---|---|---|
| `createTempView` / `createOrReplaceTempView` | **session** | `name` |
| `createGlobalTempView` / `createOrReplaceGlobalTempView` | **application** | `global_temp.name` |
| `df.write.saveAsTable("name")` | **database** (persistent, §8.8) | `name` |

Drop with `spark.catalog.dropTempView` / `dropGlobalTempView`; "there are some limitations to drop Tables with spark.catalog, use %sql instead" (`%sql DROP VIEW IF EXISTS diamondsview`).

**Global views** store metadata in the cluster's `global_temp` catalog. **They do not appear in the Databricks data list ("it's not registered on the metastore")**, are accessible to other users/notebooks on the cluster, and **must be re-registered if the application restarts**.

```python
dfLoans.createOrReplaceTempView("loans_delta")   # SQL2: now listed by show tables
spark.sql("SELECT count(*) FROM loans_delta").show()
```

*Sources: #05 s36–37; ABD05 nb c107–116; ABD06 SQL2 nb c1–13.*

## 8.8 Persistent tables: managed vs unmanaged (external)

**`saveAsTable` "will materialize the contents of the DataFrame and create a pointer to the data in the catalog (unlike the createTempView method)"**, so **persistent tables survive a Spark restart**. Read back with `spark.table` or `spark.sql` (§9.5). **Without catalog.schema, the table goes to the default schema.**

```python
spark.sql("DROP TABLE IF EXISTS dep_delays")
dep_delays.write.saveAsTable("dep_delays")
```

| | Managed | Unmanaged (external) |
|---|---|---|
| Spark manages | "both the metadata and the data in the catalog" | "only the metadata. You manage the data" |
| SQL | `CREATE TABLE t (col type, …)` | `... USING format OPTION (PATH 'path')` |
| DataFrame API | `df.write.saveAsTable("t")` | `df.write.option("path", path).saveAsTable("t")` |

**"Unmanaged Tables are also called External Tables."** Unity Catalog (deck #02) makes the same split: managed = governance + storage lifecycle; external = governance only (§4.7).

> [!lab] **ABD07 Silver/Gold.** `SV_Sales_Full.write.saveAsTable('DL.SV_Sales')` after writing Delta files to the Volume; the external `CREATE TABLE ... USING delta LOCATION` is commented out: **"ignore as it will not run on the Databricks Free Edition"**. Gold is saved directly ("no tech reason (need) to save the data first in a file in the gold layer"). Full lab: Ch 16.

> [!trap] **Unanswered notebook questions.** No `abd.` prefix → saved in **default** (#05 s27). `saveAsTable('DL.SV_Sales')` with no path → **managed** (#05 s39). `True` in `dbutils.fs.rm(path, True)` → **recurse** (§9.8).

*Sources: #05 s27, 38–39; ABD05 nb c44–48, 122–129; ABD07 nb c56–60, 69–76; #02 s31; ABD03 nb c64.*

## 8.9 Running SQL: spark.sql() and %sql

**"Spark SQL also supports declarative free-form SQL"**, but **"The DataFrame must be registered as a View or Table"**. Two ways:

```python
diamonds.createTempView("diamondsview")
spark.sql(" select * from diamondsview where color = 'E' ").show(5)   # returns a DataFrame
```

or a `%sql` cell (§5.2). Triple quotes `spark.sql(""" … """)` let the string contain quotes without escaping.

| Command | Shows |
|---|---|
| `show tables` | tables and views (= `listTables()`) |
| `show functions` / `like "w*"` | built-in functions / those starting with `w` |
| `show user functions` | your UDFs (§11.10) |

*Sources: #05 s40; ABD05 nb c115–120; #02 s23; ABD06 SQL1 nb c1–4, 18–21.*

## 8.10 Spark SQL is not a standard RDBMS

**"Spark SQL is not like a Standard RDBMS"**:

- **Compute separate from storage:** "acts purely as an analytical engine that reads from and writes to external distributed filesystems or data lakes".
- **OLAP, not OLTP:** "designed for massive batch processing, heavy aggregations, and analytical queries (OLAP) over large datasets".
- **A DataFrame is not a typical table:** "in-memory partitioned data spread across executors".
- **Lazy:** "a DAG … that Spark plans and optimizes only when you call an action" (§7.7).
- **Bottlenecks** are network I/O, shuffle, serialization, skew and resources (Ch 12), not just query plans.

**"Spark is not a replacement for a Data Base."**

*Sources: #05 s63, 66.*

### Key takeaways

- Spark SQL: module for structured data, on Spark Core, entry point `spark`.
- Engine = Catalyst (Catalog in analysis, cost model in physical planning, output RDDs) + Tungsten (binary storage, runtime bytecode).
- DataFrame = in memory, in your session; table = on disk, registered in the Catalog.
- Temp view → session; global view (`global_temp.`) → application; `saveAsTable` → persistent; managed vs external = who manages the data.
- SQL (`spark.sql`, `%sql`) only on registered views/tables; Spark is OLAP, not a database.

---

# Chapter 9: DataFrames: Creating, Reading and Writing

How to get data into DataFrames, inspect them, and write them back.

## 9.1 Ways to create a DataFrame

| Source | How |
|---|---|
| In-memory data | define schema and data (§9.2) |
| Existing RDD | `createDataFrame(rdd)` / `toDF` (§9.3, not examined) |
| Known file format | Parquet, JSON, CSV, text… (§9.4) |
| Catalog table | `spark.table` (§9.5) |
| External database | ODBC/JDBC (§9.5) |
| Another DataFrame | an operation on it (Ch 10) |

*Sources: #05 s16.*

## 9.2 From in-memory data: range, createDataFrame, schemas

"Range is a method to create a DF"; "Show is the standard method to display a DF". **`spark.range(5)` gives one column `id` (long), rows 0–4.**

```python
df_ex0 = spark.createDataFrame([["Porto", 1900], ["Lisboa", 4000]], ["Name", "Zip-Code"])
df_ex0.printSchema()     # Name: string, Zip-Code: long (nullable = true)
```

- **Without names, Spark creates `_1, _2, …`.**
- **Inferred types: text → `string`, whole numbers → `long`** (`bigint` in `dtypes`).
- Usual pattern (ABD04, "defining the data and the schema upfront"): `spark.createDataFrame(data, columns)`.

**Explicit schema:** a `StructType` ("the Schema") of `StructField(name, type(), nullable)`:

```python
schema = StructType([StructField('City', StringType(), True)])
df_city0 = spark.createDataFrame([['Porto'], ['Lisboa'], ['Faro']], schema)
df_city2 = spark.createDataFrame(["Porto", "Lisboa", "Faro"], "string").toDF("city")   # DDL type string
```

> [!key] **Three ways to set names and types:** a list of names (inferred types); a `StructType`; a type string plus `toDF(...)`.

*Sources: #05 s17–19; ABD05 nb c31–34, 52–74; ABD03 nb c41–42; ABD04 nb c21–35.*

## 9.3 DataFrames from and to RDDs

> [!notexam] **Out of the program.** Slides 20–21 carry the "Out of the program" badge; slide 20 uses `sc`, unsupported on Free Edition serverless (§6.8).

```python
people = sc.parallelize([('Mark',25),('Tom',22)]).map(lambda x: Row(name=x[0], age=int(x[1])))
df_people = spark.createDataFrame(people)      # or people.toDF(); schema name: string, age: long
```

Back to an RDD: "DataFrames are base RDDs containing Row objects", so `peopleDF.rdd`; `collect()` gives `Row(age=..., name=...)` objects.

*Sources: #05 s20–21.*

## 9.4 Reading files: CSV, JSON, text, Parquet

Readers hang off `spark.read`: a per-format shortcut or the generic `format(...).option(...).load(...)` (§9.7).

**CSV.**

```python
diamonds = spark.read.csv(".../ggplot2/diamonds.csv", header="true", inferSchema="true")
df = spark.read.load("path/people.csv", format="csv", sep=":", inferSchema="true", header="true")
```

`header` = first line holds names; `inferSchema` = Spark works out types; `sep` = separator (SQL2 class files use `sep=';'`). **"inferSchema means Spark will automatically figure out column types at a cost of reading the data more than once."** Without it, every column is a string.

**JSON.** `spark.read.json(path)` (people sample: `age` long, `name` string). ABD05 writes a small file with `dbutils.fs.put`, one record per line with an **array** and a **nested dict**; `read.json` reads nested fields too.

**Text.** `spark.read.text(path)` → **one string column `value`**, one row per line; filter with `filter`/`where` (string functions: §10.7).

**Parquet.** `spark.read.format("parquet").load('.../loan-risks.snappy.parquet')`.

> [!warning] **`price` read as a string.** ABD04 (c37) reads diamonds with `header=True` only, so #04 s33 shows `price: string`, yet filters `where("price >= 200 and price < 400")`. Deck #05 uses `inferSchema` and gets typed columns.

*Sources: #05 s22–25; ABD05 nb c35–43, 75–97; ABD02 nb c84–87; ABD04 nb c36–41; ABD06 SQL2 nb c7, 16, 30, 37.*

## 9.5 Reading catalog tables, JDBC sources and internet data

**Catalog table:** `spark.table("diamonds_5")` ≡ `spark.sql("select * from diamonds_5")`.

**JDBC:**

```python
df = spark.read.format("jdbc").option("url", url).option("dbtable", "tablename").load()
```

**Internet (curl/wget)**, per the Databricks docs:
- Databricks "does not provide any native tools for downloading data from the internet", but open-source tools work.
- Store non-tabular data in Unity Catalog **Volumes**.
- With no output path, tools write to **ephemeral storage**; **"Volumes do not support random writes"**, so unzip there before moving to Volumes.
- For cloud object storage, read directly with Spark.
- **"Some workspace configurations might prevent access to the public internet."**

*Sources: #05 s27–29; ABD05 nb c127.*

## 9.6 Inspecting DataFrames

| Method | Returns / shows |
|---|---|
| `show(n)` | text table; `vertical=True`, `truncate=False` options |
| `display(df)` | Databricks table "with visual options" (bar, scatter, map, pie, histogram, box plot, pivot…) |
| `collect()` | **all rows** as a list of Row objects |
| `take(n)` | first `n` Rows; **`take(1)` = `first()`** |
| `count()` | number of rows (diamonds: 53,940) |
| `schema`, `printSchema()`, `dtypes`, `columns` | schema object, tree, (name, type) pairs, names |
| `describe()` / `summary()` | statistics as a new DataFrame |
| `explain()` | the plan (§7.8, §12.5) |

**"A DataFrame is a collection of Row objects"**, hence `collect`/`take` return `Row(...)` values while `show`/`display` draw tables.

> [!warning] **Loose wording.** Materials call `take`/`collect` "Show the DF as a RDD" (#05 s31) and say `take(2)` "will show the underlying data types" (ABD04 c34). They return a **list of Row objects**; `collect` returns every row.

*Sources: #05 s30–32; ABD05 nb c55–57, 63–71, 109; ABD04 nb c22–35; #04 s31.*

## 9.7 Generic load/save and save modes

**"To read/write data, Spark relies on generic load/save functions"**, with a path and optional `format`:

```python
df = spark.read.load("path/file.json", format="json")
df.select("col1", "col2").write.save("file.parquet", format="parquet")
```

**Formats can be converted** (read one, write another): json, parquet, delta, jdbc, orc, libsvm, csv, text.

| Mode | If data exists |
|---|---|
| `"error"` / `"errorifexists"` **(default)** | exception |
| `"append"` | adds the rows |
| `"overwrite"` | replaces the data |
| `"ignore"` | saves nothing ("similar to a CREATE TABLE IF NOT EXISTS in SQL") |

```python
d_good.write.mode("overwrite").parquet("/Volumes/workspace/abd/abd/d_good")
```

**Writing is an action** (§7.5).

> [!compare] **Default format.** Spark: Parquet (#03 s33). Databricks: "Delta is the default format" (#05 s33; #03 s44, "for all operations on Databricks").

> [!trap] **A file name does not set the format.** #05 s33 writes to `file.parquet` with no `format`; on Databricks that writes Delta.

*Sources: #05 s33–34; ABD04 nb c57–61; #03 s33, 44.*

## 9.8 Writing DataFrames to files on a Volume

**A write produces a directory of partitioned files, not one file.**

```python
df.write.format("parquet").save("/Volumes/Workspace/abd/abd/df.parquet")   # folder df.parquet/
dbutils.fs.rm("/Volumes/Workspace/abd/abd/df.parquet", recurse=True)
```

List it with `%fs ls` or `dbutils.fs.ls`. **On Databricks, `save(path)` with no format writes Delta**, adding a `_delta_log` folder (§15.6).

*Sources: ABD03 nb c40–50, 53–56; ABD04 nb c58–61.*

### Key takeaways

- Sources: in-memory data, RDDs, files, catalog tables, JDBC, other DataFrames.
- `range` → `id`; unnamed columns → `_1, _2`; inferred types string/long; `StructType` sets the schema.
- CSV: `header`, `inferSchema` (extra read pass); text → one `value` column.
- `show`/`display` draw tables; `collect`/`take` return Rows.
- Save modes: errorifexists (default), append, overwrite, ignore; writes are directories; Databricks default is Delta.

---

# Chapter 10: DataFrame Operations

Changing a DataFrame's structure (DDL-style) and its data (DML-style): select, filter, sort, join, combine, and text.

## 10.1 Queries vs actions on DataFrames

Deck #05 calls transformations **queries**: they return a **new DataFrame**, are lazy and chainable; **"Actions trigger 'lazy' execution of queries"** and return a result to the driver (§7.5, §7.7). **"DataFrames are immutable"**: hence chaining or assigning to a new variable.

*Sources: #05 s42.*

## 10.2 Metadata (DDL-style) operations

These work **"with the DataFrame Metadata (DDL) rather than its data"**: `schema`, `printSchema()`, `columns`, `dtypes` (§9.6), plus:

| Operation | What it does |
|---|---|
| `toDF(...)` | all new column names, **by position** |
| `withColumnRenamed(old, new)` | renames one column **by name** |
| `withColumn(name, expr)` | adds a column, or **replaces** one with the same name |
| `alias` | renames a column in a query |
| `drop(col)` | removes a column |

```python
df = df.toDF("Name", "Age", "City", "Salary")       # fixes _1.._4; a name for every column
df.withColumnRenamed("Name", "1stName")
dfw = df.withColumn("Age", df.Age.cast("int"))      # bigint → int; or cast(IntegerType())
df.withColumn('Salary24', df['Salary']*1.05).drop('Salary')
```

*Sources: #05 s43–46; ABD05 nb c130–149.*

## 10.3 Data (DML-style) operations: limit, select, selectExpr, expressions

These work **"with the DataFrame Data (DML) rather than its metadata"** and return new DataFrames: `limit(n)`, `select`, `selectExpr` (SQL expression), `filter`/`where`, `distinct`, `sort`/`orderBy` (§10.4), `join` (§10.5), `union` (§10.6).

```python
diamonds.limit(3).show()
diamonds.select("carat", "cut", "color").show(5)
diamonds.select(diamonds["clarity"], (diamonds["price"] * 1.10).alias('up_price'))
diamonds.selectExpr("(price * 1.01) as new_price")
```

**`select` takes names or column objects** (`df["c"]`, `col("c")`), usable in arithmetic. Without `alias`, the column is named `(price * 1.1)` and shows floats like `359.70000000000005`. **`selectExpr` takes SQL strings and can rename** (`spark.range(5).selectExpr("(id * 2) as value")` → 0, 2, 4, 6, 8).

::: example
*SQL2 "HRate" = rating × views:*
```python
moviesDF.selectExpr('id', 'name', 'rating * views as HRate').sort('HRate', ascending=False)
```
:::

**Imports.** ABD05 uses `from pyspark.sql.functions import *` but recommends `from pyspark.sql import functions as F` (slide 26: `F.col(...)`); why: §11.1.

*Sources: #05 s47–51; ABD05 nb c150–164, 182; ABD06 SQL2 nb c18–25, 68–69.*

## 10.4 Filtering, sorting, distinct

**`where` ≡ `filter`** ("Where and Filter work the same way"), with an SQL string or a column expression:

```python
diamonds.where("color = 'E'")
diamonds.filter(diamonds['color'] == 'E')
moviesDF.filter("(rating > 3) and (id%2 == 0)")          # even ids
moviesDF.where("genres like '%Comedy%' and rating == 3").count()
```

**Prefer `df['col']`**: "for better error checking and future compatibility (like column names that are also attributes on the DataFrame class)".

**`sort` ≡ `orderBy`:**

```python
diamonds.sort(diamonds.table.asc(), diamonds.price.desc())
diamonds.orderBy('table', ascending=False)
```

**Distinct:** `moviesDF.select("genres").distinct().count()` ≡ `select(countDistinct("genres"))` (§11.2).

ABD04: `d_good = diamonds.select('cut','price').where("price >= 200 and price < 400")`, later written to Parquet (§9.7).

*Sources: #05 s52–54; ABD05 nb c157–161; ABD04 nb c50–56; ABD06 SQL2 nb c39–49, 64–65.*

## 10.5 Joins

**`df1.join(df2, on, how)`**:

| `how` | Returns |
|---|---|
| `inner` **(default)** | "only the rows that have matching values in both relations" |
| `left` / `right` (outer) | all rows of that side + matches; NULL where none |
| `full` (outer) | all rows from both; NULL on the side without a match |
| `leftsemi` | left rows with a match; no right columns |
| `leftanti` | left rows with **no** match; no right columns |
| `cross` / `.crossJoin()` | cartesian product: "Use with caution on large datasets" |

```python
EmpDF.join(DeptDF, "dept_id")                                  # or on="dept_id"
EmpDF.join(DeptDF, EmpDF["dept_id"] == DeptDF["dept_id"])      # expression
.join(managersDF, ["teamID", "yearId"])                        # several keys: list...
.join(managersDF, (t["teamID"] == m["teamID"]) & (t["yearID"] == m["yearID"]))   # ...or &
```

::: example
*#05 s57:* EmpDF (Paul/10, Mary/20, Tom/10, Sandy/30) inner-joined with DeptDF (Finance 10, Marketing 20, Sales 30, HR 40) on `dept_id` → **4 rows** (`dept_id, emp_id, emp_name, dept_name`); **HR has no employees, so it is absent.**
:::

> [!warning] **Expression joins keep both key columns** (s55 shows `name, id, name, id`), so later selects must name the side: `teamsDF["teamID"]`. Joining on a name or list keeps one key column.

**Three or more DataFrames: chain** `df1.join(df2, c).join(df3, c)`.

> [!lab] **Teams and managers (ABD05, SQL2).** `Team_Teams.csv`, `Team_Managers.csv` uploaded to the Volume; wins/losses by team and manager:
> ```python
> teamsDF.select("yearID","teamID","name","W","L") \
>   .join(managersDF.select("teamID","yearID","managerID"), ["teamID","yearID"]) \
>   .toDF(...)   # SQL version: from teams t, managers m where m.teamID == t.teamID and m.yearID == t.yearID
> ```

*Sources: #05 s55–57; ABD05 nb c166–173; ABD06 SQL2 nb c70–83.*

## 10.6 Union, unionByName and de-duplication

**`union` appends rows matching columns by position** ("different columns will give an error"); **`unionByName` matches by name**. **`union` keeps duplicates (SQL `UNION ALL`)**; remove them with `distinct()` (all columns) or `dropDuplicates([cols])` ("allows selection of columns").

```python
EmpDF.union(EmpDFplus).dropDuplicates(['emp_id'])   # 8 rows; the 2nd Carol removed by dropDuplicates
df_sales_24.union(df_sales_23).agg(avg('Quantity').alias('Avg_Sales_23-24'))   # SQL2: stack then aggregate
```

> [!flag] **"Union ignores duplicate data."** Slide 58 / ABD05 c176 say so, yet the same example needs `dropDuplicates` to remove the duplicate Carol.
> **Resolution (external source).** `union` (and `unionAll`, `unionByName`) keeps every row, `UNION ALL` semantics, "no automatic deduplication". [PySpark DataFrame.union](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.union.html)

*Sources: #05 s58; ABD05 nb c174–177; ABD06 SQL2 nb c84–90.*

## 10.7 Working with text: string functions

A text DataFrame has one column `value` (§9.4); ABD05 reads `alice_in_wonderland.txt` and renames it with `withColumnRenamed('value','text')`. Filter with `contains`, `like` (`%` wildcards), `startswith`, `endswith` (true/false columns):

```python
textFile.filter(textFile.value.contains("Spark"))
df.filter(F.col("name").like("%mes%"))
```

| Function | Example | Result |
|---|---|---|
| `upper` / `lower` | `lower(col("text"))` | case change |
| `length` | `length(col("text"))` | number of characters |
| `trim` | `trim(col("text"))` | surrounding spaces removed |
| `isNull` | `col("text").isNull()` | true if null |
| `split` | `split(col("text"), " ")` | array of words |
| `substring` | `substring(col("text"), 1, 5)` | "5 characters starting at position 1" |
| `instr` | `instr(col("text"), "Alice")` | position of the word |
| `regexp_replace` | `regexp_replace(col("text"), "Alice", "Maria")` | pattern replaced |

```python
df_text.where(length(col("text")) > 25)
df_text.groupBy("text").agg(count("*").alias("count"))
```

`split` + `explode` is the basis of word count (§11.6, §13.3).

*Sources: #05 s24–26; ABD05 nb c178–200.*

## 10.8 Lab: the Spark SQL exercises notebook

> [!lab] **ABD06 SQL2 ("Spark SQL Exercises").** Data from `/databricks-datasets/` and Moodle files in `/Volumes/workspace/abd/abd/`; many solutions give the DataFrame version with SQL commented beside it.

| Exercise | Technique | Section |
|---|---|---|
| Temp view `loans_delta` from Parquet | `createOrReplaceTempView`, `spark.sql` | §8.7 |
| Avg, sum, max, min, kurtosis, skewness of flight `count` | `agg`, `lit` | §11.3 |
| km → nautical miles; °C → °F | `selectExpr`, `withColumn`, `expr`, `cast` | §13.2 |
| Days between dates; year/quarter/month/week | `datediff`, `year`, `quarter`, `month`, `weekofyear` | §11.8 |
| Load `movielensABD.csv` (`;`), rename columns | `read.csv(sep=";")`, `toDF` | §9.4, §10.2 |
| Filtered movies; distinct genres | `where`, `like`, `%`, `countDistinct` | §10.4 |
| Avg/max rating per genre, ordered | `agg`, `groupBy`, `orderBy` | §11.3–11.4 |
| Genre with highest avg rating | `split(…, "[|]")` + `explode` | §11.6 |
| HRate; wins/losses; two-year sales | `selectExpr`; joins; `union` + `agg` | §10.3, §10.5, §10.6 |

*Sources: ABD06 SQL2 nb c1–90.*

### Key takeaways

- DataFrames are immutable; queries return new DataFrames, actions trigger execution.
- `toDF` renames by position, `withColumnRenamed` by name; `withColumn` adds/replaces (incl. `cast`).
- `where` ≡ `filter`, `sort` ≡ `orderBy`; `selectExpr` takes SQL strings; prefer `df['col']` and `functions as F`.
- Joins: inner by default; left, right, full, leftsemi, leftanti, cross; list or `&` for several keys; chain for 3+.
- `union` by position, `unionByName` by name; both keep duplicates → `distinct` / `dropDuplicates`.

---

# Chapter 11: Built-in Functions and UDFs

Once the data is loaded (Chapters 9–10), you work on it with Spark's built-in functions: profiling, aggregating, reshaping, dates and windows. When no built-in fits, you write a user-defined function (UDF).

> [!info] **Where this chapter comes from.** Class #06 ("Spark SQL Programming"), its programming notebook (ABD06 SQL1) and exercises notebook (ABD06 SQL2, mapped in §10.8).

## 11.1 What built-in functions are, and the function families

Built-in functions are **"Pre-defined and optimized operations provided by Apache Spark to transform, manipulate, aggregate, and analyze data directly within SQL queries, DataFrames or Tables"**. They let you understand data structure and types, validate content and patterns, run data quality checks and corrections, transform data and build standardization pipelines, prepare data for ML, and do Exploratory Data Analysis (EDA).

**Families:** Normal, Math, Datetime, Collections, Partition transformation, Aggregate, Window, Sort, String, UDF and Miscellaneous. `%sql show functions` lists them, `show functions like "w*"` filters by name, `show user functions` lists your own (§8.9).

Most functions come from `pyspark.sql.functions`:

```python
from pyspark.sql.functions import *      # short, but risky
import pyspark.sql.functions as f        # f.explode, f.split, f.avg ...
```

> [!warning] **The wildcard import can hide Python's own functions.** `import *` "may lead to namespace coverage, such as pyspark sum function covering python built-in sum function". The `f.` form "is more 'verbose' but considered a good practice".

Take-away reading: Databricks blog "Statistical and Mathematical Functions with DataFrames in Apache Spark" (2015): random data, summary statistics, sample covariance and correlation, cross tabulation, frequent items, math functions.

*Sources: #06 s11–12, 34, 55–56; ABD06 SQL1 c1–4, 18–21, 23–24.*

## 11.2 Data profiling: counts, distinct values, duplicates and nulls

Profiling is used **"when we need to assess data type, volumes, values, and quality"**. On a small `people` DataFrame (`id, name, height, weight, age, gender, job`, with `None`s and a duplicated row):

| Question | Code |
|---|---|
| Rows / schema | `people.count()`, `people.printSchema()` |
| Summary statistics | `people.summary()` (or `describe()`) |
| Remove fully duplicated rows | `people.distinct()` |
| Distinct names / how many | `people.select(['name']).distinct()`, `people.select(countDistinct("name"))` |
| One row per name | `people.dropDuplicates(["name"])` |
| Null age or job | `people.filter(people.age.isNull() \| people.job.isNull())` |
| Drop rows with nulls | `people.dropna()` |

Also listed: `select()`, `first()`, `last()`. In filters, **`|` is "or", `&` is "and"**. `dropna(how='any')` drops a row with **at least one** null; `how='all'` only if **all** values are null; `subset=[...]` limits the check to those columns.

> [!trap] **`.alias` on the result does not rename.** `people.select(countDistinct("name")).alias('Distinct Names')` fails because **"the result of select(countDistinct("name")) is a DF and not a column and thus, can't be renamed."** Put the alias on the column inside `select`.

*Sources: #06 s13; ABD06 SQL1 c5–17.*

## 11.3 Aggregate functions

**"Aggregate functions operate on a group of rows and calculate a single return value for every group."** Without `groupBy`, the whole DataFrame is one group. Class list: `sum()`, `sumDistinct()`, `max()`, `min()`, `avg()`, `variance()`, `skewness()`, `kurtosis()`, `count()`.

::: example
*Example (deck #06).* `dfGB`: 10 people (5 UK, 5 PT), columns `country, name, gender, age`.

```python
dfGB.agg(avg("age"), sum("age"), max("age"), min("age")).display()
```

One row: avg 31.6, sum 316, max 45, min 20.
:::

- **`count('*')`** in `agg` counts rows; "a similar and simpler command" is `df.count()`.
- **`alias`** names results: `df.agg(count('*').alias('number_of_rows'), avg('age').alias('average_age'))`.
- **`lit`** adds a constant label: `df2013.agg(lit('File 2013 Stats:'), avg("count"), sum("count"), max("count"), min("count"), kurtosis("count"), skewness("count"))`.
- **`groupby()` with no keys** aggregates everything: `moviesDF.groupby().max("rating")` = `moviesDF.select(max("rating"))` = `moviesDF.agg(max("rating"))`.

*Sources: #06 s13–14; ABD06 SQL1 c22–34; SQL2 c14–17, 50–54.*

## 11.4 Grouping with groupBy

`groupBy` **"groups rows sharing common key values into distinct subsets, allowing you to apply aggregate functions"**:

```python
df.groupBy("country").avg("age")                                    # one shortcut aggregate
df.groupBy("country").agg(avg("age").alias("avg_age"), sum("age"))  # several, named
df.groupBy("country","gender").agg(avg("age"))                      # several keys
```

Deck #05: `diamonds.select("color","price").groupBy("color").agg(avg("price"))`.

**Sorting the result.** A shortcut aggregate yields a generated name like `max(rating)`; sort on it or rename first:

```python
moviesDF.groupBy("genres").max("rating").orderBy("max(rating)", ascending=True)
moviesDF.groupBy("genres").avg("rating").toDF("genres","avg_rating").orderBy("avg_rating", ascending=False)
```

**`toDF` renames all columns by position; `alias` renames one column inside `agg`.** Also shown: the dictionary form `.agg({"rating": "avg"})` and SQL `select genres, avg(rating) as avg_rating from movies group by genres`.

> [!warning] **Slide 15's PT row is wrong.** It shows sum 173, avg 34.6, min 27, but the PT ages (28, 40, 34, 45, 28) give 175, 35.0, 28. Slide 14 (316 = 141 + 175) and slide 16 agree with the data.

*Sources: #06 s15; ABD06 SQL1 c35–39; SQL2 c55–63; ABD05 c165.*

## 11.5 Expanding functions: pivot

Pivot and explode are **expanding functions**: they **"generate multiple rows or columns from a single input row"**. **`pivot` "is used to rotate row values into distinct columns to reshape data into a cross-tabulated (wide) format"**, typically after `groupBy`.

::: example
*Example (deck #06).*

```python
dfGB.groupBy("country","gender").agg(avg("age"))          # long: UK F 22.5, UK M 32.0, PT M 34.0, PT F 36.5
dfGB.groupBy("country").pivot("gender").agg(avg("age"))   # wide:
```

| country | F | M |
|---|---|---|
| PT | 36.5 | 34.0 |
| UK | 22.5 | 32.0 |

The `gender` values became columns. The reverse, `groupBy("gender").pivot("country")`, puts countries as columns.
:::

*Sources: #06 s16; ABD06 SQL1 c40–43.*

## 11.6 Expanding functions: split and explode

Problem: each client has a comma-separated product list in one cell (`Client1: p1,p2,p3`); you want **one row per client–product pair**. **`explode` transforms "each element of an array-like column into a separate row"**; `split` first turns the string into an array.

```python
dfExp  = spark.createDataFrame([('Client1','p1,p2,p3'),('Client2','p1,p3,p5'),('Client3','p3,p4')],
                               ['Client','List_Products'])
dfExp1 = dfExp.withColumn('Product', split('List_Products', ','))
dfExp2 = dfExp1.select('Client', explode('Product').alias('Product'))   # Client1 p1, Client1 p2, ...
```

**If the column is already an array, skip `split`:** `explode(df3['acquisitions'])`.

> [!lab] **ABD06 SQL2: best single genre.** MovieLens `genres` is pipe-separated (`Adventure|Animation|Children`):
>
> ```python
> moviesDF.select(explode(split(moviesDF["genres"], "[|]")).alias("genre"), "rating") \
>         .groupBy("genre").avg("rating").orderBy("avg(rating)", ascending=False)
> ```
>
> The separator is the regex `"[|]"`. Same pattern as word count (§13.3).

*Sources: #06 s17–18; ABD06 SQL1 c44–52; SQL2 c66–67.*

## 11.7 Conditional logic: when, otherwise, lit

"Conditional formatting": `when(condition, value)` applies when true, `otherwise(value)` is the default, and `lit()` "creates a new column of literal value". **Chained `when`s: the first match wins.**

```python
moviesDF.withColumn('sensitive', when(col('genres').contains('Crime'),  lit('Yes'))
                                 .when(col('genres').contains('Drama'),  lit('Yes'))
                                 .when(col('genres').contains('Horror'), lit('Yes'))
                                 .otherwise(lit('No')))
```

Heat (`Action|Crime|Thriller`) and Waiting to Exhale (`Comedy|Drama|Romance`) get Yes, Toy Story No. A single `when(col('genres').contains('Comedy'), ...)` gives Toy Story Yes, Jumanji No.

*Sources: #06 s19–21; ABD06 SQL1 c23, 53–59; SQL2 c37.*

## 11.8 Datetime functions

Datetime functions are **"used to parse, manipulate, extract, and convert date and timestamp values"**. `spark.range(3).withColumn("current_date", current_date())` gives every row today's date (`2025-11-09` on the slide).

| Purpose | Functions |
|---|---|
| Now | `current_date()`, `current_timestamp()` |
| Convert a string | `to_date(lit('2025-12-31'))`, `to_timestamp(...)` |
| Format | `date_format("current_date", 'dd-MM-yyyy')` |
| Add / subtract | `date_add(col, 5)`, `date_sub(col, 5)`, `add_months(col, 2)` |
| Difference in days | `datediff(end, start)`; `selectExpr("DATEDIFF(new_date, current_date) as days_diff")` |
| Intervals | `expr("current_date + INTERVAL 1 HOURS")` (also MINUTES, SECONDS) |
| Extract | `year`, `quarter`, `month`, `weekofyear`, `dayofyear`, `dayofmonth`, `dayofweek`, `hour`, `minute`, `second` |

Filtering: `df.filter(col("current_date") > "2025-01-01")` or `.between("2025-01-01", "2025-12-31")`.

> [!lab] **ABD06 SQL2: `ABD_Data_24.csv`** (read with `sep=';'`). Days between dates for id 102: `withColumn('Dif_days', datediff("Date2", "Date1"))`; **`datediff` takes the end date first.** For id 299, chained `withColumn`s add `year`, `quarter`, `month`, `weekofyear` of `"Date"`.

*Sources: #06 s22–23; ABD06 SQL1 c60–76; SQL2 c28–34.*

## 11.9 Window functions

**"A window function performs a calculation across a set of table rows that are somehow related to the current row."** Unlike an aggregate (§11.3), which collapses a group into one row, **window functions "allow you to perform calculations while retaining the identity of each individual row"**. The group of rows is "called the Frame/Window".

**"The defining characteristic of a window function is the OVER clause"**, with three components:

| Component | Role |
|---|---|
| **PARTITION BY** | splits rows into partitions; the function runs on each separately |
| **ORDER BY** | logical order inside each partition |
| **ROWS / RANGE** | optional frame, e.g. "the last 3 rows" |

| Kind | Returns | Examples |
|---|---|---|
| Ranking | the rank of each row in its partition | `row_number`, `rank`, `dense_rank`, `percent_rank`, `ntile` |
| Analytic | a result over a finite set of rows | `first`, `last`, `cume_dist`, `lag`, `lead` |
| Aggregate | a summary over a group | count, mean, min, max, sum… |

In PySpark: `from pyspark.sql.window import Window`, then `func().over(window)`. Data `peopleIn`: `date` (2023-01 to 2023-03), `country`, `name`, `gender`, `age`. Outputs below are the slides' (10 rows); the SQL1 notebook adds three 2023-01 rows (Brad 20, a second Denny 32, Dave 30), so its outputs differ.

**1. `row_number`** **"assigns a unique, sequential number to each row in a window partition, starting from 1"**. `Window.orderBy("date", "age")` numbers across the whole DataFrame: Brooke 1, Jules 2, Denny 3, Mary 4, Pedro 5…

**2. With `partitionBy`**, **"we can restart the row_number count for every occurrence of the partitionBy parameter"**. `Window.partitionBy("date").orderBy("age")`: 2023-01 Brooke 1, Jules 2, Denny 3; 2023-02 Mary 1, Pedro 2, Tom 3, Rui 4. Descending: `orderBy(col("age").desc())`.

**3. `rank`** **"assigns a rank to each distinct value in a window partition based on its order"**. With `Window.orderBy("age")`: Brooke 20 → 1, Mary 25 → 2, Pedro 28 → 3, Sandra 28 → 3, Jules 30 → **5**, Denny 31 → 6. **Ties share a rank and `rank` leaves a gap; `dense_rank` would give Jules 4.**

> [!flag] **The notebook's `dense_rank` description fits `rank`.** The notebook says `dense_rank` ranks "skipping over ranks for ties"; the slide's output (3, 3, 5) shows that skipping is `rank`. **Resolution (external source).** `rank` leaves gaps (1, 2, 3, 3, 5); `dense_rank` returns ranks "without any gaps" (1, 2, 3, 3, 4). [PySpark functions.dense_rank](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.functions.dense_rank.html)

**4. Moving average:** **"calculated by creating a window of a predefined size and applying the mean function over it"**.

```python
window_ma = Window.partitionBy("country","gender").orderBy("date").rowsBetween(-2, Window.currentRow)
peopleIn.withColumn("moving_avg", round(mean("age").over(window_ma), 2))
```

The frame is **the current row and the two before it**, per country-and-gender, in date order.

> [!flag] **A "3-day" moving average that is really 3 rows.** The notebook says 3-day window; the slide's ROWS frame means "the last 3 rows". **Resolution (external source).** `rowsBetween` counts rows, so it equals 3 days only with exactly one row per consecutive date; a true 3-day window needs `rangeBetween(-2, 0)` on a numeric date, e.g. `orderBy(F.unix_date("date"))`. [Spark SQL window functions](https://spark.apache.org/docs/latest/sql-ref-syntax-qry-select-window.html)

The SQL form (`COUNT(*) OVER (PARTITION BY e.src)`) appears in PageRank SQL, §20.9. Take-away reading: Databricks blog "Introducing Window Functions in Spark SQL" (2015).

*Sources: #06 s24–29, 54; ABD06 SQL1 c77–98.*

## 11.10 User-defined functions (UDFs)

Three class rules: **"Functions are very useful to operate on data"; "You must register a function to use it in a Spark query"; "UDF's are expensive operations and a black box to Spark"**. Workflow: Python function → register as UDF → use in DataFrame code or SQL.

```python
def price_plus(num): return num*2
def price_tag(num):
    return 'Good' if num < 330 else 'Bad'
```

| Registration | Usable from |
|---|---|
| `price_plusUDF = udf(price_plus)` | DataFrame code only ("works only with DFs, not with SQL") |
| `price_plusUDF = spark.udf.register("price_plusUDF", price_plus)` | DataFrame **and** SQL, under the registered name |

`%sql show user functions` lists temporary UDFs; `DROP TEMPORARY FUNCTION IF EXISTS price_plusudf` removes one.

```python
diamonds.select("table", price_plusUDF("table").alias("Price_Plus"),
                price_tagUDF("table").alias("Price_Note")).show(2)   # 55.0 → 110.0 Good; 61.0 → 122.0 Good
```

In SQL, **"for using free SQL statements you must create a view or table"** first (§8.7):

```sql
-- after diamonds.createOrReplaceTempView("TempTable")
%sql select table, price_plusUDF(table) as TablePlus, price_tagUDF(price) as PriceNote from TempTable limit 3
```

> [!warning] **Prefer a native expression.** A UDF is "a black box to Spark"; the same doubling is just `diamonds.withColumn("table", col("table")*2)`.

> [!flag] **Slides and notebook register UDFs differently.** The slides call `udf(price_plus)` "registering" and register SQL names `pricePlus`/`priceTag`; the notebook registers `price_plusUDF`/`price_tagUDF` with `spark.udf.register` and says `udf()` is DataFrame-only. **Resolution (external source).** Consistent; the notebook is precise: `udf(f)` wraps for the DataFrame API only, `spark.udf.register(name, f)` registers a SQL function (and returns a DataFrame-usable UDF). The SQL name is arbitrary. [PySpark UDFRegistration.register](https://github.com/apache/spark/blob/master/python/pyspark/sql/udf.py)

*Sources: #06 s30–33; ABD06 SQL1 c99–115.*

### Key takeaways

- Built-in functions are optimized operations for profiling, quality, EDA and ML prep; import as `pyspark.sql.functions as f` to avoid hiding Python's `sum`/`max`/`min`.
- Profiling: `count`, `printSchema`, `summary`, `distinct`, `countDistinct`, `dropDuplicates`, `isNull`, `dropna(how, subset)`.
- Aggregates return one value per group; `groupBy` defines groups; `pivot` turns row values into columns; `split` + `explode` turn a list into rows.
- Window functions keep every row; OVER = PARTITION BY + ORDER BY + optional ROWS/RANGE frame. `rank` leaves gaps after ties, `dense_rank` does not.
- UDFs need `spark.udf.register` for SQL; they are expensive black boxes, so prefer native expressions.

---

# Chapter 12: Query Optimization

Catalyst and Tungsten (Chapter 8) optimize plans automatically, but some operations stay expensive. This chapter explains why (the shuffle) and the course's remedies: broadcast joins, Dynamic Partition Pruning, Adaptive Query Execution and data layout.

## 12.1 Why some operations are expensive: the shuffle

Four reasons operations are "expensive": **data shuffling, disk spill, serialization overhead and garbage collection**. The central one: **"Operations become expensive when they trigger a shuffle - the process of redistributing data across nodes in a cluster."**

Narrow transformations (`.map()`, `.filter()`) run locally in memory; operations that need all rows of a key together move data across the network, "which introduces a massive overhead". **Expensive: `join()`, `groupBy()`, `sort()`, `distinct()`/`dropDuplicates()`, `repartition()`.** Why a shuffle costs so much, and narrow vs wide: §7.6. Deck #05 adds that **"your bottlenecks are not just query plans"**: also network I/O, shuffle, serialization, skew and cluster resources (§8.10).

*Sources: #06 s4; #04 s32; #05 s63.*

## 12.2 Join strategies and Broadcast Hash Join

**"Join merges two datasets based on a common key."** Unoptimized, it needs a **shuffle-and-sort** (Sort-Merge Join) so matching keys land on the same worker. Four strategies are listed: **Sort Merge Join, Broadcast Hash Join, Cartesian Product Join, Bucket Join**; only BHJ is explained.

**Broadcast Hash Join** is used to **"join two datasets where one is small (fits in the driver and executors' memory)"**: the driver broadcasts the small dataset to every executor, which joins it with its local partition of the large one. **The large dataset never moves, so there is no shuffle.** **"By default, Spark will use a broadcast join if the smaller data set is less than the 10 MB threshold (configurable)."**

| | Sort-Merge Join | Broadcast Hash Join |
|---|---|---|
| When | general case | one side small (< 10 MB default) |
| What moves | both sides shuffled and sorted by key | only the small side, copied to every executor |

*Sources: #06 s5–6.*

## 12.3 Dynamic Partition Pruning

DPP is **built for star-schema queries** and **enabled by default** on two-table joins. Goal: **"Skip over data you don't need in the join results."**

1. build a hash table from the **dimension** table;
2. broadcast it to the executors;
3. use it to filter the **fact** table;
4. apply the filter dynamically in the **file scan**.

In the class diagram (*Learning Spark*), a date dimension feeds a broadcast exchange that injects a "Dynamic Filter" into the scan of the partitioned fact table, then both meet in a BHJ: **"Spark injects a dimension table filter into the fact table during a broadcast join."** **Fact files that cannot match are never read.**

*Sources: #06 s7.*

## 12.4 Adaptive Query Execution

AQE **"reoptimizes and adjusts query plans based on runtime metrics collected during the execution of the query"**, instead of keeping the pre-execution plan fixed.

**When?** Operators run as a pipeline, but **"a shuffle or broadcast exchange breaks this pipeline"**. These breaks are **materialization points**; the pieces between them are **query stages**. When a stage completes, Spark has statistics on all its partitions and nothing downstream has started, so it re-plans the unexecuted part, then loops until no stages remain.

| Feature | What it does |
|---|---|
| Dynamically coalescing shuffle partitions | fewer reducers by decreasing shuffle partitions |
| Dynamically switching join strategies | e.g. switch to BHJ (§12.2) |
| Dynamically optimizing skew joins | handles skewed joins |

Check it with `spark.conf.get("spark.sql.adaptive.enabled")`.

> [!flag] **AQE on Free Edition?** Deck #06: "Enable[d] by default since Spark 3.2"; ABD05 comments out the check with "# Not available in Serverless". **Resolution (external source).** AQE is on by default since Spark 3.2.0 and on Databricks; serverless just doesn't let you read/set `spark.sql.adaptive.*`, so the optimization still applies (medium confidence). [Databricks: Adaptive query execution](https://docs.databricks.com/aws/en/optimizations/aqe)

Take-away reading: Databricks blog "Adaptive Query Execution: Speeding Up Spark SQL at Runtime" (2020).

*Sources: #06 s8, 49–50; ABD05 c5.*

## 12.5 Tuning knobs and reading plans

On serverless, **"many configuration variables and parameters are not available"** (ABD03). Two partition settings:

| Setting | Meaning (class wording) |
|---|---|
| `spark.sql.shuffle.partitions` | "Number of partitions to use when shuffling data for joins or aggregations" |
| `spark.sql.files.maxPartitionBytes` | "Maximum number of bytes to pack into a single partition when reading files" |

Read with `spark.conf.get(...)`, change with `spark.conf.set(...)`. ABD05 uses `"100"` for shuffle partitions; the streaming demo (§18.15) sets `"2"` to "keep the size of shuffles small".

**Reading a plan:** `df.explain()` prints the physical plan; for `df.sort('age').explain()` the notebook says **"Ignore this for now, only useful for advanced topics."**

*Sources: ABD05 c6–7; ABD03 c11, 18; ABD06 SQL1 c28; #08 s47.*

## 12.6 Data layout: partitioning and Liquid Clustering

The last mechanism acts on storage. **Liquid Clustering is "a Delta Lake feature that automatically and continuously organizes data in tables, based on query patterns"** and "replaces the traditional methods like partitioning and z-order" (full coverage, and the flag on "replaces", in §15.9).

The class's **"mechanisms for query performance optimization"**: Partitioning, Broadcast Hash Join, Dynamic Partition Pruning, Adaptive Query Execution, Liquid Clustering.

*Sources: #06 s3, 9, 53, 60.*

### Key takeaways

- An operation is expensive when it shuffles; `join`, `groupBy`, `sort`, `distinct`/`dropDuplicates` and `repartition` all do.
- Sort-Merge Join shuffles and sorts both sides; Broadcast Hash Join copies the small side (< 10 MB default) and avoids the shuffle.
- DPP (star schemas) filters the fact table's file scan with a broadcast hash table from the dimension.
- AQE re-plans at materialization points: coalesces partitions, switches joins, handles skew; on by default since Spark 3.2.
- `spark.sql.shuffle.partitions` and `spark.sql.files.maxPartitionBytes` control partitions; `explain()` shows the plan.
- Optimization list: partitioning, BHJ, DPP, AQE, Liquid Clustering (§15.9).

---

# Chapter 13: Big Data Algorithms with Spark SQL

The functions of Chapter 11 are enough to write the classic big data algorithms: unit conversion, word count, and PageRank (Chapter 20).

## 13.1 Why classic algorithms

**"With the support of the Built-in function, you can implement typical big data algorithms that you can replicate to solve real big data problems."** The class lists three examples (converting metrics, word count, PageRank); the notebook adds a Monte Carlo estimate of Pi.

**Module goal:** "By the end of this module, you should be able to solve basic to intermediate big data problems on structured and unstructured data, by executing SQL queries and doing exploratory data analysis."

*Sources: #06 s35, 60; ABD06 SQL1 c116.*

## 13.2 Metric conversion

Apply a formula to every row, then aggregate. Two exercises:

```math
\begin{aligned}
{}^{\circ}F &= ({}^{\circ}C \times 1.8) + 32 \\
\text{Nm} &= \text{km} \times 0.539956803
\end{aligned}
```

where °C/°F are temperatures, km a distance and Nm the same distance in nautical miles. Each converts 11 values and averages them.

```python
df_temp_C.withColumn('value_F', (col('value_C') * 1.8) + 32).agg(avg('value_F').alias('avg_value_F'))
df_temp_C.selectExpr("((value_C * 1.8) + 32) as value_F").agg(avg('value_F'))
df_km.selectExpr("km * 0.539956803 as Nm").agg(avg('Nm'))          # slide 37, "Using selectExp"
```

SQL2 adds named temperatures (freezing 0, room 22, body 37, boiling 100, extreme cold −40) converted with **`expr()`** and rounded with a cast:

```python
df_f = df_celsius.withColumn("fahrenheit_temp", expr("celsius_temp * 9/5 + 32"))
df_f.withColumn("fahrenheit_temp", col("fahrenheit_temp").cast("decimal(10, 2)"))
```

| Style | Formula |
|---|---|
| `withColumn` + column expression | `(col('value_C') * 1.8) + 32` |
| `selectExpr` | SQL string, renames with `as` |
| `withColumn` + `expr()` | `expr("celsius_temp * 9/5 + 32")` |

*Sources: #06 s36–37; ABD06 SQL1 c117–122; SQL2 c18–27.*

## 13.3 Word count

Count how often each word appears. In Spark SQL it is **three steps: split lines into words, explode to one word per row, group and count**.

::: example
*Example (deck #06).* `purplecow.txt` read as text, one `value` column (§9.4):

```python
fileText = spark.read.text('/Volumes/workspace/abd/abd/purplecow.txt')
fileText.select(split('value', ' ').alias('word')) \
        .select(explode(col('word')).alias('word')) \
        .groupBy(col('word')).count().display()
```

Counts include I 3, never 2, see 2, purple 1, and tokens like `one!` 1 and `you,` 1.
:::

**Cleaning (optional, says the notebook):** punctuation sticks to words, so remove `.`, `,`, `!` first:

```python
fileText.select(translate(col("value"), ".,!", "").alias("text"))        # delete these characters
fileText.select(regexp_replace(col("value"), "[.,!]", "").alias("text")) # replace a regex with ""
```

> [!compare] **A — MapReduce (deck #03):** Java WordCount needs a Map class (split, emit `(word, one)`), a Reduce class (sum per word) and a `main()` to configure and run the job. **B — Spark SQL (deck #06):** one chained `split → explode → groupBy().count()`.

Deck #09 shows a word-count job in the Spark UI as **2 stages** around the shuffle (§19.3); streaming word count is §18.13.

*Sources: #06 s38; ABD06 SQL1 c123–130; #03 s23; #09 s11.*

## 13.4 Monte Carlo estimation of Pi

The Monte Carlo method is **"a classic probabilistic technique that uses random sampling to approximate a mathematical constant instead of calculating it through direct analytical formulas"**. Sample `num_samples` random points \((x, y)\) with `random.random()` (10,000 in the notebook) and count those `inside` the circle, \(x^2 + y^2 \le 1\):

```math
\pi \approx 4 \times \frac{\text{inside}}{\text{num\_samples}}
```

> [!warning] **Pi is not computed with Spark.** Although listed among algorithms "in Spark SQL", the cell is a plain single-machine Python loop, with no DataFrame or SQL.

*Sources: ABD06 SQL1 c135–138.*

## 13.5 PageRank

The third algorithm of deck #06, "a classical example of big data analysis (like word count)": lots of data, so it needs a distributable, scalable algorithm, and it is iterative. Since it works on a graph, **§20.9** covers it: the algorithm, hand-worked iterations, the deck #06 DataFrame and SQL versions, and the GraphFrames version (deck #09).

*Sources: #06 s39; ABD06 SQL1 c132.*

### Key takeaways

- Built-in functions alone implement typical big data algorithms; the goal is basic-to-intermediate problems via SQL and EDA.
- Metric conversion: formula per row (`withColumn`, `selectExpr` or `expr()`), then `agg(avg(...))`; `cast("decimal(10,2)")` rounds.
- Word count = split → explode → `groupBy(word).count()`; clean with `translate` or `regexp_replace`.
- A full Java MapReduce program becomes one chained Spark SQL expression.
- Monte Carlo Pi = `4 × inside / total`, plain Python in class; PageRank is in §20.9.

---

# Part V: The Data Lakehouse

Part V shows how data is organized for analysis at scale: from the warehouse and the lake to the lakehouse, the Delta Lake format that makes it possible, and the Medallion pattern that structures it.

---

# Chapter 14: Data Architectures: Warehouse, Lake and Lakehouse

Spark can process any amount of data (Parts III and IV), but analysts need it organized, trusted and easy to reach. This chapter follows the three generations of data architecture that tried to provide this.

## 14.1 Why organize data for decision support

A Decision Support System (DSS) exists to help a business decide. Deck #07 asks "Why do you need to organize data for DSS" and answers: integrate multiple source systems; refresh data periodically or in real time; data quality; apply business rules; **one single source of truth** (schema and data); precalculated analytical KPIs; simplified access for business users; computational resources and performance; data management, privacy and security; governance and compliance ("...": not complete). **Every architecture in this chapter is an attempt to deliver these goals.**

*Sources: #07 s2–3.*

## 14.2 The data warehouse era

The class presents "the 3 generations", labelled 1990, 2011 and 2021, with a diagram from the CIDR 2021 lakehouse paper (Armbrust, Ghodsi, Xin, Zaharia). The first (1990) is the **data warehouse**:

```flow
Structured data → ETL → Data Warehouses → BI, Reports
```

Only structured data enters, transformed by ETL before loading. Warehouses were already very large before Big Data (the 82-terabyte and 12.1 PB records of §1.1).

*Sources: #07 s4; #01 s17, 33.*

## 14.3 The Data Lake

Deck #01: "the new kid on the block".

- **The term was coined by James Dixon, CTO of Pentaho (about 2010).**
- Objective: "have one data pool", breaking data out of silos; it contrasts with the traditional "Data Mart".
- It stores "data of any type": structured, unstructured, streaming.
- It stores "non modeled" data: **"raw data" with no formal governance and no (business) structure: "Schema on-read"**.
- Solves cost, scalability and accessibility; used by "Business Users for experimentation", not operations. Similar terms: Data Sandbox, Analytical Lab.

> [!key] **Schema-on-read.** The lake stores raw data and applies structure only when it is read. The warehouse structures data by ETL before loading.

*Sources: #01 s30.*

## 14.4 The two-tier architecture and its problems

The second generation (2011) puts the two side by side:

```flow
Structured, semi-structured & unstructured data → Data Lake → ETL → Data Warehouses → BI, Reports
```

ML reads from the lake, Data Science from both, BI through the warehouse: "a small subset of data in the lake would later be ETLed to a downstream data warehouse". From 2015 cloud data lakes (S3, ADLS, GCS) replaced HDFS and this two-tier design became dominant. The paper lists the "major challenges with data warehouses" that lakehouses "can help address": **data staleness, reliability, total cost of ownership, data lock-in, limited use-case support.**

**It works, but copies data and adds an ETL step between lake and warehouse.**

> [!flag] **Two dates for the two-tier era.** The diagram labels it 2011; the paper text says cloud lakes replaced HDFS "from 2015" (#01 s32–33).

*Sources: #01 s32–33; #07 s4.*

## 14.5 The Data Lakehouse

The third generation (2021) is the **Data Lakehouse**. The CIDR 2021 paper argues the warehouse "will wither in the coming years and be replaced by … the Lakehouse, which will":

1. "be based on open direct-access data formats, such as Apache Parquet";
2. "have first-class support for machine learning and data science";
3. "offer state-of-the-art performance".

```flow
All data types → Data Lake → Metadata, Caching, and Indexing Layer (ETL inside) → BI, Reports, Data Science, Machine Learning
```

**A lakehouse is a data lake with a metadata, caching and indexing layer on top, so BI and ML consume from one single layer.** Deck #02: Databricks unifies "data lakes, data warehouses, and analytics tools into a single platform – the Data Lakehouse".

Decks #01 and #07 ("The Data Lakehouse – Unified Data Architecture"):

| | Data warehouse | Data lake | Data lakehouse |
|---|---|---|---|
| Data format | Closed, proprietary | Open | Open |
| Types of data | Structured, limited semi-structured | All (incl. textual, unstructured raw) | All |
| Data access | SQL-only, no file access | Open APIs, direct file access (SQL, R, Python) | Same as lake |
| Reliability | High quality, ACID | Low quality, **data swamp** | High quality, ACID |
| Governance | Fine-grained, row/column level | Poor: security on files | Fine-grained |
| Performance | High | Low | High |
| Scalability | Exponentially more expensive | Any amount at low cost | Any amount at low cost |
| Use case support | BI, SQL, decision support | Machine learning | **One architecture for BI, SQL and ML** |

**The lakehouse takes the lake's openness, data types, access and scalability, and the warehouse's reliability (ACID), governance and performance.** ACID comes from table formats such as Delta Lake (Chapter 15). Keeping data raw and ungoverned is what makes the lake both cheap and a "data swamp".

> [!flag] **Lakehouse and Delta dates do not line up.** #01 titles the lakehouse slide "2017 Snowflake, 2020 Databricks"; the diagram says 1990/2011/2021; the paper is CIDR, Jan 2021; "Databricks Delta" Oct 2017 (#02 s4); Delta paper VLDB 2020 (#03 s36); Medallion "Databricks 2019" (#07 s10); "Unified Lakehouse" streaming "2019+" (#08 s56). They refer to different events.

*Sources: #01 s32–34; #07 s4–5; #02 s5.*

## 14.6 Lakehouse layers and technologies

Deck #07 stacks five layers, fed by files, databases and streams:

| Layer (top to bottom) | Technologies | Role |
|---|---|---|
| 1. Analytics & BI | Dashboards, Power BI, Tableau, Jupyter | unified access for BI, ML, data science |
| 2. Compute & governance | Snowflake, Databricks, BigQuery, Redshift | query engines, fine-grained governance, unified catalog |
| 3. Open table formats | Delta Lake (Time Travel & Schema Enforcement), Iceberg (Schema Enforcement & Upserts), Hudi (Upserts/Deletes & File Listing Optimization) | "Transactional Safety with ACID Transactions" |
| 4. Storage formats | Parquet, ORC, JSON, Avro | "Optimized Columnar Formats for Object Storage" |
| 5. Lake storage | S3, GCS, ADLS, HDFS | distributed, low-cost object storage, all data types |

**The key layer is the third: open table formats add ACID transactions on top of ordinary files.** Calling Avro and JSON "columnar" is wrong; see the flag in §3.5 (only Parquet and ORC are columnar).

The Databricks docs view:

```flow
Delta Lake / Iceberg / Parquet → Unity Catalog → Mosaic AI · Databricks SQL · Lakeflow · AI/BI
```

Databricks SQL is warehousing, Lakeflow is ingest/ETL/streaming. Pillars of a "well-architected" lakehouse: lakehouse-specific — **Data Governance; Interoperability and Usability**; common — Operational Excellence; Security, Privacy, Compliance; Reliability; Performance Efficiency; Cost Optimization.

*Sources: #07 s6, 18.*

## 14.7 The big data architecture style: batch and real-time paths

Deck #01 quotes Microsoft Azure: "A big data architecture is designed to handle the ingestion, processing, and analysis of data that's too large or complex for traditional database systems."

```flow
Data sources → Data storage → Batch processing → Analytical data store → Analytics and reporting
Data sources → Real-time message ingestion → Stream processing → Analytical data store → Analytics and reporting
```

**Orchestration spans the whole pipeline.** Batch and real-time paths side by side is the Lambda idea (§17.6).

*Sources: #01 s29.*

## 14.8 Data modeling in the Lakehouse

Deck #07 points to two books ("available on Moodle ABD page in Technical Resources").

**Building the Data Lakehouse** (Bill Inmon, "father of data warehousing", with Mary Levins and Ranjeet Srivastava). Figure 1-6, "The data lakehouse architecture":

structured (ETL), textual (taxonomies, textual ETL) and other unstructured sources (streaming, integrations, APIs) land in **"Raw data in open file formats"**; at the core, **"Curated data with governance"** (labels such as Metadata, Taxonomies, Lineage, Granular, Summarization, KPI); on top, "Open API's with direct file access using SQL, R, Python and other languages", feeding BI/SQL, real-time apps, data science and ML.

**Big Book of Data Warehousing and BI** (Databricks eBook), §3.1 "Data Warehousing Modeling Techniques and Their Implementation on the Lakehouse"; source of the §16.2 modeling diagram.

Take-away slides add "The Data Lakehouse Platform for Dummies" (Databricks, Ulrika Jägare) and "Data Modeling in 2025: Complete Guide to Kimball, Data Vault 2.0, and Data Mesh for Modern Data Teams" (Shabana Khanum, Medium). **Kimball, Data Vault 2.0 and Data Mesh are only named as "Modern Data Design Patterns"**; they reappear as labels in the Medallion layers (§16.2).

> [!info] **Readings for #07.** *Big Book of DW & BI* §3.1; Inmon ch. 1 and 6; *Delta Lake: The Definitive Guide* ch. 5; *Learning Spark* ch. 9; "Run the class (#7) notebook available in Moodle".

*Sources: #07 s7–8, 17, 21, 23.*

### Key takeaways

- Data is organized for DSS to integrate sources, ensure quality, apply business rules and give one single source of truth.
- Warehouse (1990): structured, ETL, BI. Lake (~2010, James Dixon): all types, raw, schema-on-read, no governance.
- Two-tier (lake + warehouse): staleness, reliability, cost, lock-in, limited use cases.
- Lakehouse (CIDR 2021): metadata/caching/indexing layer over the lake; open formats + ACID + governance + performance; one architecture for BI, SQL and ML.
- Five layers: lake storage, storage formats, open table formats (ACID), compute/governance, analytics/BI.

---

# Chapter 15: Delta Lake

The lakehouse needs warehouse reliability on cheap lake files. Delta Lake provides it, and it is the default format on Databricks: every lab table is Delta unless you say otherwise.

## 15.1 What Delta Lake is

Deck #03: "an open-source storage layer based on parquet", developed by Databricks and donated to the Linux Foundation. It "adds reliability, quality, performance to data lakes – Delta Tables"; "includes a transaction log on top of parquet files and the schema enforcement and evolution"; "provides ACID transactions, scalable metadata handling, and unifies streaming and batch data processing"; "brings together the best of data warehousing and data lakes - Data Lakehouse".

Deck #01: **Delta Lake = Parquet files + transactional log**, handling batch, streaming and updates/deletes. The Databricks docs: it "extends Parquet data files with a file-based transaction log for ACID transactions and scalable metadata handling"; **"Delta Lake is the default format for all operations on Databricks. Unless otherwise specified, all tables on Databricks are Delta tables."** This does not contradict Parquet being Spark's default (§3.4, §9.7): different platforms.

Deck #02: "In October 2017 introduced Databricks Delta".

> [!flag] **Open source or "proprietary"? — resolved.** #08 s29 lists Delta as "A proprietary sink"; #02 s7, #03 s35, 37, 47 and #08 s61–62 present it as open source (Linux Foundation; "open standards, open protocol"; with Iceberg and Hudi).
>
> **Resolution (external source).** Delta Lake is open source (Linux Foundation, Apache 2.0, "not controlled by any single company"); Databricks adds proprietary features on its platform, which the #08 slide probably means. [delta.io](https://delta.io/); [Delta Lake licence](https://github.com/delta-io/delta/blob/master/LICENSE.txt).

*Sources: #03 s35, 44; #01 s31; #02 s4, 7; ABD03 nb c53.*

## 15.2 The Delta Lake paper: ACID tables over cloud object stores

VLDB 2020 paper "Delta Lake: High-Performance ACID Table Storage over Cloud Object Stores" (Armbrust et al.):

- **Cloud object stores (e.g. S3) are cheap, but their key-value semantics make ACID and performance hard.**
- Delta uses a **transaction log compacted to Parquet** for ACID, time travel and fast metadata operations over billions of partitions and files.
- Tables are accessible from Spark, Hive, Presto, Redshift and others.
- Further features: time travel (point-in-time queries, rollback of erroneous updates); UPSERT/DELETE/MERGE (e.g. GDPR); efficient streaming I/O (small objects coalesced later; "tailing" reads, so a table acts as a message bus); caching (immutable objects, transparent SSD cache in Databricks); layout optimization (object size, Z-order); schema evolution (read old Parquet without rewriting); audit logging from the log.

*Sources: #03 s36.*

## 15.3 Key features

delta.io feature list (deck #03):

| Feature | Class wording |
|---|---|
| ACID transactions | "serializability, the strongest level of isolation" |
| Scalable metadata | petabyte-scale, billions of files |
| Time travel | "Access/revert to earlier versions of data for audits, rollbacks, or reproduce" |
| Open source | "open standards, open protocol" |
| Unified batch/streaming | "Exactly once semantics ingestion to backfill to interactive queries" |
| Schema evolution / enforcement | "Prevent bad data from causing data corruption" |
| Audit history | full audit trail |
| DML operations | "SQL, Scala/Java and Python APIs to merge, update and delete datasets" |

Deck #01's headline four: **ACID transactions, schema enforcement, unified batch and streaming, time travel (data snapshots).** Deck #03's summary: "The Delta Lake format: Parquet and Columnar-based; ACID".

*Sources: #03 s37, 52; #01 s31.*

## 15.4 ACID guarantees

From the Databricks page "What are ACID guarantees on Databricks?" (deck #03):

| Property | Definition |
|---|---|
| Atomicity | "all transactions either succeed or fail completely" |
| Consistency | "how a given state of the data is observed by simultaneous operations" |
| Isolation | "how simultaneous operations potentially conflict with one another" |
| Durability | "committed changes are permanent" |

**These guarantees come from the Delta transaction log** on top of the Parquet files. One consequence: Delta accepts `DELETE` (§15.7).

*Sources: #03 s35, 45.*

## 15.5 Components and the open-table-format ecosystem

Four Delta components: **Delta engine, Delta tables, Delta format and storage layer.**

```flow
Data Lake for all your data → DELTA LAKE ("Structured transactional layer") → DELTA ENGINE ("High performance query engine") → BI dashboards · Data Science Workspace · ML lifecycle (mlflow)
```

Caption: "One platform for every use case". delta.io: engines include Spark, Presto, Flink, Trino, Hive, Snowflake, BigQuery, Athena, Redshift, Databricks; APIs Scala, Java, Rust, Python; **UniForm ("Delta Universal Format")** lets Iceberg and Hudi clients read Delta tables; banner "Delta Lake 4.4.0 on Apache Spark 4.2.0".

The other open table formats are **Apache Iceberg and Apache Hudi** (features in the §14.6 table). Deck #08 groups all three as the ACID formats that "bring ACID transactions to cloud object storage" (§15.10).

> [!info] **Reference book.** *Delta Lake: The Definitive Guide* (Denny Lee, Tathagata Das, Vini Jaiswal; O'Reilly): ch. 1 for #03, ch. 5 for #07.

*Sources: #03 s38, 46–47; #07 s6; #08 s61.*

## 15.6 Lab: Delta files on a Volume and the transaction log

> [!lab] **ABD03 and ABD05.** Save a small DataFrame in Parquet and in Delta on a Volume and compare the folders.

```python
df = spark.createDataFrame([["Porto", 1900], ["Lisboa", 4000]], ["Name", "Zip-Code"])
df.write.format("parquet").save("/Volumes/Workspace/abd/abd/df.parquet")
#df.write.format("delta").save("/Volumes/Workspace/abd/abd/df.delta")
# Remember that Delta is the standard format for Databricks
df.write.save("/Volumes/Workspace/abd/abd/df.delta")
```

**With no `format(...)`, `write.save` produces Delta on Databricks**; likewise `spark.read.load(path)` (#05: "Delta is the default format"). Both outputs are directories of partitioned files; the difference: **"Delta files use a directory log (_delta_log)"**.

```python
%fs ls /Volumes/Workspace/abd/abd/df.delta/_delta_log/
```

```flow
df.delta/ → _delta_log/ (000…0.json, .crc) + part-00000-… data files (Parquet)
```

ABD05 repeats it with diamonds and deletes the folder with `dbutils.fs.rm(path, True)` (§5).

*Sources: ABD03 nb c41–61; #05 s33, 35; ABD05 nb c98–106.*

## 15.7 Lab: Delta tables — UPDATE, DELETE, MERGE (upsert)

> [!lab] **ABD03 (postal codes) and ABD07 (Gold sales table).** Register a Delta table, then change rows with SQL DML.

ABD03 writes 21 postal codes (id, city, p_code), including a dummy `(0, "XXX", 0000)`:

```python
df_pc.write.format("delta").mode("overwrite").saveAsTable("abd.postal_codes")
```

"What happens if your don't prefix the name of your table with abd?" — it goes to the default schema (§8.8).

```sql
UPDATE abd.postal_codes SET City = "porto" WHERE City="Porto"
DELETE FROM abd.postal_codes WHERE City="XXX"
-- ABD07, on the Gold table (Chapter 16):
UPDATE DL.GD_Sales_by_city SET Quantity = 10 WHERE City='Coimbra' AND Type_Product='AAB'
DELETE FROM DL.GD_Sales_by_city WHERE City='Lisbon' AND Type_Product='AAB'
```

> [!warning] **Delta vs plain Parquet.** ABD07 has a commented demo that tries to "delete one record" from a table over the Bronze Parquet files; it "will not work on Databricks Free Edition", so no result is shown. **Updates/deletes and DML are listed as Delta features** (#01 s31; #03 s37).

**MERGE (upsert):** rows whose key exists are updated, new keys inserted.

```python
items = [('Coimbra','AAB', 20), ('Coimbra','AAD', 40), ('Lisbon','AAB', 1)]
spark.createDataFrame(items, ['City','Type_Product','Quantity']).createOrReplaceTempView("merge_table")
```

```sql
MERGE INTO DL.GD_Sales_by_city as a
USING merge_table as b
on a.City = b.City and a.Type_Product = b.Type_Product
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *
```

::: example
*Example.* On keys (City, Type_Product), after the UPDATE and DELETE:

- (Coimbra, AAB): if it exists (Quantity 10) → **matched**, updated to 20.
- (Lisbon, AAB): deleted → **not matched**, re-inserted with 1.
- (Coimbra, AAD): matched if it exists, else inserted with 40 (the notebook has no saved outputs, so it does not say).
:::

The notebook: MERGE "could be usefull to manage tables in the Gold layer that are organized with a Star Schema". The Databricks page "Upsert into a Delta Lake table using merge" (#05): MERGE supports inserts, updates and deletes; uses: **deduplication, SCD and CDC, incrementally syncing a Delta table with its source.** Cleanup: `DROP TABLE IF EXISTS abd.postal_codes` ("to have a clean space").

*Sources: ABD03 nb c62–70, 75–77; ABD07 nb c42, 81–90; #05 s14, 62; #01 s31; #03 s37.*

## 15.8 Time travel and table history

"Databricks Delta's time travel capabilities simplify building data pipelines for": **audit data changes; reproduce experiments and reports; rollbacks.** **"As you write into a Delta table or directory, every operation is automatically versioned."** Query an older state "1. Using a timestamp 2. Using a version number"; the labs use only the version:

```sql
DESCRIBE HISTORY abd.postal_codes
SELECT * FROM abd.postal_codes VERSION AS OF 2
SELECT * FROM abd.postal_codes VERSION AS OF 0
```

`DESCRIBE HISTORY` lists versions; `VERSION AS OF n` reads version `n` (0 = first). In ABD03 the versions are the creation, the UPDATE and the DELETE; ABD07 runs the same on `DL.GD_Sales_by_city` after its DML.

*Sources: ABD03 nb c71–74; ABD07 nb c91–94; #03 s36–37.*

## 15.9 Data layout: partitioning, Z-order and Liquid Clustering

The Delta paper lists layout optimization (object size, Z-order). Deck #06 presents **Liquid Clustering** among its query optimizations (Chapter 12): **"Delta Lake feature that automatically and continuously organizes data in tables, based on query patterns."**

- "uses a tree-based algorithm to optimize the physical data arrangement".
- Clusters by the `CLUSTER BY` definition, collocating similar values in the optimal set of files.
- **"Replaces the traditional methods like partitioning and z-order".**

Recommended for tables filtered by high-cardinality columns, skewed, fast-growing, with frequent concurrent writes, or with changing access patterns.

```sql
CREATE TABLE table1(col0 INT, col1 string) CLUSTER BY (col0);
```

Enable at creation or on an existing table with `CLUSTER BY`; then run `OPTIMIZE` as usual to cluster incrementally; from DBR 14.2 the DataFrame and DeltaTable APIs can also enable it.

> [!warning] **"Clustering is not compatible with partitioning or ZORDER, and requires that you use Databricks to manage all layout and optimization operations."** Choose one, not both.

delta.io blog: "more flexible and requires less compute than Z-ordering and Hive-style partitioning", without "having to rewrite existing data files". A Databricks blog (March 2025) announces **Automatic Liquid Clustering, powered by Predictive Optimization**: picks keys automatically, monitors tables, weighs gains against costs, "up to 10x faster queries".

> [!flag] **Is partitioning in or out? — resolved.** #06 s9, 53 say Liquid Clustering replaces partitioning and is incompatible with it; the summary (s60) lists "Partitioning" as a Spark SQL optimization, with no slide on it.
>
> **Resolution (external source).** Both are right: on a Delta table, Liquid Clustering replaces Hive-style partitioning/Z-order and is recommended "for all new tables" (partition only tables over 1 TB, with ≥1 GB per partition); the summary's "Partitioning" is Spark's general mechanism (in-memory/shuffle partitions, `repartition`). [Databricks: Use liquid clustering](https://docs.databricks.com/aws/en/delta/clustering); [When to partition tables](https://docs.databricks.com/aws/en/tables/partitions).

*Sources: #06 s9, 51–53, 60; #03 s36.*

## 15.10 Delta beyond batch: one table for batch and streaming

**Delta unifies batch and streaming on one copy of the data** ("a single copy of data for both batch and streaming", #03 s44; exactly-once ingestion, §15.3; "tailing" reads, §15.2). In Structured Streaming it is a sink via `format("delta")` (§18.5). Deck #08's "Unified Streaming Lakehouse" (2019): Delta, Iceberg and Hudi bring ACID to object storage, and **standard tables support simultaneous streaming writes and batch SQL reads** — "ACID storage convergence", "breaking down the wall between stream and warehouse" (§17.8).

*Sources: #03 s35–37, 44; #08 s29, 61–62.*

### Key takeaways

- Delta Lake = Parquet + transaction log (`_delta_log`), schema enforcement/evolution; open source (Linux Foundation), default on Databricks.
- The log brings ACID to cheap object stores, plus scalable metadata, time travel and audit history.
- Delta accepts `UPDATE`, `DELETE`, `MERGE` (upsert); DML is a Delta feature.
- Every write is a version: `DESCRIBE HISTORY`, `VERSION AS OF n` (audit, reproducibility, rollback).
- Liquid Clustering (`CLUSTER BY` + `OPTIMIZE`) replaces, and is incompatible with, partitioning and Z-order.
- One Delta table serves batch and streaming; Iceberg and Hudi are the other open table formats.

---

# Chapter 16: The Medallion Architecture

The Medallion architecture organizes lakehouse data in three layers of increasing quality; the class notebook builds one end to end.

## 16.1 Definition and purpose

Deck #07: "An end-to-end enterprise data foundation (Databricks 2019)". **"The Medallion Architecture is a data design pattern used to logically organize data in a Lakehouse"**, through "progressive quality tiers—commonly known as the Bronze, Silver, and Gold layers"; "each layer incrementally refines, cleans, and structures data" for analytics, BI and ML.

It solves four operational challenges: **incremental quality enforcement; replayability and auditability; lineage and tracking data origins; different types of consumers by data layers.**

"Databricks recommends taking a multi-layered approach to building a single source of truth for enterprise data products"; **"This architecture guarantees atomicity, consistency, isolation, and durability as data passes through multiple layers of validations and transformations…"**; **"Medallion architectures are sometimes also referred to as multi-hop architectures."**

*Sources: #07 s10, 12, 19–20; ABD07 nb c1–4.*

## 16.2 The three layers

From the Databricks glossary (linked in ABD07):

```flow
Batch & streaming sources → Bronze (Raw Integration) → Silver (Filtered, Cleaned, Augmented) → Gold (Business-Level Aggregates) → BI and ML
```

"The data flow is logically organized to incrementally improve the structure and quality of data to each layer"; the arrow reads "Improve Data Quality".

- **Bronze:** "Landing zone" for raw data, **no schema needed**.
- **Silver:** "Define structure, **enforce schema**, evolve schema as needed".
- **Gold:** "Deliver continuously updated, clean data to downstream users and apps".

From the *Big Book of DW & BI*:

| | Bronze | Silver | Gold |
|---|---|---|---|
| Role | "Replica of Source" | "Central Enterprise Data Repository" | "Presentation Layer" |
| Modeling | one block per source system | 3NF-like, Data Vault-like, **write-optimized** | Star Schema, Kimball, **read-optimized** |
| Content | "Mainly for landing, archiving, reprocessing and lineage purposes" | key domains (Customer, Product, Sales); business units enrich master domains "to create a Data Mesh"; master, X-REF data, replicated data marts | use cases (C360, marketing analytics, demand forecasting); departmental sandboxes; feature store, data science sandboxes |

**Bronze keeps the source as it came, Silver is the clean, detailed enterprise history, Gold is shaped for one business use.** Other wordings (slides / notebook): Bronze "Raw data" / "Raw data from data source files"; Silver "validated", "Curated data" / "Curated and normalized data"; Gold "enriched", "Summarized data" / "Aggregated data for specific use cases".

> [!flag] **What exactly goes into Bronze? — resolved.** The slides: Bronze is a "replica of source", a "landing zone" with "no schema needed", and Medallion relies on "the Delta format for data storage" (#07 s11–12, 26). ABD07 applies the data rules (nulls, duplicates, types) *before* writing Bronze, writes it in Parquet, and titles the section "Bronze file in Delta Lake" (nb c7, 25–37, 40).
>
> **Resolution (external source).** The slides are right: Bronze is raw, append-only, stored as **Delta** (fields may stay string/VARIANT to survive schema changes), with only minimal validation so no data is lost and loads can be replayed; data rules and business rules belong in **Silver** ("Data cleanup and validation are performed in silver layer"). A Parquet Bronze works but loses ACID, time travel and MERGE, and the "Delta Lake" title is inaccurate. [Databricks: What is the medallion lakehouse architecture?](https://docs.databricks.com/aws/en/lakehouse/medallion)

*Sources: #07 s11–12, 20, 26; ABD07 nb c5.*

## 16.3 Data rules vs business rules

| | Data rules | Business rules |
|---|---|---|
| Notebook marker | "This is where the "Data" rules should apply" | "This is where the "Business" rules should apply" |
| Applied in | Bronze in the notebook (good practice: Silver, §16.2) | Silver |
| Purpose | technical quality of records | "conform" data to the business model |
| Lab examples | drop nulls, drop duplicates, correct types | derived columns (product type, total with VAT), reference data (city names), drop code columns |

**Data rules fix the records; business rules give them business meaning.** "Here we should apply the business rules to "conform" our data to the Silver layer." Note the clash: type conversion is a schema decision, which the slides place in Silver ("enforce schema"); see §16.2.

*Sources: ABD07 nb c25, 35, 47–48; #07 s12.*

## 16.4 Advantages

Deck #07: simple data (architecture) design pattern; easy to understand and implement; **3 main data layers for different types of business usage; enables incremental ETL/ELT and data management;** optimal use of Big Data technologies; best of RDBMS and DSS concepts; flexible, scalable, optimized performance; open standards and cost-oriented; focus on business data needs.

*Sources: #07 s13.*

## 16.5 Design choices: formats, schemas or catalogs per layer, naming

ABD07's "Notes to consider":

1. Layers are managed "with folders in the Unity Catalog but could also be done with Databases or different Catalogs".
2. **"Parquet format will be used for Bronze layer and the Delta format for the Silver and Gold layer"**: Bronze "because it's efficient and this data is read only no need for updates"; Silver because it "will be used by many users and purposes and will require updates".
3. A new schema `DL` stores the lakehouse tables (Silver and Gold).
4. "a good alternative … is to create a schema/database for each of the data layers".
5. Some steps (e.g. `display`) are only for pipeline development.

```python
DL_Bronze_Path = "/Volumes/workspace/abd/abd/DL/Bronze/"
DL_Silver_Path = "/Volumes/workspace/abd/abd/DL/Silver/"
DL_Gold_Path = "/Volumes/workspace/abd/abd/DL/Gold/"
```

**In enterprise, one catalog per layer** ("create each layer in a differente catalog"). Prefixes: **`SV_` for Silver, `GD_` for Gold** (`DL.SV_Sales`, `DL.GD_Sales_by_city`). The slide demo uses the same design.

> [!flag] **Which notebook is "the #7 notebook"?** The demo on #07 s15 is "ABD08 Lakehouse (ABD)" with Lending Club loans ("funded loans from 2012 to 2017"); s23 says run "the class (#7) notebook", i.e. ABD07 with SalesABD.csv. The labs follow ABD07.

*Sources: ABD07 nb c7, 11, 40, 57; #07 s15.*

## 16.6 Lab — Bronze: ingest, data rules, daily Parquet folders

> [!lab] **ABD07 — Bronze.** Read daily sales of 3 cities from `SalesABD.csv` (Moodle; uploaded under `abd/abd`), apply the data rules, save in a dated folder.

**1. Set up** (after removing leftovers):

```python
spark.sql(f"DROP SCHEMA IF EXISTS workspace.DL CASCADE")
spark.sql(f"CREATE SCHEMA IF NOT EXISTS workspace.DL COMMENT 'Schema for the ABD Data Lakehouse Tables'")
```

`CASCADE` drops the schema with its contents; the commented `RESTRICT` deletes it "only if empty".

**2. Read and inspect.**

```python
raw = spark.read.option("header",True).option("sep",';').csv("/Volumes/workspace/abd/abd/salesABD.csv")
```

Inspection (`count`, `columns`, `printSchema`, `limit(5).display()`) shows "we have numeric columns that are defined as string (Quantity, Unit Price and VAT)". Columns: Date, Shop, SKU, Quantity, Unit_Price, VAT.

**3. Nulls.** Three ways to check, then drop:

```python
raw.summary().display()
raw.select([count(when(isnull(c), True)).alias(c) for c in raw.columns]).display()  # nulls per column, one pass
raw.filter("Date is null or SKU is null ").count()
raw_noNulls = raw.dropna()
```

**Date and SKU are "the keys of our sales data".**

**4. Duplicates.** `raw_noDup = raw_noNulls.dropDuplicates()` (all columns equal). Advice: "Make sure you drop duplicates only for your relevant columns". The comments suggest a subset (Date for nulls, relevant columns for duplicates), but the code uses all columns.

**5. Types.**

```python
raw_Final = raw_noDup.withColumn('Date', to_date('Date', 'dd/mm/yyyy'))
raw_Final = raw_Final.withColumn('Quantity', raw_Final.Quantity.cast('int'))
raw_Final = raw_Final.withColumn('Unit_Price', raw_Final.Unit_Price.cast('float'))
raw_Final = raw_Final.withColumn('VAT', raw_Final.VAT.cast('float'))
```

**6. Save in a dated folder.** **"This data should be able to be reprocessed with minimal efford and this is why we are adding a time-stamp and creating a folder for each day."**

```python
date_tag = datetime.datetime.now().strftime("%Y-%b-%d")
file_path_BZ_Sales = DL_Bronze_Path + date_tag + '/' + 'Sales'
raw_Final.write.format('parquet').mode('overwrite').save(file_path_BZ_Sales)
```

```flow
salesABD.csv → nulls · duplicates · types → /DL/Bronze/<YYYY-Mon-DD>/Sales (Parquet)
```

Each day's folder stays on disk, so any day can be reprocessed. Good practice differs (raw Delta Bronze; see §16.2).

*Sources: ABD07 nb c9–42.*

## 16.7 Lab — Silver: business rules, reference join, Delta append, table registration

> [!lab] **ABD07 — Silver.** Read Bronze, apply business rules, append to a Delta table.

**1. Read Bronze:** `daily_Sales = spark.read.parquet(file_path_BZ_Sales)` ("Or if the pipeline is the same, you may continue using daily_Sales = raw_Final"). In a corporate scenario you might read more raw files than the current day's; a commented line suggests an optional ELT log with row counts.

**2. Derived columns** (product type = first 3 SKU characters; total with VAT as a percentage):

```python
sales = daily_Sales.withColumn('SKU_Type', substring('SKU', 0,3))
sales = sales.withColumn('Total_with_VAT', sales.Quantity * sales.Unit_Price * (sales.VAT / 100+1) )
```

**3. Reference data:** shop code → city.

```python
Cities = spark.createDataFrame([(1,'Lisbon'),(2,'Porto'),(3,'Coimbra')], ['City_Code','City_Name'])
Sales_Silver = sales.join(Cities, sales.Shop == Cities.City_Code).drop("Shop", "City_Code")
```

In a real pipeline reference data "should already exist in your Lakehouse in a reference data specific location". Whether to drop the code columns is called debatable.

**4. Append to Delta.** **"The data on the Silver layer should represent a clean history of business facts/entities with the maximum data detail (granularity)."**

```python
file_path_SV_Sales = DL_Silver_Path + 'SV_Sales'
Sales_Silver.write.format('delta').mode('append').save(file_path_SV_Sales)
```

`append` because each day adds to the existing history (alternative: insert into an existing Silver table).

**5. Register for SQL users** (once, not every run):

```python
spark.read.format("delta").load(file_path_SV_Sales).write.saveAsTable('DL.SV_Sales')
#spark.sql(f"CREATE TABLE SV_Sales USING delta LOCATION '{file_path_SV_Sales}'")
```

"Are we creating a managed or unmanaged table?" ("Unmanaged Tables are also called External Tables"). **`saveAsTable` without a path creates a managed table** (§8.8); the external `LOCATION` version "will not run on Free Edition". Check with `spark.catalog.listTables('DL')`.

*Sources: ABD07 nb c43–60.*

## 16.8 Lab — Gold: aggregation, managed table, SQL for business users

> [!lab] **ABD07 — Gold.** Aggregate Silver for one business use and expose it to SQL.

**"Gold Tables are created to provide summarized data for a specific business unit or use case."** "Silver tables often feed multiple downstream Gold tables." This one (city, product type, products sold) could feed a BI tool.

```python
GD_Sales_0 = spark.table('DL.SV_Sales')
GD_Sales_by_city = GD_Sales_0.groupBy(col('City_Name').alias('City'),
                                      col('SKU_Type').alias('Type_Product')) \
                             .agg(sum('Quantity').alias('Quantity'))
spark.sql('drop table if exists dl.gd_sales_by_city')
GD_Sales_by_city.write.saveAsTable("DL.GD_Sales_by_city")
```

There is "no tech reason (need) to save the data first in a file in the gold layer. But again, that could be debatable"; a commented alternative writes Delta to the Gold folder and registers an unmanaged table (`USING delta LOCATION`). `%sql show tables in DL;` ≡ `spark.catalog.listTables('DL')`.

**"We'll explore the Gold layer with standard SQL free format as this is the natural option used by business users."**

```sql
SELECT City, sum(Quantity) AS Products_Sold FROM DL.GD_Sales_by_city GROUP BY City
```

The `%sql` cells at the top (c3, 6, 8) are dashboard-style outputs of this table, shown before the code that builds it.

*Sources: ABD07 nb c3, 6, 8, 61–80, 82.*

## 16.9 Lab — maintaining Gold with Delta, and cleanup

> [!lab] **ABD07 — Delta features on Gold, then cleanup.**

The notebook ends with `UPDATE`, `DELETE`, `MERGE`, `DESCRIBE HISTORY` and `VERSION AS OF` on `DL.GD_Sales_by_city` (see §15.7, §15.8). The Medallion point: **MERGE "could be usefull to manage tables in the Gold layer that are organized with a Star Schema".** Cleanup ("just for house cleaning purposes"), in order tables → folders → schema:

```python
spark.sql("drop table if exists DL.GD_Sales_by_city"); spark.sql("drop table if exists DL.SV_Sales")
dbutils.fs.rm("/Volumes/workspace/abd/abd/DL", True)
spark.sql(f"DROP SCHEMA IF EXISTS workspace.DL CASCADE")
```

*Sources: ABD07 nb c81–98.*

## 16.10 Pipeline tooling: Lakeflow

Databricks' declarative pipeline tooling is only mentioned in class:

- **Lakeflow** is the "Ingest, ETL, streaming" layer of the Databricks lakehouse (§14.6).
- Deck #07: "The New Way to Build Pipelines on Databricks: Introducing the IDE for Data Engineering", **Lakeflow Designer**: "A new developer experience purpose-built for authoring Lakeflow Spark Declarative Pipelines".
- Deck #02 tutorials: "Build an ETL pipeline (Lakeflow Declarative Pipelines)" next to "Build an ETL pipeline (Apache Spark)".
- Deck #08: Auto Loader works from Python and SQL "in Delta Live Tables" (Chapter 18).

> [!flag] **Three names for the pipeline tool.** "Delta Live Tables" (#08 s41), "Lakeflow Declarative Pipelines" (#02 s47), "Lakeflow Spark Declarative Pipelines" (#07 s22); the class does not say if they are one product.

*Sources: #07 s18, 22; #02 s47; #08 s41.*

### Key takeaways

- #07: "Apache Spark functionalities have a very good fit for data engineering and data pipelines"; Medallion "relies on the Apache Spark functionalities for data processing and the Delta format for data storage".
- Medallion (Databricks 2019, multi-hop): progressive quality tiers — **Bronze raw, Silver curated (write-optimized, 3NF/Data Vault), Gold summarized (read-optimized, star/Kimball).**
- Lab: data rules at Bronze, business rules at Silver; good practice: both in Silver, Bronze raw Delta (§16.2).
- ABD07: dated Parquet folders for Bronze (reprocessing), Delta append for Silver, managed `GD_` table for Gold queried with SQL; `SV_`/`GD_` prefixes; one schema or catalog per layer in enterprise.
- Delta's `MERGE` and time travel keep Gold maintainable.

---

# Part VI: Streaming and Graph Analytics

Part VI leaves static tables behind: first data that never stops arriving (streams), then data whose value lies in its connections (graphs).

---

# Chapter 17: Streaming Data and Streaming Architectures

So far the data was already there, complete. This chapter covers data that arrives continuously, why it needs different technology, and the architectures built to handle it.

## 17.1 What streaming data is

Streaming data is **"a new type of data processing"**: data **"generated at a high volume and speed, in a continuous and incremental manner, with the purpose of representing real-time events"**, as opposed to data at rest. Its four characteristics:

- *chronologically significant and timestamped*;
- *fast and continuously flowing*;
- *uniqueness of events, and noise*;
- *nonhomogeneous data sources and raw data*.

**"The differentiator value is when it is analyzed in near or real-time"**, which needs specific technology for continuous, incremental processing on top of Big Data technology. Typical sources: web and system logs, social media, sensors and IoT, network sockets, mobile devices and wearables. The slide asks "Is this just a complementary topic?"; the answer is no.

*Sources: #08 s4–5.*

## 17.2 Impact and use cases

**"The world is moving towards digital and real-time"**: more devices are connected to the internet than there are people, and the number grows much faster.

::: example
*Statista, active device connections (billions).* IoT: 0.8 (2010), 8 (2018), 10 (2019), 30.9 forecast (2025). Non-IoT: 8 (2010), 10 (2019), 10.3 (2025). **IoT overtakes non-IoT in 2019** (10 vs 10).
:::

Ten use cases: real-time recommendations (online engagement); monitoring critical production-line devices (industry); continuous patient monitoring (health); intrusion detection and surveillance (security); card and bank fraud detection (finance); real-time inventory (retail); traffic monitoring and dynamic routing (transport); sports analytics; threshold and anomaly detection (sensors); pattern detection in social streams. **"Real-time is the key difference".**

*Sources: #08 s6–7.*

## 17.3 Challenges of processing streams

- many small, fast inputs (streams or events);
- latency and **out-of-order data**;
- fault tolerance and (re)transmission;
- raw data (noise, quality, integrity);
- the flow and patterns matter (chronologically significant);
- you **"can't have all data ready upfront"**;
- actions are required in real time.

So large volumes must be processed on the fly, in memory and in parallel, by a running program, the *agent*, which must cope with unpredictable network latency.

> [!key] **"You have the queries but not all the data".** Batch runs queries over all the data; streaming fixes the queries while data keeps arriving. Hence **"you store the outputs, not the input raw data"**, and the agent must keep running.

A streaming system is usually a component of a larger system, so connectivity and integration are key.

> [!flag] **Storing outputs vs keeping raw history.** Slide 11: "you store the outputs, not the input raw data"; yet Kappa (§17.6) relies on log retention and replay, and Bronze keeps raw history (Ch. 16) (#08 s11, 12, 58; #07 s10–11).
>
> **Resolution (external source).** No conflict: slide 11 describes the engine, which keeps only results and minimal state; the raw input is retained upstream (Kappa log, Bronze) for reprocessing. [Structured Streaming Programming Guide](https://spark.apache.org/docs/latest/streaming/getting-started.html); [Kreps, Questioning the Lambda Architecture](https://www.oreilly.com/radar/questioning-the-lambda-architecture/).

*Sources: #08 s9, 11.*

## 17.4 Batch vs streaming

| | Batch | Streaming |
|---|---|---|
| Data scope | all the data in the dataset | most recent data or a rolling time window |
| Data size | large datasets | individual records or micro-batches of a few records |
| Latency ("time taken for the data to be received and processed") | a few hours | seconds or milliseconds |
| Analysis | on-demand, complex | continuous, simple: filters, aggregates, rolling averages |

**The trade-off is depth vs speed.**

*Sources: #08 s10.*

## 17.5 The stream processing engine: source, agent, sink

A streaming data processing engine (**ESP** or **CEP**) must process large volumes in real time, offer low latency and high scalability, be fault tolerant and reliable, and integrate with on-line sources and RDBMS. Its three roles recur through Part VI:

```flow
Source (ingestion layer: transport) → Agent (processing layer: computation) → Sink (serving layer: delivery + storage layer: state and persistence)
```

The class compares six engines (2016 Databricks blog):

| Property | Structured Streaming | Spark Streaming | Storm | Flink | Kafka Streams | Google Dataflow |
|---|---|---|---|---|---|---|
| Streaming API | incrementalize batch queries | integrates with batch | separate | separate | separate | integrates with batch |
| Prefix integrity guarantee | yes | yes | no | no | no | no |
| Internal processing | exactly once | exactly once | at least once | exactly once | at least once | exactly once |
| Transactional sources/sinks | yes | some | some | some | no | no |
| Interactive queries | yes | yes | no | no | no | no |
| Joins with static data | yes | yes | no | no | no | no |

**Structured Streaming "incrementalizes" ordinary batch queries** instead of offering a separate streaming API (Ch. 18).

*Sources: #08 s13–14.*

## 17.6 Architectures: batch-first, Lambda, Kappa

**"Streaming evolved from faster paths to explicit time semantics"**, in four stages; the fourth is §17.7.

| Architecture | Shape | How it works | Trade-off |
|---|---|---|---|
| **Batch-first** | DATA → BATCH → VIEW | scheduled recomputation over full history gives a trusted view | **correct, but delayed** |
| **Lambda** | batch + speed layer | batch (cold) layer: exact view; speed (hot) layer: fast view; queries combine both | **low latency, but duplicated logic** |
| **Kappa** | LOG → STREAM → VIEW | one stream path for live events and historical replay; distributed, highly scalable | **simpler, but relies on retention + replay** |

> [!compare] **Lambda:** dual pipeline; speed layer low-latency but approximate, batch layer high-latency but exact. Drawback: **"Dual codebase maintenance and data divergence".** **Kappa:** single stream replaying an immutable log (Apache Kafka) through one processor. Advance: **"Unified codebase for real-time and historical replay".** (#08 s58)

The general batch + real-time big data architecture is in §14.7. References: Marz (Lambda); Kreps, "Questioning the Lambda Architecture" (2014); Akidau et al., "The Dataflow Model" (PVLDB 2015). Take-away reading: Feick, Kleer and Kohn, "Fundamentals of Real-Time Data Processing Architectures Lambda and Kappa" (SKILL 2018), comparing both with a case study.

*Sources: #08 s12, 36, 58.*

## 17.7 The Dataflow model: event time, watermarks, triggers

The **Dataflow Model** (Google), "widely considered the most influential modern streaming paper", gives streaming *explicit time semantics*: data is unbounded and out of order, so the engine must reason about time. Five ideas:

1. **Bounded vs unbounded data.** Classify datasets as *bounded* (historic, finite) or *unbounded* (continuous, infinite) instead of treating batch and streaming as different.
2. **Event time vs processing time.** Process by **event time** (when it occurred), not **processing time** (when ingested), to handle delays and out-of-order data.
3. **Watermarks.** A **"progressive, system-wide metric to estimate progress and handle out-of-order data, defining when a system can assume it has seen all data up to a specific timestamp"**.
4. **Triggers and windowing.** Separate *what* is computed (transformations, windows) from *when* it is materialized (triggers): WHAT → transforms; WHERE → event-time windows; WHEN → watermarks + triggers; HOW → accumulation modes. The choice is always **correctness vs latency vs cost**.
5. **Unifying batch and streaming.**

> [!key] **"Batch processing is merely a specialized, bounded subset of stream processing."** (Akidau et al., VLDB 2015). Groundwork for unified engines (Apache Flink, Apache Beam).

Milestone slide: results by event time; watermarks measure completeness; triggers and lateness define when window results materialize and update.

*Sources: #08 s12, 38–39, 59.*

## 17.8 Evolution of streaming systems: six milestones and three lessons

"From early Complex Event Processing to the unified event-driven Lakehouse":

| # | Period | Milestone | What it brought |
|---|---|---|---|
| 1 | 2000s | **Early CEP**, streaming DBs (Esper, Aurora) | static SQL rule matching over event streams, in memory, single node. **Could not scale horizontally** |
| 2 | 2011–13 | **Distributed micro-batching** (Storm, Spark Streaming DStreams) | horizontal scale; streams discretized into small batches; high throughput, deterministic recovery via MapReduce mechanics |
| 3 | 2011–14 | **Lambda & Kappa** | dual-layer pipelines, log replay (§17.6) |
| 4 | 2015 | **Dataflow Model** | decoupled event time, watermarks (§17.7) |
| 5 | 2015–17 | **Stateful Flink** | asynchronous state, exactly-once |
| 6 | 2019+ | **Unified Lakehouse** | streaming tables over ACID storage |

Milestone 5: *managed local state* (embedded RocksDB, aggregations over billions of keys, zero remote lookup latency); *Chandy-Lamport checkpoints* (asynchronous state-barrier snapshots without stopping the flow); *true exactly-once* via Two-Phase Commit sinks (Flink, Spark Structured Streaming).

Milestone 6, **"The Unified Streaming Lakehouse"**: *ACID storage formats* (Delta Lake, Iceberg, Hudi) on cloud object storage (Ch. 15); *unified table semantics*, simultaneous streaming writes and batch SQL reads (§15.10); *streaming Auto Loader*, continuous ingestion with incremental, automatic schema evolution (§18.11).

> [!key] **"Key Lessons from 20 Years of Evolution".** **1. Event time is mandatory** (with watermarks). **2. One engine for all data:** separate batch and speed layers add complexity. **3. ACID storage convergence:** Delta and Iceberg make storage streaming-native, **"breaking down the wall between stream and warehouse"**.

Take-away survey: Fragkoulis, Carbone, Kalavri and Katsifodimos, "A survey on the evolution of stream processing systems" (VLDB Journal 2024): first ('00–'10) and second ('11–'23) generation systems.

*Sources: #08 s37, 55–62.*

### Key takeaways

- Streaming data is continuous, high-volume, timestamped; its value comes from near/real-time analysis.
- "You have the queries but not all the data": the agent keeps running; data arrives late and out of order.
- Batch: all data, hours of latency, complex analysis; streaming: recent data, s/ms, simple analysis.
- Lambda = low latency but duplicated logic; Kappa = one replayable stream, relies on retention.
- Dataflow: event time, watermarks, triggers vs windows; "batch is a bounded subset of streaming".
- Evolution ends in one engine, event time and ACID table formats (unified lakehouse).

---

# Chapter 18: Spark Structured Streaming

Structured Streaming lets you write a streaming job with the DataFrame API of Part IV.

## 18.1 Overview and engine internals

**"Apache Spark Structured Streaming is a scalable and fault-tolerant stream processing engine"**:

- since **Spark 2.0**, **built on top of the Spark SQL library** (a separate engine based on DataFrames);
- **"Streams are unbounded DataFrames - Streaming DataFrames"** (§18.2);
- **micro-batch processing** plus a low-latency **continuous processing** mode.

**"Spark continuously reads new data arriving from a stream, processes it incrementally, and updates the final results (sinks)."** You **express a streaming computation the same way as a batch computation on static data**; Spark SQL runs it incrementally (API in Scala, Java, Python, R: streaming aggregations, event-time windows, stream-to-batch joins). Selling points: *easy to use* (abstracts incremental processing, checkpointing, watermarks); *unified batch and streaming APIs* (no second stack, easy migration); *low latency and cost effective* (same architecture as Spark).

**Inside the engine** (SIGMOD paper "Structured Streaming: A Declarative API for Real-Time Applications in Apache Spark"):

```flow
input streams + Spark tables → Incrementalizer → Optimizer → micro-batch execution or continuous processing → output sink
```

Underneath sit a **log** and a **state store**. The user submits an ordinary query, e.g. `data.where($"state" === "CA").groupBy(window($"time","30s")).avg("latency")`; the incrementalizer turns it into a streaming job.

| | Micro-batch (default) | Continuous processing (since Spark 2.3) |
|---|---|---|
| How | "a series of small, timed batch jobs (e.g., every 1 second)" | low-latency mode |
| Latency | as low as 100 ms | as low as 1 ms |
| Guarantee | exactly-once | at-least-once |
| Limits | | "restricted support for certain operations and windows" |

The mode is chosen without changing the DataFrame operations. Databricks: **"a near real-time processing engine that offers end-to-end fault tolerance with exactly-once processing guarantees using familiar Spark APIs"**.

> [!flag] **Latency figures.** Slide 19 (programming guide): continuous "as low as 1 millisecond", micro-batch "as low as 100 milliseconds"; the notebook says "sub-millisecond" and micro-batches "every 1 second" (#08 s19; ABD08 nb c2).
>
> **Resolution (external source).** The guide is right; nothing official supports "sub-millisecond", and "every 1 second" is just an example trigger interval. Since Spark 4.1 a Real-time Mode (`Trigger.RealTime`, a few ms) is meant to replace Continuous Processing (medium confidence). [Structured Streaming overview](https://spark.apache.org/docs/latest/streaming/index.html); [Real-time Mode](https://spark.apache.org/docs/latest/streaming/real-time-mode.html).

*Sources: #08 s16–19, 40; ABD08 nb c1–2.*

## 18.2 Programming model: the unbounded table

**"DataFrames and Datasets can represent static, bounded data, as well as streaming, unbounded data"**: a stream is a table that never ends.

> [!key] **"New data in the data stream = new rows appended to an unbounded table".**

So everything in Chapters 10–11 applies. A query over the input table produces a **Result Table**, updated at every trigger and written to the sink (§18.5–18.7).

*Sources: #08 s21; ABD08 nb c2.*

## 18.3 Structure of a streaming program

Three steps, matching source → agent → sink (§17.5):

| Step | What happens | API |
|---|---|---|
| 1. Receiving | create the input streaming DataFrame | `readStream` (§18.4) |
| 2. Transforming | the query logic | DataFrame operations |
| 3. Pushing out | query name and output to external systems | `writeStream` (§18.5) |

> [!warning] **The agent must always be running.** It does not finish like a batch job: you `start()` and `stop()` it (§18.9).

The notebook's word count follows it: text stream from a folder → split/group/count → console in the description, memory table in the code (§18.13).

*Sources: #08 s30; ABD08 nb c21.*

## 18.4 Reading streams: readStream and input sources

`spark.readStream` returns a **DataStreamReader**, configured like `spark.read` (Ch. 9):

```python
streamingDF = (spark.readStream.format("socket")
    .option("host", "localhost").option("port", port_number).load())
```

| Source | Reads | Use |
|---|---|---|
| **File** | files written into a directory: `.json()`, `.csv()`, `.parquet()`, `.text()`… | labs (§18.13–18.15) |
| **Kafka** | Kafka topics | — |
| **Socket** | UTF-8 text from a socket | **testing only** |
| **Rate** | rows at a given rate, each with `timestamp` and `value` (a sequence number) | **testing only**: "testing, benchmarking, and prototyping … without needing an external data source" |

Transformations (rename, drop, select, filter) are written as on a static DataFrame. `isStreaming` checks for a stream (`streamingCountsDF.isStreaming  # True`, §18.15).

*Sources: #08 s22–23, 46; ABD08 nb c12–13, 23, 48–50.*

## 18.5 Writing streams: the query and output sinks

`DataFrame.writeStream` returns a **DataStreamWriter**. The class calls its configuration the **query**: **"the query that is continually being executed over streaming data"**.

| Parameter | Meaning |
|---|---|
| Query name (optional) | unique among active queries; the table name for the memory sink |
| Trigger (optional) | when to process next; default `ProcessingTime(0)`, "as fast as possible" (§18.7) |
| Checkpoint directory | where progress is stored (§18.8) |
| Output mode | `append`, `update`, `complete` (§18.6) |
| Output sink + options | e.g. Kafka host/port/topic; file format/destination; custom `foreach` |
| `.start()` | launches the job |

```python
query = (inputDF.writeStream
    .format("parquet").option("path", output_path)
    .option("checkpointLocation", checkpoint_path)
    .outputMode("append").queryName("query_name")
    .trigger(processingTime='10 seconds')
    .start())
```

| Sink | Code | What it does |
|---|---|---|
| File | `format("parquet")`, `"csv"`… | dumps the Result Table to files |
| Kafka | `format("kafka")` | writes to Kafka topics |
| Console | `format("console")` | prints; **debugging** |
| Memory | `format("memory")` | **in-memory table queryable with Spark SQL**; labs |
| foreach | `foreach(writer)` | "escape hatch" for a custom sink |
| Delta | `format("delta")` | Delta table (§15.10) |

Slide 29 calls Delta "A proprietary sink"; see the flag in §15.1 (Delta Lake is open source).

*Sources: #08 s24–25, 29.*

## 18.6 Output modes

The output mode decides **which Result Table rows are written to the sink at each trigger**.

| Mode | Writes |
|---|---|
| **Append** (default) | only **new rows** since the last trigger |
| **Complete** | the **entire updated Result Table** (the sink decides how) |
| **Update** (since 2.1.1) | only rows **updated** since the last trigger |

**Update outputs only changed rows; without aggregations it equals Append.** In the labs, non-aggregating streams (§18.12, §18.14) use append; running aggregations (§18.13, §18.15) use complete, so "all the counts should be in the table".

*Sources: #08 s26, 47; ABD08 nb c14, 25, 56.*

## 18.7 Triggers and processing modes

**"The trigger specifies when the system should process the next set of data."**

| Trigger | Frequency | Suits | Example |
|---|---|---|---|
| Default | "continuous" (as fast as possible) | low-latency processing | fraud detection, live dashboards |
| `processingTime` | fixed intervals | predictable scheduled load | metrics every 2 minutes |
| `availableNow` | once, batch-like | historical/catch-up | migrating historical data |

Fixed interval: `.trigger(Trigger.ProcessingTime("3 seconds"))`; Python: `.trigger(processingTime='10 seconds')`.

> [!flag] **"Default: continuous" is not Continuous Processing.** Slide 24 defines the default as `ProcessingTime(0)`; slide 27 calls it "continuous frequency", easily confused with the at-least-once Continuous Processing mode (§18.1) (#08 s16, 19, 24, 27).
>
> **Resolution (external source).** The default is back-to-back micro-batches; Continuous Processing must be requested explicitly (`trigger(continuous="1 second")`). [Structured Streaming: triggers](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html).

**The path to real time:** "Latency goes subsecond in Apache Spark Structured Streaming" (2023, offset-management improvements in Project Lightspeed); "Introducing Real-Time Mode…" (19 Aug 2025): **"Process events in milliseconds using Spark's new Real-Time trigger"**.

*Sources: #08 s16, 19, 24, 27, 50–51.*

## 18.8 Checkpointing and fault tolerance

**Checkpointing stores the streaming job's state on reliable storage (filesystem or database)**, so a terminated stream restarts and **continues from where it stopped**. The class says it **"ensures end-to-end exactly-once fault-tolerance guarantees"**; the guide adds "through checkpointing and Write-Ahead Logs". Same idea family as Chandy-Lamport snapshots and 2PC sinks (§17.8).

```python
.option("checkpointLocation", checkpointPath)
```

> [!flag] **Exactly-once depends on the mode.** Slide 28, slide 40 and the notebook say exactly-once; slide 19 gives Continuous Processing only **at-least-once** (#08 s14, 19, 28, 40; ABD08 nb c2).
>
> **Resolution (external source).** End-to-end exactly-once needs micro-batch + replayable source + checkpoint/WAL + idempotent sink (File, Delta; Kafka and `foreach` are at-least-once; console/memory not fault-tolerant). Continuous = at-least-once. [Programming Guide: fault-tolerance semantics](https://spark.apache.org/docs/latest/streaming/getting-started.html).

*Sources: #08 s19, 28, 60; ABD08 nb c10, 41, 56.*

## 18.9 Managing running queries

`start()` returns "a handle to the streaming query that is running in the background".

| Code | Does |
|---|---|
| `spark.streams.active` | list of active queries |
| `query.id`, `.runId`, `.name`, `.isActive` | identity and state |
| `query.status` | "shortcut to check the status of the query" |
| `query.stop()` / `.awaitTermination()` | stop / wait for the end |
| `query.explain()` | query plan |

```python
for q in spark.streams.active:
    print(q.id, q.name, q.isActive)
```

**Every lab ends by stopping its query** (`rate_query.stop()`, `wc_query.stop()`, `strmDF_query.stop()`).

> [!warning] **On Free Edition, use `query.stop()`.** Slide 30's `spark.sparkContext.stop()` is unsupported on serverless (§6.8).

*Sources: #08 s25, 30, 47; ABD08 nb c16–18, 32–34, 60–61.*

## 18.10 Windows, joins and SQL on streams

**Event-time windows** group the stream into intervals: **tumbling** (fixed-size, non-overlapping, contiguous), **sliding** (overlapping, contiguous), **session** (dynamic length). The slide lists `window(w_length, slide_interval)` (new streaming DataFrame over windowed batches), `countByWindow(w_length, slide_interval)` and `reduceByWindow(func, w_length, slide_interval)`. The demo uses `groupBy(action, window(time, "1 hour")).count()` (§18.15).

> [!flag] **Two APIs on one slide.** `countByWindow`/`reduceByWindow` come from the older DStream API (milestone 2, §17.8) (external check); Structured Streaming uses `window()` in `groupBy` (#08 s31).

**Joins:** `Join_output = StrmDF1.join(StrmDF2, "join_type")`; joins with static data are also supported (§17.5). **SQL:** register a temp view (Ch. 8):

```python
StrmDF.createOrReplaceTempView("New_Strm")
spark.sql("select count(*) from New_Strm")
```

*Sources: #08 s31–32, 45–46.*

## 18.11 Auto Loader

Databricks' streaming file ingestion: it **"incrementally and efficiently processes new data files as they arrive in cloud storage without any additional setup"**. A Structured Streaming source called **`cloudFiles`**; processes new files in a directory (optionally existing ones too); Python and SQL in Delta Live Tables; **billions of files** for migration/backfill and **millions of files per hour** near real time; incremental, automatic schema evolution (§17.8).

```python
spark.readStream.format("cloudFiles").option("cloudFiles.format", "json").load("/input/path")
```

*Sources: #08 s41, 61.*

## 18.12 Lab: rate source and memory sink

> [!lab] **ABD08 Colab notebook, cells 3–18.** Generated rows, kept in memory, queried with SQL (Colab, §4.8).

#### 1. Prepare the environment

Build the SparkSession (§6.9) and three folders: input (`/content/streaming_input`, `file_path`), output (`/content/streaming_output`, `file_output`) and checkpoint (`/content/streaming_cp_location`, `file_cp_location`), each with `!mkdir -p` / `!rm -rf …/*`.

```python
spark = SparkSession.builder.appName("ABD_Streaming_Example").getOrCreate()
```

#### 2. Read a rate stream

```python
rate_stream_df = spark.readStream.format("rate").option("rowsPerSecond", 1).load()
```

#### 3. Write it to a memory sink

```python
rate_query = (rate_stream_df.writeStream.format("memory")
    .queryName("q_rate").outputMode("append").start())   # query name = table name
```

#### 4. Query the table, several times

```python
spark.sql("SELECT * FROM q_rate ORDER BY timestamp DESC").show(10, truncate=False)
```

**Re-running shows a new row per second**: the agent runs in the background.

#### 5. Monitor and stop

`spark.streams.active`, `rate_query.status`, `rate_query.stop()`.

*Sources: ABD08 nb c3–18.*

## 18.13 Lab: streaming word count

> [!lab] **ABD08 notebook, cells 19–35.** The word count of §13.3 as a stream: each new text file updates the counts.

#### 1. Source: a text stream on a folder

```python
lines = spark.readStream.format("text").load(file_path)   # one column: value
```

#### 2. Transformation: split, explode, count

```python
wc = lines.select(explode(split(lines.value, " ")).alias("word")).groupBy("word").count()
```

#### 3. Sink: complete mode, memory table

```python
wc_query = (wc.writeStream.outputMode("complete")
    .queryName("q_wc").format("memory").start())
```

**Complete, because the counts are a changing aggregation** (§18.6).

#### 4. Feed the stream and watch the result grow

`spark.sql("select * from q_wc").show()` starts blank; "The Streaming agent keeps running and checking the /content/streaming_input for new data". Then:

```
!echo "Hello to Spark Streaming" > /content/streaming_input/file1.txt
!echo "Hello to Spark Streaming in ABD" > /content/streaming_input/file2.txt
!echo "Hello to ABD 2026" > /content/streaming_input/file3.txt
```

**After each file and a few seconds, the counts grow.** (No stored outputs.)

#### 5. Stop and clean up

`wc_query.status`, `wc_query.stop()`, `status` again, `!rm -rf /content/streaming_input/*`.

*Sources: ABD08 nb c19–35.*

## 18.14 Lab: a file pipeline to Parquet with a checkpoint

> [!lab] **ABD08 notebook, cells 36–61.** A producer writes small files; the stream parses, filters and saves them as Parquet with a checkpoint.

```flow
producer (randomapi) → text files "timestamp;name;flag" → readStream text → split/getItem → where flag == 'Yes' → Parquet sink (append + checkpoint) → batch read of the output
```

#### 1. A producer that writes one-line files

Calls `https://randomapi.dev/api/users?count=1&unwrap=true`, keeps `firstName`, and writes one file per call with the line `timestamp;firstName;Yes` (or `;No` in a second cell). Input/output folders are cleared and the checkpoint folder recreated empty "to avoid Streaming reading errors".

```python
word = requests.get(url_w).json()[0]['firstName']
date_tag = datetime.datetime.now().strftime("%Y-%b-%d_%H-%M-%S")
line = date_tag + ';' + word + ';' + 'Yes'
```

#### 2. Read the folder as a stream

```python
strmDF_raw = spark.readStream.format("text").load(file_path)
```

#### 3. Parse and filter

```python
strmDF = strmDF_raw.withColumn('timeStamp', split('value', ';').getItem(0)) \
     .withColumn('tag', split('value', ';').getItem(1)) \
     .withColumn('flag', split('value', ';').getItem(2)).drop('value')
strmDF_final = strmDF.where("flag == 'Yes'")
```

#### 4. Write to Parquet with a checkpoint

```python
strmDF_query = (strmDF_final.writeStream.queryName("q_tags")
    .outputMode("append").format("parquet")
    .option("path", file_output)
    .option("checkpointLocation", file_cp_location).start())
```

**Append, because there is no aggregation.** The file sink needs `path` and `checkpointLocation`.

#### 5. Read the output as a batch

`spark.read.parquet(file_output).show()`, wrapped in `try/except` for "Unable to infer schema" (no files yet); then `status` and `stop()`. **A streaming writer and a batch reader share the same files.**

*Sources: ABD08 nb c36–61.*

## 18.15 Worked example: the Databricks events demo (windowed counts)

The slides' take-away example is the Databricks notebook "Structured Streaming Demo in Python": running and windowed counts over timestamped "Open"/"Close" actions. **The streaming query is "pretty much the same" as the batch query.**

> [!flag] **Which example is the class example?** Slides 2 and 34 announce a Colab "Structured Streaming example with text input streams" (the ABD08 notebook, §18.12–18.14); the slides then show this Databricks JSON demo (Spark 2.x) (#08 s2, 34, 42–48).

::: example
*Data.* `/databricks-datasets/structured-streaming/events/file-N.json`, lines like `{"time":1469501509,"action":"Open"}`.

**1. Batch first**, with an explicit schema:

```python
jsonSchema = StructType([StructField("time", TimestampType(), True),
                         StructField("action", StringType(), True)])
staticInputDF = spark.read.schema(jsonSchema).json(inputPath)
staticCountsDF = staticInputDF.groupBy(staticInputDF.action,
                                       window(staticInputDF.time, "1 hour")).count()
```

As a temp view `static_counts`, `sum(count)` by action gives about 50k Close and 50k Open.

**2. The same query as a stream**, one file at a time:

```python
streamingInputDF = (spark.readStream.schema(jsonSchema)
    .option("maxFilesPerTrigger", 1).json(inputPath))
streamingCountsDF = streamingInputDF.groupBy(streamingInputDF.action,
                                             window(streamingInputDF.time, "1 hour")).count()
```

Only `read` → `readStream` and `maxFilesPerTrigger` changed.

**3. Start into a memory table, complete mode** (memory table "for testing purpose only"):

```python
spark.conf.set("spark.sql.shuffle.partitions", "2")   # keep shuffles small
query = (streamingCountsDF.writeStream.format("memory")
    .queryName("counts").outputMode("complete").start())
```

**4. Query it as it grows.** After `sleep(5)`, `select action, date_format(window.end, "MMM-dd HH:mm") as time, count from counts order by time, action` shows about 1.0k actions per one-hour window (Jul-26 03:00 to Jul-28 04:00). **Re-running later shows more windows.**
:::

*Sources: #08 s2, 34, 42–48.*

### Key takeaways

- Structured Streaming: write a batch-style query on Spark SQL; Spark runs it incrementally.
- A stream is an unbounded table: new data = new rows appended.
- `readStream` → DataFrame ops → `writeStream` + `start()`; runs until `stop()`.
- Query = sink + output mode (append/complete/update) + trigger + checkpoint.
- Micro-batch (default, ~100 ms, exactly-once with replayable source + checkpoint + idempotent sink) vs continuous (~1 ms, at-least-once).
- Auto Loader (`cloudFiles`) is Databricks' incremental file-ingestion source.

---

# Chapter 19: Graph Concepts

Graph data is about how things connect. This chapter gives the vocabulary and the questions graphs answer; Chapter 20 analyzes them with Spark.

## 19.1 What graphs are

Graphs are also called networks: **"graph theory is the study of graphs, which are mathematical structures used to model pairwise relations between objects"** (Wikipedia, quoted in class). A graph has **vertices** (nodes, points) and **edges** (links, lines; Databricks adds **relationships or arcs**). The notebook: **"Nodes or vertices are the units while edges are the relationships that are defined between those."**

> [!compare] **A — Undirected:** edges link two vertices *symmetrically* (a shared border, a friendship). **B — Directed:** edges go *asymmetrically* from a start node to an end node (a follower, a web link).

**Origin.** Euler's "Seven Bridges of Königsberg" (1736): 4 main areas joined by 7 bridges — "Can you cross each bridge only once and return to your starting point?" His insight: **"the only relevant data is the main areas and the bridges connecting them"**. He abstracted the city to 4 nodes and their relationships, and created "generalized rules based on nodes and relationships that apply to any connected system."

Other class examples: the Lisbon metro ("The Lisbon Tube Network"); Databricks' six western/central European countries, with an edge per shared border.

*Sources: #09 s3, 5–6, 55; ABD09 nb c1–3.*

## 19.2 Where graphs appear: use cases

**"The world is driven by elements and interactions/connections"**: sociology, biology, physics, medicine, communications, transport, energy, computer science. Why analyze them: **"Graph algorithms provide one of the most potent approaches to analyzing connected data because their mathematical calculations are specifically built to operate on relationships"**; and relationships bring new insights (data structures, patterns).

| Domain | Slide example |
|---|---|
| Infrastructure | route finding: the shortest of three alternative routes |
| Infrastructure | air transport: 3,200 airports, 60,000 routes, hub-and-spoke structures |
| Social | gaming community: 382 communities, 5 of which connect up to 50% of the audience |
| Biology | ADRB2 gene, cancer-specific genes and a drug (propranolol) |
| Computing | Spark's DAG (§19.3) |
| Computing | Google Knowledge Graph: tripled in seven months after launch (570M entities, 18B facts); May 2020: 500B facts on 5B entities |

Also: influencers, vulnerable components, best path, fraud detection, customer and disease behavior, transport cost/route optimization, weather, web click paths, sequences in logistics or computing. The notebook adds central people in a social network, most-referenced papers, shortest path between two nodes, ranking web pages. Databricks adds fraud/threat detection and product recommendation, and notes graphs are powerful combined with ML.

*Sources: #09 s4, 7–13, 55; ABD09 nb c2.*

## 19.3 Spark's DAG as a graph

You have met a graph already (§7.8): **"In Spark, a job is associated with a chain of a Data Object (RDD/Dataset) dependencies organized in a directed acyclic graph (DAG)"**. Vertices = the data objects (RDDs/Datasets); edges = the operations applied to them.

::: example
*Word count in the Spark UI* ("Details for Job 0", SUCCEEDED, 2 stages): Stage 0 `textFile → flatMap → map`; Stage 1 `reduceByKey`, after a shuffle (ShuffledRDD). See §13.3.
:::

*Sources: #09 s11.*

## 19.4 Graph databases vs relational databases

Graph databases shown: TigerGraph, Neo4j, JanusGraph ("Distributed, open source, massively scalable graph database"), plus one illegible on the slide.

| | Graph database | Relational database |
|---|---|---|
| Structure | nodes and edges, properties on both | tables with rows and columns |
| Relationships | **direct connections (pointers)**, native | **foreign keys** + **joins** |
| Schema | **flexible** (schema-less/evolving) | **rigid, predefined**; complex migrations |
| Query performance | **fast, predictable multi-hop traversal**, roughly constant as data grows | "degrades exponentially as the number of joins (depth of relationships) increases" |
| Algorithms/visualization | pathfinding, centrality, community detection; visualization | SQL-like queries only |
| Intuition | "closely maps to how humans often think about real-world networks" | complex SQL for simple traversals |

**A graph database follows pointers; a relational database recomputes the link with a join every time.**

*Sources: #09 s14–15.*

## 19.5 Terminology and types of graphs

**"Graphs are defined with Vertices and Edges"**, written as two sets. A triangle:

```math
\begin{aligned}
V &= \{a, b, c\} \\
E &= \{\{a,b\}, \{b,c\}, \{c,a\}\}
\end{aligned}
```

\(V\) is the set of vertices; \(E\) the set of edges, each a pair of vertices. Graph type affects algorithms (table from Needham & Hodler, *Graph Algorithms*):

| Attribute | Key factor | Algorithm consideration |
|---|---|---|
| Connected vs disconnected | a path between any two nodes? | "Islands of nodes can cause unexpected behavior, such as getting stuck in or failing to process disconnected components." |
| Weighted vs unweighted | (domain) values on relationships or nodes? | "Many algorithms expect weights…" — big differences in performance and results if ignored |
| Directed vs undirected | start and end node? | direction "adds rich context to infer additional meaning"; some algorithms use one, both or no direction |
| Cyclic vs acyclic | paths that return to their start? | algorithms must store traversal state "or cycles may prevent termination"; acyclic graphs (spanning trees) underlie many algorithms |
| Sparse vs dense | ratio of relationships to nodes | extremes "can cause divergent results"; data modeling may help |

Shape gallery: directed/undirected graphs, complete graph K6, directed tree, stars K1,5, 6-cycles, path P5.

*Sources: #09 s17–19, 61.*

## 19.6 Questions graph analytics answers: path, centrality, community

| Family | Theme | Question |
|---|---|---|
| **Path** | propagation pathways | **"How do things spread?"** |
| **Centrality** | flow and influence | **"What are the capacities, costs, and control points?"** |
| **Community** | interactions and resiliency | **"How do things interact and will that change?"** |

These are the same three algorithm families named in §19.4.

*Sources: #09 s15, 20.*

### Key takeaways

- A graph = vertices (nodes) + edges (links, relationships, arcs), directed or undirected.
- Graph theory began with Euler's Königsberg bridges (1736).
- Graph databases store relationships as pointers (fast multi-hop); relational databases need joins that slow with depth.
- Graph type (connected, weighted, directed, cyclic, sparse/dense) changes algorithm behavior.
- Graph analytics answers path, centrality and community questions.

---

# Chapter 20: Graph Analytics with GraphFrames

This chapter does graph analysis in Spark with GraphFrames: building, querying, pattern finding and the classic algorithms, including PageRank.

## 20.1 Spark graph libraries: GraphX vs GraphFrames

Spark offers **"Graph analysis and graph-parallel computation"** through two libraries:

| | GraphX (legacy) | GraphFrames |
|---|---|---|
| Part of Spark? | yes | **"Not Spark itself, it's a separate package"** |
| Languages | Scala | Scala, Python, Java ("GraphX extended") |
| Abstraction | **Property Graph**: RDD extension, directed multigraph with user objects on vertices/edges | **DataFrame graphs**, with Catalyst and Tungsten (Chapter 8) |
| Offers | graph operators and algorithms | GraphX + "motif finding, DataFrame-based serialization, and highly expressive graph queries" |

GraphFrames: a Databricks / UC Berkeley / MIT collaboration; "a package for Apache Spark that provides DataFrame-based graphs" with Java, Python and Scala APIs (graphframes.io, GitHub). **The course uses GraphFrames from Python.**

> [!flag] **Who makes GraphFrames?** Slide 22 says "a separate package (GitHub – neo4j)" and "Collaboration between Databricks, UC Berkeley & MIT"; slide 61 just "GitHub". The neo4j attribution is doubtful (external check).

*Sources: #09 s22–26, 56–57, 61.*

## 20.2 Setting up GraphFrames

**GraphFrames is not in the Databricks Free Edition**, so the class uses Google Colab (§4.8). On Databricks, use Databricks Runtime for ML ("includes an optimized installation of GraphFrames"), or load the JAR to a Volume and install it on the cluster. In Colab, it is both a Python and a Spark package:

```python
!pip install graphframes-py==0.12.3   # "latest stable release of GraphFrames for Spark 4.x"
spark = SparkSession.builder.appName("ABD_Graph_Analytics") \
    .config("spark.jars.packages", "io.graphframes:graphframes-spark4_2.13:0.12.3") \
    .getOrCreate()
from graphframes import GraphFrame
```

> [!warning] **Spark packages go in at session start.** Unlike `pip` packages, GraphFrames "needs to be explicitly added to your SparkSession"; Spark packages are "included at the time the SparkSession is initialized."

> [!flag] **Where does it run?** Slide 22: "Included in Databricks RunTime for ML and Colab"; slide 56: "not available in Free Edition"; an agenda says "Databricks Graph Analytics examples", slide 37 "We'll use Colab". In practice: Colab, not Free Edition.

*Sources: #09 s21–22, 37–39, 56; ABD09 nb c4–13.*

## 20.3 Creating a GraphFrame

**"A GraphFrame is created from a Vertices DataFrame and an Edges DataFrame"**: `GraphFrame(verticesDF, edgesDF)`.

> [!key] **Required columns.** Vertices: **`id`** (unique). Edges: **`src`** and **`dst`**. Other columns are attributes. **GraphFrames treats every graph as directed by default.**

The class example (7 people, 8 edges), used throughout:

```python
vertices = spark.createDataFrame([
  ("a", "Alice", 34), ("b", "Bob", 36), ("c", "Charlie", 30),
  ("d", "David", 29), ("e", "Esther", 32), ("f", "Fanny", 36),
  ("g", "Gabby", 60)], ["id", "name", "age"])
edges = spark.createDataFrame([
  ("a","b","friend"), ("b","c","follow"), ("c","b","follow"), ("f","c","follow"),
  ("e","f","follow"), ("e","d","friend"), ("d","a","friend"), ("a","e","friend")
], ["src", "dst", "relationship"])
g = GraphFrame(vertices, edges)
```

```flow
a → b ⇄ c ← f ← e → d → a,  a → e,  g alone
```

**b and c follow each other** (the only reciprocal pair); a, e, d form a "friend" loop; **Gabby has no edges**. Printing shows both schemas: `GraphFrame(v:[id: string, name: string ... 1 more field], e:[src: string, dst: string ... 1 more field])`.

> [!trap] **No built-in graph picture.** "Spark doesn't have a visualization function for GraphFrame objects": `g.show()` does not run; inspect `g.vertices` / `g.edges`.

*Sources: #09 s27–28, 40–41; ABD09 nb c14–19.*

## 20.4 Basic queries: vertices, edges, degrees, filter, groupBy

`g.vertices` and `g.edges` are DataFrames, so **any DataFrame query (Chapter 10) runs on them**, and **`g.vertices.filter` / `g.edges.filter` return DataFrames**, not graphs. GraphFrames adds `g.degrees` (edges per vertex), `g.inDegrees` (pointing **in**) and `g.outDegrees` (pointing **out**).

```python
g.vertices.filter("age = 36").show()            # Bob and Fanny
youngest = g.vertices.groupBy().min("age")      # 29 (David)
g.edges.groupBy("relationship").count().show()
g.edges.filter("relationship = 'follow'").count()   # 4: b→c, c→b, f→c, e→f
```

*Sources: #09 s29, 42; ABD09 nb c20–38.*

## 20.5 Motif finding

**Motif finding is "searching for structural patterns in a graph"**, written in a DSL `(vertex)-[edge]->(vertex)`, pieces separated by `;`. **The result is a DataFrame "in which the column names are given by the motif keys"**.

| Pattern | Meaning |
|---|---|
| `(a)-[e]->(b); (b)-[e2]->(a)` | edges in **both directions** |
| `(v1)-[]->(v2); !(v2)-[]->(v1)` | v1→v2 with **no edge back** (`!` = negation) |
| `(a)-[]->(b)` | any connected pair; `[]` = **anonymous edge** |
| `(a)-[]->(a)` | self-loop |
| `(a)-[e1]->(b); (b)-[e2]->(c); (c)-[e3]->(a)` | **triangle** |

::: example
`g.find("(a)-[e]->(b); (b)-[e2]->(a)")` returns two rows: a = Charlie, b = Bob (c→b, b→c, both "follow") and the reverse. Each key holds the whole vertex/edge, so it can be filtered:

```python
g.find("(x)-[e1]->(y); (y)-[e2]->(x)").filter("e1.relationship == 'follow'")
motifs.filter("b.age > 30 or a.age > 30")   # still Bob/Charlie
```
:::

*Sources: #09 s30, 43–44; ABD09 nb c39–49.*

## 20.6 Subgraphs

**Subgraphs filter edges and vertices** (optionally with motifs): `filterEdges(cond)`, `filterVertices(cond)` (keeps matching vertices and edges between them), `dropIsolatedVertices()`. Each returns a GraphFrame, so they chain.

::: example
```python
g.filterEdges("relationship = 'friend'").filterVertices("age > 30").dropIsolatedVertices().vertices.show()
```
Result: **Alice (34), Bob (36), Esther (32)**. Friend edges: a→b, e→d, d→a, a→e; age > 30 drops Charlie and David (and e→d, d→a), leaving a→b, a→e; Fanny and Gabby are isolated and dropped.
:::

> [!flag] **DataFrame or graph?** Slide 30 says "(output is a DataFrame)"; the notebook says "a Graph", and both read `.vertices` from the result. **Resolution (external source).** Each method returns a new **GraphFrame** (hence chaining); the slide likely confuses this with `find()`, which returns a DataFrame. [GraphFrames user guide: subgraphs](https://graphframes.io/04-user-guide/07-subgraphs.html)

*Sources: #09 s30, 45; ABD09 nb c50–54.*

## 20.7 Path algorithms: BFS and shortest paths

Built-ins: BFS, connected components, strongly connected components, label propagation (LPA), PageRank (regular and personalized), shortest paths, triangle count.

**BFS** **"finds the shortest path(s) from one vertex to another vertex"**; start and end are SQL-like expressions on any attribute, and **"the search may also be limited by edge filters and maximum path lengths"**:

```python
g.bfs("name = 'Alice'", "name = 'Charlie'")
g.bfs(fromExpr="name = 'Esther'", toExpr="age < 32",
      edgeFilter="relationship != 'friend'", maxPathLength=3)
```

::: example
`g.bfs("name = 'Esther'", "age < 32")` returns one row with columns `from` / `e0` / `to` (start, edge, end): Esther, e→d (friend), David (29).
:::

**Shortest paths** **"computes shortest paths to the given set of landmark vertices, where landmarks are specified by vertex ID"**, adding a `distances` map (landmark → hops): `g.shortestPaths(landmarks=["a", "d"])`.

::: example
e: {d: 1, a: 2}; a: {a: 0, d: 2}; d: {d: 0, a: 1}; b, f, g: {} (directed edges cannot reach a or d). Row c not visible. The notebook's active cell runs it on a subgraph: `g.filterVertices("id in ('e','d','a')").shortestPaths(["a"])` (nb c62).
:::

*Sources: #09 s31, 46, 51; ABD09 nb c55–62.*

## 20.8 Connectivity and community: connected components, SCC, LPA, triangle count

Each returns the vertices DataFrame with a new column.

| Algorithm | Call | New column |
|---|---|---|
| Connected components | `connectedComponents()` (checkpoint dir) | `component` |
| Strongly connected components | `stronglyConnectedComponents(maxIter)` | `component` |
| Label propagation | `labelPropagation(maxIter)` | `label` |
| Triangle count | `triangleCount()` | `count` |

**Connected components** **"computes the connected component (linked vertices) membership (id) of each vertex"**. It "can take advantage of checkpointing to improve performance":

```python
sc.setCheckpointDir("/tmp/graphframes-example-connected-components")
result = g.connectedComponents()
```

Example: g = 146028888064; a–f share 412316860416. **Two components: Gabby alone, everyone else.** IDs are just labels.

> [!warning] **Slow in class.** Commented out: "this example may take more than 10m to run (don't do it during the class)". It also uses `sc`, unavailable on serverless (§6.8).

> [!flag] **What is a connected component?** Slide: "linked vertices"; notebook: "a subgraph in which each pair of nodes is connected with each other" (reads as direct links), and says G "is the only element that is connected only to A" — yet Gabby has no edges. **Resolution (external source).** A maximal set of vertices joined pairwise by some **path** (direct links for all pairs = a clique); `connectedComponents()` ignores direction, `stronglyConnectedComponents` follows it; an isolated vertex is its own component. [Wikipedia: Component (graph theory)](https://en.wikipedia.org/wiki/Component_%28graph_theory%29)

**SCC**: the component in which **"every vertex is reachable from every other vertex"**, following direction. **`maxIter` is mandatory**: `g.stronglyConnectedComponents(maxIter=10).select("id", "component")`. Example: {b, c}, {a, d, e} (loop a→e→d→a), {f} (reaches b/c, not reached back), {g}. **Four SCCs vs two connected components: direction matters.**

**LPA** detects communities: "Each node in the network is initially assigned to its own community. At every superstep, nodes send their community affiliation to all neighbors and update their state to the most frequent community affiliation of incoming messages." `g.labelPropagation(maxIter=5)`; example: a, d, f in one community; b, c, e, g one label each.

> [!warning] **Cheap, no guarantees.** "very inexpensive computationally, although (1) convergence is not guaranteed and (2) one can end up with trivial solutions (all nodes are identified into a single community)."

**Triangle count** **"computes the number of triangles passing through each vertex"**: `g.triangleCount(storage_level=StorageLevel.MEMORY_ONLY)`. Example: a, d, e = 1 each; others 0.

*Sources: #09 s32–34, 47–49, 52; ABD09 nb c63–68, 76–77.*

## 20.9 PageRank: the algorithm and three implementations

PageRank appears twice: as a "big data algorithm" in Spark SQL (#06) and as a GraphFrames built-in (#09). "Originally developed by search engines to rank web pages by link authority"; now used for influencers, recommendation systems, and critical nodes in biological or transport networks. **"PageRank assigns a numerical score to each node in a graph, reflecting its relative importance. The fundamental idea is: a node is important if other important nodes link to it."**

It is **"a classical example of big data analysis (like word count)"**: lots of data (needs a distributable, scalable algorithm) and iterative (**"the more iterations, the better the answer will be"**).

**Algorithm.** Start each page at rank 1; each iteration, each page sends rank / (number of neighbors it links to) to each neighbor; the new rank comes from the sum received. "Each iteration incrementally improves the page ranking".

```math
\begin{aligned}
\text{contrib}_p &= \frac{\text{rank}_p}{\text{neighbours}_p} \\
\text{newrank}_q &= 0.15 + 0.85 \times \sum_{p \to q} \text{contrib}_p
\end{aligned}
```

The sum runs over pages \(p\) linking to \(q\); 0.85/0.15 are the class weights (GraphFrames examples use `resetProbability=0.15`).

::: example
*Four pages.* Page1→Page3; Page2→Page1; Page3→Page1, Page4; Page4→Page1, Page2.

| Page | Iteration 1: receives | New rank |
|---|---|---|
| Page1 | 1 (P2) + 0.5 (P3) + 0.5 (P4) = 2.0 | 0.15 + 0.85 × 2.0 = **1.85** |
| Page2 | 0.5 (P4) | **0.58** |
| Page3 | 1 (P1) | **1.0** |
| Page4 | 0.5 (P3) | **0.58** |

Iteration 2 (contribs 0.58, 1.85, 0.5, 0.29): Page1 1.31, Page2 0.39, **Page3 1.7**, Page4 0.57. After 10: Page1 1.43, Page2 0.46, Page3 1.38, Page4 0.73. Page3, linked only by the important Page1, ends second: **important pages pass on their importance.**
:::

Input: one "source-page destination-page" pair per line. Helper (neighbors `[page1, page2]`, rank 1.0 → `(page1, .5)`, `(page2, .5)`):

```python
def computeContribs(neighbors, rank):
    for neighbor in neighbors: yield( neighbor, rank/len(neighbors) )
```

**1. One iteration with DataFrames** (#06):

```python
links = edges.groupBy("src").count().withColumnRenamed("count", "links")
edges_with_links = edges.join(links, "src")
contribs = edges_with_links.join(ranks, edges_with_links.src == ranks.id) \
    .withColumn("contrib", col("rank") / col("links"))
new_ranks = contribs.groupBy("dst").sum("contrib").withColumnRenamed("sum(contrib)", "rank")
```

**2. One iteration in SQL** (ABD06 SQL1): a window `COUNT` (§11.9) replaces the `groupBy` + join:

```sql
SELECT dst AS id, SUM(rank/links) AS rank
FROM (SELECT e.dst, r.rank, COUNT(*) OVER (PARTITION BY e.src) AS links
      FROM edges e JOIN ranks r ON e.src = r.id)
GROUP BY dst
```

**3. GraphFrames** (#09): **returns a GraphFrame** whose vertices gain `pagerank`. Stop by tolerance or fixed iterations; personalized with `sourceId`:

```python
g.pageRank(resetProbability=0.15, tol=0.01).vertices.show()   # until convergence
g.pageRank(resetProbability=0.15, maxIter=10)                  # fixed iterations
g.pageRank(resetProbability=0.15, maxIter=10, sourceId="a")    # personalized
```

Example: b = 2.66 (highest), a = 0.45, e = 0.37, f = 0.33, g = 0.18 (c, d hidden). **Bob is most important**: pointed to by Alice and Charlie, and Charlie by Bob and Fanny, so importance circulates in b–c.

> [!compare] **A — Taught (#06 slides):** four pages, 0.85/0.15, 10 iterations. **B — Code:** the #06 DataFrame code uses a 3-node graph (A→B, B→C, C→A), SQL1 adds C→B; both run **one iteration, no damping, no loop**. Only GraphFrames iterates with `resetProbability=0.15`. Source: #06 s40–47; ABD06 SQL1 nb c133–134; #09 s50.

*Sources: #06 s39–47; ABD06 SQL1 nb c131–134; #09 s34, 50; ABD09 nb c69–75.*

## 20.10 Loading and saving graphs

No special format: **a GraphFrame is saved as two Parquet datasets (Chapter 3), vertices and edges**, and rebuilt with the constructor.

```python
g.vertices.write.parquet("path_vertices"); g.edges.write.parquet("path_edges")
g = GraphFrame(spark.read.parquet("path_vertices"), spark.read.parquet("path_edges"))
```

*Sources: #09 s35.*

## 20.11 Lab: the Game of Thrones network

> [!lab] **ABD09 Colab notebook, cells 78–103.** Characters of *A Song of Ice and Fire* (Andrew Beveridge's "Network of Thrones", all books). No solution outputs stored.

**Data** (downloaded with `urllib.request.urlretrieve`): `asoiaf-all-nodes.csv` (id, name, attributes) and `asoiaf-all-edges.csv`. **"Two characters are connected if their names (or aliases) appear within a window of up to 15 words of each other in the book text."** Edge columns: Source, Target, Type (Undirected), **Weight** ("the total number of times that proximity/interaction occurred throughout the entire series").

**Rename to the GraphFrame conventions** (§20.3):

```python
nodes_df = spark.read.csv("nodes.csv", header=True, inferSchema=True) \
    .withColumnRenamed("Id", "id").withColumnRenamed("Label", "name")
edges_df = spark.read.csv("edges.csv", header=True, inferSchema=True) \
    .withColumnRenamed("Source", "src").withColumnRenamed("Target", "dst")
g_gt = GraphFrame(nodes_df, edges_df)
```

| Question | Tool |
|---|---|
| Number of characters and interactions | `vertices.count()`, `edges.count()` |
| Does Jon Snow exist? / his interactions | `vertices.filter("id = 'Jon-Snow'")`; `edges.filter` on `src` **or** `dst` |
| Most/least "solicited" | `inDegrees` desc/asc, `limit(5)` |
| Most/least important | `pageRank(resetProbability=0.15, maxIter=5)`, order by `pagerank` ("higher pagerank means a higher importance") |
| Message path Jon Snow → Theon Greyjoy, → Varys | `bfs(..., maxPathLength=10)` |

**"Remember that we are using maxPathLength=10; Longer paths will not be detected in this query"**. The edges are mutual, but GraphFrames is directed (§20.3): each row is one Source→Target edge, hence filtering on both `src` and `dst`.

*Sources: ABD09 nb c78–103.*

## 20.12 Further examples

- **Book:** Needham & Hodler, *Graph Algorithms: Practical Examples in Apache Spark & Neo4j* (O'Reilly), source of several Chapter 19 figures.
- **Bike stations** (Databricks "Graph Analysis with GraphFrames"): "get the distinct id values in the Vertices table and rename the start and end stations to src and dst respectively for your edges tables. These are required conventions" — the same renaming as the lab.
- **Flights** (Databricks blog "On-Time Flight Performance with GraphFrames for Apache Spark", 2016): airports = vertices, flights = edges.

*Sources: #09 s54, 58–59.*

### Key takeaways

- GraphX (legacy, Scala, RDDs) vs GraphFrames (separate package, DataFrames, Python); GraphFrames runs in Colab, not Free Edition.
- `GraphFrame(v, e)`: vertices need `id`, edges `src`/`dst`; directed by default.
- `g.find` and `vertices/edges.filter` return DataFrames; subgraph methods return GraphFrames.
- Algorithms: BFS, shortest paths, connected/strongly connected components, LPA, PageRank, triangle count.
- PageRank: rank 1, contrib = rank / neighbours, new = 0.15 + 0.85 × Σ; `pageRank(resetProbability, tol | maxIter)` returns a GraphFrame.
- A graph is saved as two Parquet datasets.

---

# Part VII: Wrap-up

Part VII puts Parts I–VI back together, for revision and the group project.

---

# Chapter 21: Putting It All Together

One storyline: data outgrew single machines, so processing moved to clusters, then Spark, then the lakehouse. This chapter gives the map, each session's conclusion, and the project pipeline.

## 21.1 The Big Data map and engine examples

Deck #03's concept map:

| Data categories | Technologies | Data processing platforms |
|---|---|---|
| Unstructured | Data formats (Ch 3) | Persistent / Real-time (Ch 15, 17–18) |
| Semi-structured | Parallelization (Ch 2, 7) | Unstructured / Structured (Ch 9–10) |
| Structured | Data locality (§2.7) | SQL / Graphs / Machine Learning (Ch 8–13, 19–20) |
| | In-memory (§1.4, Ch 6) | |

Two engine families as examples:

- **Google — BigQuery on Dremel.** BigQuery: "enterprise data warehouse", "serverless architecture", GA since 2011. Dremel: "scalable, interactive ad-hoc query system for analysis of read-only nested data", "multi-level execution trees and columnar data layout", "aggregation queries over trillion-row tables in seconds"; complements MapReduce.
- **Databricks — Apache Spark:** "Unified engine for large-scale data analytics"; "a multi-language engine for executing data engineering, data science, and machine learning on single-node machines or clusters" (Ch 6–13, 18, 20).

Parquet uses Dremel optimizations (§3.4); Spark answers MapReduce's drawbacks (§2.10): different products, shared technologies.

*Sources: #03 s34, 40–43.*

## 21.2 What each session concluded

One line per deck summary slide (details in the chapters cited):

| Deck | Conclusion | Ch |
|---|---|---|
| #01 Big Data processing | data growth after 2000 (3Vs) broke SMP/relational tech → MPP, Hadoop, Spark as open-source standard → Data Lake, Lakehouse. "Big Data shifted from a hyped trend to a foundational layer of modern IT." | 1, 2, 14 |
| #02 Databricks | "the company from the creators of Spark"; "an open-data platform for data engineering and analytics based on Apache Spark"; Free Edition for learning; multi-language notebooks, versioning, visualizations; dbutils | 4, 5 |
| #03 Distributed processing | decoupled storage/processing, MPP, scalability, fault tolerance; engine + resource manager + storage; YARN runs tasks where data is; text/row/columnar formats; Delta = "Parquet and Columnar-based; ACID" | 2, 3, 15 |
| #04 Spark basics | "Spark is an in-memory distributed computation framework"; driver + executors; RDD/DataFrame/Dataset; transformations vs actions, lazy; lineage; "passing functions as parameters; anonymous functions" | 6, 7 |
| #05 Spark SQL | Catalog + optimizer; DataFrame as key unit. **"Spark is not a replacement for a Data Base."** | 8–10 |
| #06 Spark SQL programming | partitioning, Broadcast Hash Join, DPP, AQE, Liquid Clustering; aggregate/expanding/conditional/datetime/window functions → "you can implement typical big data algorithms" | 11–13 |
| #07 The Data Lakehouse | Spark for pipelines, Delta for storage; Medallion: **Bronze raw, Silver curated, Gold summarized** | 14–16 |
| #08 Streaming | near-real-time analysis, IoT/social media; Spark Streaming → Structured Streaming; lessons: event time mandatory, one engine for all data, ACID storage convergence | 17, 18 |
| #09 Graph analysis | "mathematical structures to model relationships between objects"; GraphX vs GraphFrames; operators and algorithms | 19, 20 |

```flow
data growth (3Vs) → scale out on clusters → Hadoop / MapReduce → Spark (in-memory, lazy, DataFrames) → Spark SQL (catalog, functions, optimization) → lakehouse (Delta + Medallion) → streaming and graphs on the same engine
```

*Sources: #01 s43; #02 s50; #03 s51–52; #04 s54–55; #05 s66; #06 s60; #07 s26; #08 s53, 62; #09 s61.*

## 21.3 An end-to-end use case: the lakehouse project pipeline

The project: "create a structured, incremental data analysis process that replicates a real-world situation" with a small Data Lakehouse. **Each deck #07 step maps onto the manual:**

```flow
read the source (more than once) → Bronze: read history → normalize → Silver: modeled detail → prepare for analysis → Gold: optimized for analysis → query · dashboard · ML
```

| Project step (deck #07) | Layer | See |
|---|---|---|
| Read data from a source, ideally more than once | ingestion | §9.4–9.5, §18.4 |
| Layer with the read history, ready for reprocessing | Bronze | §16.6 (raw vs data rules: §16.2) |
| Normalize (profiling, data rules) | Bronze → Silver | §11.2, §16.3 |
| Store normalized detail, modeled for the theme | Silver | §16.7, §8.8, §15.7 |
| Prepare and create a layer optimized for analysis | Gold | §16.8, Ch 11, §15.9 |
| Analyses: query, dashboard, ML model | consumption | §8.9, §16.8; ML below |

Grading (front matter): 4/20 for "Big Data technologies use", 4/20 for "overall presentation and structure"; deck #07 adds a conclusion on "the data, process and technology used".

> [!warning] **The notebook is the deliverable.** "A working Databricks Notebook in html format" is mandatory (#01), with a header naming the group and members (#07); a one-page document is optional.

> [!warning] **ML is not covered** in sessions 1–9 (see the course-facts flag). **Resolution (external source).** `pyspark.ml` `Pipeline` + `VectorAssembler`, `LinearRegression` / `RandomForestClassifier`, scored with `RegressionEvaluator` (RMSE, R²) / `BinaryClassificationEvaluator` (AUC); split by date (never `randomSplit`) and compare with a naive baseline. [Spark MLlib guide](https://spark.apache.org/docs/latest/ml-guide.html)

*Sources: #07 s24; #01 s5, 11–13; ABD07 nb c25–38.*

### Key takeaways

- One storyline: data growth → scale-out → MapReduce → Spark (in-memory, lazy, SQL-friendly).
- Map: three data categories, four technologies (formats, parallelization, locality, in-memory), persistent/real-time SQL, graph and ML platforms.
- **Spark SQL + Delta turn a data lake into a lakehouse; Medallion (Bronze raw, Silver curated, Gold summarized) organizes it.**
- Streaming and graphs run on the same engine, with DataFrames as the common abstraction.
- The project is the Medallion pipeline end to end: read → Bronze → normalize → Silver → Gold → query, dashboard, ML.

---

# Appendix A: Cheat Sheet

Commands, APIs and SQL constructs from the class materials; the last column gives the section.

### Notebook and shell

| Construct | What it does | § |
|---|---|---|
| `%python` `%sql` `%scala` `%r` · `%md` | cell language (own REPL each; variables not shared) · Markdown | 5.1, 5.2 |
| `%sh` (`-e`) · `%lsmagic` | shell code (`-e`: fail on non-zero exit) · list magics | 5.2, 5.3 |
| `%sh pwd` `ls -l` `ps` `free` `env` `df` | inspect the driver (mounts `/dbfs`, `/Volumes`, `/Workspace`) | 5.3 |
| `%sh touch` `mv` `rm` | create, move, delete files | 5.6 |
| Shift+Enter · Ctrl+Shift+P · Esc H | run cell · command palette · shortcuts | 5.1 |

### dbutils and the file system

| Construct | What it does | § |
|---|---|---|
| `dbutils.help()` · `dbutils.fs.help('cp')` | list utilities · help on a command | 5.5 |
| `display(dbutils.fs.ls(path))` | list a folder as a table | 5.5 |
| `dbutils.fs.cp` · `put` · `rm(path, recurse=True)` | copy · write small file · delete recursively | 5.6, 9.4 |
| `%fs ls` `head` `cp` `rm -r` | same, as magic ("uses dbutils filesystem commands"); `dbfs:` optional | 5.5, 5.6 |
| `dbutils.widgets` | notebook input widgets *(out of the program)* | 5.7 |
| `/Volumes/<catalog>/<schema>/<volume>` | UC Volume path (class: `/Volumes/workspace/abd/abd`) | 4.7, 5.4 |
| `/databricks-datasets` | sample data (deprecated, still used) | 5.4 |
| `/Workspace/Users` `/Shared` `/Repos` | workspace folders (control plane) | 4.5, 5.4 |

### Session, configuration and catalog

| Construct | What it does | § |
|---|---|---|
| `spark` · `spark.version` | ready-made SparkSession · version | 5.8, 6.8 |
| `spark.conf.get/set(key)` | read/set config (many not on serverless) | 5.8, 12.5 |
| `spark.sql.shuffle.partitions` · `files.maxPartitionBytes` | shuffle partitions · max bytes per file partition | 12.5 |
| `spark.sql.adaptive.enabled` · `ansi.enabled` · `session.timeZone` | AQE · ANSI mode · time zone | 5.8, 12.4 |
| `SparkSession.builder…getOrCreate()` | own session (Colab) *(out of the program)* | 4.8, 6.9 |
| `spark.catalog.listDatabases()` `listTables()` `currentDatabase()` | explore the catalog | 8.4 |
| `spark.catalog.dropTempView` · `dropGlobalTempView` | remove a view | 8.7 |

### Creating and reading DataFrames

| Construct | What it does | § |
|---|---|---|
| `spark.range(5)` | one column `id` | 9.2 |
| `spark.createDataFrame(data, cols)` | from tuples; unnamed → `_1, _2` | 7.3, 9.2 |
| `StructType([StructField(name, type, nullable)])` · DDL string | explicit schema | 9.2 |
| `spark.read.csv(path, header=True, inferSchema=True, sep=';')` | CSV; `inferSchema` reads data more than once | 9.4 |
| `spark.read.json` · `.text` (column `value`) · `.parquet` | JSON · text · Parquet | 7.3, 9.4 |
| `spark.read.format(f).option(k,v).load(path)` | generic reader | 9.4, 9.7 |
| `spark.table(name)` · `spark.sql("…")` · `format("jdbc")` | from table · query · JDBC | 9.5 |

### Inspecting

| Construct | What it does | § |
|---|---|---|
| `show(n, truncate, vertical)` · `display(df)` | print rows · Databricks table/charts | 9.6 |
| `take(n)` · `first()` · `collect()` · `count()` | rows to the driver · row count | 7.5, 9.6 |
| `printSchema()` · `schema` · `dtypes` · `columns` | schema info | 9.6, 10.2 |
| `describe()` · `summary()` | statistics (a DataFrame) | 9.6, 11.2 |
| `explain()` | show the (physical) plan | 7.8, 12.5 |

### Writing

| Construct | What it does | § |
|---|---|---|
| `df.write.format(f).mode(m).save(path)` | generic writer; no format → Delta on Databricks | 9.7, 9.8 |
| `df.write.mode("overwrite").parquet(path)` | output = folder of partitioned files | 9.7, 9.8 |
| `errorifexists` (default) · `append` · `overwrite` · `ignore` | save modes | 9.7 |
| `saveAsTable("s.t")` · `.option("path", p).saveAsTable` | managed · external table (`LOCATION` variant fails on Free Edition) | 8.8 |

### SQL statements

| Construct | What it does | § |
|---|---|---|
| `SHOW DATABASES` · `TABLES [IN s]` · `FUNCTIONS [LIKE 'w*']` · `USER FUNCTIONS` | list objects | 8.5, 8.9 |
| `CREATE SCHEMA IF NOT EXISTS s` · `USE s` · `DESCRIBE SCHEMA EXTENDED s` | create · select · details | 8.5 |
| `DROP SCHEMA s [CASCADE\|RESTRICT]` | CASCADE drops contents too | 8.5, 16.9 |
| `CREATE TABLE … [USING … LOCATION …]` · `CLUSTER BY (col)` | table · Liquid Clustering | 8.6, 8.8, 15.9 |
| `INSERT INTO` · `UPDATE … SET` · `DELETE FROM` | add · change · remove rows (Delta) | 8.6, 15.7 |
| `MERGE INTO t USING s ON … WHEN MATCHED … WHEN NOT MATCHED …` | upsert | 15.7 |
| `DROP TABLE\|VIEW IF EXISTS` · `DROP TEMPORARY FUNCTION` | remove table/view · UDF | 15.7, 11.10 |

### Views

| Construct | What it does | § |
|---|---|---|
| `createTempView` · `createOrReplaceTempView` | session view | 8.7 |
| `createGlobalTempView(v)` · `global_temp.v` | application view (not in metastore) | 8.7 |
| `spark.sql("""…""")` · `%sql` | SQL on views/tables | 8.9 |

### Transformations and actions

| Transformations (lazy) | Actions (trigger) | § |
|---|---|---|
| narrow: `filter`/`where`, `map` | `count`, `collect`, `show`, `take`, `first` | 7.5, 7.6 |
| wide (shuffle): `groupBy`, `join`, `sort`, `distinct`, `dropDuplicates`, `repartition` | `write` / `save` | 7.6, 12.1 |

### Column operations

| Construct | What it does | § |
|---|---|---|
| `select` · `selectExpr("sql")` | choose/compute columns | 10.3 |
| `withColumn` · `cast` · `drop` | add/replace · retype · remove | 10.2 |
| `withColumnRenamed` · `toDF(…)` · `alias` | rename by name · by position · name expression | 10.2 |
| `where`/`filter` · `sort`/`orderBy` · `limit` | filter · order · keep n | 10.3, 10.4 |
| `distinct()` · `dropDuplicates([cols])` | dedupe all · chosen columns | 10.6, 11.2 |
| `dropna(how, subset)` · `isNull()` | drop · detect nulls | 11.2 |
| `lit` · `col` · `expr` · `when().otherwise()` | literal · column · SQL expr · conditional | 10.3, 11.7, 13.2 |

### Functions by family

| Family | Functions | § |
|---|---|---|
| Aggregate | `count`, `countDistinct`, `sum`, `avg`, `min`, `max`, `variance`, `kurtosis`, `skewness` | 11.3 |
| Grouping | `groupBy(k).agg(…)`, `.avg(m)`, `.pivot(c).agg(…)` | 11.4, 11.5 |
| String | `upper`, `lower`, `length`, `trim`, `split`, `substring`, `instr`, `regexp_replace`, `translate`, `contains`, `like`, `startswith`, `endswith` | 10.7, 13.3 |
| Collection | `explode`, `getItem` | 11.6, 18.14 |
| Datetime | `current_date`, `current_timestamp`, `to_date`, `to_timestamp`, `date_format`, `date_add`, `date_sub`, `add_months`, `datediff`, `year`…`second`, `between`, `INTERVAL` | 11.8 |
| Window | `Window.partitionBy().orderBy().rowsBetween(-2, Window.currentRow)`, `row_number`, `rank`, `dense_rank`, `percent_rank`, `ntile`, `lag`, `lead`, `mean().over(w)` | 11.9 |

**Import tip:** `import pyspark.sql.functions as f`, not `import *` (clashes with built-ins like `sum`) (§11.1).

### Joins, unions, UDFs

| Construct | What it does | § |
|---|---|---|
| `df1.join(df2, on, how)` | join (chain for more); multi-key `on=(a.k1==b.k1) & (a.k2==b.k2)` | 10.5 |
| `how`: `inner` (default) `left` `right` `full` `leftsemi` `leftanti` `cross` | join types | 10.5 |
| `union` · `unionByName` | stack by position · by name | 10.6 |
| `udf(f)` · `spark.udf.register("name", f)` | DataFrame API only · DataFrames + SQL | 11.10 |

### Delta Lake

| Construct | What it does | § |
|---|---|---|
| `write.format("delta").save` · `saveAsTable` | Delta files · table | 15.6, 15.7 |
| `_delta_log/000…0.json` | transaction log | 15.6 |
| `DESCRIBE HISTORY t` · `VERSION AS OF n` | versions · time travel | 15.8 |
| `CLUSTER BY (col)` · `OPTIMIZE` | Liquid Clustering · cluster incrementally | 15.9 |

### Optimization

| Mechanism | Key fact | § |
|---|---|---|
| Broadcast Hash Join | small table to every executor; default 10 MB (configurable) | 12.2 |
| Dynamic Partition Pruning | dimension hash table → broadcast → dynamic filter on fact scan | 12.3 |
| AQE | coalesce shuffle partitions · switch join · skew joins | 12.4 |

### Structured Streaming

| Construct | What it does | § |
|---|---|---|
| `readStream.format("rate"\|"socket"\|"text"\|"json"\|"kafka"\|"cloudFiles")` | source | 18.4, 18.11 |
| `option("rowsPerSecond")` · `option("maxFilesPerTrigger")` | rate speed · files per micro-batch | 18.12, 18.15 |
| `df.isStreaming` | is it a stream? | 18.4 |
| `writeStream.format("memory"\|"console"\|"parquet"\|"kafka"\|"delta")` | sink | 18.5 |
| `outputMode("append"\|"complete"\|"update")` | rows written per trigger | 18.6 |
| `queryName` · `option("path")` · `option("checkpointLocation")` | memory-table name · output · checkpoint | 18.5, 18.8 |
| `trigger(ProcessingTime("3 seconds"))` · `availableNow` | when to process | 18.7 |
| `start()` · `status` · `stop()` · `awaitTermination()` · `spark.streams.active` | manage queries | 18.9 |
| `window(col, "1 hour")` | event-time window | 18.10, 18.15 |

### GraphFrames

| Construct | What it does | § |
|---|---|---|
| `pip install graphframes-py==0.12.3` + `spark.jars.packages io.graphframes:graphframes-spark4_2.13:0.12.3` | Colab setup (at session build) | 20.2 |
| `GraphFrame(v, e)` | graph (`id`; `src`, `dst`) | 20.3 |
| `vertices` `edges` `degrees` `inDegrees` `outDegrees` | basic queries | 20.4 |
| `find("(a)-[e]->(b); (b)-[e2]->(a)")` | motifs → DataFrame | 20.5 |
| `filterEdges().filterVertices().dropIsolatedVertices()` | subgraph | 20.6 |
| `bfs(fromExpr, toExpr, edgeFilter, maxPathLength)` · `shortestPaths(landmarks)` | paths | 20.7 |
| `connectedComponents` · `stronglyConnectedComponents(maxIter)` · `labelPropagation(maxIter)` · `triangleCount` | connectivity, community | 20.8 |
| `pageRank(resetProbability=0.15, maxIter\|tol)` | scores in `.vertices.pagerank` | 20.9 |

### Formulas and numbers

| Formula / number | Meaning | § |
|---|---|---|
| rank = 0.15 + 0.85 × Σ(rank / #neighbours) | PageRank update | 20.9 |
| °F = °C × 1.8 + 32 · Nm = km × 0.539956803 | lab conversions | 13.2 |
| π ≈ 4 × inside / total | Monte Carlo Pi | 13.4 |
| 100 ms / 1 ms | micro-batch / continuous latency ("as low as") | 18.1, 18.7 |
| 3+4+4+3+3+3 = 20 | project grading grid | Front matter |

---

# Appendix B: Glossary

Definitions from the class materials; quotes are the class's words.

| Term | Meaning | § |
|---|---|---|
| 3Vs | volume, velocity, variety (META Group, 2001) | 1.2 |
| ACID | atomicity ("succeed or fail completely"), consistency, isolation, durability ("committed changes are permanent") | 15.4 |
| Action | triggers computation; returns to driver or writes (`count`, `collect`, `show`, `write`) | 7.5 |
| AQE | runtime re-optimization at query stages: coalesce partitions, switch joins, skew joins | 12.4 |
| Append / Complete / Update | output modes: new rows (default) / whole result / changed rows | 18.6 |
| Application Master | launched by ResourceManager; asks Name Node for block locations, requests containers | 2.7 |
| Auto Loader | `cloudFiles` source; "incrementally and efficiently processes new data files" | 18.11 |
| Avro | row-based compact binary, embedded JSON schema, schema evolution | 3.3 |
| Batch processing | all data at once; hours of latency, complex analysis | 17.4 |
| BFS | "finds the shortest path(s) from one vertex to another" | 20.7 |
| Big Data | data growth impacting society; volume, variety, velocity | 1.2 |
| Broadcast Hash Join | small dataset broadcast to every executor; default below 10 MB | 12.2 |
| Bronze / Silver / Gold | raw / curated (cleaned, validated) / aggregated | 16.2 |
| Catalyst | optimizer: analysis → logical optimization → physical planning → code generation | 8.2 |
| Catalog (Spark) | "central registry" of metadata: databases, tables, views, functions | 8.4 |
| Catalog (Unity) | top level of `catalog.schema.object` | 4.7 |
| Checkpointing | stores query state on reliable storage; restart where it stopped | 18.8 |
| Cluster | computers working together: storage, processing, resource management | 2.2 |
| Columnar format | stores by column; good for few-column queries (RCFile, ORC, Parquet) | 3.4 |
| Connected component | the set of linked vertices a vertex belongs to | 20.8 |
| Continuous Processing | low-latency mode (as low as 1 ms) vs micro-batch | 18.1, 18.7 |
| Control plane | web app, compute orchestration, Unity Catalog, notebooks | 4.5 |
| Compute plane | where Spark runs: Databricks account (serverless) or your cloud (classic) | 4.5 |
| DAG | directed acyclic graph; vertices = data objects, edges = operations | 7.8, 19.3 |
| Data lake | "one data pool", raw data of any type, no governance, schema on-read (Dixon, ~2010) | 14.3 |
| Data lakehouse | lake + metadata/caching/indexing layer; open formats, ML, performance (CIDR 2021) | 14.5 |
| Data locality | move computation to the data | 2.7 |
| Data / business rules | nulls, duplicates, types / derived columns, reference data, conformance | 16.3 |
| Data warehouse | structured data via ETL for BI (1st generation, 1990) | 14.2 |
| Dataflow model | Google: bounded/unbounded, event time, watermarks, triggers; batch = bounded subset of streaming | 17.7 |
| DataFrame | named columns with schema; "in-memory partitioned data spread across executors" | 7.1, 8.10 |
| Dataset | type-safe extension of DataFrames *(out of the program)* | 7.1 |
| dbutils | notebook-only utilities: object storage, chaining notebooks, secrets | 5.5 |
| DBFS | `dbfs:/` namespace; root and mounts are deprecated | 4.6 |
| DDL / DML | CREATE, ALTER, DROP / INSERT, UPDATE, DELETE, MERGE | 8.6 |
| Delta Lake | open storage layer: Parquet + transaction log; ACID, schema enforcement, time travel; Databricks default | 15.1 |
| Driver | holds SparkSession; I/O, negotiates resources with the cluster manager, schedules executors | 6.6 |
| Dynamic Partition Pruning | filters fact table at scan with broadcast filter from dimension | 12.3 |
| Vertex / Edge | graph node / link between two vertices (link, relationship, arc) | 19.1 |
| Event vs processing time | when it occurred vs when received | 17.7 |
| Executor | worker JVM that runs tasks, reports to the driver | 6.6 |
| External table | data at a given path; Spark manages metadata only | 8.8 |
| Fault tolerance | "designed for failure"; Spark uses lineage | 2.3, 7.8 |
| GFS | Google File System (2003), model for distributed storage | 2.1 |
| GraphFrames | separate package, DataFrame graphs (Catalyst, Tungsten), Python/Scala/Java, motifs | 20.1 |
| GraphX | legacy Scala RDD-based graph library | 20.1 |
| Hadoop / HDFS | open-source distributed storage + processing on commodity hardware / its file system | 2.1 |
| Job | work triggered by an action; sequence of stages | 6.7 |
| Kappa | single stream pipeline over replayable log (Kafka); simpler, needs retention | 17.6 |
| Label Propagation | nodes adopt neighbours' most frequent label; cheap, convergence not guaranteed | 20.8 |
| Lakeflow | Databricks pipelines; Designer = IDE for Spark Declarative Pipelines | 16.10 |
| Lambda | batch layer (exact, slow) + speed layer (approximate, fast); duplicated logic | 17.6 |
| Lazy execution | transformations build a plan; actions run it | 7.7 |
| Lineage | "pointers stored in a graph (DAG)" to a job's RDDs; basis of fault tolerance | 7.8 |
| Liquid Clustering | `CLUSTER BY`; replaces (incompatible with) partitioning and Z-order | 15.9 |
| Managed table | Spark manages data and metadata | 8.8 |
| MapReduce | "batch query processor": map to key-value, shuffle/sort, reduce | 2.8 |
| Medallion | "data design pattern" organizing a lakehouse in Bronze/Silver/Gold; multi-hop | 16.1 |
| Micro-batch | default execution in small batches (as low as 100 ms) | 18.1 |
| Motif | structural pattern, `(a)-[e]->(b)` DSL | 20.5 |
| MPP | shared-nothing nodes, partitioned data, one task in parallel | 1.5 |
| Name Node / Data Node | master knowing block locations / workers storing blocks | 2.1, 2.7 |
| Narrow / wide | one output partition per input / many (shuffle) | 7.6 |
| Node | one computer; masters distribute work to workers | 2.2 |
| NodeManager / ResourceManager | YARN daemon on each worker / on the master | 2.6 |
| Open table format | layer over files adding ACID (Delta, Iceberg, Hudi) | 3.5, 14.6, 15.5 |
| PageRank | "a node is important if other important nodes link to it" | 20.9 |
| Parquet | columnar (Cloudera, Twitter), embedded schema; "default in Spark" | 3.4 |
| Partition | "a logical chunk of data"; unit of parallelism | 7.4 |
| RDD | "immutable collection of data elements partitioned across the cluster" | 7.1 |
| Real-Time Mode | 2025 trigger for millisecond processing | 18.7 |
| REPL | per-language context; variables not shared | 5.2 |
| Scale up / out | bigger machines (SMP, limited) / more commodity machines (MPP) | 1.5 |
| Schema-on-read | structure applied only when reading | 14.3 |
| Serialization | data to bytes (deserialization reverses) | 3.3 |
| Serverless | Databricks-managed compute; only option in Free Edition | 4.3, 4.5 |
| Shuffle | "redistributing data across nodes"; costly (disk, serialization, network) | 7.6, 12.1 |
| Shortest paths | "shortest distance from every vertex to the given vertices" (landmarks) | 20.7 |
| SMP | processors share memory, bus, I/O | 1.5 |
| Spark Core | memory, fault recovery, scheduling, monitoring, storage I/O | 6.3 |
| Spark SQL | structured-data module on Spark Core | 8.1 |
| SparkContext | first entry point; `sc` unsupported on serverless | 6.8 |
| SparkSession | entry point since 2.0, subsumes other contexts; `spark` | 6.8 |
| Stage | set of tasks (one per partition) in a job | 6.7 |
| Streaming data | "high volume and speed", "continuous and incremental" real-time events | 17.1 |
| Strongly Connected Component | "every vertex is reachable from every other vertex" | 20.8 |
| Structured Streaming | "scalable and fault-tolerant stream processing engine" on Spark SQL | 18.1 |
| Task | "individual unit of work sent to an executor" | 6.7 |
| Transformation | defines a new immutable DataFrame without computing it | 7.5 |
| Trigger | "specifies when the system should process the next set of data" | 18.7 |
| Triangle count | "number of triangles passing through each vertex" | 20.8 |
| Tungsten | execution engine: compact binary storage, runtime bytecode generation | 8.3 |
| UDF | must be registered; "expensive", "a black box to Spark" | 11.10 |
| Unbounded table | new stream data = rows appended to an unbounded input table | 18.2 |
| Unity Catalog | governance, three-level namespace; auto-enabled in new accounts since 8 Nov 2023 | 4.7 |
| UniForm | lets Iceberg/Hudi clients read Delta tables | 15.5 |
| Volume | UC object governing non-tabular files (managed / external) | 4.7 |
| Watermark | "progressive, system-wide metric" of event-time progress; handles late data | 17.7 |
| Window function | over related rows, keeps every row; OVER (PARTITION BY, ORDER BY, frame); ranking/analytic/aggregate | 11.9 |
| YARN | Hadoop resource management and job scheduling for several engines | 2.6 |
| Z-order | Delta layout optimization, replaced by Liquid Clustering | 15.2, 15.9 |

---

# Appendix C: Inconsistencies and Open Points

An index of the `[!flag]` cards: content contradictions between class materials, and course-administration points. Sources, full text and links are in each chapter card. "—" = not resolved (versions, naming, dates, admin; no content impact). Code typos and uncovered topics are not listed.

### C.1 Content contradictions

**Part I: Big Data Foundations**

| Topic | Short issue | Resolution | § |
|---|---|---|---|
| Spark speed | "100x faster than MapReduce" vs paper's "10x" on iterative jobs | Neither general: 100x old in-memory claim; 10x iterative ML | 2.11 |
| Decoupling storage/compute | Explained as "moving computation is cheaper" (that is data locality) | Decoupling = separate, independently scaling storage and compute layers | 2.3 |
| Storage formats layer | Avro and JSON labelled "Optimized Columnar Formats" | Only Parquet, ORC columnar; Avro row binary, JSON text | 3.5, 14.6 |

**Part II: The Working Environment**

| Topic | Short issue | Resolution | § |
|---|---|---|---|
| DBFS status | Storage example vs "deprecated"; `/databricks-datasets` deprecated yet used | — | 2.5, 4.6, 5.4 |
| `%fs` | Notebook calls it "Linux"; slides: Databricks magic over dbutils | — | 5.5 |

**Part III: Apache Spark Core**

| Topic | Short issue | Resolution | § |
|---|---|---|---|
| Spark versions | Screenshots and recipes span Spark 2.x, 3.2.1, 4.0.0–4.2.0 | — | 6.5 |
| SparkContext on serverless | `sc`/RDD examples vs "sc is not supported in serverless" | No `sc`/RDDs on serverless (Spark Connect); use `spark` | 6.8, 7.2, 18.9, 20.8 |
| Spark SQL entry point | SparkSession subsumes SQLContext vs "requires SQLContext" | — | 6.8 |
| Operation lists | `groupByKey` among DF transformations; `write.save` as action | `groupByKey` RDD transformation; saves are actions | 7.5 |
| Datasets | "Out of the program" yet in streaming and graph definitions | — | 7.1 |

**Part IV: Spark SQL**

| Topic | Short issue | Resolution | § |
|---|---|---|---|
| Hive-era wording | HiveQL/"Hive table" wording vs Unity Catalog namespace in labs | — | 8.1 |
| `union` and duplicates | "Union ignores duplicate data", yet example needs `distinct` | `union` keeps duplicates (UNION ALL) | 10.6 |
| dense_rank | Described as "skipping ranks for ties" (that is `rank`) | `rank` leaves gaps; `dense_rank` does not | 11.9 |
| "3-day" moving average | `rowsBetween(-2, 0)` is 3 rows, not 3 days | 3 rows; 3 days needs `rangeBetween` | 11.9 |
| UDF registration | Slides call `udf(f)` "registering"; notebook uses `spark.udf.register` | `udf()` DataFrame only; SQL needs `register` | 11.10 |
| AQE on serverless | "Default since Spark 3.2" vs "Not available in Serverless" | On by default; config not settable on serverless | 12.4 |

**Part V: The Data Lakehouse**

| Topic | Short issue | Resolution | § |
|---|---|---|---|
| Two-tier date | Labelled 2011; paper says cloud lakes replaced HDFS from 2015 | — | 14.4 |
| Lakehouse/Delta timeline | Conflicting dates: 2017/2019/2020/2021 across decks | — | 14.5 |
| Delta: open or proprietary | "Proprietary sink" vs open source (Linux Foundation) | Open source; Databricks adds proprietary extensions | 15.1, 18.5 |
| Partitioning vs Liquid Clustering | LC "replaces" partitioning, yet summary lists partitioning | One or the other on Delta; LC recommended | 15.9, 12.6 |
| Bronze layer | Slides: raw Delta replica; notebook: data rules, Parquet | Slides right: raw, append-only, Delta; rules in Silver | 16.2, 16.3 |
| Pipeline tool name | Delta Live Tables vs Lakeflow (Spark) Declarative Pipelines | — | 16.10 |

**Part VI: Streaming and Graph Analytics**

| Topic | Short issue | Resolution | § |
|---|---|---|---|
| Storing raw input | "Store outputs, not raw input" vs Kappa/Bronze keep raw history | No conflict: raw input retained upstream (Kafka, Bronze) | 17.3 |
| Latencies | 1 ms vs "sub-millisecond"; 100 ms vs "every 1 second" | ~100 ms micro-batch, ~1 ms continuous | 18.1 |
| Default trigger | `ProcessingTime(0)` vs "Default: continuous frequency" | Default = micro-batch ASAP, not Continuous Processing | 18.7 |
| Exactly-once | "Checkpointing ensures exactly-once" vs continuous at-least-once | Micro-batch + replay + checkpoint + idempotent sink; continuous at-least-once | 18.8 |
| Window operations | DStream names (`countByWindow`) mixed into Structured Streaming | — | 18.10 |
| Demo description | Agenda: Colab text stream; demo: Databricks JSON, Spark 2.x | — | 18.15 |
| GraphFrames origin | "GitHub – neo4j" vs "Databricks, UC Berkeley & MIT" | — | 20.1 |
| Subgraph output type | "Output is a DataFrame" vs notebook: returns a Graph | Notebook right: returns a GraphFrame | 20.6 |
| Connected component | "Linked vertices" vs "each pair connected" (clique) | Slide right: reachable via paths; all pairs = clique | 20.8 |

### C.2 Course administration

| Topic | Short issue | Resolution | § |
|---|---|---|---|
| Presentation length | Deck "10m (max)" vs later e-mail 15 min | — | Front matter |
| Submission time | Deck "16 Nov (24:00)" vs later e-mail 23:59 | — | Front matter |
| Final exam dates | January/February dates still "TBD" | — | Front matter |
| ML in the project | "ML model" required; ML sessions come after presentations | `pyspark.ml` pipeline, temporal split, naive baseline | Front matter, 21.3 |
| Project data requirements | "Combine structured/unstructured/streaming" vs "one source (minimum)" | — | Front matter |
| Free vs Community Edition | Free Edition course still shows CE screenshots and names | — | 4.3 |
| Folder and notebook names | ABD26 vs ABD25 screenshots; ABD08 demo vs ABD07 notebook | — | 4.4, 16.5 |
| GraphFrames environment | "In Databricks ML runtime and Colab" vs "not in Free Edition" | — | 20.2 |
