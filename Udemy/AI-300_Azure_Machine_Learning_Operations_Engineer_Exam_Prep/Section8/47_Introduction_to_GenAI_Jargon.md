# Generative AI Jargon and Core Terminology

## Key Concepts
- Tokens
- Tokenization
- System Prompts
- User Prompts
- Chat Completion APIs
- Multimodal Models
- Unimodal Models
- Model Inputs and Outputs
- LLM Pricing

## Definitions

### Token
A unit of text processed by a large language model. Tokens can represent words, subwords, punctuation, whitespace, characters, or combinations of words.

### Tokenization
The process of converting human-readable text into tokens that can be interpreted mathematically by a machine learning model.

### System Prompt
Instructions that define the role, behavior, constraints, tone, expertise, and safety rules for an AI model.

### User Prompt
The task, question, or instruction provided by the user.

### Chat Completion API
An API endpoint that sends prompts and conversation history to a large language model and returns generated responses.

### Unimodal Model
A model that accepts only a single type of input modality, such as text only.

### Multimodal Model
A model capable of processing multiple input modalities, such as text, images, audio, and video.

### Modality
The type of data being processed, such as text, image, audio, or video.

## Examples

### Token Examples
- A sentence with 204 characters may contain approximately 40 tokens.
- A short phrase may contain only a handful of tokens despite containing many characters.
- Tokens may represent:
  - Entire words
  - Partial words
  - Punctuation marks
  - Whitespace
  - Word combinations

### Prompt Examples
**System Prompt**
```
You are a renowned poet.
```

**User Prompt**
```
Write a poem on LLMs.
```

The AI response is generated from the combination of both prompts.

### Multimodal Example
A model that accepts:
- Text questions
- Images
- Audio

and generates:
- Text answers
- Image descriptions
- Context-aware responses

## Step-by-Step Processes

### How Tokenization Works
1. A user enters text.
2. The text is converted into tokens.
3. The model processes token IDs instead of raw text.
4. Internal computations are performed.
5. Output tokens are generated.
6. Tokens are decoded back into human-readable text.

### How Prompt Processing Works
1. Define a system prompt.
2. Accept a user prompt.
3. Combine prompts into a request payload.
4. Send the request to a model.
5. Generate a response.
6. Return the output to the user.

### Chat Completion API Workflow
1. Select a deployed model.
2. Create a message payload.
3. Include a system prompt.
4. Include a user prompt.
5. Submit the API request.
6. Receive generated output.
7. Process and display the response.

### Multimodal Processing Workflow
1. Accept one or more input modalities.
2. Convert all inputs into model-understandable representations.
3. Combine contextual information.
4. Generate a response.
5. Return output in one or more formats.

## Code Blocks

### Example System and User Prompt Payload
```json
{
  "messages": [
    {
      "role": "system",
      "content": "You are a renowned poet."
    },
    {
      "role": "user",
      "content": "Write a poem on LLMs"
    }
  ]
}
```

### Conceptual Chat Completion Call
```python
response = client.chat.completions.create(
    model="gpt-model",
    messages=messages
)
```

## Summary
This lesson introduces foundational generative AI terminology. Large language models process tokens rather than raw text, use system and user prompts to determine behavior and tasks, interact through chat completion APIs, and may operate as either unimodal or multimodal systems. Understanding these concepts is essential for building generative AI applications.

## Actionable Takeaways

- Think in tokens rather than characters when designing prompts.
- Remember that token usage directly affects cost and latency.
- Use system prompts to define behavior and constraints.
- Use user prompts to specify tasks.
- Understand the distinction between unimodal and multimodal models.
- Learn the structure of chat completion API requests.
- Prefer multimodal models when richer context is beneficial.

## Tags

#AI300
#GenerativeAI
#LLM
#Tokens
#PromptEngineering
#ChatCompletionAPI
#MultimodalAI
#AzureOpenAI

## Backlink Suggestions

- [[Prompt Engineering]]
- [[System Prompts]]
- [[User Prompts]]
- [[Large Language Models]]
- [[Chat Completion API]]
- [[Generative AI Fundamentals]]
- [[Multimodal AI]]
- [[Azure OpenAI Service]]
- [[Tokenization]]
