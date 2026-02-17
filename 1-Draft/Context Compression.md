# Soft Compression
**uses the model** to compress the original input token sequence into a shorter summary token sequence; *soft tokens which act as compression representation but do not correspond to words with actual meaning*. 
They are inserted into the original token sequence to form a new input. During the forward pass of the model, the information from the original token sequence is gathered into the summary token sequence, which represents the original input for subsequent operations. 
Since summary tokens do not appear during the model’s pre-training, additional training is necessary for the model to learn how to generate and utilize these tokens
## [Gisting](https://huggingface.co/papers/2304.08467)
trains an LM to compress prompts into smaller sets of "gist" tokens which can be cached and reused for compute efficiency.
Gist models can be trained with no additional cost over standard instruction finetuning by simply modifying Transformer attention masks to encourage prompt compression.
On decoder (LLaMA-7B) and encoder-decoder (FLAN-T5-XXL) LMs, **gisting enables up to 26x compression** of prompts with minimal loss in output quality
# Hard Compression
*directly shorten plain text sequence length.* 
This process can be achieved through **selection and summarization** and **doesn't introduce additional tokens and targeted training**, which makes **it can be applied to black box models**
## Translating to mandarin
“Bing chilling”是一个网络流行语梗，起源于2021年演员**约翰·塞纳（John Cena）** 用中文说“冰淇淋”的视频。
## [LLMLingua](https://github.com/microsoft/LLMLingua)
**utilizes a small language model** to evaluate the perplexity of each prompt token, removing those with lower perplexities; *premised on the idea that tokens with lower perplexities have a negligible effect on the language model’s overall entropy gain (contextual understanding)*

a coarse-to-fine prompt compression method that involves:
- **budget controller** to maintain semantic integrity under high compression ratios; *assigns varying compression ratios to different parts of the prompt (i.e. instruction, demonstrations, question)*
- **token-level iterative compression** algorithm to better model the interdependence between compressed contents
- **instruction tuning** based method for distribution alignment between language models
### [LongLLMLingua](https://arxiv.org/abs/2310.06839)


