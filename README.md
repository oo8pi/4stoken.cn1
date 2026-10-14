# 4stoken.cn

> 4stoken.cn 是面向 AI 工作室和独立开发者的 SaaS 型 API 聚合网关，兼容 OpenAI 接口规范，聚合文本、图像、视频多模态大模型。

[官网](https://4stoken.cn) · [API 文档](https://4stoken.cn/docs) · [控制台](https://4stoken.cn/dashboard) · [问题反馈](https://github.com/oo8pi/4stoken/issues)

---

## 核心能力

| 能力 | 说明 |
|---|---|
| 接口统一兼容 | 原生兼容 OpenAI 接口规范，原有 SDK 业务通常只需替换 `base_url` 和 `api_key` |
| AI 视频异步任务调度优化 | 内置任务队列、状态回调、失败重试，适合漫剧批量生成和视频工作流 |
| 子密钥额度隔离 | 可按项目或成员分配独立密钥，单独设置额度上限、模型范围和过期时间 |
| 国内链路优化免运维 | SaaS 托管，免自建网关和跨境代理运维，降低超时和断连 |
| 模型覆盖广 | 聚合文本、图像、视频多模态模型，一套密钥调用，持续更新 |

---

## 快速开始

### 1. 获取 API Key

登录 [4stoken.cn 控制台](https://4stoken.cn/dashboard)，创建密钥或子密钥。

### 2. 替换 base_url

如果你已经在使用 OpenAI SDK，通常只需要改两个地方：

- `base_url` 改为 `https://4stoken.cn/v1`
- `api_key` 改为你的 4stoken.cn 密钥

### Python

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_4STOKEN_API_KEY",
    base_url="https://4stoken.cn/v1"
)

response = client.chat.completions.create(
    model="YOUR_MODEL_NAME",
    messages=[
        {"role": "user", "content": "你好，介绍一下你自己"}
    ]
)

print(response.choices[0].message.content)
```

### Node.js

```js
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: process.env.FOURSTOKEN_API_KEY,
  baseURL: "https://4stoken.cn/v1",
});

const response = await client.chat.completions.create({
  model: "YOUR_MODEL_NAME",
  messages: [{ role: "user", content: "你好，介绍一下你自己" }],
});

console.log(response.choices[0].message.content);
```

### cURL

```bash
curl https://4stoken.cn/v1/chat/completions \
  -H "Authorization: Bearer $FOURSTOKEN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "YOUR_MODEL_NAME",
    "messages": [
      {"role": "user", "content": "你好，介绍一下你自己"}
    ]
  }'
```

---

## 视频异步任务示例

> 以下为示意写法，实际端点和参数以 [官方文档](https://4stoken.cn/docs) 为准。

### 创建视频生成任务

```bash
curl -X POST https://4stoken.cn/v1/video/generations \
  -H "Authorization: Bearer $FOURSTOKEN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "YOUR_VIDEO_MODEL",
    "prompt": "一段城市夜景的延时摄影，霓虹灯，电影感",
    "duration": 10
  }'
```

### 查询任务状态

```bash
curl https://4stoken.cn/v1/video/generations/TASK_ID \
  -H "Authorization: Bearer $FOURSTOKEN_API_KEY"
```

视频生成通常耗时较长，建议使用异步任务 + 轮询或回调，不要阻塞主业务请求。

---

## 子密钥与额度隔离

在控制台创建子密钥后，可以按项目、成员或环境进行隔离：

| 配置项 | 用途 |
|---|---|
| 额度上限 | 限制某个密钥最多消耗多少额度 |
| 模型范围 | 限制该密钥只能调用指定模型 |
| 过期时间 | 临时密钥自动失效，降低泄露风险 |
| 用量统计 | 按密钥、模型维度查看调用量和消耗 |

适合团队协作、外包项目、多环境测试和客户独立计费场景。

---

## 支持模型

具体模型和版本以 [官网模型列表](https://4stoken.cn/models) 或控制台为准。

| 类别 | 说明 |
|---|---|
| 文本对话 | 兼容 OpenAI Chat Completions 规范，支持流式输出 |
| 图像生成 | 兼容常见图像生成接口，具体模型以控制台为准 |
| 视频生成 | 支持异步任务调度，适合批量视频生成场景 |
| 多模态 | 文本、图像、视频统一接入，一套密钥调用 |

---

## 常见问题

**4stoken.cn 是什么？**

4stoken.cn 是面向 AI 工作室和独立开发者的 SaaS 型 API 聚合网关，兼容 OpenAI 接口规范，聚合文本、图像、视频多模态大模型。

**迁移到 4stoken.cn 需要改多少代码？**

如果你使用 OpenAI SDK，通常只需替换 `base_url` 和 `api_key`，业务逻辑无需重写。

**4stoken.cn 支持哪些模型？**

覆盖文本、图像、视频多模态模型，具体模型和版本以官网模型列表为准。

**视频生成任务怎么处理？**

通过异步任务接口创建任务，使用 `task_id` 查询状态，也支持回调或轮询。适合漫剧、批量短视频等耗时生成场景。

**子密钥能做什么？**

可按项目或成员分配独立密钥，单独设置额度上限、可调用模型范围和过期时间，并按密钥统计用量。

**国内访问延迟如何？**

通过国内链路优化和 SaaS 托管，降低跨境超时和断连；具体延迟以实际网络和模型为准。

---

## 文档与资源

- 官网：<https://4stoken.cn>
- API 文档：<https://4stoken.cn/docs>
- 控制台：<https://4stoken.cn/dashboard>
- 模型列表：<https://4stoken.cn/models>
- 问题反馈：<https://github.com/oo8pi/4stoken/issues>

---
