# agents.md

## 项目概览

- 项目名称：`aifriends backend`
- 类型：Django 6 + Django REST Framework 后端服务
- 目标：为 AI 陪伴/虚拟角色产品提供用户、角色、好友关系、消息会话、语音识别、语音合成和记忆更新能力
- 默认时区：`Asia/Shanghai`
- 当前数据库：`SQLite`（`db.sqlite3`）

## 主要模块

- `backend/`
  - Django 项目配置
  - `settings.py` 中定义了 JWT、静态资源、媒体文件、CORS 等配置
- `web/models/`
  - `user.py`：用户扩展资料 `UserProfile`
  - `character.py`：角色 `Character` 与音色 `Voice`
  - `friend.py`：好友关系 `Friend`、消息 `Message`、系统提示词 `SystemPrompt`
- `web/views/`
  - `user/`：注册、登录、刷新 token、获取用户信息、更新资料
  - `create/character/`：角色创建、更新、删除、详情、列表、音色列表
  - `friend/`：建立好友关系、好友列表、删除好友
  - `friend/message/`：聊天、历史消息、ASR、记忆更新
  - `homepage/`：首页角色流与搜索
- `web/documents/utils/`
  - 向量知识库相关逻辑，包括自定义 Embedding 与文档入库

## AI 能力链路

- 聊天主流程位于 `web/views/friend/message/chat/`
- `graph.py` 使用 `LangGraph + ChatOpenAI` 构建 agent
- 内置工具：
  - `get_time`：查询当前时间
  - `search_knowledge_base`：从 `LanceDB` 向量库检索知识片段
- `chat.py` 负责：
  - 拼接系统提示词、角色设定、长期记忆、近 10 条历史消息
  - 通过 SSE 返回文本流
  - 同时调用 WebSocket TTS 服务返回音频分片
  - 记录消息 token 用量
  - 在对话后更新好友长期记忆

## 外部依赖线索

项目依赖主要记录在 `pyproject.toml` 和根目录 `requirements.txt` 中，包括：

- `django`
- `djangorestframework`
- `djangorestframework-simplejwt`
- `django-cors-headers`
- `python-dotenv`
- `websockets`
- `langchain-core`
- `langchain-community`
- `langchain-openai`
- `langgraph`
- `lancedb`

## 关键环境变量

源码中直接读取了以下环境变量：

- `API_KEY`
- `API_BASE`
- `WSS_URL`
- `VOICE_URL`

用途：

- `API_KEY` / `API_BASE`：聊天模型调用
- `WSS_URL`：ASR/TTS WebSocket 服务地址
- `VOICE_URL`：自定义音色管理接口地址

## 现有接口分组

- 用户认证
  - `/api/user/account/login/`
  - `/api/user/account/logout/`
  - `/api/user/account/register/`
  - `/api/user/account/refresh_token/`
  - `/api/user/account/get_user_info/`
- 用户资料
  - `/api/user/profile/update/`
- 角色管理
  - `/api/create/character/create/`
  - `/api/create/character/update/`
  - `/api/create/character/remove/`
  - `/api/create/character/get_single/`
  - `/api/create/character/get_list/`
  - `/api/create/character/voice/get_list/`
- 首页与关系
  - `/api/homepage/index/`
  - `/api/friend/get_or_create/`
  - `/api/friend/remove/`
  - `/api/friend/get_list/`
- 会话能力
  - `/api/friend/message/chat/`
  - `/api/friend/message/get_history/`
  - `/api/friend/message/asr/asr/`

## 风险与待补充项

- 生产部署前应设置真实 `DJANGO_SECRET_KEY`，并将 `DJANGO_DEBUG` 设为 `False`
- 登录与注册时设置了 `secure=True` 的 cookie，本地 HTTP 调试时可能需要额外注意
- 多处视图使用宽泛的 `except:`，后续建议补日志与精确异常处理

## 后续协作建议

- 为核心接口补测试，优先覆盖登录、角色创建、聊天、ASR
- 把知识库构建和初始化步骤写成脚本或管理命令
