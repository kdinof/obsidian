# Prompt Templates for Content Creation Pipeline

This document provides detailed prompt templates for each stage of the content creation pipeline, with explanations of their structure and customization options.

## 1. Idea Generation Prompt

### Purpose
Generate creative article ideas based on raw content, blog theme, and current trends.

### Template Structure
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

### Customization Options
- **Number of Ideas**: Adjust `{num_ideas}` based on how many options you want
- **Content Length**: Limit `{raw_content}` length to avoid token limits
- **Topic Focus**: Modify `{blog_theme}` to emphasize specific topics
- **Audience Specificity**: Make `{audience}` more or less specific
- **Trend Sources**: Update `{trend_sources}` to focus on particular trends

### Example Customization
For more technical content:
```
BLOG THEME:
Main Topics: ["Technical Design Systems", "Frontend Architecture", "Design-Development Collaboration"]
Style: "In-depth technical analysis with practical code examples"
Tone: "Professional but accessible to intermediate developers"

TARGET AUDIENCE:
Primary: "Frontend developers and UX engineers"
Interests: ["Component architecture", "Design systems", "Performance optimization"]
Knowledge Level: "Intermediate to advanced"
```

## 2. Draft Creation Prompt

### Purpose
Create a well-structured initial draft in a neutral tone that covers all key points from the selected idea.

### Template Structure
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

### Customization Options
- **Structure Requirements**: Modify `{draft_guidelines[structure]}` for different article formats
- **Length**: Adjust `{draft_guidelines[length]}` based on desired word count
- **Tone Baseline**: Change `{draft_guidelines[tone]}` for different baseline tones
- **Content Focus**: Emphasize specific aspects of the key points
- **Format Requirements**: Add specific formatting instructions

### Example Customization
For a how-to guide format:
```
DRAFT GUIDELINES:
Structure:
- "Problem statement and why it matters"
- "Step-by-step solution with numbered steps"
- "Common pitfalls and how to avoid them"
- "Before/after comparison showing impact"
Length: "1200-1800 words with 40% dedicated to the step-by-step section"
Tone: "Practical and solution-oriented"

Please create a complete draft that:
1. Clearly defines the problem in the introduction
2. Provides detailed, actionable steps with code examples where relevant
3. Includes troubleshooting advice for common issues
4. Shows concrete before/after scenarios
5. Ends with next steps or advanced applications
```

## 3. Style Transformation Prompt

### Purpose
Transform the approved draft into content that perfectly matches the user's unique writing style and tone.

### Template Structure
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

### Customization Options
- **Style Guide Detail**: Expand or focus `{style_guide}` on specific style elements
- **Style Examples**: Add more diverse `{style_examples}` for better style matching
- **Transformation Focus**: Modify instructions to emphasize particular style aspects
- **Language Balance**: Adjust Russian/English code-switching requirements
- **Rhetorical Elements**: Specify different rhetorical devices to include

### Example Customization
For more technical content with stronger personal voice:
```
TRANSFORMATION INSTRUCTIONS:
1. Maintain the same technical information and key points as the original draft
2. Transform the language, tone, and rhetorical approach to match the style guide and examples
3. Ensure the following elements are present:
   - Technical explanations broken down with simple analogies
   - Personal anecdotes about solving similar problems
   - Direct questions to the reader about their experiences
   - Code examples commented in a conversational style
   - At least two "Imagine if..." scenarios relating technical concepts to everyday experiences
   - Parenthetical asides that add humor or personal insights
   - 70% Russian with English technical terms
   - Section transitions that reference previous points

4. The content should feel like a technical mentor sharing insights, not a formal tutorial
```

## 4. Quality Evaluation Prompt

### Purpose
Evaluate the quality of the generated content against specific criteria and provide improvement suggestions.

### Template Structure
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

### Customization Options
- **Evaluation Criteria**: Modify `{criteria}` to focus on specific quality aspects
- **Scoring System**: Change the scoring scale or add weighting
- **Comparison Elements**: Add original style examples for comparison
- **Feedback Detail**: Adjust the level of detail requested in feedback
- **Improvement Focus**: Direct the evaluation toward specific improvement areas

### Example Customization
For style-focused evaluation:
```
EVALUATION CRITERIA:
- Style Authenticity: How well the content matches the user's unique style
- Voice Consistency: Consistency of voice throughout the piece
- Rhetorical Effectiveness: How well rhetorical devices engage the reader
- Multilingual Balance: Appropriate balance and flow between Russian and English
- Personal Connection: Strength of personal perspective and connection with reader

For each criterion, provide:
1. A score from 1-10
2. At least 3 specific examples from the content
3. Detailed suggestions for improvement with alternative phrasings

Also compare the styled content with the original style examples and identify:
1. Successfully matched style elements
2. Missing style elements
3. Opportunities to strengthen style authenticity

Format your response as a JSON object with the following structure:
{
  "criteria_scores": {
    "criterion_name": {
      "score": 0,
      "examples": ["example1", "example2", "example3"],
      "suggestions": ["suggestion1", "suggestion2", "suggestion3"]
    }
  },
  "style_comparison": {
    "matched_elements": ["element1", "element2"],
    "missing_elements": ["element1", "element2"],
    "strengthening_opportunities": ["opportunity1", "opportunity2"]
  },
  "overall_assessment": {
    "score": 0,
    "summary": "Overall assessment text"
  }
}
```

## Advanced Prompt Techniques

### 1. Chain-of-Thought Prompting
Enhance reasoning by asking the model to think step-by-step:

```
Before generating article ideas, analyze the raw content step by step:

1. Identify the main topics and themes in the raw content
2. Determine what aspects would be most relevant to the target audience
3. Consider how these topics relate to current trends
4. Identify unique angles or insights that could differentiate the content
5. Evaluate which formats would best present this information

Based on this analysis, generate {num_ideas} article ideas...
```

### 2. Few-Shot Learning
Provide examples of desired outputs:

```
Here are examples of high-quality article ideas:

EXAMPLE 1:
Title: "The Paradox of Choice: Why Fewer Options Lead to Better UX"
Description: "Explores how choice overload affects user decisions and how strategic limitation of options can improve conversion rates and user satisfaction."
Key Points: 
- "Psychological research on decision paralysis"
- "Case studies of simplified interfaces that improved metrics"
- "Practical framework for determining optimal number of choices"
- "Implementation strategies for different product types"
Target Audience: "UX designers and product managers working on complex interfaces"
Engagement Potential: "Addresses a common pain point in product design with actionable solutions"
Trend Relevance: "Aligns with minimalist design trends and focus on user cognitive load"

EXAMPLE 2:
[Another example...]

Now, generate {num_ideas} article ideas based on the following raw content...
```

### 3. Persona-Based Prompting
Frame the prompt from a specific perspective:

```
As an experienced UX writer with deep knowledge of behavioral psychology, generate article ideas that would provide unique insights into user behavior based on the following raw content...
```

### 4. Constraint-Based Prompting
Add specific constraints to guide the output:

```
Generate article ideas with the following constraints:
- Each idea must connect to at least one cognitive bias
- Ideas should be implementable with examples in both B2B and B2C contexts
- Each idea should include a counterintuitive insight or challenge a common assumption
- Ideas should be suitable for both beginners and advanced practitioners
```

## Prompt Optimization Tips

1. **Start Broad, Then Refine**: Begin with general prompts and iteratively refine based on results

2. **Use Clear Formatting**: Separate sections with clear headers and use consistent formatting

3. **Balance Specificity and Creativity**: Too specific prompts limit creativity, too vague prompts produce inconsistent results

4. **Include Context**: Provide relevant background information for more informed outputs

5. **Test Temperature Settings**: Lower temperature (0.2-0.5) for more predictable outputs, higher (0.7-0.9) for more creative results

6. **Iterate Based on Results**: Analyze outputs and adjust prompts to address any shortcomings

7. **Use System and User Role Separation**: For APIs that support it, use system messages to set context and user messages for specific instructions

8. **Prompt Versioning**: Keep track of prompt versions and their performance to identify what works best
