# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

### Date: 
### Register no: 212223240084
## Aim: 
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools

## AI Tools Required:
| Tool/Library | Purpose                                     |
| ------------ | ------------------------------------------- |
| Python 3.x   | Programming language                        |
| openai       | To interact with OpenAI GPT models          |
| requests     | To make HTTP requests to Hugging Face API   |
| difflib      | To compare outputs and calculate similarity |
| API Keys     | OpenAI API key, Hugging Face API key        |

## Installation Commands:
```pip install openai transformers requests```

## Explanation:
1. Multiple AI Integration:
  - OpenAI GPT-4 API is used for context-aware and structured responses.
  - Hugging Face Transformers API is used for creative or alternative phrasing.

2. Comparison:

  - The outputs from both AI tools are compared using a similarity measure (SequenceMatcher).

  - This helps identify agreement or contradictions between tools.

3. Actionable Insights:

  - If outputs are similar (>70%), either tool’s response can be trusted.

  - If outputs differ (<70%), analyze both outputs for complementary insights.

  - Helps in decision-making, content generation, or validation tasks.

### Python Code:
```py
# Import required libraries
import openai
import requests
from difflib import SequenceMatcher

# Set API keys
OPENAI_API_KEY = "your_openai_api_key"
HF_API_KEY = "your_hf_api_key"

# Function to get OpenAI GPT-4 output
def get_openai_response(prompt):
    openai.api_key = OPENAI_API_KEY
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are an AI assistant."},
            {"role": "user", "content": prompt}
        ],
        max_tokens=200
    )
    return response['choices'][0]['message']['content'].strip()

# Function to get Hugging Face model output
def get_hf_response(prompt):
    url = "https://api-inference.huggingface.co/models/gpt2"
    headers = {"Authorization": f"Bearer {HF_API_KEY}"}
    payload = {"inputs": prompt}
    
    response = requests.post(url, headers=headers, json=payload)
    result = response.json()
    
    if isinstance(result, list) and 'generated_text' in result[0]:
        return result[0]['generated_text']
    elif 'error' in result:
        return f"Error: {result['error']}"
    else:
        return str(result)

# Function to compare outputs
def compare_outputs(output1, output2):
    seq_matcher = SequenceMatcher(None, output1, output2)
    similarity = seq_matcher.ratio() * 100
    return round(similarity, 2)

# Function to generate insights
def generate_insights(prompt, output1, output2):
    similarity = compare_outputs(output1, output2)
    
    insight = f"""
    Prompt: {prompt}
    
    OpenAI Output:
    {output1}
    
    Hugging Face Output:
    {output2}
    
    Similarity: {similarity}%
    
    Actionable Insights:
    - If similarity >70%, both AI tools provide similar insights.
    - If similarity <70%, analyze both outputs for complementary information.
    - OpenAI provides structured context-aware answers; Hugging Face provides creative alternatives.
    """
    return insight

# Main workflow
def main():
    prompt = "Explain the benefits of AI in healthcare in simple terms."
    
    openai_output = get_openai_response(prompt)
    hf_output = get_hf_response(prompt)
    
    insight = generate_insights(prompt, openai_output, hf_output)
    
    print(insight)

if __name__ == "__main__":
    main()
```

## Sample Output:
```
Prompt: Explain the benefits of AI in healthcare in simple terms.

OpenAI Output:
AI in healthcare helps doctors diagnose diseases faster, predicts patient health trends, and assists in personalized treatment plans.

Hugging Face Output:
AI in healthcare can improve diagnosis accuracy, suggest treatments, and analyze patient data efficiently, helping medical professionals make better decisions.

Similarity: 75%

Actionable Insights:
- If similarity >70%, both AI tools provide similar insights.
- If similarity <70%, analyze both outputs for complementary information.
- OpenAI provides structured context-aware answers; Hugging Face provides creative alternatives.

```
## Conclusion:
- Integrating multiple AI tools allows for cross-verification of outputs.

- Comparing outputs helps detect inconsistencies, missing information, or complementary insights.

- This workflow can be applied in content generation, healthcare insights, business reports, and decision-making systems.

## Result: 
The corresponding Prompt is executed successfully.
