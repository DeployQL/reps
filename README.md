Retrieval Evaluation Pipelines
---
RAG evaluation framework for faster iteration

# Problem Statement
Retrieval Augmented Generation (RAG) relies heavily on retrieval mechanisms
to reduce the chance of hallucination. Within RAG, we've seen many combinations
of ways to build these pipelines. However, performance of RAG depends a lot on the dataset
and the use case, and many blog posts cover evaluation of the same set of parameters -- chunking,
embedding models, and retrieval algorithms. 

As vector indexes grow, it's harder to return precise results.
Top results may include near-duplicate documents or irrelevant information. When
this data is included into the context of an LLM, the LLM gets confused and generates 
a worse response.

Users of that LLM app need to spend more time writing responding and talking to the LLM
without having a lot of context as to how to improve the systems responses.

For LLM app providers, this increases the cost of operations as users send more data to the LLM.

## Developer Pain Points with Retrieval
A lot of development is dependent on evaluating a black box. Retrieval has more well-known
ways to evaluate, but generating an internal dataset can be costly and time consuming. 

Below, we list a few of the different areas where changes can play a large role in improving 
how well RAG performs.
1. Text cleaning
2. Chunking
3. Embedding Model
4. Query expansions
4. Retrieval algorithm (sparse vs dense vs hybrid)
5. Reranking algorithm
6. Number of results to return

Many developers are already evaluating how chunking, processing, and different embeddings
change performance, but this information isn't widely shared or recorded. This may be because
the outcome is so dependent on the data and the use case.

Some popular database solutions are not great at indexing or reindexing a lot of data. 
Elasticsearch has its own set of parameters that drastically change indexing performance. Therefore, the more
we can tune our indexing pipeline towards its optimal setting, the less toil and cost that needs
to be incurred later on.

# Using REPS
REPS enables faster iteration cycles to evaluate retrieval and ranking pipelines. We do this
through integration with MTEB and BEIR datasets. 

Further, REPS helps you build custom datasets for retrieval and ranking through LLM generated
questions and scoring. This isn't a replacement for human labeling, but can be very close.

We believe that moving retrieval pipelines to production is a critical component of this
iteration cycle, and REPS helps make this process simpler and less error prone.

# Benefits of REPS
- Lightweight interfaces with minimal assumptions.
- Enables faster evaluations with pre-built indexes if none of the index parameters are changing.
- Exposes both document and query processing pipelines.
- Built on top of standard LLM evaluation metrics.

## REPS and LlamaIndex
REPS isn't a replacement for LlamaIndex, although we have some overlap.

Both packages provide a Pipeline class to transform a raw document into an embedding. 
REPS is flexible enough that this could wrap LlamaIndex's Pipeline if you wanted. 

One of the ways that REPS is looking to differentiate itself is through support in moving
to production. One of the expectations of REPS' Pipeline is that teams will inherit and version these pipelines.
As such, REPS will continue to add support for making it easier to track how these
pipelines perform and are used across evaluation and production use cases.

## REPS and MTEB
One of the assumptions of MTEB is that it only performs an exhaustive, exact search across all documents.
This is ok for academic purposes where we may not want to evaluate the loss
from an approximate nearest neighbor algorithm, but this could be a concern in a real world scenario where
we want to understand latency and storage tradeoffs. MTEB doesn't provide a way to evaluate this.

REPS integrates into MTEB by replacing some of the MTEB base task classes to remove this assumption.
This also allows better index management, where we can skip indexing and potentially reuse a prebuilt index.

# Roadmap
- [ ] Support all MTEB datasets
- [ ] Pipeline Versioning
- [ ] Index tracking and automatic index reuse
- [ ] Support for automatic dataset generation
- [ ] Support for evaluation over time

# Differentiators
1. Lightweight Pipeline Interfaces
2. Versioning and tracking pipelines
3. Automatic Dataset generation.

# Usage
REPS exposes four fundamental interfaces for you to build upon.
1. Index
2. Query Processor
3. Document Processor
4. Pipeline

```python
    model = FlagModel("BAAI/bge-small-en-v1.5",
                      query_instruction_for_retrieval="Represent this sentence for searching relevant passages: ",
                      use_fp16=True)
    index = QdrantIndex("msmarco", size=5)
    processing = DocumentProcessor()
    eval = MTEB(tasks=[MSMARCOv2()])
    results = eval.run(model, verbosity=2, indexer=index, processor=processing)
```

# What dataset to evaluate on
Building your own internal evaluation dataset is going to be the highest signal while
also being the most time consuming.

REPS is integrated into MTEB and BEIR, enabling evaluation against multiple types of tasks.

Below is a list of the open source evaluation datasets that can be evaluated against. 

{{ MTEB dataset listing }}


# Have questions?
Reach out! Our team has experience working on petabyte-scale search and analytics applications.
We'd love to hear what you're working on.