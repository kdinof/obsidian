# Style Transfer Prompt Template


```
You are an AI writing assistant that has been trained to write in the exact style and tone of Kirill. Your task is to transform raw information into content that perfectly matches Kirill's unique writing style.

## STYLE CHARACTERISTICS:

[attached to the message]

## EXAMPLES OF KIRILL'S WRITING:

[attached to the message]

## RAW INFORMATION TO TRANSFORM:
[attached to the message]

## INSTRUCTIONS:
1. Transform the raw information into content written exactly in Kirill's style
2. Maintain the conversational and approachable tone characteristic of Kirill's writing
3. Use a mix of short, direct sentences and longer, more complex ones
4. Incorporate rhetorical questions to engage readers
5. Include concrete examples and analogies to illustrate points
6. Structure content with clear sections and smooth transitions
7. Use first-person perspective where appropriate
8. Balance professional expertise with casual accessibility
9. Write primarily in Russian unless the topic specifically calls for English

Write a complete piece in Kirill's authentic style based on the raw information provided:
```

## Example Implementation

```python
def generate_content_in_user_style(raw_content, content_type="article"):
    """
    Generate content in the user's style from raw data
    
    Parameters:
    - raw_content: The raw content to transform
    - content_type: Type of content (article, video_transcript, research_paper)
    
    Returns:
    - Styled content in the user's voice
    """
    import os
    from openai import OpenAI
    
    # Initialize OpenAI client
    client = OpenAI()
    
    # Load the base prompt template
    with open("/home/ubuntu/style_transfer_prompt.md", "r") as f:
        base_prompt = f.read()
    
    # Extract the actual prompt template (between triple backticks)
    import re
    prompt_template = re.search(r'```\n(.*?)\n```', base_prompt, re.DOTALL).group(1)
    
    # Prepare the prompt with the raw content
    full_prompt = prompt_template.replace("[Insert raw content here]", raw_content)
    
    # Add content-specific instructions
    if content_type == "video_transcript":
        full_prompt += "\nThis content is based on a video transcript. Maintain a conversational flow and organize the information logically."
    elif content_type == "research_paper":
        full_prompt += "\nThis content is based on a research paper. Preserve the key findings and methodology while making it accessible and engaging."
    elif content_type == "article":
        full_prompt += "\nThis content is based on an article. Preserve the main points while adapting it to your personal style."
    
    # Generate content using the model
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are an AI assistant that writes in the exact style of Kirill."},
            {"role": "user", "content": full_prompt}
        ],
        temperature=0.7,
        max_tokens=2000
    )
    
    # Extract and return the generated content
    generated_content = response.choices[0].message.content
    
    return generated_content
```

## Usage Examples

### For YouTube Video Content

```python
# Extract transcript from a YouTube video
video_id = "RlOhS_mRrV4"  # IDEO design thinking video
transcript = extract_youtube_transcript(video_id)

# Generate content in user's style
styled_content = generate_content_in_user_style(transcript, content_type="video_transcript")

# Save the generated content
with open(f"/home/ubuntu/generated_content/youtube_{video_id}_styled.md", "w") as f:
    f.write(styled_content)
```

### For Research Paper Content

```python
# Extract text from a research paper
paper_id = "2304.03442"  # Paper on behavioral design
with open(f"/home/ubuntu/raw_data/paper_{paper_id}.txt", "r") as f:
    paper_content = f.read()

# Generate content in user's style
styled_content = generate_content_in_user_style(paper_content, content_type="research_paper")

# Save the generated content
with open(f"/home/ubuntu/generated_content/paper_{paper_id}_styled.md", "w") as f:
    f.write(styled_content)
```

### For Article Content

```python
# Extract text from an article
article_filename = "familiarity_bias"
with open(f"/home/ubuntu/raw_data/{article_filename}.txt", "r") as f:
    article_content = f.read()

# Generate content in user's style
styled_content = generate_content_in_user_style(article_content, content_type="article")

# Save the generated content
with open(f"/home/ubuntu/generated_content/{article_filename}_styled.md", "w") as f:
    f.write(styled_content)
```
