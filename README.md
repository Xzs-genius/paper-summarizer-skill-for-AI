# paper-summarizer-skill-for-AI
An skill that automatically summarizes academic PDF papers into structured  reading notes with 11 key fields.
What it does
Feed it any PDF paper, and it produces a complete structured report covering:

Field	Description
Title	Bilingual (Chinese + English)
Abbreviation	Method/system acronym
Journal	Source, publisher, DOI
Year	Publication year
SCI Zone	CAS (Chinese Academy of Sciences) updated tier
Methods	Proposed method (★) + baselines
Background	Research context
Problem	Specific gap addressed
Comparisons	Baseline methods table
Results	Quantitative metrics + improvement %
Conclusions	Key findings + future work

Installation
# Clone into your opencode skills directory
cd ~/.opencode/skills
git clone https://github.com/<your-username>/paper-summarizer.git

Usage
Simply provide a PDF paper and ask:
帮我总结这篇论文
or in English:
Summarize this paper

Dependencies
pdf skill (for PDF text extraction)
Web search (for SCI zone lookup via LetPub/CAS JCR)
