# Build 

memary assumes the local installation method. memary currently supports the use of:

- `Llama 3 8B` running locally using Ollama OR `gpt-3.5-turbo`
- `LLaVA` running locally using Ollama OR `gpt-4-vision-preview`

memary will default to the locally run models unless explicitly specified. Additionally, memary allows developers to **easily switch between downloaded models** via Ollama 

## To run memary 
1. (Optional) If running models locally using Ollama, follow this the instructions in this [repo] (https://github.com/ollama/ollama). 

2. Ensure that an `.env` exists with any necessary API keys and Neo4j credentials 
```
OPENAI_API_KEY=YOUR_API_KEY
NEO4J_PW=YOUR_NEO4J_PW
NEO4J_URL=YOUR_NEO4J_URL
PERPLEXITY_API_KEY=YOUR_API_KEY
GOOGLEMAPS_API_KEY=YOUR_API_KEY
ALPHA_VANTAGE_API_KEY=YOUR_API_KEY
```

3. Update user persona which can be found in `streamlit_app/data/user_persona.txt`. Instructions have been provided for customization - **replace curly brackets with relevant information**.

4. (Optional) Update system persona, if needed, which can be found in `streamlit_app/data/system_persona.txt`. 

5. Run: 
```
cd streamlit_app
streamlit run app.py
```