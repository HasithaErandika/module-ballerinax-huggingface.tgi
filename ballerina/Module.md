## Overview

[Hugging Face Text Generation Inference (TGI)](https://huggingface.co/docs/text-generation-inference/index) is a purpose-built solution for deploying and serving Large Language Models (LLMs) in production. It enables high-performance text generation on models like Llama, Mistral, Falcon, and Qwen.

The `ballerinax/huggingface.tgi` package offers functionality to connect and interact with the TGI REST API, enabling seamless integration of advanced text generation, chat completions, tokenization, and model metadata retrieval into your applications.

### Key Features

- Native `/generate` and `/generate_stream` endpoints with fine-grained sampling control (temperature, top-k, top-p, stop sequences, repetition penalty)
- OpenAI-compatible chat completions via `/v1/chat/completions` (drop-in replacement for OpenAI SDK users)
- OpenAI-compatible completions via `/v1/completions`
- Tokenization via `/tokenize` and `/chat_tokenize` to inspect and manage model context limits
- Promethean metric scraping, model info retrieval, and health check support

## Setup guide

To use the Hugging Face TGI connector, you need a Hugging Face account and an API token. If you do not have an account, sign up at [huggingface.co](https://huggingface.co/join).

#### Create a Hugging Face Access Token

1. Navigate to your profile settings by clicking your avatar in the top-right corner and selecting **Settings**.
2. In the left sidebar, click on **Access Tokens**.
3. Click **New token**, give it a name, select the appropriate role (`read` is sufficient for inference), and click **Generate a token**.
4. Store the API token securely to use in your application.

## Quickstart

To use the `Hugging Face TGI` connector in your Ballerina application, update the `.bal` file as follows:

### Step 1: Import the module

Import the `ballerinax/huggingface.tgi` module.

```ballerina
import ballerinax/huggingface.tgi;
```

### Step 2: Create a new connector instance

Create a `tgi:Client` with the obtained API Key and initialize the connector.

```ballerina
configurable string token = ?;

final tgi:Client hfClient = check new({
    auth: {
        token
    }
});
```

### Step 3: Invoke the connector operation

Now, you can utilize available connector operations.

#### Generate a chat completion

```ballerina
public function main() returns error? {
    tgi:ChatCompletion completion = check hfClient->/v1/chat/completions.post({
        messages: [{role: "user", content: "What is Deep Learning?"}],
        temperature: 0.7
    });
}
```

### Step 4: Run the Ballerina application

```bash
bal run
```

## Examples

The `Hugging Face TGI` connector provides practical examples illustrating usage in various scenarios. Explore these [examples](https://github.com/ballerina-platform/module-ballerinax-huggingface.tgi/tree/main/examples/), covering the following use cases:

1. [Text Generation](https://github.com/ballerina-platform/module-ballerinax-huggingface.tgi/tree/main/examples/text-generation) - Native `/generate` endpoint with fine-grained sampling control.
2. [Token Counter](https://github.com/ballerina-platform/module-ballerinax-huggingface.tgi/tree/main/examples/token-counter) - Using `/tokenize` and `/info` endpoints to count tokens and check context windows.
