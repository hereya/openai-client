# OpenAI Client

Hereya package that stores an OpenAI API key securely in AWS SSM Parameter Store (as a `SecureString`) and exposes it to your project as the `OPENAI_API_KEY` environment variable.

## What it does

- Prompts for your OpenAI API key (`openai_api_key` parameter) when added to a project.
- Stores the key in AWS SSM Parameter Store under `/openai_client/<random-pet>/api_key`, encrypted as a `SecureString`.
- Exports the parameter as the `OPENAI_API_KEY` output. The stored value is the SSM parameter ARN; hereya resolves it to the decrypted key transparently when injecting environment variables (e.g. with `hereya run`), so your application receives the actual key at runtime.

## Usage

```shell
hereya add hereya/openai-client
```

You will be prompted for your OpenAI API key. You can also provide it upfront:

```shell
hereya add hereya/openai-client -p openai_api_key=sk-...
```

Then read it from your application like any other environment variable:

```javascript
const apiKey = process.env.OPENAI_API_KEY
```

## Parameters

| Name             | Description                                        | Required |
| ---------------- | -------------------------------------------------- | -------- |
| `openai_api_key` | API key for interacting with OpenAI programmatically | Yes      |

## Outputs

| Name             | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `OPENAI_API_KEY` | Your OpenAI API key, injected as an environment variable |

## Requirements

- AWS infrastructure (`infra: aws`) — the key is stored in your AWS account via SSM Parameter Store.
- Terraform (`iac: terraform`).
