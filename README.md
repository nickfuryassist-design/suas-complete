# AI Trading Research Platform — Prototype

An AI-native trading research interface that translates natural language market hypotheses into structured backtest experiments, identifies missing strategy parameters, and executes simulated backtests.

## Overview

The **AI Trading Research Platform** allows users to describe a trading idea in natural language instead of manually configuring every backtesting parameter.

For example:

> "Does buying NIFTY after a 1% fall work in high volatility?"

The platform uses AI to understand the hypothesis, convert it into a structured experiment state, identify missing parameters, and ask the user for clarification when required.

Once all required parameters are available, the user can execute a simulated backtest.

## How It Works

### 1. User Query Input

The user submits a natural language trading hypothesis.

Example:

```text
Does buying NIFTY after a 1% fall work in high volatility?
```

### 2. State Merging

The frontend sends the new query along with the existing `previousState`.

This allows the system to preserve information from previous interactions when the user is answering a clarification question.

### 3. Structured Parsing

The FastAPI backend invokes OpenAI's native `.parse()` mode using a strict `ExperimentState` Pydantic model.

The natural language query is converted into a structured experiment representation.

### 4. Ambiguity Resolution

If critical backtesting parameters are missing, the system sets:

```json
{
  "isReadyToRun": false
}
```

and returns a direct clarification question to the user.

For example, the system may ask for a missing date range, entry condition, or other required strategy parameter.

### 5. Execution Trigger

Once all required parameters have been populated:

```json
{
  "isReadyToRun": true
}
```

the UI enables the full backtest execution.

## Technologies Used

### Frontend

* **React.js**
* **Tailwind CSS**
* **Lucide Icons**

### Backend

* **FastAPI**
* **Python**
* **Pydantic**
* **uv**

### AI Integration

* **OpenAI API**
* **Pydantic Structured Outputs**
* Guaranteed JSON extraction

### Containerization

* **Docker**

## Project Structure

```text
.
├── frontend/
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── ...
│
|── backend/
   ├── main.py
   ├── models/
   ├── ...
   ├── pyproject.toml
   └── ...
```
