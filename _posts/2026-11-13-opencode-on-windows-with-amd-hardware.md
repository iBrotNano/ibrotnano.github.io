---
title: OpenCode on Windows with AMD Hardware
date: 2026-11-13 11:19:00 +0200
categories: [software, amd, windows, ai, agentic coding, coding, development]
---

Claude Code, GitHub Copilot, and OpenAI Codex are AI-powered coding agents that help developers write code faster, reduce errors, and solve complex tasks more efficiently. These tools provide the tools and access to the user's own system needed to carry out programming tasks—the so-called harness. The LLM is the central element that provides the intelligence. OpenCode is an open-source approach to this kind of AI-assisted programming. In addition to allowing integration with the providers mentioned above, it lets developers use similar functionality on their own hardware.

I want to use OpenCode on my AMD-powered laptop running Windows and explore the possibilities of this platform. This is a somewhat unusual setup, since although AMD offers promising AI acceleration capabilities, they are not always optimally supported on Windows. I want to see how far I can use a system like this and where its limitations lie.

First, OpenCode needs to be installed. There are several ways to do this. It runs on all platforms. On Windows, for example, you can simply download and run an executable file; a traditional installation is not required. You can do this with `curl`:

```powershell
mkdir "C:\Tools\OpenCode" -Force
curl -L -o "C:\Tools\OpenCode\opencode.exe.zip" https://opencode.ai/update/api/latest/cli/opencode/opencode-windows-x64.zip
Expand-Archive -Path "C:\Tools\OpenCode\opencode.exe.zip" -DestinationPath "C:\Tools\OpenCode" -Force
Remove-Item "C:\Tools\OpenCode\opencode.exe.zip" -Force
```

OpenCode can update itself:

```powershell
& "C:\Tools\OpenCode\opencode.exe" update --method curl
```

AMD has it's own server to host LLMs, the Lemonade Server. Because they are optimizing the Modells for their hardware acceleration, it can provide better performance on AMD GPUs compared to generic LLM servers. This makes it an interesting option for leveraging AMD hardware effectively with OpenCode. They also provide an OpenAI compatible API, which allows OpenCode to interact with the Lemonade Server in the same way it would with any other OpenAI service.

But the first step is to run a coding model on the Lemonade Server OpenCode can connect to. The model needs to be a coding model which can call tools. Not every model can do this and some models that can do this need a lot of RAM. More than my machine has available. Fortunately, there are also some very interesting models that have a relatively small size, running on AMD hardware and are [optimized for agentic coding](https://huggingface.co/ornith-ai/Ornith-1.5-9B-GGUF#coding-clis) workflows. One of this models is [ornith-ai/Ornith-1.5-9B-GGUF · Hugging Face](https://huggingface.co/ornith-ai/Ornith-1.5-9B-GGUF) and you can download it directly via Lemonade Server.

To use it with OpenCode, you need to configure the Lemonade Server as a provider in your OpenCode configuration. Save the following configuration under `C:\Users\<Username>\.config\opencode\opencode.json`.

```json
{
    "$schema": "https://opencode.ai/config.json",
    "provider": {
        "ornith": {
            "npm": "@ai-sdk/openai-compatible",
            "name": "Ornith (local)",
            "options": {
                "baseURL": "http://127.0.0.1:13305/api/v1",
                "apiKey": "EMPTY"
            },
            "models": {
                "Ornith-1.5-9B-GGUF": {
                    "name": "Ornith-1.5-9B-GGUF"
                }
            }
        }
    }
}
```

Check that the `baseURL` and `apiKey` in your configuration match the settings of your Lemonade Server and that the model is registered with the name `Ornith-1.5-9B-GGUF`. You can check this in the command line with `lemonade list`. Now we are good to go. When you start OpenCode, it will be able to use the configured model from the Lemonade Server. User `/models` to select this model and type something into the prompt. You should see some response from the model.

Depending on your machine's hardware and available resources, the performance of the model may vary. I had to configure it down to a bare minimum. I set the context length to 16384 and preserved thinking to false (`'{"preserve_thinking":false}'`). It works but not fast.

My dream of freedom and independence died before it began, crushed by the harsh reality of limited hardware resources and the slow performance of even the most optimized models. Damn. For agentic coding workflows is the used model just to small. It works but it thinks, tries something out, fails, thinks again, and repeats this cycle endlessly, making progress painfully slow. Larger models would perform better, but you need much better hardware than I have.

But is it totally useless? Not exactly. There are some tasks that works. OpenCode could describe what a unknown codebase does. Impressive for such a small and constrained setup. OpenCode analyzed a test project quite fast and generated a nice little document summarizing its structure and functionality. You can see the result at [overview.md](https://github.com/iBrotNano/python_workshop_project/tree/main/docs/articles/overview.md). So the setup seems to be usable at least to write documentation and perform code analysis on smaller projects.