---
name: paper-summarizer
description: >-
  Summarize an academic PDF paper into a structured Chinese report. Extracts
  title (Chinese+English), abbreviation, journal, year, SCI zone (CAS updated),
  methods, background, problem, comparisons, results, and conclusions. Use this
  skill whenever the user provides a PDF paper and asks for a summary, review,
  analysis, or structured reading note. Triggers on: 论文总结, 论文摘要, 论文分析,
  总结论文, 帮我看这篇论文, paper summary, paper review, paper analysis,
  读论文, 阅读笔记, 文献总结, 论文笔记, summarize this paper, what is this
  paper about, 给我总结一下, 帮我分析这篇论文, 论文里用了什么方法,
  这篇论文的结论是什么, 论文的实验结果, 论文的研究问题.
---

# Paper Summarizer

Summarize an academic PDF paper into a structured Chinese report with 11 fields.

## Workflow

### 1. Load the PDF

The user provides a PDF file. Use the `pdf` skill to extract the full text.

```bash
# If pdf-parse is available (Node.js)
node -e "const fs=require('fs');const pdf=require('pdf-parse');const buf=fs.readFileSync('<path>');pdf(buf).then(d=>console.log(d.text))"
```

If the PDF cannot be parsed directly, inform the user and ask them to provide
the text content or the DOI so you can fetch the paper from the web.

### 2. Read the reference template

Read `references/template.md` for the output format and field definitions.

### 3. Extract and fill each field

Go through the extracted text and fill in all 11 fields from the template.
For fields that require external lookup (e.g., SCI zone), use web search.

### 4. Output the report

Output the completed report in Chinese, following the template format exactly.
Use Markdown formatting for readability.

## Field Extraction Guide

### Title (中英文标题)
- Look for the main title on the first page
- If the paper is in English, provide both the English title and a Chinese translation
- If the paper is in Chinese, provide both the Chinese title and an English translation

### Abbreviation (论文简称)
- If the paper introduces a method/system name (e.g., "We propose TARRAQ"), use that as the abbreviation
- If no explicit abbreviation, create one from the first letters of key method words
- Format: **[ABBREVIATION]** Full Name

### Journal (来源期刊)
- Look for journal name, volume, issue, page numbers on the first/last page
- Include the publisher (e.g., IEEE, Elsevier, Springer)

### Year (出版年份)
- Publication year from the paper metadata

### SCI Zone (中科院升级版分区)
- Use web search to look up the journal on LetPub (https://www.letpub.com.cn/) or
  CAS JCR to determine the zone (1区/2区/3区/4区)
- Specify "中科院升级版"分区

### Methods (论文中用到的方法)
- List all methods/algorithms/techniques used in the paper
- Include the specific variant names (e.g., "Modified AntHocNet based on ACO")
- Group by category if many methods are used (e.g., "baseline methods", "proposed method"

### Background (研究背景)
- 2-3 sentences on the broader context (e.g., "FANETs are emerging...")
- Why this research area matters

### Problem (研究的问题)
- The specific gap or challenge this paper addresses
- What existing methods fail to do

### Comparisons (比较对象)
- List the baseline/competing methods the paper compares against
- Include both traditional and state-of-the-art methods

### Results (实验结果)
- Key quantitative results (e.g., "proposed method improves throughput by 15%")
- Metrics used (e.g., PDR, delay, energy, throughput)
- Simulation tools (e.g., NS2, NS3, OMNeT++, MATLAB)

### Conclusions (论文的结论)
- The main takeaways from the paper
- Future work mentioned by the authors

## Output Format

Follow the exact template in `references/template.md`. The output should be
a single Markdown document that the user can directly use as a reading note.

## Edge Cases

- **No PDF provided**: Ask the user to provide the PDF file path or upload it
- **PDF unreadable**: Suggest the user provide the DOI or paper title for web lookup
- **Non-English paper**: Still extract both language versions of the title
- **Conference paper**: Use conference name instead of journal; note "会议论文"
- **Preprint**: Note "预印本" and mark SCI zone as "N/A"
- **Multiple methods**: List all methods; highlight the proposed method with ★
