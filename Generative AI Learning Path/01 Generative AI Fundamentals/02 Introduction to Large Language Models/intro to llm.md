# Introduction to large language models 

Large language models (LLMs) are a subset of deep learning that are trained for general purposes to solve common language problems, such as text classification, question answering, document summarization, and text generation across industries. LLMs are large general-purpose language models that can be pre-trained and then fine-tuned for specific purposes.


## What are large language models?

Large language models, or LLMs, are a subset of deep learning. LLMs refer to large general-purpose language models that can be pre-trained and then fine tuned for specific purposes. 

**Pre-trained** means that the models are trained for general purposes to solve common language problems such as text classification, question answering, document summarization, and text generation across industries. The models can then be **fine tuned** to solve specific problems in different fields such as retail, finance, and entertainment using a relatively small size of field data sets.

LLMs have three major features:

1. Large: Enormous size of the training data set and huge parameter count. Parameters refer to the memories and knowledge that the model learned. 
2. General purpose: Sufficient to solve common problems due to the commonality of human language and resource restrictions. 
3. Pre-trained and fine tuned: Pre-train a large model for general purpose with large data set and fine tune for specific aims with small data set.

The benefits of LLMs are:

1. A single model can solve different tasks. 
2. Require minimal field training data. Can be used for few shot or even zero-shot scenarios.
3. Performance grows with more data and parameters. For example, PaLM, Google's 540 billion-parameter model achieves state-of-the-art performance on multiple language tasks.

## Comparing LLM development and traditional ML development

With LLM development:

- No need to be an expert. No training examples or model training needed. 
- Focus on prompt design: Creating a prompt that is clear, concise, and informative. 

With traditional ML:

- Need training examples, compute time, and hardware to train a model.

Let's look at an example text generation use case, question answering (QA). QA systems are trained on large data to answer questions. Generative QA generates free text based on context without domain knowledge. For example, here are three questions for Google's LLM chatbot, Bard:

1. Question: This year's sales are $100,000. Expenses are $60,000. How much is net profit? 
Bard: Sales - Expenses = Net Profit
$100,000 - $60,000 = $40,000

2. Question: Inventory on hand is 6,000 units. A new order requires 8,000 units. How many units do I need to fill to complete the order?
Bard: 8,000 units are required for the new order. 
Inventory on hand is 6,000 units.
So you need 8,000 - 6,000 = 2,000 additional units to complete the order.

3. Question: We have 1,000 sensors in 10 geographic regions. How many sensors do we have on average in each region?
Bard: 
   * Total sensors: 1,000
   * Total regions: 10
   * To calculate average per region:
     * Total sensors / Total regions
     * 1,000 / 10 = 100
   * So the average number of sensors in each region is 100

In each example, the desired response was obtained through prompt design.

## Prompt design and prompt engineering

Prompt design is creating a prompt tailored to a task. Prompt engineering improves performance, e.g. using domain knowledge, examples, or keywords. Prompt design is general while prompt engineering is specialized. Prompt design is essential; prompt engineering is for high accuracy.

There are three kinds of LLMs:

1. Generic language models predict the next word based on training data. Like autocomplete.
2. Instruction tuned models predict a response to input instructions. E.g. summarize text, generate a poem, give keywords. 
3. Dialogue tuned models have a dialogue by the next response. Dialogue tuning works better with natural questions.

Chain of thought reasoning: Models are better at getting the right answer by first explaining the reason. For example, with the question "Roger has five tennis balls. He buys two more cans of tennis balls. Each can has three tennis balls. How many tennis balls does he have now?", the model is more likely to end with the correct answer after explaining the reasoning.

Task-specific tuning makes LLMs more reliable. For example, use a sentiment analysis model for gathering customer sentiments. Tuning adapts a model to new domains or use cases by training on new data. Fine tuning retrains all model weights, requiring big jobs and hosting fine-tuned models.

Parameter-efficient tuning methods (PETM) tune LLMs on custom data without duplicating the model. Small add-on layers are tuned and swapped at inference time.

## Google's Gen AI development tools 

Generative AI Studio helps developers create and deploy generative AI models. It provides:

- Pre-trained models 
- A model fine tuning tool
- A model deployment tool
- A community forum

Gen AI App Builder lets you create Gen AI apps without code. It has:

- A drag-and-drop interface to design and build apps
- A visual editor to create and edit app content  
- A built-in search engine to search within the app
- A conversational AI engine for natural language interaction

You can create chatbots, digital assistants, search engines, knowledge bases, training apps, and more.

PaLM API lets you test and experiment with Google's LLMs and Gen AI tools. Developers can access the API through a graphical user interface using Maker Suite.