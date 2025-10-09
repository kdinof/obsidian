# AI Style Cloning Agent: User Guide

## Overview

This guide provides instructions for using the AI style cloning agent that has been developed to generate content in your unique writing style. The agent can transform raw information from various sources (YouTube videos, research papers, articles) into content that authentically reflects your distinctive tone, structure, and rhetorical devices.

## How It Works

The AI style cloning agent uses a hybrid approach combining:

1. **Prompt Engineering**: A detailed style guide and examples from your writing are used to instruct the AI model on how to generate content in your style.

2. **Content Processing Pipeline**: Raw information from various sources is extracted, structured, and then transformed into your writing style.

## Using the Style Cloning Agent

### Step 1: Prepare Raw Content

First, collect the raw information you want to transform:

- **YouTube Videos**: Copy the video URL or ID
- **Research Papers**: Save the PDF file or URL
- **Articles**: Copy the article URL or text

### Step 2: Process Raw Content

Use the provided Python scripts to extract and process the raw content:

```python
# For YouTube videos
from youtube_transcript_api import YouTubeTranscriptApi

def extract_youtube_transcript(video_id):
    transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=['en', 'ru'])
    full_text = " ".join([entry['text'] for entry in transcript])
    return full_text

# Example usage
video_id = "VIDEO_ID_HERE"  # e.g., "RlOhS_mRrV4"
transcript = extract_youtube_transcript(video_id)
```

```python
# For articles or web pages
import requests
from bs4 import BeautifulSoup

def extract_article_content(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    # Extract main content (simplified approach)
    article_tag = soup.find('article') or soup.find('main')
    if article_tag:
        paragraphs = article_tag.find_all('p')
        content = "\n\n".join([p.get_text() for p in paragraphs])
    else:
        paragraphs = soup.find_all('p')
        content = "\n\n".join([p.get_text() for p in paragraphs])
    
    return content

# Example usage
url = "ARTICLE_URL_HERE"
article_content = extract_article_content(url)
```

```python
# For PDF documents
import os

def extract_pdf_content(pdf_path):
    txt_path = pdf_path.replace('.pdf', '.txt')
    os.system(f"pdftotext {pdf_path} {txt_path}")
    
    with open(txt_path, 'r', encoding='utf-8') as f:
        content = f.read()
    
    return content

# Example usage
pdf_path = "PATH_TO_PDF_FILE"
pdf_content = extract_pdf_content(pdf_path)
```

### Step 3: Generate Content in Your Style

Use the style transfer function to transform the raw content into your style:

```python
from openai import OpenAI

def generate_content_in_user_style(raw_content, content_type="article"):
    # Initialize OpenAI client
    client = OpenAI()
    
    # Load the style transfer prompt
    with open("style_transfer_prompt.md", "r") as f:
        base_prompt = f.read()
    
    # Extract the actual prompt template
    import re
    prompt_template = re.search(r'```\n(.*?)\n```', base_prompt, re.DOTALL).group(1)
    
    # Prepare the prompt with the raw content
    full_prompt = prompt_template.replace("[Insert raw content here]", raw_content)
    
    # Add content-specific instructions
    if content_type == "video_transcript":
        full_prompt += "\nThis content is based on a video transcript. Maintain a conversational flow."
    elif content_type == "research_paper":
        full_prompt += "\nThis content is based on a research paper. Preserve key findings while making it accessible."
    elif content_type == "article":
        full_prompt += "\nThis content is based on an article. Preserve main points while adapting to your style."
    
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

# Example usage
styled_content = generate_content_in_user_style(raw_content, content_type="article")

# Save the generated content
with open("generated_content.md", "w", encoding="utf-8") as f:
    f.write(styled_content)
```

## Customizing the Style Transfer

If you want to adjust the style transfer to better match specific aspects of your writing:

1. Open the `style_transfer_prompt.md` file
2. Modify the style guide section to emphasize particular characteristics
3. Add or update examples to better represent your preferred style
4. Adjust the instructions section to focus on specific stylistic elements

## Best Practices

1. **Start with Short Content**: Begin with shorter pieces to test and refine the style transfer.

2. **Review and Edit**: While the AI agent aims to match your style closely, always review and edit the generated content for accuracy and authenticity.

3. **Provide Feedback**: If you notice aspects of your style that aren't being captured correctly, update the style guide and examples accordingly.

4. **Topic Relevance**: The agent works best when transforming content on topics similar to those in your original writing samples (design, technology, behavioral psychology).

5. **Multilingual Content**: For optimal results with multilingual content, specify the primary language for the output when generating content.

## Limitations

1. **Sample Size**: The current implementation is based on limited writing samples. As more samples become available, the style matching can be further refined.

2. **Technical Terminology**: Very specialized technical content may require additional review to ensure accuracy while maintaining your style.

3. **Content Length**: Very long content may need to be processed in sections for optimal style consistency.

## Future Enhancements

As you continue to use the style cloning agent, consider these potential enhancements:

1. **Fine-tuning**: With more writing samples, a fine-tuned model could be developed for even more accurate style matching.

2. **Automated Workflow**: The entire process could be automated into a single application or service.

3. **Voice Integration**: The style transfer could be extended to voice content, allowing for spoken content in your style.

## Support and Troubleshooting

If you encounter issues with the style cloning agent:

1. **Check Raw Content**: Ensure the raw content is properly extracted and formatted.

2. **Review Prompt**: Verify that the style transfer prompt is correctly formatted and includes all necessary components.

3. **API Access**: Confirm that you have proper access to the OpenAI API and that your API key is valid.

4. **Content Limitations**: Very short or highly technical content may not provide enough context for effective style transfer.

## Conclusion

This AI style cloning agent provides a powerful tool for generating content in your unique writing style from various raw information sources. By following the guidelines in this document, you can effectively transform YouTube videos, research papers, and articles into content that authentically reflects your voice and perspective.
