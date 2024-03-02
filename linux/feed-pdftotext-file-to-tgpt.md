---
title: Feed pdftotext file to TGPT
date: 2024-03-01-2100 (March 1, 2024 9:00 PM)
tags: 
- Productivity
---

# Using pdftotext and tgpt together
- use `tldr` for quick reference
- no need for paid SaaS services for this feature

### Basic Usage
- Feed pdftoteext output file to `tgpt`
```bash
pdf2text -layout input.pdf output.txt && tgpt "<prompt>" < output.txt

# optionally remove the output file after (script time?)
pdf2text -layout input.pdf output.txt && tgpt "<prompt>" < output.txt && rm output.txt
```

- Use `pdftotext` to convert a PDF file to a text file
```bash
pdf2text -layout input.pdf output.txt
```

- Display to stdout with `less` for better readability
```bash
pdf2text -layout input.pdf - | less
```


