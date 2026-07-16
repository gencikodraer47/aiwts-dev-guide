# AIWTS API Gateway — 开发者接入指南

> 200+ AI 模型，一个 API 密钥，中国大陆直连，注册即送 ¥10 额度。

**AIWTS**（[www.aiwts.com](https://www.aiwts.com)）是一个聚合型 AI API 网关，以 OpenAI 兼容协议统一对接 GPT-5.6、Claude 4.5、Grok、Gemini 3、DeepSeek V3、Seedance 2.0 等 200+ 主流模型。本文档面向开发者，提供完整的接入流程、代码示例、模型选型对比和场景实践。

---

## 目录

- [为什么选择 AIWTS](#为什么选择-aiwts)
- [快速开始](#快速开始)
- [可用模型列表](#可用模型列表)
- [价格优势对比](#价格优势对比)
- [代码示例](#代码示例)
  - [Python](#python)
  - [Node.js](#nodejs)
  - [cURL](#curl)
- [SDK 兼容性](#sdk-兼容性)
- [短剧与内容创作场景](#短剧与内容创作场景)
- [关联站点](#关联站点)
- [API 文档与支持](#api-文档与支持)
- [License](#license)

---

## 为什么选择 AIWTS

| 特性 | 说明 |
|------|------|
| **200+ 模型** | 覆盖 OpenAI、Anthropic、Google、xAI、DeepSeek 等主流厂商 |
| **中国大陆直连** | 无需代理，国内服务器直接访问，延迟低至 30ms |
| **OpenAI 兼容** | 现有代码只需改 `base_url` 和 `api_key`，零成本迁移 |
| **99.99% SLA** | 企业级稳定性保障，多节点自动故障转移 |
| **注册送 ¥10** | 新用户注册即获赠额度，可体验全部模型 |
| **统一计费** | 按量付费，一个钱包管理所有模型，无需多平台充值 |

---

## 快速开始

### 1. 注册账号

访问 [www.aiwts.com](https://www.aiwts.com) 注册账号，注册即送 **¥10** 额度。

### 2. 获取 API Key

登录后进入控制台 → **API Keys** 页面，点击「创建密钥」，复制生成的 `sk-...` 格式密钥。

### 3. 发起第一次请求

将下面的 `YOUR_API_KEY` 替换为你的密钥，即可发送请求：

```bash
curl https://api.wts.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-5.6",
    "messages": [{"role": "user", "content": "Hello, AIWTS!"}]
  }'
```

就是这么简单——如果你已经在用 OpenAI SDK，只需修改 `base_url` 为 `https://api.wts.ai/v1` 即可。

---

## 可用模型列表

AIWTS 支持 200+ 模型，以下列出核心模型及推荐用途：

### 文本对话模型

| 模型 | 上下文 | 推荐场景 |
|------|--------|----------|
| `gpt-5.6` | 256K | 通用对话、代码生成、复杂推理 |
| `claude-4.5-sonnet` | 200K | 长文分析、结构化输出、学术写作 |
| `grok-4` | 128K | 实时信息、创意写作 |
| `gemini-3-pro` | 1M | 超长上下文、多模态理解 |
| `deepseek-v3` | 128K | 数学推理、代码补全、高性价比 |
| `deepseek-r1` | 128K | 深度推理、逻辑链思维 |

### 图像与视频模型

| 模型 | 类型 | 推荐场景 |
|------|------|----------|
| `seedance-2.0` | 视频 | 短剧生成、AI 视频创作 |
| `flux-pro` | 图像 | 高质量图像生成 |
| `dall-e-3` | 图像 | 创意插画、营销素材 |
| `gpt-image` | 图像 | 图像编辑与理解 |

> 完整模型列表请参考 [API 文档](https://aiwts.apifox.cn)。

---

## 价格优势对比

AIWTS 采用聚合采购模式，价格通常为官方 API 的 **30%-70%**。以下为部分模型对比（每百万 Token）：

| 模型 | 官方价格 | AIWTS 价格 | 节省 |
|------|----------|------------|------|
| GPT-5.6 | $15.00 | $5.20 | **65%** |
| Claude 4.5 Sonnet | $15.00 | $4.80 | **68%** |
| Gemini 3 Pro | $10.00 | $3.50 | **65%** |
| DeepSeek V3 | $2.20 | $0.70 | **68%** |
| Grok 4 | $15.00 | $5.00 | **67%** |

> 以上价格为参考值，实际价格以 [控制台](https://www.aiwts.com) 实时显示为准。

**为什么能做到更低价？**

- 聚合多平台采购，获取批量折扣
- 自建推理基础设施，降低单位成本
- 将节省的成本让利给开发者

---

## 代码示例

### Python

使用 `openai` 官方 SDK，只需修改 `base_url`：

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_API_KEY",
    base_url="https://api.wts.ai/v1"
)

# --- 对话补全 ---
response = client.chat.completions.create(
    model="gpt-5.6",
    messages=[
        {"role": "system", "content": "你是一个专业的技术写作助手。"},
        {"role": "user", "content": "写一段关于 API 网关的简介，100字以内。"}
    ],
    temperature=0.7,
    max_tokens=500
)

print(response.choices[0].message.content)

# --- 流式输出 ---
stream = client.chat.completions.create(
    model="deepseek-v3",
    messages=[{"role": "user", "content": "用 Python 实现一个快速排序。"}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

### Node.js

使用 `openai` npm 包：

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "YOUR_API_KEY",
  baseURL: "https://api.wts.ai/v1",
});

// --- 对话补全 ---
const response = await client.chat.completions.create({
  model: "claude-4.5-sonnet",
  messages: [
    { role: "system", content: "你是一个专业的翻译助手。" },
    { role: "user", content: "将以下内容翻译成英文：API 网关是微服务架构的核心组件。" },
  ],
  temperature: 0.3,
});

console.log(response.choices[0].message.content);

// --- 流式输出 ---
const stream = await client.chat.completions.create({
  model: "gemini-3-pro",
  messages: [{ role: "user", content: "解释什么是 OAuth 2.0。" }],
  stream: true,
});

for await (const chunk of stream) {
  process.stdout.write(chunk.choices[0]?.delta?.content || "");
}
```

### cURL

```bash
# 基础对话
curl https://api.wts.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "grok-4",
    "messages": [{"role": "user", "content": "今天的科技新闻有哪些？"}],
    "stream": false
  }'

# 流式对话
curl https://api.wts.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-5.6",
    "messages": [{"role": "user", "content": "写一首关于编程的俳句。"}],
    "stream": true
  }'

# 图像生成
curl https://api.wts.ai/v1/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "dall-e-3",
    "prompt": "A futuristic API gateway connecting multiple AI clouds, digital art style",
    "n": 1,
    "size": "1024x1024"
  }'
```

---

## SDK 兼容性

AIWTS 采用 OpenAI 兼容协议，以下 SDK 可直接使用（仅需修改 `base_url` 和 `api_key`）：

| 语言 / SDK | 兼容性 | 配置方式 |
|------------|--------|----------|
| **Python** `openai` ≥ 1.0 | ✅ 完全兼容 | `base_url="https://api.wts.ai/v1"` |
| **Node.js** `openai` ≥ 4.0 | ✅ 完全兼容 | `baseURL: "https://api.wts.ai/v1"` |
| **Go** `go-openai` | ✅ 完全兼容 | `baseURL: "https://api.wts.ai/v1"` |
| **Java** `openai-java` | ✅ 完全兼容 | 配置 `OPENAI_BASE_URL` 环境变量 |
| **PHP** `openai-php` | ✅ 完全兼容 | `->withBasePath("https://api.wts.ai/v1")` |
| **Ruby** `ruby-openai` | ✅ 完全兼容 | 环境变量 `OPENAI_API_BASE` |
| **.NET** `OpenAI-DotNet` | ✅ 完全兼容 | `OpenAIClientSettings(baseUri: ...)` |
| **Rust** `async-openai` | ✅ 完全兼容 | `OpenAIConfig::default().with_api_base(...)` |
| **LangChain** | ✅ 完全兼容 | `ChatOpenAI(openai_api_base=...)` |
| **LlamaIndex** | ✅ 完全兼容 | `OpenAILike` 或修改 `api_base` |
| **AutoGen** | ✅ 完全兼容 | 修改 `base_url` 配置 |
| **Dify** | ✅ 完全兼容 | 模型供应商选择 OpenAI 兼容 |
| **FastGPT** | ✅ 完全兼容 | 配置自定义 API 地址 |

**迁移只需两步：**

1. 将 `base_url` 改为 `https://api.wts.ai/v1`
2. 将 `api_key` 改为你的 AIWTS 密钥

---

## 短剧与内容创作场景

AIWTS 不仅是 API 中转，更是内容创作的全栈基础设施。以下是典型场景的模型组合方案：

### 场景一：AI 短剧生成

```
剧本创作          →    分镜设计         →    视频生成
(DeepSeek V3)      →    (GPT-5.6)        →    (Seedance 2.0)
```

```python
from openai import OpenAI

client = OpenAI(api_key="YOUR_API_KEY", base_url="https://api.wts.ai/v1")

# Step 1: 生成短剧剧本
script = client.chat.completions.create(
    model="deepseek-v3",
    messages=[
        {"role": "system", "content": "你是一位短剧编剧，擅长都市情感题材。"},
        {"role": "user", "content": "写一个30秒短剧脚本，包含3个场景，主题是'深夜加班的惊喜'。"}
    ]
)

# Step 2: 生成分镜描述
storyboard = client.chat.completions.create(
    model="gpt-5.6",
    messages=[
        {"role": "system", "content": "将剧本转化为详细的分镜描述，包含画面、景别、时长。"},
        {"role": "user", "content": script.choices[0].message.content}
    ]
)

# Step 3: 调用 Seedance 2.0 生成视频（示例）
# video = client.video.generations.create(
#     model="seedance-2.0",
#     prompt=storyboard.choices[0].message.content,
#     duration=30
# )
```

### 场景二：AI 图文创作

```python
# 生成营销文案
copywriting = client.chat.completions.create(
    model="claude-4.5-sonnet",
    messages=[
        {"role": "user", "content": "为一款智能手表写3条社交媒体推广文案，风格活泼。"}
    ]
)

# 生成配套图片
image = client.images.generations.create(
    model="flux-pro",
    prompt="Modern smartwatch on a minimalist desk, soft morning light, lifestyle photography",
    size="1024x1024"
)
```

### 场景三：批量内容生产流水线

```python
import json

topics = ["AI 教育", "智能家居", "新能源出行", "远程办公", "健康饮食"]

for topic in topics:
    # 大纲生成
    outline = client.chat.completions.create(
        model="deepseek-v3",
        messages=[
            {"role": "system", "content": "你是内容运营专家，输出 JSON 格式的大纲。"},
            {"role": "user", "content": f"为「{topic}」生成一篇2000字科普文章的大纲，JSON格式。"}
        ],
        response_format={"type": "json_object"}
    )

    # 正文扩写
    article = client.chat.completions.create(
        model="gpt-5.6",
        messages=[
            {"role": "system", "content": "根据大纲扩写为完整文章，语言通俗专业。"},
            {"role": "user", "content": outline.choices[0].message.content}
        ],
        max_tokens=4000
    )

    print(f"=== {topic} ===")
    print(article.choices[0].message.content)
    print()
```

---

## 关联站点

| 站点 | 地址 | 说明 |
|------|------|------|
| **AIWTS 主站** | [www.aiwts.com](https://www.aiwts.com) | API 平台主站，注册管理、充值、模型选型 |
| **WTS 短剧** | [wtsdj.mom](https://wtsdj.mom) | AI 短剧在线生成工具，一键创作短剧内容 |
| **WTS 生图** | [wtsgf.mom](https://wtsgf.mom) | AI 在线生图工具，支持多种风格图像生成 |
| **API 文档** | [aiwts.apifox.cn](https://aiwts.apifox.cn) | 完整 API 接口文档，含在线调试与示例 |

---

## API 文档与支持

- **完整 API 文档**：[aiwts.apifox.cn](https://aiwts.apifox.cn)
- **API 基础地址**：`https://api.wts.ai/v1`
- **认证方式**：Bearer Token（在请求头中携带 `Authorization: Bearer YOUR_API_KEY`）
- **协议兼容**：OpenAI API 兼容
- **SLA 保障**：99.99%

### 常见问题

<details>
<summary><b>API 地址是什么？</b></summary>

API 地址为 `https://api.wts.ai/v1`，与 OpenAI API 格式完全兼容。
</details>

<details>
<summary><b>中国大陆可以直接访问吗？</b></summary>

可以。AIWTS 在中国大陆部署了直连节点，无需代理即可访问，延迟通常在 30ms 以内。
</details>

<details>
<summary><b>支持哪些模型？</b></summary>

支持 200+ 模型，包括 GPT-5.6、Claude 4.5、Grok 4、Gemini 3 Pro、DeepSeek V3、Seedance 2.0 等。完整列表请参考 [API 文档](https://aiwts.apifox.cn)。
</details>

<details>
<summary><b>如何计费？</b></summary>

按 Token 用量计费，支持按量付费。注册即送 ¥10 额度，可在控制台查看实时用量和余额。
</details>

<details>
<summary><b>现有 OpenAI 代码怎么迁移？</b></summary>

只需将 `base_url` 改为 `https://api.wts.ai/v1`，`api_key` 改为 AIWTS 的密钥即可，其余代码无需修改。
</details>

---

## License

[MIT](./LICENSE)

---

<p align="center">
  <a href="https://www.aiwts.com">www.aiwts.com</a> ·
  <a href="https://wtsdj.mom">WTS 短剧</a> ·
  <a href="https://wtsgf.mom">WTS 生图</a> ·
  <a href="https://aiwts.apifox.cn">API 文档</a>
</p>

<p align="center">
  注册即送 ¥10 · 200+ 模型 · 中国大陆直连 · 99.99% SLA
</p>
