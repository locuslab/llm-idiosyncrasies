# Idiosyncrasies in Large Language Models
Official code of Idiosyncrasies in Large Language Models

> [**Idiosyncrasies in Large Language Models**](https://arxiv.org/abs/2502.12150) </br>
> *[Mingjie Sun](https://eric-mingjie.github.io)\*, [Yida Yin](https://davidyyd.github.io)\*, [Zhiqiu Xu](https://oscarxzq.github.io), [J. Zico Kolter](https://zicokolter.com), [Zhuang Liu](https://liuzhuang13.github.io)* (* indicates equal contribution) <br>
> Carnegie Mellon University, UC Berkeley, University of Pennsylvania, and Princeton University<br>
>[[Paper]](https://arxiv.org/abs/2502.12150) [[Project page]](https://eric-mingjie.github.io/llm-idiosyncrasies/index.html)

```bibtex
@article{sun2025idiosyncrasies,
    title    = {Idiosyncrasies in Large Language Models}, 
    author   = {Sun, Mingjie and Yin, Yida and Xu, Zhiqiu and Kolter, J. Zico and Liu, Zhuang},
    year     = {2025},
    journal  = {arXiv preprint arXiv:2502.12150}
}
```

--- 
<p align="center">
<img src="https://github.com/user-attachments/assets/de7a87f0-8a4e-43d4-bfd1-4778d0393274" width=100% height=100% 
class="center">

</p>
We study idiosyncrasies in Large Language Models (LLMs) -- unique patterns in their outputs. We consider a simple classification task: given a particular text output, a neural network is trained to predict the source LLM that generates that text.


## Setup
Installation instructions can be found in [INSTALL.md](INSTALL.md).



## Pre-generated Responses
We host a collection of pre-generated responses for Chat APIs, Instruct LLMs, and Base LLMs.


| | ChatGPT | Claude | Grok | Gemini | DeepSeek | Phi-4 |
| :-- | :--: | :--: | :--: | :--: | :--: | :--: |
| links | [download](https://drive.google.com/file/d/1O1dEROw21KePNMF9ewlkXkkzL8Z-5qrN/view?usp=sharing) | [download](https://drive.google.com/file/d/1sifL_hsFiSDKZgnEeahiT20wPW8NDmRG/view?usp=sharing) | [download](https://drive.google.com/file/d/1yUA-8RYYXIkSV2xMbUCqTU8o6F6LrEFg/view?usp=share_link) | [download](https://drive.google.com/file/d/1dsvpXmLCNa4Gehd9jmantMNSDiw2eS4f/view?usp=share_link) | [download](https://drive.google.com/file/d/1a31HZgMwppwXjzEiY1fj3VfhAco5RWhG/view?usp=share_link) | [download](https://drive.google.com/file/d/1C6xDdvOuczJq1j4OSXJgqxB75kvwSoVK/view?usp=share_link) |

| | Llama3.1-8b-it | Gemma2-9b-it | Qwen2.5-7b-it | Mistral-7b-v3-it |
| :-- | :--: | :--: | :--: | :--: |
| links |[download](https://drive.google.com/file/d/1JuT1UpCw6ijDIgYSa2JM1AmDcSTxrTLu/view?usp=sharing) | [download](https://drive.google.com/file/d/1gw_z-XsUHSip71qkHdoM4SnpflwcM_g_/view?usp=sharing) | [download](https://drive.google.com/file/d/1EnVOL4WhxU3-hFvPOEZ21moOeEyX5eSb/view?usp=sharing) | [download](https://drive.google.com/file/d/1uIRtNvapwfmOWBhlknOP8rRvdExn5wNW/view?usp=sharing) |

| | Llama3.1-8b | Gemma2-9b | Qwen2.5-7b | Mistral-7b-v3 |
| :-- | :--: | :--: | :--: | :--: |
| links |[download](https://drive.google.com/file/d/1b37J7btQ1jFhs0bwfUPpXRzxp5Yxm_eS/view?usp=sharing) | [download](https://drive.google.com/file/d/1o3TTBxOBaytFKyGf6D7T5b8iCH-0kwLu/view?usp=share_link) | [download](https://drive.google.com/file/d/1py9tJBpZaZPh0ryvMBS08SlB-LWjWdOh/view?usp=share_link) | [download](https://drive.google.com/file/d/1S1nAojlpMrl9LKkYYA6EBDS2cfLzVk1W/view?usp=share_link) |


## Response Generation

### Chat APIs
We call official APIs to generate responses for Chat APIs.

Below is an example command to generate 11K responses for ``ChatGPT`` on ``UltraChat`` dataset.

- Change the ``--model`` argument to generate responses for different Chat API models, including ``ChatGPT``, ``Claude``, ``Grok``, ``Gemini``, and ``DeepSeek``.
```bash
python generate_responses.py \
    --model ChatGPT --api_key $api_key \
    --dataset UltraChat --num_samples 11_000 \
    --output_path /path/to/output.json
```

### Instruct and Base LLMs
We use [vLLM](https://github.com/vllm-project/vllm) to generate responses for instruct / base LLMs in our paper.

Below is an example command to generate 11K responses for ``Llama3.1-8b-it`` on ``UltraChat`` dataset with greedy decoding.

- ``--model`` argument controls the LLM used to generate responses. Our code currently supports generating responses for nine LLMs in our paper, including ``Llama3.1-8b-it``, ``Gemma2-9b-it``, ``Qwen2.5-7b-it``, ``Mistral-7b-v3-it``, ``Phi-4``, ``Llama3.1-8b``, ``Gemma2-9b``, ``Qwen2.5-7b``, and ``Mistral-7b-v3``. We recommend using temperature ``0.6`` and repetition penalty ``1.1`` for base LLMs.
- ``--dataset`` argument specifies the prompt dataset to generate responses on, including ``UltraChat``, ``Cosmopedia``, ``LmsysChat``, ``WildChat``, and ``FineWeb``.
- It is also possible to use multiple GPUs to generate responses. Simply change the ``--num_gpus`` argument. This is implemented through tensor parallelism by vLLM.

```bash
python generate_responses.py \
    --model Llama3.1-8b-it --temperature 0 \
    --dataset UltraChat --num_samples 11_000 \
    --output_path /path/to/output.json
```

## Transformations
Below we provide scripts to perform various transformations on the generated responses. The supported transformations are ``remove_special_characters``, ``shuffle_word``, ``shuffle_letter``, ``markdown_elements_only``, ``paraphrase``, ``translate``, and ``summarize``.

Here is the example command to shuffle words from the generated responses.

```bash
python transform.py \
    --input_path /path/to/input.json \
    --output_path /path/to/output.json \
    --transform_mode shuffle_word
```

To rewrite (e.g., paraphrase, translate, summarize) the generated responses, you also need to provide the API key for the rewriting model (e.g., GPT-4o-mini) through the ``--api_key`` argument.

```bash
python transform.py \
    --input_path /path/to/input.json \
    --output_path /path/to/output.json \
    --transform_mode paraphrase \
    --api_key $api_key
```

## Classification
Below is an example command to classify responses from two different models. For $N$-way classification, you can change the ``--response_paths`` argument to include $N$ response paths (with white space separated).

You can change the ``--classifier`` argument to use different classifiers. Our code currently supports the following classifiers: ``llm2vec``, ``gpt2``, ``t5``, and ``bert``. Each classifier can be run on a single GPU (supported bfloat16) with 24 GB memory.
```bash
python classification.py \
    --response_paths /path/to/model1.json /path/to/model2.json \
    --classifier llm2vec \
    --output_dir /path/to/output_dir
```


## License
This project is released under the MIT license. Please see the [LICENSE](LICENSE) file for more information.

## Questions
Feel free to discuss papers/code with us through issues/emails!

mingjies at cs.cmu.edu  
davidyinyida0609 at berkeley.edu
