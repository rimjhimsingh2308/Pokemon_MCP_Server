# 🧠 Pokémon Battle Simulation – MCP Server

This project implements a Model Context Protocol (MCP) Server that provides AI models with access to two resources:

1. **Pokémon Data Resource** — exposes comprehensive Pokémon data
2. **Battle Simulation Tool** — allows LLMs to simulate battles between any two Pokémon

---

## 📚 What is MCP?

MCP (Model Context Protocol) is an open protocol that standardizes how applications provide context to LLMs. More info: [modelcontextprotocol.io](https://modelcontextprotocol.io/introduction)

This project implements an MCP-compatible server using FastAPI, allowing LLMs to interact with Pokémon data and simulate turn-based battles through structured APIs.

---

## 🚀 Features

### ✅ Pokémon Data Resource
- Connects to public Pokémon datasets using [PokeAPI](https://pokeapi.co)
- Provides information such as:
  - Base Stats: HP, Attack, Defense, Sp. Attack, Sp. Defense, Speed
  - Pokémon Types
  - Abilities
  - Available Moves and their Effects
  - Evolution Data
- Exposes endpoints following MCP resource design to make this accessible to LLMs

### ✅ Battle Simulation Tool
- Accepts any two Pokémon names
- Simulates a turn-based battle based on:
  - Type effectiveness (e.g. Water > Fire)
  - Base stats and move power
  - Speed for turn order
  - Random damage variation
  - Status effects (e.g. Burn, Paralysis, Poison)
- Returns detailed battle logs and winner

---

## 📁 Project Structure

pokemon-mcp-server/
│
├── app/
│ ├── init.py
│ ├── main.py # FastAPI app
│ ├── data_resource.py # Pokémon data logic
│ ├── battle_simulator.py # Battle logic
│
├── requirements.txt
├── README.md


---

## ⚙️ Installation & Setup

### 🐍 Prerequisites
- Python 3.8 or later
- pip

### ✅ Step-by-Step

1. **Clone the repo**
bash
git clone <your-repo-url>
cd pokemon-mcp-server

2. **Create a virtual environment**

bash
Copy code
python -m venv venv
source venv/bin/activate      # On Unix/macOS
venv\Scripts\activate         # On Windows

3. **Install dependencies**

bash
pip install -r requirements.txt

4. **Run the FastAPI server**

bash
uvicorn app.main:app --reload

You should now see:

nginx
Uvicorn running on http://127.0.0.1:8000


🔍 API Usage
🧪 Test from Swagger UI
Visit:

http://127.0.0.1:8000/docs


📌 Endpoints
1. Get Pokémon Info
Endpoint: /pokemon/{name}

Method: GET

Example:

bash
GET http://127.0.0.1:8000/pokemon/pikachu
Response:

json
{
  "name": "pikachu",
  "types": ["electric"],
  "stats": {
    "hp": 35,
    "attack": 55,
    ...
  },
  "abilities": ["static", "lightning-rod"],
  "moves": [...],
  "evolution_chain": [...]
}
2. Simulate Battle
Endpoint: /simulate_battle

Method: POST

Body:

json
{
  "pokemon1": "pikachu",
  "pokemon2": "squirtle"
}
Response:

json
{
  "battle_log": [...],
  "winner": "squirtle"
}


🤖 MCP Compliance
This project follows the MCP protocol by:

Structuring API responses in JSON for LLM consumption

Using standardized resource (GET) and tool (POST) interfaces

Simulating interactive tools accessible via LLM prompts

📄 How an LLM Would Query This Resource
Example (natural language prompt to an LLM):

"Use the Pokémon Data Resource to fetch stats for Pikachu."

LLM converts to:

http
GET /pokemon/pikachu
"Simulate a battle between Charmander and Bulbasaur using the Battle Simulation Tool."

LLM converts to:

http
POST /simulate_battle
{
  "pokemon1": "charmander",
  "pokemon2": "bulbasaur"
}
🧪 Testing via Postman
You can also test APIs manually with Postman:

Pokémon Info
Method: GET

URL: http://127.0.0.1:8000/pokemon/bulbasaur

Battle Simulation
Method: POST

URL: http://127.0.0.1:8000/simulate_battle

Body (raw JSON):

json
{
  "pokemon1": "bulbasaur",
  "pokemon2": "charmander"
}