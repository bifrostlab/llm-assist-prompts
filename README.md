# llm-assist-prompts
Prompts library for LLM Assist bot


Hard prompts are text prompts handcrafted by user. It may instruct a LLM how to carry out a downstream task. Templating hard prompt or Prompt Engineering can significantly improve the quality of LLM response. This repo archives the prompt templates that are useful in some use cases.

Different models have different ways of using special tokens as "instruction tag" (e.g. INST, Instruction, \<INST\>, \<\<system\>\>, Output ...), so we have to add templates for each model we intend to use.


## Example usage
Currently, the prompts are stored in `prompt_templates.json` and organized by models (`'gpt-3.5-turbo'`, `'gpt-4'`, `'phi-2'`). You can load the JSON and get the use case of your choice.

```python
import json
from openai import OpenAI

client = OpenAI()

with open("prompt_templates.json", "r", encoding="utf-8") as handler:
  prompt_templates = json.load(handler)

use_case = "general"
model_name = "gpt-3.5-turbo"
seed = 1024
max_tokens = 200
# response_format={ "type": "json_object" }
temperature = 0.1

user_prompt = "What is the capital city of Wakanda?"
full_prompt = prompt_templates[model_name][use_case].replace("<prompt>", user_prompt)

completion = client.chat.completions.create(
  model=model_name,
  messages=[
    {"role": "user", "content": full_prompt}
  ],
  seed=seed,
  max_tokens=max_tokens,
  temperature=temperature,
)
print(completion.choices[0].message)
```
Note: as of Mar 2024, avoid using JSON mode (`response_format={ "type": "json_object" }`) in OpenAI API as it does not output consistent JSON (i.e. not producing the same attributes)

## Resources
- [Prompt engineering strategies by OpenAI](https://platform.openai.com/docs/guides/prompt-engineering)
- [[Video] Prompt-Engineering for Open-Source LLMs](https://www.youtube.com/watch?v=f32dc5M2Mn0)