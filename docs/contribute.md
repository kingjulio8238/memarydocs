# Future Integrations 

As mentioned, memary will benefit from the following integrations:

- Create an LLM Judge that scores the ReACT agent forming a feedback loop. See [Zooter] (https://arxiv.org/abs/2311.08692) for insights.
- Expand the knowledge graph’s capabilities to support multiple modalities, i.e., images.
- Optimize the graph to reduce latency of search times.
- Instead of extracting the top N entities from the entity knowledge store deploy more advanced memory compression techniques such as extracting only the entities included in the agent’s response.

Currently memary is structured so that the ReAct agent can only process one query at a time. We hope to see **multiprocessing** integrated so that the agent can process many subqueries simultaneously. We expect this to improve the relevancy and accuracy of responses. The source code for both decomposing the query and reranking the many agent responses has been provided, and once multiprocessing has been added to the system, these components can easily be integrated into the main `ChatAgent` class. The diagram below shows how the newly integrated system would work.

<!-- insert pic of memary as a whole with QD & reranking -->

## Query Decomposition 
<!-- insert pic of QD -->
### Why decompose? 
- User queries are complex and multifaceted, and base-model LLMs are often unable to fully understand all aspects of the query in order to create a succinct and accurate response.
- Allows an LLM of similar capabilities to answer easier questions and synthesize those answers to provide an improved response.

### How it works 
- Initially, a LlamaIndex fine-tuned query engine approach was taken. However, the LangChain query engine was found to be faster and easier to use. LangChain’s PydanticToolsParser framework was used. The query_engine_with_examples has been given 87 pre-decomposed queries (complex query + set of subqueries) to determine a pattern. Users can invoke the engine with individual queries or collate them into a list and invoke them by batch.

- Individual Invocation: 
``` py title="Individual decomposition" 
sub_qs = query_analyzer_with_examples.invoke( {"question": "What is 2 + 2? Why is it not 3?"} )
```

- Batch Invocation:
``` py title="Batch decomposition" 
questions = ["Where can I buy a Macbook Pro with an M3 chip? What is the difference to the M2 chip? How much more expensive is  the M3?", "How can I buy tickets to the upcoming NBA game? What is the price of lower bowl seats versus nosebleeds? What is the view like at either seat?", "Between a Macbook and a Windows machine, which is better for systems engineering? Which chips are most ideal? What is the price difference between the two?",] 

responses = [] for question in questions: 
responses.append(query_analyzer_with_examples.invoke({"question": question}))
```

### Purpose in larger system 
- In a parallel system, the agent will be able to parse multiple queries at once. The query decomposer (QD) will pass all subqueries (or original query if no subqueries exist) to the agent at once.
- Simultaneously, QD will pass the original query to the reranking module to rerank the agent responses based on their relevance to the pre-decomposed query.

### Future Contributions 
- Self-Learning: Whenever queries are decomposed, those examples will be appended to the engine’s example store as a **feedback loop** for improved future performance.

## Reranking 
<!-- insert pic of reranking -->
### Why rerank responses? 
Ensure that the various responses to subqueries, when merged, are relevant to the original query prior to decomposition.

### Our approach 
- We benchmarked three models to determine which one would best work for reranking: **BM25 Reranking Fusion, Cohere Rerank, and ColBERT Rerank**. After testing BM25, it was clear that the model was not able to classify the different responses and provide a simple merged answer. Instead of answering the question, it combined all the information on the page, introducing irrelevant information.
- Next, when testing out Cohere, the model performed better than BM25 but was still not classifying the paragraphs well. The reranking was not always accurate, as it performed well for some questions but was not able to rank others. Furthermore, the ranking was still pretty inaccurate, performing between 0.25 - 0.5 out of 1.
- Finally, we tested ColBERT rerank, and it was found that this model performed best compared to the other two. ColBERT was able to synthesize results from the given data and ranked them very accurately, with reranking scores between 0.6 - 0.7 out of 1. With this, ColBERT had the most potential, being able to determine which responses were most related and important to answering the query.

### Purpose in larger system 
- Passes the reranking result to the knowledge graph for storage and to the new context window as one source of context for inference.

### Future Contributions 
- Future Benchmarking: Include the Cohere Rerank 3 model and others in the [reranking analysis] (https://docs.google.com/document/d/1gHzvgktqnHcg7wbIuKHr6W5NMYk6UVlJkRQfSqzk9e4/edit). The data used for benchmarking can be found [here] (https://docs.google.com/document/d/1knfJRsoEzjKziilmF_ZwSwMRBvYbF0yNlRdpDteDiW4/edit?usp=sharing). Add to it!

