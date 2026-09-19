
# AI Stock Screener Agent

An AI-powered stock screening agent built with **LangGraph, Llama 3.2, LangChain, and yfinance**. The agent accepts natural-language stock screening requests, selects an appropriate screening tool, retrieves market data through Yahoo Finance, and presents the results in a human-readable format.

## Overview

Traditional stock screeners require users to manually select predefined filters. This project adds an AI layer that allows users to interact with the stock screener using natural language.

For example, instead of manually selecting a screening category, a user can ask:

> "Find some undervalued large-cap stocks."

The LLM interprets the request, selects the appropriate screening tool, retrieves the results, and generates a human-readable response.

## Architecture

```text
                    User
                     │
                     ▼
                Llama 3.2
                     │
                     │ Tool Selection
                     ▼
             simple_screener()
                     │
                     ▼
                  yfinance
                     │
                     ▼
              Yahoo Finance
                     │
                     ▼
              Screened Stocks
                     │
                     ▼
                Llama 3.2
                     │
                     ▼
             Final Response
````

## How It Works

1. The user provides a stock-related request in natural language.
2. The LLM interprets the request.
3. LangGraph manages the agent workflow.
4. The LLM decides whether the `simple_screener` tool is required.
5. The tool calls `yfinance` to access Yahoo Finance's screening functionality.
6. Yahoo Finance returns screened stock data.
7. The tool extracts relevant fields from the response.
8. The results are returned to the LLM.
9. The LLM converts the structured data into a human-readable response.

## LangGraph Workflow

The agent follows a tool-calling workflow:

```text
START
  │
  ▼
Chatbot
  │
  ▼
Does the LLM need a tool?
  │
  ├── No ──► END
  │
  └── Yes
       │
       ▼
     Tools
       │
       ▼
     Chatbot
       │
       ▼
      END
```

LangGraph is used to maintain the conversation state and control the transition between the LLM and the tool.

## Technologies Used

* **Python**
* **LangGraph** – Agent workflow and state management
* **LangChain** – Tool integration
* **Llama 3.2** – Local LLM
* **Ollama** – Local LLM runtime
* **yfinance** – Financial market data and stock screening
* **Yahoo Finance** – Financial data source
* **UV** – Python dependency and environment management

## Stock Screening

The current implementation uses predefined Yahoo Finance screening categories available through `yfinance`, including:

* Day Gainers
* Day Losers
* Most Actives
* Undervalued Large Caps
* Undervalued Growth Stocks
* Aggressive Small Caps
* Growth Technology Stocks
* Most Shorted Stocks

The screening tool supports pagination through an `offset` parameter and retrieves a limited number of results per request.

## Example Prompts

```text
Find today's top gaining stocks.
```

```text
Find some undervalued large-cap stocks.
```

```text
Show me stocks that are actively traded.
```

```text
Find small-cap stocks with strong growth characteristics.
```

## Project Structure

```text
stock-screener-ai-agent/
│
├── flow.py              # LangGraph agent and workflow
├── tool.py              # Stock screening tool
├── pyproject.toml       # Project dependencies and configuration
├── uv.lock              # Locked Python dependencies
├── graphdiagram.png     # Agent workflow diagram
├── toolhelpers.txt      # Tool-related notes
├── .gitignore
└── README.md
```

## Running the Project

### 1. Install UV

```bash
pip install uv
```

### 2. Install Project Dependencies

```bash
uv sync
```

### 3. Make Sure Ollama Is Running

The project uses a local Llama 3.2 model through Ollama.

Check installed models:

```bash
ollama list
```

If Llama 3.2 is not installed:

```bash
ollama pull llama3.2
```

Start the Ollama server if required:

```bash
ollama serve
```

### 4. Run the Agent

```bash
uv run flow.py
```

The application will prompt:

```text
🤖 Pass your prompt here:
```

Enter a natural-language stock screening request.

## Example Workflow

A request such as:

```text
Find some undervalued large-cap stocks.
```

is processed approximately as:

```text
Natural-Language Request
          │
          ▼
        Llama
          │
          ▼
  Select Screening Tool
          │
          ▼
   simple_screener()
          │
          ▼
       yfinance
          │
          ▼
    Yahoo Finance
          │
          ▼
   Screened Stock Data
          │
          ▼
        Llama
          │
          ▼
    Final Response
```

## Why Use an AI Agent?

A traditional stock screener could directly call a screening function when a user selects a predefined category.

The AI layer provides a natural-language interface and allows the LLM to decide when a screening tool should be used.

For example:

```text
User:
"Show me some undervalued large companies."

        ↓

LLM:
Understands the user's intent

        ↓

Tool:
simple_screener("undervalued_large_caps", 0)

        ↓

Yahoo Finance:
Returns screened stocks

        ↓

LLM:
Converts the results into a natural-language response
```

This demonstrates **LLM tool calling and agent-based workflow orchestration**.

## Current Limitations

* The current version relies on predefined screening queries.
* The tool returns only a selected subset of available stock fields.
* The project is intended as an AI-agent prototype rather than a production-grade financial platform.
* Financial data availability depends on the underlying data provider.
* LLM-generated explanations should not be treated as independently verified financial information.
* The project does not provide personalized investment advice.

## Future Improvements

Potential improvements include:

* Support for custom screening conditions
* Natural-language-to-financial-filter conversion
* Additional financial data providers
* Data validation before analysis
* More detailed fundamental analysis
* Historical performance analysis
* Company news integration
* Multi-tool financial research
* Structured output and ranking
* Web/API deployment
* Automated evaluation of agent responses

## Disclaimer

This project is intended for educational and research purposes only. It does not provide financial advice or guarantee investment performance.

````




