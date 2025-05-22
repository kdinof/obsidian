# Content Pipeline Implementation Instructions

This guide provides detailed instructions for implementing each step of the local content creation pipeline.

## Prerequisites

Before starting, ensure you have:

1. **Python Environment**: Python 3.9+ installed
2. **OpenAI API Key**: Valid API key for accessing GPT-4
3. **Required Libraries**: Install the following packages:
   ```bash
   pip install openai python-markdown pyyaml tqdm colorama
   ```

## Step 1: Project Setup

### Create Project Structure

1. Create the project directory structure:
   ```bash
   mkdir -p content-pipeline/{modules,prompts,utils,data/{raw,ideas,drafts,final},examples}
   ```

2. Create a virtual environment (optional but recommended):
   ```bash
   cd content-pipeline
   python -m venv venv
   # On Windows
   venv\Scripts\activate
   # On macOS/Linux
   source venv/bin/activate
   ```

### Create Configuration File

Create a `config.yaml` file in the project root:

```yaml
# config.yaml
api:
  provider: "openai"
  model: "gpt-4"
  api_key: "${OPENAI_API_KEY}"  # Will be read from environment variable

blog_theme:
  main_topics:
    - "UX/UI Design"
    - "Behavioral Psychology"
    - "Technology Trends"
  style: "Educational with practical examples"
  tone: "Conversational and insightful"

audience:
  primary: "Design professionals and tech enthusiasts"
  interests:
    - "Design thinking"
    - "User behavior"
    - "Technology applications"
  knowledge_level: "Intermediate to advanced"

trend_sources:
  - "Recent industry publications"
  - "Social media trends"
  - "Conference topics"

draft_guidelines:
  structure:
    - "Clear introduction with hook"
    - "Logical progression of ideas"
    - "Concrete examples"
    - "Actionable conclusion"
  length: "1000-1500 words"
  tone: "Neutral and informative"

style_guide:
  language_usage:
    - "Mix of Russian and English"
    - "Specialized terminology with everyday language"
    - "Code-switching between languages for technical terms"
  sentence_structure:
    - "Varied length sentences"
    - "Conversational flow"
    - "Rhetorical questions"
    - "Parenthetical asides for supplementary information"
  tone:
    - "Informal and friendly"
    - "First-person perspective"
    - "Balance of expertise and accessibility"
    - "Thoughtful reflection mixed with factual information"
  rhetorical_devices:
    - "Storytelling with concrete examples"
    - "Analogies and metaphors for complex concepts"
    - "Direct reader address"
    - "Imagine if... scenarios"
  content_structure:
    - "Clear sections with headers"
    - "Example-driven explanations"
    - "Occasional lists for clarity"
    - "Circular structure that returns to initial points"

style_examples:
  - |
    Вы когда-нибудь сталкивались с тем, что иногда на первый взгляд абсурдные идеи становились понятными только после их реализации? Всё как будто складывается в цепочку, и мы удивляемся: «О, а что, так можно было?»

    Давайте приведём несколько примеров:

    Представьте, что я предложил вам купить две посудомоечные машины. И вы, конечно, посмотрите на меня, как на дурака, которому главное продать и навариться. Но на самом деле две посудомойки могут освободить место для хранения в небольших квартирах. В одной всегда хранится чистая и сухая посуда, а во второй – грязная. Как только грязная заполнилась, вы её включаете, а пустая посудомойка уже готова принимать грязную.
  - |
    Нашел исследование, которое говорит о том, что около 80% фичей так и остаются неиспользованными или используются очень редко т.к. пользователи не находят в них ценности. 

    Пост не о том, как сделать так, устроить революцию и перестроить компанию в user-centred. Бывает так, что это просто не нужно или ещё не время. Пост про то, на какие подходы можно разделить инновации и улучшение продукта, их всего два:
     
    **Inside-out**
    Когда решения о том, что делать и как появляются внутри организации и потом транслируются наружу. Это хорошо работает, когда у компании в руках уникальная технология, очень сложный рынок или просто компания хочет «взорвать» его своей инновацией, новым брендом или продуктом.

quality_criteria:
  content_accuracy: "Information is factually correct and well-sourced"
  style_match: "Content matches user's unique style"
  engagement: "Content is engaging and maintains reader interest"
  structure: "Content is well-organized with clear flow"
  value: "Content provides actionable insights or valuable information"
```

## Step 2: Implement Utility Modules

### LLM Client

Create `utils/llm_client.py`:

```python
# utils/llm_client.py
import os
import openai
import yaml
import json

class LLMClient:
    def __init__(self, config_path):
        self.config = self._load_config(config_path)
        self._setup_client()
        
    def _load_config(self, config_path):
        with open(config_path, 'r', encoding='utf-8') as f:
            config = yaml.safe_load(f)
        return config
        
    def _setup_client(self):
        # Get API key from environment if specified with ${ENV_VAR} syntax
        api_key = self.config['api']['api_key']
        if api_key.startswith('${') and api_key.endswith('}'):
            env_var = api_key[2:-1]
            api_key = os.environ.get(env_var)
            if not api_key:
                raise ValueError(f"Environment variable {env_var} not set")
        
        # Set up OpenAI client
        openai.api_key = api_key
        
    def generate(self, prompt, temperature=0.7, max_tokens=1000, model=None):
        if model is None:
            model = self.config['api']['model']
            
        response = openai.ChatCompletion.create(
            model=model,
            messages=[
                {"role": "system", "content": "You are a helpful assistant."},
                {"role": "user", "content": prompt}
            ],
            temperature=temperature,
            max_tokens=max_tokens
        )
        
        return response.choices[0].message.content
        
    def generate_with_json_output(self, prompt, temperature=0.5, max_tokens=1000, model=None):
        """Generate content with structured JSON output."""
        if model is None:
            model = self.config['api']['model']
            
        # Add instruction to format as JSON
        prompt += "\n\nProvide your response in valid JSON format."
        
        response = openai.ChatCompletion.create(
            model=model,
            messages=[
                {"role": "system", "content": "You are a helpful assistant that always responds with valid JSON."},
                {"role": "user", "content": prompt}
            ],
            temperature=temperature,
            max_tokens=max_tokens,
            response_format={"type": "json_object"}
        )
        
        content = response.choices[0].message.content
        
        # Parse JSON response
        try:
            return json.loads(content)
        except json.JSONDecodeError:
            # Fallback: try to extract JSON from the response
            import re
            json_match = re.search(r'```json\n(.*?)\n```', content, re.DOTALL)
            if json_match:
                try:
                    return json.loads(json_match.group(1))
                except json.JSONDecodeError:
                    pass
            
            # Return as plain text if JSON parsing fails
            return {"error": "Failed to parse JSON", "content": content}
```

### Markdown Utilities

Create `utils/markdown_utils.py`:

```python
# utils/markdown_utils.py
import re
import yaml
import markdown

def extract_metadata(content):
    """Extract YAML frontmatter from markdown content."""
    # Look for YAML frontmatter between --- markers
    frontmatter_match = re.match(r'^---\n(.*?)\n---\n', content, re.DOTALL)
    
    if frontmatter_match:
        # Extract and parse YAML
        frontmatter = frontmatter_match.group(1)
        try:
            metadata = yaml.safe_load(frontmatter)
            # Remove frontmatter from content
            content = content[frontmatter_match.end():]
            return metadata, content
        except yaml.YAMLError:
            # If YAML parsing fails, assume no valid frontmatter
            pass
    
    # Return empty metadata if no frontmatter found
    return {}, content

def clean_content(content):
    """Clean and normalize markdown content."""
    # Normalize line endings
    content = content.replace('\r\n', '\n')
    
    # Remove extra whitespace
    content = re.sub(r'\n{3,}', '\n\n', content)
    
    # Ensure content ends with newline
    if not content.endswith('\n'):
        content += '\n'
    
    return content

def markdown_to_html(content):
    """Convert markdown to HTML."""
    return markdown.markdown(content, extensions=['extra', 'smarty'])

def extract_headings(content):
    """Extract headings from markdown content."""
    headings = []
    for line in content.split('\n'):
        heading_match = re.match(r'^(#{1,6})\s+(.+)$', line)
        if heading_match:
            level = len(heading_match.group(1))
            text = heading_match.group(2)
            headings.append({
                'level': level,
                'text': text
            })
    return headings

def extract_summary(content, max_length=200):
    """Extract a summary from markdown content."""
    # Remove headings, code blocks, and other markdown elements
    clean_text = re.sub(r'^#{1,6}\s+.*$', '', content, flags=re.MULTILINE)
    clean_text = re.sub(r'```.*?```', '', clean_text, flags=re.DOTALL)
    clean_text = re.sub(r'`.*?`', '', clean_text)
    clean_text = re.sub(r'\[.*?\]\(.*?\)', '', clean_text)
    
    # Get first paragraph
    paragraphs = [p for p in clean_text.split('\n\n') if p.strip()]
    if not paragraphs:
        return ""
    
    summary = paragraphs[0].strip()
    
    # Truncate if needed
    if len(summary) > max_length:
        summary = summary[:max_length].rsplit(' ', 1)[0] + '...'
    
    return summary
```

### File Utilities

Create `utils/file_utils.py`:

```python
# utils/file_utils.py
import os
import json
import yaml
from datetime import datetime

def ensure_dir(directory):
    """Ensure directory exists."""
    if not os.path.exists(directory):
        os.makedirs(directory)

def save_markdown(content, filepath):
    """Save content as markdown file."""
    ensure_dir(os.path.dirname(filepath))
    with open(filepath, 'w', encoding='utf-8') as f:
        f.write(content)
    return filepath

def save_json(data, filepath):
    """Save data as JSON file."""
    ensure_dir(os.path.dirname(filepath))
    with open(filepath, 'w', encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=False, indent=2)
    return filepath

def load_markdown(filepath):
    """Load markdown file."""
    with open(filepath, 'r', encoding='utf-8') as f:
        return f.read()

def load_json(filepath):
    """Load JSON file."""
    with open(filepath, 'r', encoding='utf-8') as f:
        return json.load(f)

def generate_filename(prefix, extension, directory=None):
    """Generate a filename with timestamp."""
    timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
    filename = f"{prefix}_{timestamp}.{extension}"
    if directory:
        ensure_dir(directory)
        return os.path.join(directory, filename)
    return filename
```

## Step 3: Implement Core Pipeline Modules

### Create Module Initialization

Create `modules/__init__.py`:

```python
# modules/__init__.py
# Empty init file to make the directory a package
```

### Idea Generator Module

Create `modules/idea_generator.py`:

```python
# modules/idea_generator.py
import os
from utils.llm_client import LLMClient
from utils.markdown_utils import extract_metadata, extract_summary
from utils.file_utils import save_json, generate_filename

class IdeaGenerator:
    def __init__(self, config_path):
        self.llm_client = LLMClient(config_path)
        self.config_path = config_path
        
    def generate_ideas(self, raw_data, num_ideas=5):
        """Generate article ideas based on raw data and blog theme."""
        # Load prompt template
        prompt_path = os.path.join(os.path.dirname(os.path.dirname(__file__)), 
                                  'prompts', 'idea_generation.txt')
        with open(prompt_path, 'r', encoding='utf-8') as f:
            prompt_template = f.read()
        
        # Extract metadata and summary
        metadata, content = extract_metadata(raw_data['content'])
        summary = extract_summary(content)
        
        # Prepare prompt variables
        prompt_vars = {
            'raw_content': content[:3000],  # Limit content length
            'content_summary': summary,
            'num_ideas': num_ideas,
            'blog_theme': self.llm_client.config['blog_theme'],
            'audience': self.llm_client.config['audience'],
            'trend_sources': self.llm_client.config['trend_sources']
        }
        
        # Format prompt
        prompt = prompt_template.format(**prompt_vars)
        
        # Generate ideas
        response = self.llm_client.generate_with_json_output(
            prompt=prompt,
            temperature=0.7,
            max_tokens=2000
        )
        
        # Save ideas to file
        ideas_path = generate_filename(
            'ideas', 'json', 
            os.path.join(os.path.dirname(os.path.dirname(__file__)), 'data', 'ideas')
        )
        save_json(response, ideas_path)
        
        return {
            'ideas': response.get('ideas', []),
            'file_path': ideas_path,
            'raw_data': raw_data
        }
```

### Draft Creator Module

Create `modules/draft_creator.py`:

```python
# modules/draft_creator.py
import os
from utils.llm_client import LLMClient
from utils.file_utils import save_markdown, generate_filename

class DraftCreator:
    def __init__(self, config_path):
        self.llm_client = LLMClient(config_path)
        self.config_path = config_path
        
    def create_draft(self, raw_data, selected_idea):
        """Create initial draft in neutral tone."""
        # Load prompt template
        prompt_path = os.path.join(os.path.dirname(os.path.dirname(__file__)), 
                                  'prompts', 'draft_creation.txt')
        with open(prompt_path, 'r', encoding='utf-8') as f:
            prompt_template = f.read()
        
        # Prepare prompt variables
        prompt_vars = {
            'raw_content': raw_data['content'][:4000],  # Limit content length
            'idea_title': selected_idea['title'],
            'idea_description': selected_idea['description'],
            'key_points': '\n'.join([f"- {point}" for point in selected_idea['key_points']]),
            'target_audience': selected_idea['target_audience'],
            'draft_guidelines': self.llm_client.config['draft_guidelines']
        }
        
        # Format prompt
        prompt = prompt_template.format(**prompt_vars)
        
        # Generate draft
        draft_content = self.llm_client.generate(
            prompt=prompt,
            temperature=0.5,
            max_tokens=3500
        )
        
        # Save draft to file
        draft_path = generate_filename(
            'draft', 'md', 
            os.path.join(os.path.dirname(os.path.dirname(__file__)), 'data', 'drafts')
        )
        save_markdown(draft_content, draft_path)
        
        return {
            'content': draft_content,
            'file_path': draft_path,
            'idea': selected_idea,
            'raw_data': raw_data
        }
```

### Style Transformer Module

Create `modules/style_transformer.py`:

```python
# modules/style_transformer.py
import os
from utils.llm_client import LLMClient
from utils.file_utils import save_markdown, generate_filename

class StyleTransformer:
    def __init__(self, config_path):
        self.llm_client = LLMClient(config_path)
        self.config_path = config_path
        
    def apply_style(self, draft):
        """Transform approved draft to match user's tone and style."""
        # Load prompt template
        prompt_path = os.path.join(os.path.dirname(os.path.dirname(__file__)), 
                                  'prompts', 'style_transformation.txt')
        with open(prompt_path, 'r', encoding='utf-8') as f:
            prompt_template = f.read()
        
        # Prepare style guide and examples
        style_guide = self.llm_client.config['style_guide']
        style_examples = self.llm_client.config['style_examples']
        
        # Format style guide sections
        style_guide_formatted = ""
        for category, items in style_guide.items():
            style_guide_formatted += f"\n{category.replace('_', ' ').title()}:\n"
            for item in items:
                style_guide_formatted += f"- {item}\n"
        
        # Format style examples
        style_examples_formatted = "\n\nEXAMPLES OF WRITING STYLE:\n\n"
        for i, example in enumerate(style_examples, 1):
            style_examples_formatted += f"Example {i}:\n\"\"\"\n{example}\n\"\"\"\n\n"
        
        # Prepare prompt variables
        prompt_vars = {
            'draft_content': draft['content'],
            'idea_title': draft['idea']['title'],
            'style_guide': style_guide_formatted,
            'style_examples': style_examples_formatted
        }
        
        # Format prompt
        prompt = prompt_template.format(**prompt_vars)
        
        # Generate styled content
        styled_content = self.llm_client.generate(
            prompt=prompt,
            temperature=0.7,
            max_tokens=4000
        )
        
        # Save styled content to file
        final_path = generate_filename(
            'final', 'md', 
            os.path.join(os.path.dirname(os.path.dirname(__file__)), 'data', 'final')
        )
        save_markdown(styled_content, final_path)
        
        return {
            'content': styled_content,
            'file_path': final_path,
            'original_draft': draft['content'],
            'idea': draft['idea']
        }
```

### Quality Evaluator Module

Create `modules/quality_evaluator.py`:

```python
# modules/quality_evaluator.py
import os
from utils.llm_client import LLMClient
from utils.file_utils import save_json, generate_filename

class QualityEvaluator:
    def __init__(self, config_path):
        self.llm_client = LLMClient(config_path)
        self.config_path = config_path
        
    def evaluate_quality(self, content, original_style=None):
        """Evaluate content quality based on specified criteria."""
        # Load prompt template
        prompt_path = os.path.join(os.path.dirname(os.path.dirname(__file__)), 
                                  'prompts', 'quality_evaluation.txt')
        with open(prompt_path, 'r', encoding='utf-8') as f:
            prompt_template = f.read()
        
        # Get quality criteria
        criteria = self.llm_client.config['quality_criteria']
        
        # Format criteria
        criteria_formatted = ""
        for criterion, description in criteria.items():
            criteria_formatted += f"- {criterion.replace('_', ' ').title()}: {description}\n"
        
        # Prepare prompt variables
        prompt_vars = {
            'content': content['content'],
            'criteria': criteria_formatted
        }
        
        # Add original style for comparison if provided
        if original_style:
            prompt_vars['original_style'] = original_style
            
        # Format prompt
        prompt = prompt_template.format(**prompt_vars)
        
        # Generate evaluation
        evaluation = self.llm_client.generate_with_json_output(
            prompt=prompt,
            temperature=0.3,
            max_tokens=2000
        )
        
        # Save evaluation to file
        eval_path = generate_filename(
            'evaluation', 'json', 
            os.path.join(os.path.dirname(os.path.dirname(__file__)), 'data', 'final')
        )
        save_json(evaluation, eval_path)
        
        return {
            'evaluation': evaluation,
            'file_path': eval_path,
            'content': content
        }
```

## Step 4: Create Prompt Templates

### Idea Generation Prompt

Create `prompts/idea_generation.txt`:

```
Generate {num_ideas} article ideas based on the following raw content and blog theme.

RAW CONTENT:
{raw_content}

CONTENT SUMMARY:
{content_summary}

BLOG THEME:
Main Topics: {blog_theme[main_topics]}
Style: {blog_theme[style]}
Tone: {blog_theme[tone]}

TARGET AUDIENCE:
Primary: {audience[primary]}
Interests: {audience[interests]}
Knowledge Level: {audience[knowledge_level]}

TREND CONSIDERATIONS:
{trend_sources}

For each idea, provide:
1. A catchy and specific title
2. A brief description (2-3 sentences)
3. 3-5 key points to cover
4. Target audience segment
5. Engagement potential (why readers would be interested)
6. Relevance to current trends

Format your response as a JSON object with an "ideas" array containing each idea as an object with the fields: title, description, key_points (array), target_audience, engagement_potential, and trend_relevance.
```

### Draft Creation Prompt

Create `prompts/draft_creation.txt`:

```
Create a well-structured draft article based on the following idea and raw content. Write in a neutral, natural tone (not in the user's personal style yet).

ARTICLE IDEA:
Title: {idea_title}
Description: {idea_description}
Key Points to Cover:
{key_points}

TARGET AUDIENCE:
{target_audience}

RAW CONTENT FOR REFERENCE:
{raw_content}

DRAFT GUIDELINES:
Structure:
{draft_guidelines[structure]}
Length: {draft_guidelines[length]}
Tone: {draft_guidelines[tone]}

Please create a complete draft that:
1. Has a compelling introduction that hooks the reader
2. Covers all the key points thoroughly
3. Includes concrete examples and evidence
4. Provides actionable insights or takeaways
5. Ends with a strong conclusion

The draft should be well-organized with clear headings and a logical flow. Use markdown formatting for structure (headings, lists, emphasis).

Write the complete draft now, ready for style transformation in the next step.
```

### Style Transformation Prompt

Create `prompts/style_transformation.txt`:

```
Transform the following draft into content that perfectly matches the user's unique writing style and tone.

DRAFT CONTENT:
{draft_content}

ARTICLE TITLE:
{idea_title}

STYLE GUIDE:
{style_guide}

{style_examples}

TRANSFORMATION INSTRUCTIONS:
1. Maintain the same information and key points as the original draft
2. Transform the language, tone, and rhetorical approach to match the style guide and examples
3. Ensure the following elements are present:
   - Varied sentence structure (mix of short and long sentences)
   - Conversational tone with rhetorical questions
   - First-person perspective where appropriate
   - Concrete examples and analogies
   - At least one "Imagine if..." or "Представьте, что..." scenario
   - Parenthetical asides for supplementary information
   - Appropriate code-switching between Russian and English based on the topic
   - Clear section structure with smooth transitions

4. The final content should read as if it was written directly by the user, not as a translation or adaptation

Rewrite the entire draft in the user's authentic style, maintaining the same information and structure but completely transforming the voice and tone.
```

### Quality Evaluation Prompt

Create `prompts/quality_evaluation.txt`:

```
Evaluate the quality of the following content based on the specified criteria.

CONTENT:
{content}

EVALUATION CRITERIA:
{criteria}

For each criterion, provide:
1. A score from 1-10
2. Specific examples from the content
3. Detailed suggestions for improvement

Also provide an overall assessment and a list of the top 3 strengths and top 3 areas for improvement.

Format your response as a JSON object with the following structure:
{
  "criteria_scores": {
    "criterion_name": {
      "score": 0,
      "examples": ["example1", "example2"],
      "suggestions": ["suggestion1", "suggestion2"]
    }
  },
  "overall_assessment": {
    "score": 0,
    "summary": "Overall assessment text"
  },
  "strengths": ["strength1", "strength2", "strength3"],
  "improvements": ["improvement1", "improvement2", "improvement3"]
}
```

## Step 5: Create Main Pipeline Controller

Create `pipeline.py` in the project root:

```python
#!/usr/bin/env python3
# pipeline.py
import os
import sys
import argparse
from tqdm import tqdm
import time
import json
from colorama import init, Fore, Style

# Initialize colorama
init()

# Add project root to path
sys.path.append(os.path.dirname(os.path.abspath(__file__)))

# Import modules
from utils.file_utils import load_markdown, ensure_dir
from modules.idea_generator import IdeaGenerator
from modules.draft_creator import DraftCreator
from modules.style_transformer import StyleTransformer
from modules.quality_evaluator import QualityEvaluator

class ContentPipeline:
    def __init__(self, config_path):
        self.config_path = config_path
        self.idea_generator = IdeaGenerator(config_path)
        self.draft_creator = DraftCreator(config_path)
        self.style_transformer = StyleTransformer(config_path)
        self.quality_evaluator = QualityEvaluator(config_path)
        
    def process(self, markdown_path, interactive=True):
        """Process the content pipeline."""
        print(f"{Fore.CYAN}Starting content pipeline...{Style.RESET_ALL}")
        
        # Step 1: Load raw data
        print(f"{Fore.YELLOW}Step 1: Loading raw data...{Style.RESET_ALL}")
        raw_content = load_markdown(markdown_path)
        raw_data = {
            'content': raw_content,
            'source_path': markdown_path
        }
        print(f"{Fore.GREEN}✓ Raw data loaded from {markdown_path}{Style.RESET_ALL}")
        
        # Step 2: Generate ideas
        print(f"{Fore.YELLOW}Step 2: Generating article ideas...{Style.RESET_ALL}")
        with tqdm(total=100, desc="Generating ideas") as pbar:
            ideas_result = self.idea_generator.generate_ideas(raw_data)
            pbar.update(100)
        
        print(f"{Fore.GREEN}✓ Generated {len(ideas_result['ideas'])} ideas{Style.RESET_ALL}")
        print(f"  Ideas saved to: {ideas_result['file_path']}")
        
        # Display ideas
        for i, idea in enumerate(ideas_result['ideas'], 1):
            print(f"\n{Fore.CYAN}Idea {i}: {idea['title']}{Style.RESET_ALL}")
            print(f"  {idea['description']}")
            print("  Key points:")
            for point in idea['key_points']:
                print(f"   - {point}")
        
        # Select idea
        selected_idea = None
        if interactive:
            while True:
                try:
                    choice = int(input(f"\n{Fore.YELLOW}Select an idea (1-{len(ideas_result['ideas'])}): {Style.RESET_ALL}"))
                    if 1 <= choice <= len(ideas_result['ideas']):
                        selected_idea = ideas_result['ideas'][choice-1]
                        break
                    else:
                        print(f"{Fore.RED}Invalid choice. Please select a number between 1 and {len(ideas_result['ideas'])}.{Style.RESET_ALL}")
                except ValueError:
                    print(f"{Fore.RED}Please enter a number.{Style.RESET_ALL}")
        else:
            # Auto-select first idea
            selected_idea = ideas_result['ideas'][0]
            
        print(f"{Fore.GREEN}✓ Selected idea: {selected_idea['title']}{Style.RESET_ALL}")
        
        # Step 3: Create draft
        print(f"{Fore.YELLOW}Step 3: Creating initial draft...{Style.RESET_ALL}")
        with tqdm(total=100, desc="Creating draft") as pbar:
            draft_result = self.draft_creator.create_draft(raw_data, selected_idea)
            pbar.update(100)
            
        print(f"{Fore.GREEN}✓ Draft created{Style.RESET_ALL}")
        print(f"  Draft saved to: {draft_result['file_path']}")
        
        # Wait for draft approval
        if interactive:
            print(f"\n{Fore.CYAN}Please review the draft at: {draft_result['file_path']}{Style.RESET_ALL}")
            while True:
                approval = input(f"{Fore.YELLOW}Approve draft? (yes/no): {Style.RESET_ALL}").lower()
                if approval in ['yes', 'y']:
                    break
                elif approval in ['no', 'n']:
                    print(f"{Fore.RED}Draft rejected. Pipeline stopped.{Style.RESET_ALL}")
                    return
                else:
                    print(f"{Fore.RED}Please enter 'yes' or 'no'.{Style.RESET_ALL}")
        
        # Step 4: Apply style
        print(f"{Fore.YELLOW}Step 4: Applying user's style...{Style.RESET_ALL}")
        with tqdm(total=100, desc="Applying style") as pbar:
            styled_result = self.style_transformer.apply_style(draft_result)
            pbar.update(100)
            
        print(f"{Fore.GREEN}✓ Style applied{Style.RESET_ALL}")
        print(f"  Final content saved to: {styled_result['file_path']}")
        
        # Step 5: Evaluate quality
        print(f"{Fore.YELLOW}Step 5: Evaluating content quality...{Style.RESET_ALL}")
        with tqdm(total=100, desc="Evaluating quality") as pbar:
            eval_result = self.quality_evaluator.evaluate_quality(styled_result)
            pbar.update(100)
            
        print(f"{Fore.GREEN}✓ Quality evaluation complete{Style.RESET_ALL}")
        print(f"  Evaluation saved to: {eval_result['file_path']}")
        
        # Display evaluation summary
        print(f"\n{Fore.CYAN}Quality Evaluation Summary:{Style.RESET_ALL}")
        print(f"  Overall score: {eval_result['evaluation']['overall_assessment']['score']}/10")
        print(f"  Summary: {eval_result['evaluation']['overall_assessment']['summary']}")
        
        print(f"\n{Fore.CYAN}Top Strengths:{Style.RESET_ALL}")
        for strength in eval_result['evaluation']['strengths']:
            print(f"  - {strength}")
            
        print(f"\n{Fore.CYAN}Areas for Improvement:{Style.RESET_ALL}")
        for improvement in eval_result['evaluation']['improvements']:
            print(f"  - {improvement}")
        
        print(f"\n{Fore.GREEN}Pipeline complete!{Style.RESET_ALL}")
        print(f"Final content: {styled_result['file_path']}")
        
        return {
            'raw_data': raw_data,
            'ideas': ideas_result,
            'selected_idea': selected_idea,
            'draft': draft_result,
            'final_content': styled_result,
            'evaluation': eval_result
        }

def main():
    parser = argparse.ArgumentParser(description='Content Creation Pipeline')
    parser.add_argument('--input', '-i', required=True, help='Path to input markdown file')
    parser.add_argument('--config', '-c', default='config.yaml', help='Path to configuration file')
    parser.add_argument('--non-interactive', '-n', action='store_true', help='Run in non-interactive mode')
    
    args = parser.parse_args()
    
    # Ensure input file exists
    if not os.path.exists(args.input):
        print(f"{Fore.RED}Error: Input file {args.input} not found.{Style.RESET_ALL}")
        return 1
        
    # Ensure config file exists
    if not os.path.exists(args.config):
        print(f"{Fore.RED}Error: Config file {args.config} not found.{Style.RESET_ALL}")
        return 1
    
    # Run pipeline
    pipeline = ContentPipeline(args.config)
    pipeline.process(args.input, not args.non_interactive)
    
    return 0

if __name__ == '__main__':
    sys.exit(main())
```

## Step 6: Create Example Input File

Create an example markdown file in `data/raw/example.md`:

```markdown
---
title: Understanding User Behavior in Digital Products
source: Research notes
date: 2025-05-22
---

# Notes on User Behavior Research

## Key Findings from Recent Studies

Recent studies by Nielsen Norman Group and Baymard Institute have shown that users typically spend less than 15 seconds on a webpage before deciding whether to stay or leave. This decision is heavily influenced by:

- Clarity of value proposition
- Visual hierarchy
- Loading speed
- Mobile responsiveness

## Behavioral Biases in Digital Interactions

Several cognitive biases affect how users interact with digital products:

1. **Familiarity bias**: Users prefer interfaces that match their mental models and previous experiences.

2. **Choice paralysis**: Too many options can lead to decision fatigue and abandonment.

3. **Social proof**: Users are more likely to engage with features that show evidence of other users' positive experiences.

4. **Loss aversion**: Users are more motivated by avoiding losses than acquiring gains.

## Practical Applications

These findings can be applied to product design through:

- Simplified onboarding flows that build on familiar patterns
- Strategic limitation of choices at decision points
- Incorporation of social proof elements (reviews, usage statistics)
- Framing features in terms of preventing losses rather than gaining benefits

## Recent Case Studies

### E-commerce Platform Redesign
A major e-commerce platform reduced their checkout abandonment rate by 23% by simplifying the checkout process from 5 steps to 3 steps and adding social proof elements.

### Banking App Engagement
A banking application increased feature adoption by 34% by reorganizing features based on usage frequency and adding progress indicators for financial goals.

## Future Research Directions

- Impact of dark patterns on long-term user trust
- Cultural differences in response to various UX patterns
- Effectiveness of gamification elements across different user demographics
- Ethical considerations in behavioral design
```

## Step 7: Run the Pipeline

1. Set your OpenAI API key as an environment variable:
   ```bash
   # On Windows
   set OPENAI_API_KEY=your-api-key-here
   
   # On macOS/Linux
   export OPENAI_API_KEY=your-api-key-here
   ```

2. Run the pipeline:
   ```bash
   python pipeline.py --input data/raw/example.md
   ```

3. Follow the interactive prompts to:
   - Select an idea
   - Review and approve the draft
   - Review the final styled content and quality evaluation

## Troubleshooting

### API Key Issues
- Ensure your OpenAI API key is correctly set as an environment variable
- Check that your API key has access to the required models (GPT-4)

### Dependency Issues
- If you encounter module import errors, ensure all required packages are installed
- Check that your Python version is 3.9 or higher

### Content Generation Issues
- If generated content is too short, try increasing the `max_tokens` parameter
- If style transformation is not effective, try adding more detailed style examples

### File Path Issues
- Ensure all directories exist before running the pipeline
- Use absolute paths if relative paths cause issues
