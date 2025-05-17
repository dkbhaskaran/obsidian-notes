 I use terminal/terminal adjacent extensively in my daily routine, whether it be do programming or to listen to music or to take notes like what I am doing now. It is a pretty standard setup with zsh+tmux+neovim on Ubuntu 24.04.2 LTS on a Dell G15 5315 with Nvidia GeForce RTX 4050 card. The config files can be found in my github repo [dotfiles](https://github.com/dkbhaskaran/dotfiles). Today we are going to add AI assistance to similar terminal workflow

The first thing that comes to mind is "is there a chatgpt cli tool?". Well yes there are like
1. OpenAI CLI :- The official CLI to interact with open AI models. 
2. ChatGPT CLI :- These are community tools like chatgpt-cli that accomplishes this. 
 
However it is not free and it is not lighting fast as it depends on the underlying network and server availability. One needs to pay as they use. The pricing is usually based on tokens (almost equivalent to a word) sent/received during a query session. 

This costing model is not restricted to OpenAI, most of the AI service providers have similar costing model. Here is a table that shows some of the major vendors

| Provider                                     | Official CLI Tool                           | CLI Purpose / Features                        | Model Access Type          | Pricing Model                      | Notes                                               |
| -------------------------------------------- | ------------------------------------------- | --------------------------------------------- | -------------------------- | ---------------------------------- | --------------------------------------------------- |
| **OpenAI**                                   | Yes — OpenAI CLI & ChatGPT CLI wrappers     | Chat, code, completions, embeddings           | Hosted API                 | Pay per 1,000 tokens               | Most widely used; CLI tools free, pay for API usage |
| **GitHub Copilot**                           | Yes — Copilot CLI (beta) and plugins        | AI coding assistant integrated into editors   | Hosted API (OpenAI-based)  | Subscription ($10/mo or $100/yr)   | Focused on coding, supports Neovim/Vim, VSCode etc. |
| **Anthropic (Claude)**                       | No official CLI yet (community tools exist) | Chat & completions via API                    | Hosted API                 | Pay per usage (tokens)             | Competes with ChatGPT, offers safe completions      |
| **Google**                                   | Yes — `gcloud ai` CLI                       | Call Vertex AI models for text & chat         | Hosted API                 | Pay per usage (tokens/characters)  | Enterprise ready, requires setup                    |
| **Amazon AWS**                               | Yes — AWS CLI (for AI services)             | Invoke Bedrock, Comprehend, and other AI      | Hosted API                 | Pay per API call (varies)          | No dedicated chatbot CLI, scripting via CLI         |
| **Cohere**                                   | Yes — Cohere CLI                            | Text generation, embeddings, classification   | Hosted API                 | Pay per usage                      | Popular in enterprise & dev circles                 |
| **AI21 Labs**                                | Yes — AI21 Studio CLI                       | Text generation and language models           | Hosted API                 | Pay per usage                      | Known for Jurassic-2 family models                  |
| **Meta (LLaMA)**                             | No official CLI; community tools exist      | Open-source LLaMA models (local or cloud-run) | Local or cloud self-hosted | Free software, hardware/cloud cost | No hosted API or official CLI yet                   |
| **Open Source LLMs** (GPT-J, GPT-NeoX, etc.) | Various community CLI tools                 | Run locally or in cloud                       | Local or cloud self-hosted | Free software, hardware cost       | No official hosted service, requires setup          |

For an experimental setup, I am not ready to pay anything now, thus we are left with options of locally hosted models like LLaMa and build around it. However I would like to keep the option alive of using a paid option if they make sense in future without much changes. So this may put a small dent in the whole electrification process :)

There are two prominent options for Open Source local LLM API [Ollama](https://github.com/ollama) and [LocalAI](https://github.com/mudler/LocalAI). In [here](https://hyscaler.com/insights/ollama-vs-localai-open-source-local-llm-apis/#local-ai-the-open-source-open-ai-alternative) there is an detailed comparison of these two and based on the fact Ollama is currently most adopted and that it is easy to setup compared to LocalAI, we are going to use Ollama. Given that it needs GPU support, works in our favour and well its non-support for OpenAI API supprot is not currently a deterrent to our current goal as we are going for free and open source solutions now. If you do not have a GPU support its better to use LocalAI.
## Ollama setup

I am going to use a docker based setup with the following setup

```
docker pull ollama/ollama
docker run -d \
  --name ollama \
  -p 11434:11434 \
  -v ollama-data:${HOME}/.ollama \
  --gpus all \
  ollama/ollama
```

Download a model for Ollama, we use mistral, it is GPU friendly and it provides a good balance between speed and accuracy. One can also use CodeLLaMA, WizardCoder for coding heavy use cases or **LLaMA 2 13B** is best for reasoning followed by mistral.

```
docker exec -it ollama ollama pull mistral
```

Lets check out the if it is working 

```
curl http://localhost:11434/api/chat -d '{
  "model": "mistral",
  "messages": [{"role": "user", "content": "Write a shell script that greets the user"}]
}'
```

This is how it looks like on my setup
![[pic-1.png]]

This is the json partial output from the model. Note the HELLO WORLD! message on the right of the text.

## Shell

The following code in python uses request module to emulate a chat bot for the terminal. A more enhanced version can be found in my [dotfiles](https://github.com/dkbhaskaran/dotfiles)

```python
import requests
import json

def stream_ollama_response(prompt, model="mistral"):
    print(f"Assistant: ", end="", flush=True)

    try:
        with requests.post(
            "http://localhost:11434/api/generate",
            json={"model": model, "prompt": prompt,
                  "stream": True},
            stream=True,
        ) as response:
            for line in response.iter_lines(decode_unicode=True):
                if not line:
                    continue

                try:
                    data = json.loads(line)

                    content = ""
                    if "message" in data:
                        content = data["message"].get("content",
                                                      "")
                    elif "response" in data:
                        content = data.get("response", "")

                    if content:
                        print(f"{content}", end="", flush=True)

                    if data.get("done", False):
                        break

                except json.JSONDecodeError:
                    print(f"\n[Error decoding JSON line]")

    except requests.RequestException as e:
        print(f"\n[Request error] {e}")

    print()
    print(f"{'-'*65}")

def chat_loop():
    print(f"💬 Chat with Ollama ")
    while True:
        prompt = input(f"You: ")
        if prompt.strip().lower() in {"exit", "quit"}:
            break
        stream_ollama_response(prompt)

if __name__ == "__main__":
    chat_loop()

```


It puts out a response like 

```
💬 Chat with Ollama 
You: hello
Assistant: Hello! How can I help you today? Let's make this conversation helpful and enjoyable. Is there a specific question or topic you'd like to explore or discuss? I'm here to assist with a variety of subjects, from technology and science to entertainment and lifestyle.
-----------------------------------------------------------------
You: 
```

## tmux integration

The first thing that comes to mind is the AI assimilation in shell. Well one can ask what's the use of it -well these can translate requests described in English (or some other language ?)  to shell commands. For example if you need a command but forgot the package it is part of (often my case), you can ask the AI to install the command or write a script to monitor cpu usage. One of the ways to incorporate AI in shell is through [tmuxai](https://github.com/alvinunreal/tmuxai). 

Lets start with tmuxai the configuration needed are follows
1. Install tmuxai through below command. Once 
```
# install tmux if not already installed
curl -fsSL https://get.tmuxai.dev | bash
```
2. Create an account with https://openrouter.ai and then create a API token key at https://openrouter.ai/settings/keys for tmuxai. 