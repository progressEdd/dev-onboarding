# AI Coding aka Vibe coding
## Vibe coding background
Vibe coding is the process of using Large Language models to generate code autonomously. There have been efforts to formalize vibe coding in the form of AI Native Development, Harness/Agentic Engineering, Spec Coding. 

There are 2 concepts to understand: 
1. AI model provider: the AI model endpoint, a public api that serves models such as GLM, GPT, or Claude
2. Coding Agent/Harness: the interface that allows your AI model to interact with your computer

## Edd's AI setup
As of the writing of this guide, I use the Premium GLM Cloud Coding Plan as my model provider and the [Pi Coding Agent](pi.dev) for my Coding Agent/Harness. I prefer pi as it is minimal, very customizable, and self documenting. To install it, follow the platform specific instructions on the website. For coding plans, I don't recommend it anymore, as the plan has changed and is no longer a good deal. When picking out plans, I usually read the top reddit comments on the opencode and vibecoding subreddit currently the best deal is [opencode go + minimax](https://safereddit.com/r/opencodeCLI/comments/1sresng/ollama_cloud_vs_opencode_go/). When picking models, I follow sentdex's wisdom that terminal bench is a good benchmark to filter model performance as, ["it's a long horizon kind of agentic coding/terminal... that's a pretty good for how I do software development"](https://youtu.be/AgpeggCsRH4?t=292).

For Mac/Linux:
```
curl -fsSL https://pi.dev/install.sh | sh
```

For Windows:
```
powershell -c "irm https://pi.dev/install.ps1 | iex"
```

If you want to use a package manager, you can also use npm/pnpm/bun

## Getting setup with pi
To get setup, you can follow the instructions in the [pi documentation](https://pi.dev/docs/latest).
`
You can add a provider using the command `/login`. For custom api keys, you can edit the `~/.pi/agent/auth.json` file for a custom provider
```
{
  "anthropic": { "type": "api_key", "key": "sk-ant-..." },
  "ant-ling": { "type": "api_key", "key": "..." },
  "openai": { "type": "api_key", "key": "sk-..." },
  "deepseek": { "type": "api_key", "key": "sk-..." },
  "nvidia": { "type": "api_key", "key": "nvapi-..." },
  "google": { "type": "api_key", "key": "..." },
  "opencode": { "type": "api_key", "key": "..." },
  "opencode-go": { "type": "api_key", "key": "..." },
  "together": { "type": "api_key", "key": "..." },
  "qwen-token-plan":  { "type": "api_key", "key": "sk-sp-..." },
  "qwen-token-plan-individual": { "type": "api_key", "key": "sk-sp-..." },
  "qwen-token-plan-cn": { "type": "api_key", "key": "sk-sp-..." },
  "xiaomi": { "type": "api_key", "key": "..." },
  "xiaomi-token-plan-cn":  { "type": "api_key", "key": "..." },
  "xiaomi-token-plan-ams": { "type": "api_key", "key": "..." },
  "xiaomi-token-plan-sgp": { "type": "api_key", "key": "..." }
}
```