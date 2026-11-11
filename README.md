# Gemini 4 Carbon Free Guide

Gemini 4 Carbon is free on Respan right now. This guide shows how to use it in OpenCode.

Model ID: `respan/gemini-4-carbon:free`

## Requirements

- Respan account at https://platform.respan.ai
- Windows PowerShell
- Node.js 18 or newer

## Step 1 Create a folder and open PowerShell

1. go to your desktop
2. create a new folder
3. name the folder opencode
4. open the opencode folder
5. click on the address bar at the top of the window
6. type powershell and press enter

A blue or black PowerShell window will open inside this folder. Use this same way every time you need to open PowerShell in this guide.

## Step 2 Create an API key

Open https://platform.respan.ai and go to API keys. Create a new key and copy it. Never share this key with anyone.

## Step 3 Save the key

```
setx RESPAN_API_KEY "YOUR_KEY_HERE"
```

Put your key in place of YOUR_KEY_HERE. Then close PowerShell and open it again from the opencode folder the same way as Step 1.

## Step 4 Test the key

```
$h = @{ "Authorization" = "Bearer $env:RESPAN_API_KEY"; "Content-Type" = "application/json" }
$b = '{"model":"respan/gemini-4-carbon:free","messages":[{"role":"user","content":"say hi"}]}'
(Invoke-RestMethod -Uri "https://gateway.respan.ai/v1/chat/completions" -Method Post -Headers $h -Body $b).choices[0].message.content
```

If you get a reply then your key works.

## Step 5 Install OpenCode

```
npm install -g opencode-ai
```

Skip this step if OpenCode is already installed.

## Step 6 Connect OpenCode to Respan

Paste this whole block at once. It adds your key from Step 3 by itself.

```
New-Item -ItemType Directory -Force "$HOME\.config\opencode" | Out-Null
$cfg = @'
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "respan": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Respan Gateway",
      "options": {
        "baseURL": "https://gateway.respan.ai/v1",
        "apiKey": "KEY_PLACEHOLDER"
      },
      "models": {
        "respan/gemini-4-carbon:free": { "name": "Gemini 4 Carbon (Free)" }
      }
    }
  }
}
'@
$cfg = $cfg.Replace("KEY_PLACEHOLDER", $env:RESPAN_API_KEY)
[System.IO.File]::WriteAllText("$HOME\.config\opencode\opencode.json", $cfg)
```

Test it

```
opencode run -m "respan/respan/gemini-4-carbon:free" "say hi"
```

## Step 7 Start using it

Open PowerShell from the opencode folder the same way as Step 1 and run

```
opencode
```

Type /models and select Gemini 4 Carbon (Free). Then type your task. Type /exit to close.

## Common errors

- EEXIST means OpenCode is already installed so go to Step 6
- API key is invalid means you need to open PowerShell again and then run Step 6 again
- hi is not recognized means you need to run opencode first
- 429 means the free limit is over so try again later

## Notes

- Check your cost in Respan Logs and it should be 0
- This is a launch offer and will not stay free forever
- It is a reasoning model so replies can take a few seconds
- It does not work in Claude Code

If this guide helped you then give this repo a star
