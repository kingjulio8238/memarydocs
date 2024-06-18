# Future Features 
### Speak to Your Agents Memory 
!!! example "COMING SOON"
memary's **chat interface** offers a portal to access agent memories, integrating capabilitiies such as **searching**, **removing** and viewing agent memories **over specified periods** and more all under one umbrella available in your dashboard. 

### Track Memories 
!!! example "COMING SOON"
memary **breaks down agent memory for each response generated**. A list of agent responses with their respective memories will be avilable in your dashbord. Human input (good/bad response) can help your systems improve. 

### Audience Preferences 
!!! example "COMING SOON"
Through our proprietary memory modules, we are able to infer audience preferences for certain time periods. Audiences' **best and most recent** preferences are continously updated and will be available in your dashboard.  

### Customizable Memory  
!!! example "COMING SOON" 
memary deploys knowledge graphs to **track agent actions**. View, search and configure memory for your purposes. Join different memories together for improved retrieval and toggle between your favorite graph providers. All available in your dashboard.  

### Playgrounds 
!!! example "COMING SOON" 
- **Tool** Playground: Simply define python functions and add it as one of your agent tools. View all available tools and remove any if necessary. Do this all in your dashboard!
- **Model** Playground: Select specific models for tasks across memary to lower system LLM costs. All models deployed on HF will be avilable in your dashboard.  
- **Benchmarking** Playground: Easily run different memary configurations against each other to evaluate which memory options are more suitable for a specific task. 

### memaryParse 
!!! example "COMING SOON" 
Parse and clean your proprietry data before inserting into your agent memory. memary **supports various file types** including table and image extraction. Combine different parsers to form a **parent parser** with advanced capabilities. Also access templates for predefined database schemas and set of node relationships or **define your own!** This is all available in your dashboard. 

### memaryRetrieval 
!!! example "COMING SOON" 
Use different techniques to retrieve agent memory. Also combine various retrievers to form a **parent retriever** with advanced capabilities. All avilable in your dashboard. 

<!--
As mentioned, memary will benefit from the following integrations:

- Create an LLM Judge that scores the ReACT agent forming a feedback loop. See [Zooter](https://arxiv.org/abs/2311.08692) for insights.
- Expand the knowledge graph’s capabilities to support multiple modalities, i.e., images.
- Optimize the graph to reduce latency of search times.
- Instead of extracting the top N entities from the entity knowledge store deploy more advanced memory compression techniques such as extracting only the entities included in the agent’s response.

!!! note "memary's Future Structure" 
    Currently memary is structured so that the ReAct agent can only process one query at a time. We hope to see **multiprocessing** integrated so that the agent can process many subqueries simultaneously. We expect this to improve the relevancy and accuracy of responses. The source code for both decomposing the query and reranking the many agent responses has been provided, and once multiprocessing has been added to the system, these components can easily be integrated into the main `ChatAgent` class. The diagram below shows how the newly integrated system would work.

![memary_final](https://github.com/kingjulio8238/memary/blob/main/diagrams/final.png?raw=true) 

## Query Decomposition 
![QD](https://github.com/kingjulio8238/memary/blob/main/diagrams/query_decomposition.png?raw=true) 

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

!!! note "Purpose in larger system"
    - In a parallel system, the agent will be able to parse multiple queries at once. The query decomposer (QD) will pass all subqueries (or original query if no subqueries exist) to the agent at once.
    - Simultaneously, QD will pass the original query to the reranking module to rerank the agent responses based on their relevance to the pre-decomposed query.

!!! info "Future Contributions"
    Self-Learning: Whenever queries are decomposed, those examples will be appended to the engine’s example store as a **feedback loop** for improved future performance.

## Reranking 
![reranking](https://github.com/kingjulio8238/memary/blob/main/diagrams/reranking_diagram.png?raw=true) 

### Why rerank responses? 
Ensure that the various responses to subqueries, when merged, are relevant to the original query prior to decomposition.

### Our approach 
- We benchmarked three models to determine which one would best work for reranking: **BM25 Reranking Fusion, Cohere Rerank, and ColBERT Rerank**. After testing BM25, it was clear that the model was not able to classify the different responses and provide a simple merged answer. Instead of answering the question, it combined all the information on the page, introducing irrelevant information.
- Next, when testing out Cohere, the model performed better than BM25 but was still not classifying the paragraphs well. The reranking was not always accurate, as it performed well for some questions but was not able to rank others. Furthermore, the ranking was still pretty inaccurate, performing between 0.25 - 0.5 out of 1.
- Finally, we tested ColBERT rerank, and it was found that this model performed best compared to the other two. ColBERT was able to synthesize results from the given data and ranked them very accurately, with reranking scores between 0.6 - 0.7 out of 1. With this, ColBERT had the most potential, being able to determine which responses were most related and important to answering the query.

!!! note "Purpose in larger system"
    Passes the reranking result to the knowledge graph for storage and to the new context window as one source of context for inference.

!!! info "Future Contributions"
    Future Benchmarking: Include the Cohere Rerank 3 model and others in the reranking [analysis](https://docs.google.com/document/d/1gHzvgktqnHcg7wbIuKHr6W5NMYk6UVlJkRQfSqzk9e4/edit). The data used for benchmarking can be found [here](https://docs.google.com/document/d/1knfJRsoEzjKziilmF_ZwSwMRBvYbF0yNlRdpDteDiW4/edit?usp=sharing). Add to it!

-->