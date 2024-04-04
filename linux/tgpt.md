---
id: tgpt
aliases: []
tags:
  - Chatbot
  - AI
  - Tools and Utils
  - Terminal
date: 2024-02-02T22:22
title: tgpt
---
<!-- 2024-02-02-2222 (February 02, 2024 10:22 PM) -->

# TGPT
### [tgpt](https://github.com/aandrew-me/tgpt)
- A terminal-based AI chatbot without the need for API keys
- Can be used with available providers: openai, opengpts, koboldai, phind, llama2, blackboxai
- based af
- More information: https://github.com/aandrew-me/tgpt

# Daily Usage
- Prompt with default provider (GPT-3.5-turbo):
`tgpt "prompt_string"`

- Prompt a specific provider and without animations:
`tgpt --provider phind --quiet --whole "prompt_string"`

- Prompt with an API key, model, max-length, temperature, and  (required when using openai provider):
`tgpt --provider openai --key "sk-xxxx" --model "gpt-3.5-turbo" --max-length 10 --temperature 0.7 --top_p 0.9 "prompt_string"`

- Generate code using the default provider:
`tgpt --code "prompt_string"`

- Generate and execute shell commands using a specific provider (will be prompted for confirmation):
`tgpt --provider phind -s "prompt_string"`

- Start multi-line interactive mode:
`tgpt --multiline`

- Generate images and save to current directory:
`tgpt --image "prompt_string"`

- Feed a file as additional input pre-prompt:
`cat example.go | tgpt "prompt_string"`

# References
- [https://github.com/aandrew-me/tgpt](https://github.com/aandrew-me/tgpt)
