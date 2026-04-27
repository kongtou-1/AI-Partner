# AI-Partner

AI-Partner 是一个 AI 陪伴应用，包含 Django 后端和 Vue 前端。项目支持用户注册登录、角色创建、好友关系、聊天消息、语音识别、语音合成和长期记忆更新。

## 项目结构

```text
AI-Partner/
├─ backend/                 # Django 后端
│  ├─ backend/              # Django 项目配置
│  ├─ web/                  # 业务 app：用户、角色、好友、消息、AI 能力
│  ├─ manage.py
│  ├─ pyproject.toml
│  └─ .env.example
├─ frontend/                # Vue + Vite 前端
│  ├─ src/
│  ├─ package.json
│  └─ vite.config.js
├─ requirements.txt
└─ README.md
```

## 主要功能

- 用户注册、登录、刷新 token、退出登录
- 用户资料维护
- AI 角色创建、编辑、删除、列表和详情
- 用户与角色建立好友关系
- 聊天消息记录和历史消息查询
- 基于 LangChain / LangGraph 的对话流程
- 基于 LanceDB 的本地知识库检索
- ASR 语音识别、TTS 语音合成和自定义音色接口

## 环境变量

后端不会把真实密钥提交到 Git。首次运行前请复制环境变量模板：

```powershell
Copy-Item backend\.env.example backend\.env
```

然后在 `backend/.env` 中填入真实配置：

```env
DJANGO_SECRET_KEY=change-me
DJANGO_DEBUG=True
DJANGO_ALLOWED_HOSTS=127.0.0.1,localhost

API_KEY=
API_BASE=
WSS_URL=
VOICE_URL=
```

变量说明：

- `DJANGO_SECRET_KEY`：Django 密钥，生产环境必须使用强随机值
- `DJANGO_DEBUG`：是否开启调试模式，生产环境应设为 `False`
- `DJANGO_ALLOWED_HOSTS`：允许访问的域名或 IP，多个值用英文逗号分隔
- `API_KEY`：模型和语音服务 API Key
- `API_BASE`：OpenAI 兼容模型接口地址
- `WSS_URL`：ASR / TTS WebSocket 服务地址
- `VOICE_URL`：自定义音色管理接口地址

## 后端启动

项目后端依赖写在 `backend/pyproject.toml` 和根目录 `requirements.txt` 中。推荐重新创建虚拟环境后安装依赖，不要提交 `.venv`。

```powershell
cd backend
python -m venv ..\.venv
..\.venv\Scripts\Activate.ps1
pip install -r ..\requirements.txt
python manage.py migrate
python manage.py runserver
```

如果使用 `uv`：

```powershell
cd backend
uv sync
uv run python manage.py migrate
uv run python manage.py runserver
```

## 前端启动

```powershell
cd frontend
npm install
npm run dev
```

前端接口地址在 `frontend/src/js/config/config.js` 中配置。

## Git 忽略说明

仓库已配置 `.gitignore`，会忽略以下本地或敏感内容：

- `.env`、`backend/.env` 和其他本地环境文件
- `.venv/`、`node_modules/`
- SQLite 数据库文件，如 `db.sqlite3`
- 上传媒体文件 `media/`
- 构建产物 `dist/`、`staticfiles/`
- LanceDB 本地向量库数据
- 日志、缓存、编辑器配置

## 上传到 GitHub

```powershell
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/kongtou-1/AI-Partner.git
git push -u origin main
```
