# MECE Cart Abandonment Segmentation System

An AI-powered customer segmentation system that uses LLM jury and judge methodology to create mutually exclusive, collectively exhaustive (MECE) segments for cart abandonment campaigns.

## Overview

This system generates realistic cart abandonment data and uses multiple AI models to create marketing-friendly customer segments. It employs a "jury and judge" approach where different AI personas propose segment names, and a judge selects the best option based on marketing criteria.

## Features

- **Mock Data Generation**: Creates realistic cart abandonment datasets with 25,000+ users
- **MECE Segmentation**: Ensures segments are mutually exclusive and collectively exhaustive
- **AI-Powered Naming**: Uses LLM jury system with multiple personas for segment naming
- **Multi-Model Support**: Integrates Cerebras, Groq, and LangChain for diverse AI capabilities
- **Scoring System**: Comprehensive segment evaluation based on conversion potential, profitability, and strategic fit
- **Automated Outputs**: Generates CSV files and analysis reports

## Architecture

### Data Pipeline
1. **Data Generation** → Mock cart abandonment data with realistic distributions
2. **Universe Definition** → Filter users who abandoned carts in last 7 days
3. **Bucket Creation** → MECE combinations based on AOV, engagement, and profitability
4. **AI Segmentation** → LLM jury proposes names, judge selects best option
5. **Scoring & Validation** → Multi-dimensional segment evaluation
6. **Output Generation** → CSV exports and analysis reports

### AI Jury System
- **Creative Marketer**: Focuses on catchy, memorable names
- **Growth Strategist**: Emphasizes scalable, actionable segments  
- **Data Scientist**: Provides precise, technical definitions
- **Risk Manager**: Ensures conservative, safe naming
- **Judge**: Chief Segmentation Officer who selects optimal segments

## Installation

### Prerequisites
```bash
# Python packages
pip install pandas numpy python-dotenv nest-asyncio pydantic
pip install langchain-groq groq cerebras-cloud-sdk

# UV package manager (for MCP servers)
# See: https://docs.astral.sh/uv/getting-started/installation/
```

### Environment Setup
Create a `.env` file with your API keys:
```env
CEREBRAS_API_KEY=your_cerebras_key_here
GROQ_API_KEY=your_groq_key_here
```

## Usage

### Basic Execution
```python
# Run the complete pipeline
universe_df, results_df, segment_rule_map = main()
```

### Key Functions

#### Data Generation
```python
# Generate mock cart abandonment data
df = generate_cart_abandonment_dataset(n_users=25000)
universe = define_universe(df)
```

#### Segmentation
```python
# Create MECE bucket combinations
universe = create_bucket_combinations(universe)

# Generate AI-powered segment names
segment_name_map, segment_rule_map = generate_segment_names_with_langchain_jury(universe)
```

#### Analysis
```python
# Compute multi-dimensional segment scores
results_df = compute_segment_scores(universe, segment_rule_map)
```

## Data Schema

### Input Features
- `user_id`: Unique user identifier
- `cart_abandoned_date`: When cart was abandoned
- `last_order_date`: Previous purchase date (nullable)
- `avg_order_value`: Historical average order value
- `sessions_last_30d`: Recent session count
- `num_cart_items`: Items in abandoned cart
- `engagement_score`: User engagement metric (0-1)
- `profitability_score`: Customer profitability (0-1)

### Segmentation Dimensions
- **AOV Buckets**: High, Medium, Low (based on quantiles)
- **Engagement**: High, Low (based on 60th percentile)
- **Profitability**: High, Low (based on 60th percentile)

### Output Metrics
- **Conversion Potential**: Engagement × urgency factor
- **Lift vs Control**: Expected campaign performance
- **Size**: Segment population
- **Profitability**: Average customer value
- **Strategic Fit**: Overall business alignment
- **Overall Score**: Weighted composite score

## Configuration

### Model Settings
```python
# LangChain + Groq Models
llm_creative = ChatGroq(model="qwen/qwen3-32b", temperature=0.3)
llm_strategist = ChatGroq(model="meta-llama/llama-4-maverick-17b-128e-instruct", temperature=0.3)
llm_data_sci = ChatGroq(model="llama-3.3-70b-versatile", temperature=0.3)
llm_risk = ChatGroq(model="moonshotai/kimi-k2-instruct-0905", temperature=0.2)
llm_judge = ChatGroq(model="openai/gpt-oss-120b", temperature=0.1)
```

### Scoring Weights
```python
weights = [0.3, 0.2, 0.1, 0.3, 0.1]  # Conv, Lift, Size, Profit, Strategic
```

## Output Files

The system generates several output files:
- `mock_cart_abandoners.csv`: Generated user data with segments
- `mece_strategy.csv`: Segment analysis and scores
- Analysis reports with segment performance metrics

## Example Output

```
Segment Name                    | Rules Applied                           | Size  | Overall Score
Premium_Engaged_Profitable     | AOV_High & Engagement_High & Profit_High| 2,341 | 0.87
Growth_Moderate_Valuable       | AOV_Medium & Engagement_High & Profit_High| 3,156 | 0.82
Budget_Active_Emerging         | AOV_Low & Engagement_High & Profit_Low | 4,892 | 0.76
```

## Technical Details

### MECE Compliance
- **Mutually Exclusive**: Each user belongs to exactly one segment
- **Collectively Exhaustive**: All users are assigned to a segment
- **Rule-Based Logic**: Clear, auditable segmentation criteria

### AI Integration
- **Structured Outputs**: Uses Pydantic models for consistent LLM responses
- **Error Handling**: Fallback mechanisms for API failures
- **Multi-Model**: Leverages different models for diverse perspectives

### Performance
- Processes 25,000+ users in minutes
- Scalable to larger datasets
- Optimized for Jupyter/Colab environments

## Troubleshooting

### Common Issues
1. **API Key Errors**: Ensure `.env` file contains valid keys
2. **Model Timeouts**: Reduce batch sizes or add retry logic
3. **Memory Issues**: Decrease `n_users` parameter for large datasets

### Dependencies
- Requires active internet connection for LLM APIs
- Jupyter/Colab compatibility via `nest_asyncio`
- Python 3.8+ recommended

## Contributing

This system is designed for marketing teams and data scientists working on customer segmentation. The modular design allows for easy customization of:
- Segmentation criteria
- AI model selection
- Scoring algorithms
- Output formats

## License

This project is part of the Niti-AI-Assignment repository and follows the associated licensing terms.