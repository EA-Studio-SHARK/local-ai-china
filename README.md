<div align="center">

# 🏠 Local AI China

**中国开发者本地 AI 完全指南 | 一键启动 | 完全免费**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![GitHub stars](https://img.shields.io/github/stars/EA-Studio-SHARK/local-ai-china?style=social)
![Last Updated](https://img.shields.io/badge/最后更新-2026--05-brightgreen)

> 专为中国开发者优化：解决网络限制、API 成本、数据隐私三大痛点  
> 一条命令启动完整本地 AI 环境，支持 DeepSeek / Qwen / GLM 等国产模型

**⭐ 如果觉得有用，请给个 Star！你的支持是持续更新的动力**

</div>

---

## 🎯 为什么需要本地 AI？

| 问题 | 云端 API | 本地部署 |
|------|---------|---------|
| 访问限制 | ❌ 需要特殊网络 | ✅ 完全本地，无限制 |
| API 费用 | ❌ 持续收费 | ✅ 一次配置，永久免费 |
| 数据隐私 | ❌ 数据传到境外 | ✅ 数据不出本机 |
| 响应速度 | ❌ 依赖网络延迟 | ✅ 本地推理，毫秒响应 |
| 离线使用 | ❌ 必须联网 | ✅ 断网可用 |

---

## 🚀 一键启动（推荐方式）

### 前置要求

- Docker Desktop（[下载地址](https://www.docker.com/products/docker-desktop/)）
- 16GB+ 内存（运行 7B 模型）或 32GB+（运行 14B 模型）
- 显卡可选（有 GPU 速度快 5-10 倍，无 GPU 用 CPU 也能跑）

### 启动完整 AI 环境

```bash
git clone https://github.com/EA-Studio-SHARK/local-ai-china
cd local-ai-china
docker compose up -d
```

**启动后访问：**
- 🌐 Open WebUI（聊天界面）：http://localhost:3000
- 📊 模型管理：http://localhost:11434

**默认包含：**
- Ollama（本地模型运行引擎）
- Open WebUI（ChatGPT 风格界面）
- DeepSeek-R1:7B（预下载，开箱即用）

---

## 📦 配置文件说明

### 基础版（无 GPU，适合大多数人）

```yaml
# docker-compose.yml
```

→ 见项目根目录 `docker-compose.yml`

### GPU 加速版（有 NVIDIA 显卡）

```bash
docker compose -f docker-compose.gpu.yml up -d
```

### 仅 Ollama（最轻量）

```bash
docker compose -f docker-compose.minimal.yml up -d
```

---

## 🤖 推荐模型

### 中文能力强（首选）

| 模型 | 大小 | 内存需求 | 适合场景 | 安装命令 |
|------|------|---------|---------|---------|
| DeepSeek-R1:7B | 4.7GB | 8GB+ | 推理、编程、通用 | `ollama pull deepseek-r1:7b` |
| DeepSeek-R1:14B | 9GB | 16GB+ | 高质量推理 | `ollama pull deepseek-r1:14b` |
| Qwen2.5:7B | 4.7GB | 8GB+ | 中文对话、写作 | `ollama pull qwen2.5:7b` |
| Qwen2.5:14B | 9GB | 16GB+ | 高质量中文 | `ollama pull qwen2.5:14b` |
| GLM4:9B | 5.5GB | 10GB+ | 工具调用、Agent | `ollama pull glm4:9b` |

### 编程专用

| 模型 | 大小 | 安装命令 |
|------|------|---------|
| DeepSeek-Coder-V2:16B | 9GB | `ollama pull deepseek-coder-v2:16b` |
| Qwen2.5-Coder:7B | 4.7GB | `ollama pull qwen2.5-coder:7b` |
| CodeLlama:7B | 3.8GB | `ollama pull codellama:7b` |

---

## 🌐 中国网络优化

### 方案一：使用国内镜像（推荐）

```bash
# 设置 Ollama 使用国内镜像源
export OLLAMA_ORIGINS="*"
export OLLAMA_HOST="0.0.0.0"

# 或在 .env 文件中配置（已预置）
```

### 方案二：离线模型包

如果网络实在不好，可以从以下国内渠道下载模型文件：
- [魔搭社区 ModelScope](https://modelscope.cn) → 搜索对应模型
- [始智 AI](https://www.wisemodel.cn) → 国内模型托管

下载后放入：`~/.ollama/models/`

### 方案三：配置代理

```bash
# .env 文件中设置
HTTP_PROXY=http://127.0.0.1:7890
HTTPS_PROXY=http://127.0.0.1:7890
```

---

## 🔧 进阶配置

### 接入 Open WebUI 知识库（RAG）

1. 打开 http://localhost:3000
2. 左侧菜单 → **工作空间** → **知识**
3. 上传文档（PDF/TXT/MD）
4. 新建对话时选择知识库

### 接入 MCP 工具

```bash
# 安装 MCP 支持
pip install open-webui-mcp
```

配置参考：[awesome-mcp-zh](https://github.com/EA-Studio-SHARK/awesome-mcp-zh)

### API 模式（给开发者用）

本地 Ollama 完全兼容 OpenAI API 格式：

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama",  # 任意字符串
)

response = client.chat.completions.create(
    model="qwen2.5:7b",
    messages=[{"role": "user", "content": "你好"}]
)
print(response.choices[0].message.content)
```

---

## 📊 性能参考

> 测试设备：MacBook Pro M3 Pro 18GB

| 模型 | 速度（tokens/s） | 中文质量 |
|------|----------------|---------|
| Qwen2.5:7B | ~35 t/s | ⭐⭐⭐⭐ |
| DeepSeek-R1:7B | ~30 t/s | ⭐⭐⭐⭐⭐ |
| GLM4:9B | ~25 t/s | ⭐⭐⭐⭐ |

> Windows + NVIDIA RTX 4090：速度提升 5-8 倍

---

## ❓ 常见问题

**Q：没有显卡能跑吗？**
A：可以，用 CPU 也能跑，7B 模型大约 20-40 tokens/s，M 系列 Mac 效果最好。

**Q：MacBook 能跑多大的模型？**
A：16GB 内存推荐 7B，32GB 推荐 14B，64GB 可以跑 32B。

**Q：Windows 如何配置 GPU 加速？**
A：使用 `docker-compose.gpu.yml`，需要先安装 [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)。

**Q：模型下载太慢怎么办？**
A：参考上方「中国网络优化」章节，使用 ModelScope 下载。

---

## � 相关项目

> EA-Studio-SHARK 为中国开发者打造的 AI 工具生态：

| 项目 | 简介 | Stars |
|------|------|-------|
| [china-hot-mcp](https://github.com/EA-Studio-SHARK/china-hot-mcp) | 让 Claude 实时获取微博/知乎/B站等平台热搜的 MCP 服务 | ⭐ |
| [ai-morning-brief](https://github.com/EA-Studio-SHARK/ai-morning-brief) | Claude Code Skill：一键生成中英文双轨 AI 早报 | ⭐ |
| [awesome-mcp-zh](https://github.com/EA-Studio-SHARK/awesome-mcp-zh) | 国内可用 MCP 服务精选，附完整配置示例 | ⭐ |
| [ai-agent-guide](https://github.com/EA-Studio-SHARK/ai-agent-guide) | 从零上手 AI Agent 开发教程，含完整代码示例 | ⭐ |
| [awesome-ai-tools-zh](https://github.com/EA-Studio-SHARK/awesome-ai-tools-zh) | 500+ AI 工具中文精选清单，每周更新 | ⭐ |

---

## �📬 联系与合作

- 🚀 **企业私有化部署**：帮你在内网搭建完整 AI 环境
- 🔧 **定制 AI 工作流**：基于本地模型的业务自动化
- 📬 **Telegram**：[@ExploreAllStudio](https://t.me/ExploreAllStudio)

---

<div align="center">

**⭐ 如果觉得有用，请给个 Star！你的支持是持续更新的动力**

</div>
