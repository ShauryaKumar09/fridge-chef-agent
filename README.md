# Fridge Chef Agent

A small LangChain agent that turns the ingredients you have on hand into a
recipe. You tell it what's in your fridge (e.g. "I have two eggs, some flour,
and milk") and it suggests a beginner-friendly, budget-conscious recipe with
cooking instructions. The agent has access to a `web_search` tool (powered by
[Tavily](https://tavily.com/)) so it can look things up while planning a dish.



## What's in here

- `chef_project.ipynb` — the notebook containing the agent. It:
  - loads environment variables with `python-dotenv`
  - initializes a chat model via `init_chat_model`
  - defines a Tavily-backed `web_search` tool
  - builds an agent with `create_agent` and a "personal chef" system prompt
  - invokes the agent with a sample list of ingredients

## Setup

1. Create and activate a virtual environment (optional but recommended):

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Copy the example environment file and fill in your Tavily API key:

   ```bash
   cp .env.example .env
   # then edit .env and set TAVILY_API_KEY
   ```

   You can get a Tavily API key at https://tavily.com/.

4. Open the notebook and run the cells:

   ```bash
   jupyter notebook chef_project.ipynb
   ```

## Model note (important)

The notebook is configured to use a **local [Ollama](https://ollama.com/)
instance**:

```python
model = init_chat_model(
    model="ollama:gpt-oss:20b",
    temperature=1.0,
    base_url="http://192.168.6.136:11434",
)
```

This expects an Ollama server reachable at the `base_url` above with the
`gpt-oss:20b` model pulled. If you don't have a local Ollama instance (or it's at
a different address), you'll need to swap the `init_chat_model` call for a model
you do have access to. For example, to use Anthropic's Claude instead:

```python
model = init_chat_model(
    model="anthropic:claude-sonnet-4-6",
    temperature=1.0,
)
```

(That route requires the corresponding provider package and an `ANTHROPIC_API_KEY`
in your `.env`.) Or point `base_url` at your own Ollama host and pull a model of
your choosing.
