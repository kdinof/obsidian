# Content Creation Pipeline Design

## Architecture Overview

This document outlines the design for a local, code-based content creation pipeline that transforms raw markdown data into polished content matching the user's unique style.

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Raw Data   │    │    Idea     │    │    Draft    │    │    Style    │
│   Input     │───▶│  Generation │───▶│   Creation  │───▶│ Application │───▶ Final Content
│ (Markdown)  │    │             │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

## Technology Stack

- **Programming Language**: Python 3.9+
- **LLM Integration**: OpenAI API (GPT-4)
- **Markdown Processing**: Python-Markdown library
- **File Handling**: Standard Python libraries
- **Configuration**: YAML or JSON for settings

## Module Design

### 1. Pipeline Controller

```python
class ContentPipeline:
    def __init__(self, config_path):
        # Load configuration
        self.config = self._load_config(config_path)
        # Initialize API clients
        self.llm_client = self._init_llm_client()
        
    def process(self, markdown_path):
        # Main pipeline execution
        raw_data = self._load_markdown(markdown_path)
        ideas = self.generate_ideas(raw_data)
        selected_idea = self._select_idea(ideas)  # Could be automated or manual
        draft = self.create_draft(raw_data, selected_idea)
        # Wait for approval (could be via file system flag or interactive)
        if self._is_draft_approved(draft):
            final_content = self.apply_style(draft)
            self._save_output(final_content)
            return final_content
        return None
```

### 2. Raw Data Processor

```python
def _load_markdown(self, markdown_path):
    """Load and preprocess markdown content."""
    with open(markdown_path, 'r', encoding='utf-8') as f:
        content = f.read()
    
    # Extract metadata if present (YAML frontmatter)
    metadata, content = self._extract_metadata(content)
    
    # Basic preprocessing
    content = self._clean_content(content)
    
    return {
        'metadata': metadata,
        'content': content,
        'source_path': markdown_path
    }
```

### 3. Idea Generator

```python
def generate_ideas(self, raw_data):
    """Generate article ideas based on raw data, blog theme, and trends."""
    # Prepare prompt with raw data, blog theme, and audience info
    prompt = self._create_idea_generation_prompt(
        raw_data, 
        self.config['blog_theme'],
        self.config['audience'],
        self.config.get('trend_sources', [])
    )
    
    # Call LLM API
    response = self.llm_client.generate(
        prompt=prompt,
        temperature=0.7,
        max_tokens=1000
    )
    
    # Parse and structure ideas
    ideas = self._parse_ideas(response)
    
    return ideas
```

### 4. Draft Creator

```python
def create_draft(self, raw_data, selected_idea):
    """Create initial draft in neutral tone."""
    # Prepare prompt with raw data and selected idea
    prompt = self._create_draft_prompt(
        raw_data,
        selected_idea,
        self.config.get('draft_guidelines', {})
    )
    
    # Call LLM API
    response = self.llm_client.generate(
        prompt=prompt,
        temperature=0.5,  # Lower temperature for more focused output
        max_tokens=3000
    )
    
    # Post-process draft
    draft = self._format_draft(response)
    
    # Save draft for review
    draft_path = self._save_draft(draft)
    
    return {
        'content': draft,
        'idea': selected_idea,
        'path': draft_path
    }
```

### 5. Style Transformer

```python
def apply_style(self, draft):
    """Transform approved draft to match user's tone and style."""
    # Prepare prompt with draft and style guide
    prompt = self._create_style_prompt(
        draft['content'],
        self.config['style_guide'],
        self.config.get('style_examples', [])
    )
    
    # Call LLM API
    response = self.llm_client.generate(
        prompt=prompt,
        temperature=0.7,
        max_tokens=3000
    )
    
    # Post-process styled content
    styled_content = self._format_styled_content(response)
    
    return {
        'content': styled_content,
        'original_draft': draft['content'],
        'idea': draft['idea']
    }
```

### 6. Quality Evaluation

```python
def evaluate_quality(self, content, criteria=None):
    """Evaluate content quality based on specified criteria."""
    if criteria is None:
        criteria = self.config.get('quality_criteria', {})
    
    # Prepare evaluation prompt
    prompt = self._create_evaluation_prompt(content, criteria)
    
    # Call LLM API
    response = self.llm_client.generate(
        prompt=prompt,
        temperature=0.2,  # Low temperature for consistent evaluation
        max_tokens=1000
    )
    
    # Parse evaluation results
    evaluation = self._parse_evaluation(response)
    
    return evaluation
```

## Configuration Structure

```yaml
# config.yaml
api:
  provider: "openai"
  model: "gpt-4"
  api_key: "${OPENAI_API_KEY}"  # Use environment variable

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
  # Detailed style characteristics from previous analysis
  language_usage:
    - "Mix of Russian and English"
    - "Specialized terminology with everyday language"
  sentence_structure:
    - "Varied length"
    - "Conversational flow"
    - "Rhetorical questions"
  tone:
    - "Informal and friendly"
    - "First-person perspective"
    - "Balance of expertise and accessibility"
  # Additional style elements...

style_examples:
  - "Example 1 text..."
  - "Example 2 text..."

quality_criteria:
  content_accuracy: "Information is factually correct and well-sourced"
  style_match: "Content matches user's unique style"
  engagement: "Content is engaging and maintains reader interest"
  structure: "Content is well-organized with clear flow"
  value: "Content provides actionable insights or valuable information"
```

## File Structure

```
content-pipeline/
├── config.yaml                # Configuration file
├── pipeline.py                # Main pipeline controller
├── modules/
│   ├── __init__.py
│   ├── raw_data.py            # Raw data processing
│   ├── idea_generator.py      # Idea generation
│   ├── draft_creator.py       # Draft creation
│   ├── style_transformer.py   # Style transformation
│   └── quality_evaluator.py   # Quality evaluation
├── prompts/
│   ├── idea_generation.txt    # Prompt templates
│   ├── draft_creation.txt
│   ├── style_transformation.txt
│   └── quality_evaluation.txt
├── utils/
│   ├── __init__.py
│   ├── markdown_utils.py      # Markdown processing utilities
│   ├── llm_client.py          # LLM API client
│   └── file_utils.py          # File handling utilities
├── data/
│   ├── raw/                   # Raw markdown inputs
│   ├── ideas/                 # Generated ideas
│   ├── drafts/                # Created drafts
│   └── final/                 # Final styled content
└── examples/
    ├── raw_example.md         # Example files
    ├── idea_example.json
    ├── draft_example.md
    └── final_example.md
```

## Workflow Sequence

1. **Initialization**:
   - Load configuration
   - Set up LLM client
   - Prepare working directories

2. **Raw Data Processing**:
   - Load markdown file
   - Extract metadata if present
   - Clean and preprocess content

3. **Idea Generation**:
   - Generate multiple article ideas
   - Save ideas to file
   - Select idea (automated or manual)

4. **Draft Creation**:
   - Create initial draft based on selected idea
   - Save draft for review
   - Wait for approval

5. **Style Transformation**:
   - Transform approved draft to match user's style
   - Save final styled content

6. **Quality Evaluation**:
   - Evaluate content against quality criteria
   - Generate improvement suggestions
   - Save evaluation report

## Interaction Model

The pipeline can be designed with different interaction models:

1. **Command Line Interface**:
   ```
   python pipeline.py --input raw/data.md --output final/article.md
   ```

2. **Interactive Mode**:
   ```
   python pipeline.py --interactive
   ```
   This would prompt the user at key decision points (idea selection, draft approval).

3. **File System Monitoring**:
   The pipeline could watch specific directories for new files or status changes.

## Error Handling

- Implement robust error handling for API calls
- Provide clear error messages
- Save intermediate results to allow resuming from failures
- Log all operations for debugging

## Extension Points

The design allows for several extension points:

1. **Alternative LLM Providers**: Swap OpenAI for other providers
2. **Custom Preprocessors**: Add specialized preprocessing for different content types
3. **Quality Improvement Loops**: Add automated revision cycles
4. **Output Formats**: Add support for different output formats (HTML, PDF, etc.)
5. **Integration with Publishing Platforms**: Add direct publishing capabilities
