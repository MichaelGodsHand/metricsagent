# Brand Research MeTTa Agent

A demonstration of how to integrate **SingularityNET's MeTTa Knowledge Graph** with **Fetch.ai's uAgents** to create intelligent, autonomous agents that can understand and respond to brand research queries using structured knowledge reasoning.

## 🤖 What This Agent Does

This agent connects to your brand research knowledge graph (hosted via ngrok) and provides intelligent responses about:

- **Brand Research**: Comprehensive analysis of brands including web results, reviews, Reddit discussions, and social media sentiment
- **Sentiment Analysis**: Detailed sentiment breakdown across different platforms
- **Competitor Analysis**: Suggestions for comparing brands and competitive intelligence
- **FAQ Support**: General brand research assistance

## 🔗 Integration with Your Knowledge Graph

The agent connects to your brand research orchestrator's knowledge graph via the ngrok tunnel:

- **Base URL**: `https://ba02caeecf6f.ngrok-free.app` (update this in `brand/brandrag.py`)
- **Endpoints Used**:
  - `GET /kg/get_all_brands` - Get all available brands
  - `GET /kg/query_brand_data` - Query specific brand data
  - `GET /kg/get_brand_summary` - Get comprehensive brand summary

## 🏗️ Project Architecture

### Core Components

1. **`agent.py`**: Main uAgent implementation with Chat Protocol
2. **`brand/knowledge.py`**: Local MeTTa knowledge graph for FAQs
3. **`brand/brandrag.py`**: Brand RAG system that queries your external knowledge graph
4. **`brand/utils.py`**: LLM integration and query processing logic

### Data Flow

User Query → Intent Classification → External KG Query → Knowledge Retrieval → LLM Response → User

## 🚀 Setup Instructions

### Prerequisites

- Python 3.11+
- ASI:One API key
- AGENTVERSE_API_KEY
- Your brand research orchestrator running with ngrok tunnel

### Installation

1. **Create virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up environment variables**:
   ```bash
   cp env.example .env
   # Edit .env with your ASI:One API key and AGENTVERSE_API_KEY
   ```

4. **Update ngrok URL**:
   - Edit `brand/brandrag.py`
   - Update `self.kg_base_url` with your current ngrok URL

5. **Run the agent**:
   ```bash
   python agent.py
   ```

## 🧪 Testing the Agent

### REST API Testing (Postman)

1. **Start the agent**:
   ```bash
   python agent.py
   ```

2. **Test REST endpoints**:

   **Comprehensive Brand Research**:
   ```
   POST http://localhost:8006/brand/research
   Content-Type: application/json
   
   {
     "brand_name": "Tesla"
   }
   ```

   **General Brand Query**:
   ```
   POST http://localhost:8006/brand/query
   Content-Type: application/json
   
   {
     "query": "Tell me about Apple's sentiment analysis"
   }
   ```

   **Specific Brand Data**:
   ```
   POST http://localhost:8006/brand/data
   Content-Type: application/json
   
   {
     "brand_name": "Nike",
     "data_type": "reviews",
     "sentiment": "negative"
   }
   ```

   **Brand Summary**:
   ```
   POST http://localhost:8006/brand/summary
   Content-Type: application/json
   
   {
     "brand_name": "Samsung"
   }
   ```

   **Get All Brands**:
   ```
   GET http://localhost:8006/brands/all
   ```

### Chat Interface Testing

1. **Access the inspector**:
   Visit the URL shown in the console and connect via Mailbox

2. **Test queries**:
   - "What brands do you have data for?"
   - "Tell me about Tesla's sentiment analysis"
   - "How do Apple's reviews compare to Samsung?"
   - "What are the negative reviews for Nike?"

## 🔧 Key Features

### 1. **Dynamic Knowledge Retrieval**
The agent queries your external knowledge graph in real-time:
```python
brand_summary = rag.get_brand_summary("Tesla")
sentiment_data = rag.query_reviews("Tesla", "negative")
```

### 2. **Intent Classification**
Uses ASI:One to classify user intent:
- `brand_research`: Comprehensive brand analysis
- `sentiment_analysis`: Sentiment breakdown across platforms
- `competitor_analysis`: Competitive intelligence suggestions
- `faq`: General assistance

### 3. **Structured Reasoning**
MeTTa enables complex logical reasoning with your brand data:
```python
# Get comprehensive brand data
web_results = rag.query_web_results("Tesla")
positive_reviews = rag.query_reviews("Tesla", "positive")
negative_social = rag.query_social_comments("Tesla", "negative")
```

### 4. **Agentverse Integration**
The agent automatically:
- Registers on Agentverse for discovery
- Implements Chat Protocol for ASI:One accessibility
- Provides a web interface for testing

## 🔄 Updating the ngrok URL

When your ngrok URL changes, update it in `brand/brandrag.py`:

```python
class BrandRAG:
    def __init__(self, metta_instance):
        self.metta = metta_instance
        # Update this with your current ngrok URL
        self.kg_base_url = "https://your-new-ngrok-url.ngrok-free.app"
```

## 📋 Example Queries

- **Brand Research**: "Tell me everything about Tesla"
- **Sentiment Analysis**: "What's the sentiment around Apple products?"
- **Competitor Analysis**: "How does Nike compare to Adidas?"
- **Specific Data**: "Show me negative Reddit discussions about Meta"
- **General Help**: "What brands do you have data for?"

## 🔗 Useful Links

- [MeTTa Documentation](https://metta-lang.dev/docs/learn/tutorials/python_use/metta_python_basics.html)
- [Fetch.ai uAgents](https://innovationlab.fetch.ai/resources/docs/examples/chat-protocol/asi-compatible-uagents)
- [Agentverse](https://agentverse.ai/)
- [ASI:One](https://asi1.ai/)
