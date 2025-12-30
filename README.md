AI Agent 101 - Automated Procurement System
An intelligent multi-agent system built with CrewAI that automates the product procurement process by searching, analyzing, and comparing products across multiple e-commerce platforms.
Show Image
 #Overview
This project demonstrates the power of AI agents working together to automate complex procurement tasks. The system uses multiple specialized agents that collaborate to:

Generate intelligent search queries
Search across multiple e-commerce platforms
Extract and analyze product information
Generate comprehensive procurement reports

 #Architecture
The system consists of 4 specialized AI agents working in a sequential process:
┌─────────────────────────────────────────────────────────────┐
│                    Procurement Workflow                      │
├─────────────────────────────────────────────────────────────┤
│  Agent A                                                     │
│  ┌──────────────────────────────────────────┐               │
│  │ Search Query Recommendation Agent        │               │
│  │ - Generates targeted search queries      │               │
│  │ - Focuses on specific brands/models      │               │
│  └──────────────┬───────────────────────────┘               │
│                 │                                            │
│                 ▼                                            │
│  Agent B                                                     │
│  ┌──────────────────────────────────────────┐               │
│  │ Search Engine Agent                      │               │
│  │ - Executes searches via Tavily API       │               │
│  │ - Filters quality results                │               │
│  └──────────────┬───────────────────────────┘               │
│                 │                                            │
│                 ▼                                            │
│  Agent C                                                     │
│  ┌──────────────────────────────────────────┐               │
│  │ Web Scraping Agent                       │               │
│  │ - Extracts product details               │               │
│  │ - Analyzes specifications                │               │
│  │ - Ranks recommendations                  │               │
│  └──────────────┬───────────────────────────┘               │
│                 │                                            │
│                 ▼                                            │
│  Agent D                                                     │
│  ┌──────────────────────────────────────────┐               │
│  │ Procurement Report Author Agent          │               │
│  │ - Generates professional HTML reports    │               │
│  │ - Includes analysis and recommendations  │               │
│  └──────────────────────────────────────────┘               │
└─────────────────────────────────────────────────────────────┘
 #Features

Intelligent Search Query Generation: Automatically generates varied, specific search queries
Multi-Platform Search: Searches across multiple e-commerce platforms simultaneously
AI-Powered Web Scraping: Extracts structured product data using ScrapeGraph AI
Smart Product Ranking: Analyzes and ranks products based on value-for-money
Professional Reports: Generates detailed HTML procurement reports with Bootstrap styling
Local LLM Support: Runs on Ollama with LLaMA 3.1 for privacy and cost-efficiency

 #Prerequisites

Python 3.8+
Google Colab account (recommended) or local Python environment
API Keys:

Tavily Search API - For web search
ScrapeGraph API - For web scraping
AgentOps API (optional) - For monitoring



🛠️ Installation
Google Colab (Recommended)

Open the notebook in Google Colab
Run the installation cells:

python!pip install -U pip
!pip install crewai crewai-tools agentops
!pip install -qU tavily-python scrapegraph-py
!pip install ollama

Install and start Ollama server (handled automatically in the notebook)

Local Installation
bash# Clone the repository
git clone https://github.com/yourusername/ai-agent-101.git
cd ai-agent-101

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Start Ollama server
ollama serve

# Pull the model
ollama pull llama3.1:8b
🔧 Configuration
API Keys Setup
For Google Colab, store your API keys in Colab Secrets:
pythonfrom google.colab import userdata

os.environ["TAVILY_API_KEY"] = userdata.get('tvly-search')
os.environ["SCRAPEGRAPH_API_KEY"] = userdata.get('scrapegraph')
os.environ["AGENTOPS_API_KEY"] = userdata.get('agentops-colab')  # Optional
For local development, create a .env file:
envTAVILY_API_KEY=your_tavily_api_key
SCRAPEGRAPH_API_KEY=your_scrapegraph_api_key
AGENTOPS_API_KEY=your_agentops_api_key  # Optional
System Parameters
pythoncrew_results = rankyx_crew.kickoff(
    inputs={
        "product_name": "coffee machine for the office",
        "websites_list": ["www.amazon.eg", "www.jumia.com.eg", "www.noon.com/egypt-en"],
        "country_name": "Egypt",
        "no_keywords": 5,
        "language": "English",
        "score_th": 0.10,
        "top_recommendations_no": 5
    }
)
# Usage
Quick Start
pythonfrom crewai import Crew, Agent, Task, Process
from langchain_ollama import OllamaLLM

# Initialize LLM
basic_llm = OllamaLLM(
    model="llama3.1:8b",
    base_url="http://127.0.0.1:11434",
    temperature=0
)

# Create and run the crew
rankyx_crew = Crew(
    agents=[
        search_queries_recommendation_agent,
        search_engine_agent,
        scraping_agent,
        procurement_report_author_agent,
    ],
    tasks=[
        search_queries_recommendation_task,
        search_engine_task,
        scraping_task,
        procurement_report_author_task,
    ],
    process=Process.sequential,
    knowledge_sources=[company_context]
)

# Execute
results = rankyx_crew.kickoff(inputs={...})
Output Files
The system generates 4 output files in the ./ai-agent-output directory:

step_1_suggested_search_queries.json - Generated search queries
step_2_search_results.json - Search engine results
step_3_search_results.json - Extracted product details
step_4_procurement_report.html - Final HTML report

📊 Output Example
JSON Output Structure
json{
  "products": [
    {
      "page_url": "https://example.com/product",
      "product_title": "De'Longhi Magnifica S ECAM22.110.B",
      "product_image_url": "https://example.com/image.jpg",
      "product_url": "https://example.com/buy",
      "product_current_price": 8999.00,
      "product_original_price": 12999.00,
      "product_discount_percentage": 30.77,
      "product_specs": [
        {
          "specification_name": "Capacity",
          "specification_value": "1.8L"
        },
        {
          "specification_name": "Pressure",
          "specification_value": "15 bar"
        }
      ],
      "agent_recommendation_rank": 5,
      "agent_recommendation_notes": [
        "Excellent value for money with 30% discount",
        "Premium features at competitive price"
      ]
    }
  ]
}
HTML Report Sections

Executive Summary: Key findings and recommendations
Introduction: Purpose and scope
Methodology: Search and analysis approach
Findings: Detailed product comparisons with tables
Analysis: Trends and observations
Recommendations: Best purchase options
Conclusion: Final summary
Appendices: Raw data and supplementary information

🤖 Agent Details
Agent A: Search Query Recommendation Agent

Role: Generate targeted search queries
Output: List of specific, varied search terms
Focus: Brand names, models, and technologies

Agent B: Search Engine Agent

Role: Execute web searches
Tool: Tavily Search API
Output: Filtered, high-quality search results

Agent C: Web Scraping Agent

Role: Extract product information
Tool: ScrapeGraph AI
Output: Structured product data with specifications and pricing

Agent D: Procurement Report Author Agent

Role: Generate professional reports
Output: HTML report with Bootstrap styling
Features: Charts, tables, and detailed analysis

# Advanced Features
Knowledge Sources
pythoncompany_context = StringKnowledgeSource(
    content="Rankyx is a company that provides AI solutions..."
)
Custom Tools
python@tool
def search_engine_tool(query: str):
    """Search for products using Tavily API"""
    return search_client.search(query)

@tool
def web_scraping_tool(page_url: str):
    """Extract product details using ScrapeGraph AI"""
    return scrape_client.smartscraper(
        website_url=page_url,
        user_prompt="Extract product details..."
    )
Structured Output with Pydantic
pythonclass SingleExtractedProduct(BaseModel):
    page_url: str
    product_title: str
    product_current_price: float
    product_specs: List[ProductSpec]
    agent_recommendation_rank: int
    agent_recommendation_notes: List[str]
#Performance & Monitoring

AgentOps Integration: Track agent performance and interactions
Local LLM: Cost-effective with Ollama and LLaMA 3.1
Scalable: Process multiple products simultaneously

#Best Practices

Rate Limiting: Implement delays between requests
Error Handling: Wrap API calls in try-except blocks
Data Validation: Use Pydantic models for structured output
Monitoring: Enable AgentOps for production deployments
Caching: Store results to avoid redundant API calls

#Troubleshooting
Ollama Server Issues
bash# Check if Ollama is running
curl http://127.0.0.1:11434/api/tags

# Restart Ollama
pkill ollama
ollama serve
API Rate Limits
pythonimport time

# Add delays between requests
time.sleep(2)
Memory Issues in Colab
python# Clear memory
import gc
gc.collect()
# Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

Fork the repository
Create your feature branch (git checkout -b feature/AmazingFeature)
Commit your changes (git commit -m 'Add some AmazingFeature')
Push to the branch (git push origin feature/AmazingFeature)
Open a Pull Request

# License
This project is licensed under the MIT License - see the LICENSE file for details.
🙏 Acknowledgments

CrewAI - Multi-agent orchestration framework
Ollama - Local LLM runtime
Tavily - Web search API
ScrapeGraph AI - AI-powered web scraping
LLaMA 3.1 - Open-source language model

# Contact
For questions or support, please open an issue on GitHub or contact mohamedkhaledidris@gmail.com
