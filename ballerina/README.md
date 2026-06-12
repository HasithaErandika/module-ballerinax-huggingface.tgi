## Overview

HuggingFace Text Generation Inference (TGI) is a high-performance serving framework for large language models. It powers HuggingFace Inference Endpoints and supports models such as Llama, Mistral, Qwen, Falcon, and more.

The `ballerinax/huggingface.tgi` connector provides a Ballerina client for the TGI REST API, covering:

- OpenAI-compatible chat via `/v1/chat/completions` (drop-in replacement for OpenAI SDK users)
- OpenAI-compatible completions via `/v1/completions`
- TGI native generation via `/generate` with full sampling control
- Streaming generation via `/generate_stream` (Server-Sent Events)
- Tokenization via `/tokenize` and `/chat_tokenize`
- Model & server info via `/info` and `/v1/models`
- Health check via `/health`

Note: For text embeddings, reranking, and classification, see `ballerinax/huggingface.tei`.

## Setup guide

To use the Hugging Face connector, you need a Hugging Face account and an API token. If you do not have an account, sign up at [huggingface.co](https://huggingface.co/join).

### Step 1: Create a Hugging Face Account

1. Go to [https://huggingface.co/join](https://huggingface.co/join) and create an account.

2. Verify your email address and log in to the Hugging Face Hub.

### Step 2: Generate an Access Token

1. Navigate to your profile settings by clicking your avatar in the top-right corner and selecting **Settings**.

2. In the left sidebar, click on **Access Tokens**.

3. Click **New token**, give it a name, select the appropriate role (`read` is sufficient for inference), and click **Generate a token**.

4. Copy the generated token immediately — it will not be shown again.

### Step 3: Choose a Model Endpoint

You can either:

- Use the **Hugging Face Inference API** at `https://api-inference.huggingface.co` for serverless inference on hosted models.
- Deploy your own **TGI instance** and point the connector at your own service URL.

## Quickstart

To use the `huggingface.tgi` connector in your Ballerina application, update the `.bal` file as follows:

### Step 1: Import the module

Import the `huggingface.tgi` module.

```ballerina
import ballerinax/huggingface.tgi;
```

### Step 2: Instantiate a new connector

1. Create a `Config.toml` file and configure the obtained token as follows:

```toml
token = "<Your Hugging Face API Token>"
```

2. Create a `huggingface.tgi:ConnectionConfig` with the access token and initialize the connector with it.

```ballerina
configurable string token = ?;

final huggingface.tgi:Client hfClient = check new ({
    auth: {
        token: token
    }
});
```

### Step 3: Invoke the connector operation

Now, utilize the available connector operations.

#### Run a chat completion

```ballerina
public function main() returns error? {
    huggingface.tgi:ChatCompletion completion = check hfClient->/v1/chat/completions.post({
        messages: [{role: "user", content: "What is Deep Learning?"}],
        temperature: 0.7
    });
}
```

#### Generate text

```ballerina
public function main() returns error? {
    huggingface.tgi:GenerateResponse resp = check hfClient->/generate.post({
        inputs: "Once upon a time",
        parameters: {maxNewTokens: 50}
    });
}
```

### Step 4: Run the Ballerina application

```bash
bal run
```

## Examples

The `huggingface.tgi` connector provides practical examples illustrating usage in various scenarios. Explore these [examples](https://github.com/ballerina-platform/module-ballerinax-huggingface.tgi/tree/main/examples/), covering the following use cases:

1. **Text Generation** - Native `/generate` endpoint with fine-grained sampling control (temperature, top-k, top-p, repetition penalty)
2. **Token Counter** - Using `/tokenize` and `/info` endpoints to count tokens and check context windows
