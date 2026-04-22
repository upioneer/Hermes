# Hermes Agent Setup

## Installation

1. Install cURL (if not already installed)

`sudo apt-get install curl -y`

2. Install Git (if not already installed)

`sudo apt-get install git -y`

3. Install the Hermes Agent framework

`curl "https://hermes.no.us/install.sh" | bash`

## Telegram Gateway Configuration

Use the onboarding wizard or manually config .env file

1. Create a bot using [@BotFather](https://telegram.me/BotFather) on Telegram to obtain your TELEGRAM_BOT_TOKEN
2. Retrieve your numeric Telegram User ID
3. Open your environment file located at `~/.hermes/.env` and populate the following fields

```yaml
TELEGRAM_BOT_TOKEN=your_bot_token_here
TELEGRAM_ALLOWED_USERS=your_numeric_id_here
```

Start the messaging service

`hermes gateway`

## OpenRouter Rate Limits

If you are using OpenRouter as your LLM provider you may encounter rate limits that cause the agent to fail during long sequences. You can mitigate this by adjusting the `human_delay` parameter

### Updating config.yml

Locate your configuration file at `~/.hermes/config.yml` and update the `human_delay` setting. To correctly throttle the agent to stay under the 20 RPM OpenRouter limit change the mode to custom and set the millisecond values to ensure adequate time passes between each action

```yaml
human_delay:
  mode: 'custom'
  min_ms: 3200
  max_ms: 4500
```

### OpenRouter Tiers and Throughput

OpenRouter throughput is determined by your account credit balance. High demand models often require moving beyond the default free tier for reliable agentic performance

Tier breakdown:
* $0 Credits, Restricted Throughput
* $5 Deposit, Standard Throughput
* $10 Deposit, Maximum Throughput

Depositing $10 (account lifetime spend) into your OpenRouter account balance will move you to the highest available throughput tier for free and low cost models

Create a new API key in OpenRouter and set the credit limit to `0` to prevent any accidental charges. This in combination of the `human_delay` setting should ensure reliable agentic performance without exceeding rate limits. Use this key with Hermes for "free" tokens

## Logs and Troubleshooting

If the agent is not responding to messages you can monitor the gateway activity in real time

`tail ~/.hermes/logs/gateway.log`