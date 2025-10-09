
```

# Prompt: Generate Targeted Article Ideas from Raw Content  ## Instructions: Your goal is to generate 5-7 distinct and actionable article ideas. These ideas must be directly inspired by and synthesize insights from the provided `raw_content`, while aligning with the specified `blog_focus_and_key_trends`, `target_audience`, and `knowledge_level`. Each idea should offer unique value and perspective.

## Inputs:
### 1. Raw Content:
{raw_content_placeholder}

### 2. Blog Focus and Key Trends:  
*   **Primary Themes:**     
	*   UX Design     
	*   UI Design     
	*   Behavioral Design & Science     
	*   User Research     
	*   Product Management & Development 
*   **Emerging Tech and Methods:**     
	*   AI in Design (e.g., using AI for design tasks, designing AI-powered products)     
	*   LLMs in Product Development     
	*   Vibe Coding (if relevant to raw_content) 
*   **Current Trend Priorities:**     
	*   Practical applications of AI in product lifecycle     
	*   Advanced Behavioral Design strategies     
	*   Data-driven Product Management     
	*   Effective Team Management & Leadership in tech  

### 3. Target Audience Master List: 
*   Product Designers 
*   Graphic Desogners
*   Product Marketing Managers
*   Chief Product Officers (CPO) 
*   Product Managers 
*   Chief Design Officers (CDO) 
*   Design Directors 
*   Software Developers (with an interest in product/design)  

### 4. Knowledge Level Target: Intermediate to Advanced (assume familiarity with core concepts; focus on depth, novel applications, or strategic insights).  

## Output Requirements (For Each Idea):  
1.  **Title:**     
	*   A compelling, specific, and SEO-friendly title.     
	*   Clearly indicates the article's core topic and benefit.     
	*   Max 70 characters preferred. 
2.  **Description:**     
	*   A concise 2-3 sentence summary.     
	*   Explain the article's main argument or purpose, what problem it addresses, or what new insight it offers.     
	*   Highlight its value to the reader. 
3.  **Key Points to Cover (2-3 points):**     
	*   List primary arguments, actionable takeaways, or critical questions the article will explore.     
	*   These should be substantial and form the backbone of the content. 
4.  **Primary Target Segment (1-2 segments):**     
	*   Identify the most relevant audience segment(s) from the `Target Audience Master List` for whom this specific article would be most valuable. 
5.  **Engagement Potential:**     
	*   Briefly explain *why* this article would capture and hold the interest of the specified target segment(s).     
	*   (e.g., addresses a pressing challenge, offers a controversial viewpoint, provides a practical framework, demystifies a complex topic). 
6.  **Trend Relevance and Angle:**     
	*   Specify how the article connects to the `Current Trend Priorities` or `Emerging Tech and Methods`.     
	*   Articulate the unique angle or perspective the article will take on this trend, especially if derived from the `raw_content`.  

## Response Format:  
Provide your response as a JSON object. The JSON object should have a single key `ideas`, which is an array. Each element in the `ideas` array should be an object with the following fields:  
*   `title` (string) 
*   `description` (string) 
*   `key_points_to_cover` (array of strings) 
*   `primary_target_segment` (array of strings) 
*   `engagement_potential` (string) *   `trend_relevance_and_angle` (string)  

* **Example JSON Structure:** 
```json { "ideas": [ { "title": "Example Title: AI-Driven Personalization in UX", "description": "This article explores how AI can revolutionize UX by enabling hyper-personalized user experiences. It delves into practical methods and ethical considerations for product teams.", "key_points_to_cover": [ "Leveraging user data ethically for AI personalization.", "Tools and techniques for implementing AI-driven UX features.", "Case studies of successful AI personalization in products." ], "primary_target_segment": ["Product Designers", "Product Managers"], "engagement_potential": "Offers actionable insights into a cutting-edge topic that directly impacts user engagement and product success.", "trend_relevance_and_angle": "Connects to 'Practical applications of AI in product lifecycle' by focusing on the UX implementation aspect, moving beyond theoretical discussions to practical application." } // ... more ideas ] }

```