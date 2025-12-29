## INSTRUCTION
Convert PDF page to Markdown with 100% accuracy for RAG retrieval.

- Only output the content transcribed from the image.
- Do NOT output this instruction or any other explanation.
- If the content is missing or you do not understand the input, return an empty string.

## OUTPUT FORMAT

START WITH PAGE METADATA (before any content):
```
---
**Page Summary:** [What this page contains, document/brand context, connection to previous page if mentioned]

**Retrieve this page when asking:**
- [Specific question 1 with full context - brand, location, specifics]
- [Specific question 2]
- [Specific question 3]
- [Add more specific questions as needed]

**Keywords:** [all relevant terms, synonyms, names, values, prices, dates]
---
```

Then transcribe the page content.

## RULES
1. Do NOT generate examples, demonstrations, or templates.
2. Do NOT output any extra text such as 'Example', 'Example Output', or similar.
3. Do NOT generate any tables, headings, or content that is not explicitly present in the image.
4. Transcribe content word-for-word. Do NOT modify, translate, or omit any content.
5. Do NOT explain Markdown or mention that you are using Markdown.
6. Do NOT wrap the output in ```markdown or ``` blocks.
7. Only apply Markdown structure to headings, paragraphs, lists, and tables, strictly based on the layout of the image. Do NOT create tables unless an actual table exists in the image.
8. Preserve the original language, information, and order exactly as shown in the image.

## TABLE EXTRACTION
Convert tables to ```json blocks with just the data:
```json
{
  "data": { /* exact table structure - all rows, columns, values */ }
}
```

CRITICAL - VALUE FORMATTING:
- Strikethrough text = OLD/DEPRECATED value. Structure as: {"current": "...", "old": "..."}
- Hatched/diagonal line pattern = NOT AVAILABLE. Use: "not_available"
- If a cell has BOTH current AND strikethrough old value, capture BOTH separately
- Normal text without strikethrough = current value

Be careful with: merged cells (apply value to all spanned columns/rows), long tables, complex headers.

## RETRIEVAL QUESTIONS
The "Retrieve this page when asking" questions should be:
- SPECIFIC: Include brand names, locations, exact items (e.g., "What is the weekly tuition fee for Full-Time Morning English at ILSC Sydney?")
- NOT GENERIC: Never use vague questions like "What is the price?" or "What are the fees?"
- Include all relevant context from the page content

{% if page %}
At the end of the transcription, add the page divider: `--- Page {{ page }} ---`.
{% endif %}

> If you do not detect valid content in the image, return an empty string.

