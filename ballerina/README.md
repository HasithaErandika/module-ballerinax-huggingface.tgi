## Overview

[Hugging Face](https://huggingface.co/) is a leading AI platform that hosts thousands of open-source machine learning models, datasets, and demos. It provides a hub for the AI community to collaborate and access state-of-the-art models for natural language processing, computer vision, and more.

The `avi0ra/huggingface` connector offers APIs to connect and interact with the [Hugging Face Text Generation Inference (TGI)](https://huggingface.co/docs/text-generation-inference) API endpoints, enabling you to generate text, run chat completions, tokenize inputs, and more using hosted LLMs.

### Key Features

- Generate text completions using open-source LLMs via the TGI API
- Run OpenAI-compatible chat completions (`/v1/chat/completions`)
- Tokenize and analyze inputs with `/tokenize` and `/chat_tokenize`
- Stream token generation with Server-Sent Events (SSE) via `/generate_stream`
- Access model info, health status, and Prometheus metrics

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

To use the `huggingface` connector in your Ballerina application, update the `.bal` file as follows:

### Step 1: Import the module

Import the `huggingface` module.

```ballerina
import avi0ra/huggingface;
```

### Step 2: Instantiate a new connector

1. Create a `Config.toml` file and configure the obtained token as follows:

```toml
token = "<Your Hugging Face API Token>"
```

2. Create a `huggingface:ConnectionConfig` with the access token and initialize the connector with it.

```ballerina
configurable string token = ?;

final huggingface:Client hfClient = check new ("https://api-inference.huggingface.co", {
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
    huggingface:ChatCompletion completion = check hfClient->/v1/chat/completions.post({
        messages: [{role: "user", content: "What is Deep Learning?"}],
        temperature: 0.7
    });
}
```

#### Generate text

```ballerina
public function main() returns error? {
    huggingface:GenerateResponse resp = check hfClient->/generate.post({
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

The `Hugging Face` connector provides practical examples illustrating usage in various scenarios. Explore these [examples](https://github.com/HasithaErandika/module-ballerinax-huggingface/tree/main/examples/), covering the following use cases:

[//]: # (TODO: Add examples)
