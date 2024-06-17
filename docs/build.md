# Quickstart 

## Install memary's package  
!!! tip "1st Option"
    Make sure you are running python version <= 3.11.9, then run 
    ```
    pip install memary
    ```

!!! tip "2nd Option"
    You can also install memary locally: 

    i. Create a virtual environment with the python version set as specified above 

    ii. Install python dependencies: 
    ```
    pip install -r requirements.txt
    ```

## Specify models used   
???+ note "Local OS Support"

    At the time of writing, memary assumes installation of local models and we currently support all models available through **Ollama**:

    - LLM running locally using Ollama (`Llama 3 8B/40B` as suggested defaults) **OR** `gpt-3.5-turbo`
    - Vision model running locally using Ollama (`LLaVA` as suggested default) **OR** `gpt-4-vision-preview`

    memary will default to the locally run models unless explicitly specified. Additionally, memary allows developers to **easily switch between downloaded models**. 


## Run memary 
- (Optional) If running models locally using Ollama, follow the instructions in this [repo](https://github.com/ollama/ollama). 


- Ensure that an `.env` exists with any necessary API keys and Neo4j credentials 

??? info ".env"
    ```
    OPENAI_API_KEY=YOUR_API_KEY
    NEO4J_PW=YOUR_NEO4J_PW
    NEO4J_URL=YOUR_NEO4J_URL
    PERPLEXITY_API_KEY=YOUR_API_KEY
    GOOGLEMAPS_API_KEY=YOUR_API_KEY
    ALPHA_VANTAGE_API_KEY=YOUR_API_KEY
    ```

- Update user persona which can be found in `streamlit_app/data/user_persona.txt` using the user persona template which can be found in `streamlit_app/data/user_persona_template.txt`. Instructions have been provided for customization - **replace the curly brackets with relevant information**.

- (Optional) Update system persona, if needed, which can be found in `streamlit_app/data/system_persona.txt`. 

!!! tip "Lastly Run"
    ```
    cd streamlit_app
    streamlit run app.py
    ```

## More Basic Functionality  
``` py title="memary_usage" hl_lines="1"
from memary.agent.chat_agent import ChatAgent

system_persona_txt = "data/system_persona.txt"
user_persona_txt = "data/user_persona.txt"
past_chat_json = "data/past_chat.json"
memory_stream_json = "data/memory_stream.json"
entity_knowledge_store_json = "data/entity_knowledge_store.json"
chat_agent = ChatAgent(
    "Personal Agent",
    memory_stream_json,
    entity_knowledge_store_json,
    system_persona_txt,
    user_persona_txt,
    past_chat_json,
)
```
!!! note "Agent Configuration"
    Pass in subset of the template tools `['search', 'vision', 'locate', 'stocks']` as `include_from_defaults` for different set of default tools upon initialization. Tools can be imported to configure the agents capabilties. 

### Adding Custom Tools 
``` py title="add_tool" hl_lines="5"
def multiply(a: int, b: int) -> int:
    """Multiply two integers and returns the result integer"""
    return a * b

chat_agent.add_tool({"multiply": multiply})
```

!!! note "ReAct Custom Tools"
    More information about creating custom tools for the LlamaIndex ReAct Agent can be found [here](https://docs.llamaindex.ai/en/stable/examples/agent/react_agent/). 

### Removing Custom Tools 
``` py title="remove_tool" hl_lines="5"
def multiply(a: int, b: int) -> int:
    """Multiply two integers and returns the result integer"""
    return a * b

chat_agent.remove_tool("multiply")
```

<!-- TODO: add 3rd example>

<!-- TODO: add api key>



<!--
IDEAL PAGE SETUP 
1 Install the memary SDK 
2. Add X lines of code 
   - what happens after lines of code are executed 
3. set your api key 
4. run your agent 
5. Access Memory Dashboard for insights 
    - demo video 

Basic functionality 
- 1st ex 
- 2nd ex 
- 3rd ex 

Example Code 
- complete code from sections above 
- link to google colab notebook to run (demo) - one used for llama index 

Explore more advanced functionality! 
- 1st ex linked 
- 2nd ex linked 
-->