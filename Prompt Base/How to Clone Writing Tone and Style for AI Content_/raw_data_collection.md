# Raw Data Collection for Style Transfer Testing

## Source Types and Collection Strategy

### YouTube Videos
- **Selection Criteria**: Videos on design, technology, behavioral psychology, and AI
- **Processing Method**: Extract transcripts, segment by topic, identify key points
- **Example Sources**:
  - Design thinking and UX videos
  - AI technology explanations
  - Behavioral psychology lectures

### Research Papers
- **Selection Criteria**: Academic papers on relevant topics with clear structure
- **Processing Method**: Extract abstract, key findings, methodology, and conclusions
- **Example Sources**:
  - Behavioral design research
  - UX/UI studies
  - AI and technology adoption papers

### Articles and Blog Posts
- **Selection Criteria**: Well-structured articles with clear points and examples
- **Processing Method**: Extract main arguments, supporting evidence, and conclusions
- **Example Sources**:
  - Technology trend analyses
  - Design case studies
  - Behavioral psychology applications

## Sample Collection

### YouTube Video Sample
```python
from youtube_transcript_api import YouTubeTranscriptApi
import json

def extract_youtube_transcript(video_id):
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=['en', 'ru'])
        
        # Combine transcript segments
        full_text = " ".join([entry['text'] for entry in transcript])
        
        # Save to file
        with open(f"/home/ubuntu/raw_data/youtube_{video_id}.txt", "w", encoding="utf-8") as f:
            f.write(full_text)
            
        # Save structured data
        with open(f"/home/ubuntu/raw_data/youtube_{video_id}.json", "w", encoding="utf-8") as f:
            json.dump(transcript, f, ensure_ascii=False, indent=2)
            
        return full_text
    except Exception as e:
        print(f"Error extracting transcript: {e}")
        return None

# Example video IDs to process
video_ids = [
    "RlOhS_mRrV4",  # IDEO design thinking video mentioned in user's writing
    "DAGNhcWxGk8",  # Amazon history video mentioned in user's writing
    "_nSmkyDNulk"   # English learning with AI video mentioned in user's writing
]

# Create directory if it doesn't exist
import os
os.makedirs("/home/ubuntu/raw_data", exist_ok=True)

# Extract transcripts
for video_id in video_ids:
    extract_youtube_transcript(video_id)
```

### Research Paper Sample
```python
import requests
from bs4 import BeautifulSoup
import os

def extract_arxiv_paper(paper_id):
    try:
        # Get the PDF URL
        pdf_url = f"https://arxiv.org/pdf/{paper_id}.pdf"
        
        # Download the PDF
        response = requests.get(pdf_url)
        if response.status_code == 200:
            # Save the PDF
            os.makedirs("/home/ubuntu/raw_data", exist_ok=True)
            with open(f"/home/ubuntu/raw_data/paper_{paper_id}.pdf", "wb") as f:
                f.write(response.content)
            
            # Use pdftotext to extract text
            os.system(f"pdftotext /home/ubuntu/raw_data/paper_{paper_id}.pdf /home/ubuntu/raw_data/paper_{paper_id}.txt")
            
            return f"Paper {paper_id} downloaded and text extracted"
        else:
            return f"Failed to download paper {paper_id}: {response.status_code}"
    except Exception as e:
        return f"Error processing paper {paper_id}: {e}"

# Example arXiv paper IDs on relevant topics
paper_ids = [
    "2304.03442",  # Paper on behavioral design
    "2302.09270",  # Paper on UX/UI
    "2303.12712"   # Paper on AI applications
]

# Extract papers
for paper_id in paper_ids:
    extract_arxiv_paper(paper_id)
```

### Web Article Sample
```python
import requests
from bs4 import BeautifulSoup
import os
import json

def extract_article(url, filename):
    try:
        # Send request
        headers = {
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
        }
        response = requests.get(url, headers=headers)
        
        if response.status_code == 200:
            # Parse HTML
            soup = BeautifulSoup(response.text, 'html.parser')
            
            # Extract title
            title = soup.title.string if soup.title else "No title found"
            
            # Extract main content (this is a simplified approach)
            # For real implementation, site-specific extractors would be needed
            main_content = ""
            
            # Look for article or main tags
            article_tag = soup.find('article') or soup.find('main')
            if article_tag:
                # Get all paragraphs
                paragraphs = article_tag.find_all('p')
                main_content = "\n\n".join([p.get_text() for p in paragraphs])
            else:
                # Fallback: get all paragraphs
                paragraphs = soup.find_all('p')
                main_content = "\n\n".join([p.get_text() for p in paragraphs])
            
            # Save to file
            os.makedirs("/home/ubuntu/raw_data", exist_ok=True)
            with open(f"/home/ubuntu/raw_data/{filename}.txt", "w", encoding="utf-8") as f:
                f.write(f"Title: {title}\n\n{main_content}")
            
            # Save metadata
            metadata = {
                "url": url,
                "title": title,
                "extraction_date": str(datetime.now())
            }
            with open(f"/home/ubuntu/raw_data/{filename}_meta.json", "w", encoding="utf-8") as f:
                json.dump(metadata, f, ensure_ascii=False, indent=2)
            
            return f"Article extracted and saved as {filename}.txt"
        else:
            return f"Failed to download article: {response.status_code}"
    except Exception as e:
        return f"Error extracting article: {e}"

# Example articles
articles = [
    {
        "url": "https://www.nngroup.com/articles/ten-usability-heuristics/",
        "filename": "nielsen_usability_heuristics"
    },
    {
        "url": "https://www.interaction-design.org/literature/article/design-thinking-getting-started-with-empathy",
        "filename": "design_thinking_empathy"
    },
    {
        "url": "https://www.behavioraleconomics.com/resources/mini-encyclopedia-of-be/familiarity-bias/",
        "filename": "familiarity_bias"
    }
]

# Import datetime for metadata
from datetime import datetime

# Extract articles
for article in articles:
    extract_article(article["url"], article["filename"])
```

## Data Processing Pipeline

### 1. Content Extraction
- Extract raw text from each source type
- Preserve structural elements (headings, sections)
- Identify key information and main points

### 2. Content Structuring
- Organize extracted information into logical sections
- Identify main arguments, supporting evidence, and examples
- Create a structured outline for style transfer

### 3. Style Transfer Preparation
- Prepare content in a format suitable for the prompt template
- Ensure key information is preserved while allowing for stylistic adaptation
- Create test cases with varying complexity and topic areas

## Test Cases for Style Transfer

### Simple Test Case
- Short paragraph from a design thinking article
- Focus on clear concepts with minimal technical jargon
- Test basic style transfer capabilities

### Medium Complexity Test Case
- Section from a research paper with technical concepts
- Test ability to maintain technical accuracy while adapting style
- Include some specialized terminology

### Complex Test Case
- Full YouTube video transcript on a technical topic
- Test comprehensive style transfer with longer content
- Include multiple sections and topic transitions

## Next Steps
1. Execute the data collection scripts to gather raw content
2. Process the collected data into structured formats
3. Prepare test cases for the style transfer system
4. Implement the prompt-based style transfer using the collected data
