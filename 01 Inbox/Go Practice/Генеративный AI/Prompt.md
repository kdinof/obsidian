
# V2
```
You are a product manager who has to research user experience of your app.

Your task is to analyze a review and extract main topics (maximum 3 topics).

Each topic has to describe an exact feature or issue of the app.

It should be actionable for a product manager.

For each topic extract related sentiment – positive/negative/neutral.

Sentiment has to be related to the user’s current sentiment; words such "now" could describe current sentiment.



Here is a text of a review.
===
{{Review text}}
===

Solve this task step by step.
Output a RAW json array with format
[{
"explanation": "explain your decision here",
"topic_name": "extracted_name",
"sentiment": "extracted_sentiment"
}, ...]
```

# V3
```
You are a product manager who has to research user experience of your app.

You must analyze the following review and extract the main topics about the app (maximum 3 topics, minimum – 0 topics).
   
Each topic has to describe an exact feature or issue of the app. It should be actionable for a product manager.
   
If there are no exact topics about the app in the review, then return "No topics".
   
Step 1: Extract the first main topic about the app.

Step 2: Extract the second main topic about the app. Compare this topic with the first one. If they are similar, then only use the first topic.

Step 3: Extract the third main topic about the app. Compare this topic with the first and the second ones. If they are similar, then use only previous topics.

Step 4: Check if extracted topics are really about exact features or issues of the app.  
If not, then return "No topics".

Step 5: If you extracted topics, then for each extracted topic, define the sentiment in the review 
   – positive/negative/neutral. 

Sentiment has to be related to user’s perception in the present, words such "now" could describe current sentiment. 

Here is a text of a review.

=== 
{{Review text}}
===

Solve this task step by step.

Output a RAW json array with format
[{
"explanation": "explain your decision here",
"topic_name": "extracted_name",
"sentiment": "extracted_sentiment"
}, ...]

```


# Промпт V1 для определения важных категорий тем:

```
You are a product manager who has to research the user experience of your app.

Your task is to categorize a list of topics mentioned in user reviews.

Here is a list of topics
===
{{Topics}}
===

Please output only a RAW json of following structure:
[
"$topic_category_name", ...
]
```


# Промпт V2 для определения важных категорий тем

```
You are a product manager who has to research the user experience of your app.

Your task is to categorize a list of topics mentioned in user reviews.

Each category should be related to a single product feature related to the topic.

Categories must not intersect.

Here is a list of topics
===
{{Topics}}
===

Include each topic in a single category.

Solve this problem step by step:
- First extract major categories
- Map each topic to the best matching category
   
Please output only a RAW json of following structure:
{
"$topic_category_name": one-sentence summary of the category
}
```


Промпт для отнесения конкретной темы к категории можно сформулировать так:

```
You are a product manager who has to research the user experience of your app.

Your task is to map a topic into one most suitable category from given categories. 

Use only the given list of categories.
   
Here is a json with category names and their explanations 

===
{{Categories}}
===

Here is a topic
===
{{Topic}}
===

Solve this problem step by step.

Please output only a RAW json of following structure:
{{
"$topic": "$category_from_list_above" 
}}
```



---

```
You are a product manager who has to research user experience of your app.

Your task is to analyze a review and extract main topics (maximum 3 topics) step by step.

1. Identify Key Features or Issues: Read the review carefully and identify all mentioned features or issues of the app. Ignore any feature requests or topics unrelated to the product experience. 
2. Evaluate Relevance: For each identified feature or issue, evaluate its relevance and actionability for a product manager. Only select those that provide clear, actionable insights than will help improve product and user experience.Avoid future requests and predictions
3. Determine Sentiment: Analyze the sentiment expressed by the user for each selected topic - positive/negative/neutral. Ensure the sentiment reflects the user's current experience, paying attention to words like "now". 

Here is a text of a review:
===
{{Review text}}
===

Perform the above steps and output a RAW JSON array with the following format:
[{
"explanation": "explain your decision here",
"topic_name": "extracted_name",
"sentiment": "extracted_sentiment"
}, ...]

Solve this task step by step.


```



---
### Prompt engineer 
```

You are a best prompt engineer. You also help with creating a prompt for companies. I'm a product manager. Now I'm struggling with prompt that will help with analyzing product reviews and extract from it main topics. Each topic should describe the product feature that actually exist. No features request or topic that not about product experience. The topics should be actionable for me as a product manager of this precut. Each topic should has related sentiment – positive/negative/neutral An output should be in a RAW json array with format [{ "explanation": "explain your decision here", "topic_name": "extracted_name", "sentiment": "extracted_sentiment" }, ...] === My prompt: You are a product manager who has to research user experience of your app. Your task is to analyze a review and extract main topics (maximum 3 topics). Each topic has to describe an exact feature or issue of the app. It should be actionable for a product manager. For each topic extract related sentiment – positive/negative/neutral. Sentiment has to be related to the user’s current sentiment; words such "now" could describe current sentiment. Here is a text of a review. === {{Review text}} === Solve this task step by step. Output a RAW json array with format [{ "explanation": "explain your decision here", "topic_name": "extracted_name", "sentiment": "extracted_sentiment" }, ...] === Your job is to help me with upgrading and refining this prompt. The main problem with this prompt is that relevance of the topics are not good enough. You can suggest new technics to upgrade this prompt like: three of thoughts, prompt chaining, Automatic Prompt Engineer

```