# 论文式计划 / Paper Plan

在 Windows Codex 中，用可编辑的多级目录整理计划，再按确认版本交回原对话执行。

**[完整中文使用说明](USAGE.md)** · [下载最新版本](https://github.com/mimiaozhao/paper-plan/releases/latest)

2.0.1 新增图文说明，并修复全局目标和约束只能看到少量文字的问题，展开后可直接查看全文。

发行文件：[Windows 安装包](https://github.com/mimiaozhao/paper-plan/releases/download/v2.0.1/paper-plan-2.0.1-windows.zip) · [完整源码包](https://github.com/mimiaozhao/paper-plan/releases/download/v2.0.1/paper-plan-2.0.1-source.zip) · [SHA-256 校验](https://github.com/mimiaozhao/paper-plan/releases/download/v2.0.1/SHA256SUMS.txt)

本仓库以发行归档提供安装包和完整开发源码。下载后请先阅读 [使用说明](USAGE.md)。

## 安装

1. 从 [Releases](https://github.com/mimiaozhao/paper-plan/releases) 下载 `paper-plan-2.0.1-windows.zip`。
2. **完整解压到固定文件夹**，例如 `C:\Tools\paper-plan`，不要直接在压缩包内运行。
3. 双击 `Install.cmd`。安装器查找 Codex 自带或系统的 Python 3.11+，创建独立环境，并联网安装依赖。界面已经编译，无需 Node.js。
4. 在 Codex 中审阅并信任 `UserPromptSubmit`、`Stop`、`Interrupt` 三项钩子。这是首次安装的宿主要求，安装器不会代替你信任。
5. 如当前窗口尚未加载插件，重新加载插件或重启 Codex，再打开本地对话。

要求：Windows 10/11、支持插件/Hooks/原对话消息队列的 Codex、Python 3.11+。未发现 Python 时安装器会提示安装；首次安装依赖需要联网。本版以 Windows/Python 3.12 验证，不宣称支持 macOS、Linux 或云端对话。

安装后保留解压目录，不要移动或删除；它存放服务、运行环境和计划数据库。后续更新在同一目录完成。

## 使用

- 在 Codex **计划模式**正常讨论、回答问题；只有最终完整计划才出现在该对话右侧。
- 拖动标题调整顺序与层级，也可直接输入标题级别；展开详情编辑要求、产出和验收条件。
- 自动保存保持输入焦点，支持撤销、重做及恢复未保存草稿。
- 点击“确认计划”，手动切回执行模式，再点“开始执行”。结果按同一目录逐项回写。
- 想重新讨论，点击“再修改”，填写重点，手动切到计划模式后发送提问。Codex 先提问，明确需求后再生成完整新计划。

## 数据与保护

计划按对话隔离，保存在安装目录的 `data/plans.sqlite3`。安装包不包含作者的数据库、聊天记录、账号凭证、日志或配置备份。

执行绑定确认版本，排队和执行期间内容锁定。再修改问答时可以继续编辑，版本发生冲突则将新结果作为候选保留。未领取请求可以取消；执行中使用 Codex 原生停止按钮。

模式切换仍需用户手动操作。插件不会通过提示词声称改变宿主模式，也不会在独立模型会话中代替原对话执行。

## 更新与停用

更新前先结束任务、停止插件服务，再备份 `data` 文件夹；将新版本内容覆盖到原安装目录，运行 `Install.cmd`。包内没有 `data` 和 `.venv`，更新不会主动删除它们。钩子定义变化时需要重新审阅，定义不变则保留原信任。

在 Codex 插件管理中禁用“论文式计划”即可停用。需要停止本地服务时运行 `stop.ps1`。其他插件设置会保留。

## 常见问题

- **面板空白**：确认插件已启用、钩子已信任，且本轮生成了正式计划；宿主的原生计划卡片也受支持。
- **Python 未找到**：安装 Python 3.11+，或使用 `install-plugin.ps1 -PythonExecutable "解释器路径"`。
- **依赖下载失败**：检查 PyPI 网络访问后重新运行安装器。
- **投递失败**：服务必须由桌面用户启动，不能从 `CodexSandbox*` 隔离账户启动。可在面板取消请求或复制指令回原对话。
- **端口冲突**：默认使用本机 `127.0.0.1:8765`，不要同时运行多个安装目录的服务；先停止旧安装的服务。
- **仍是旧界面**：等保存完成后刷新右侧面板。

## 源码开发

源码不包含作者的本地运行环境。开发环境需要 Python 3.11+、Node.js 20.19+ 和 pnpm。

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r backend/requirements-bridge.txt -r backend/requirements-dev.txt
cd frontend
pnpm install --frozen-lockfile
pnpm build
cd ..
.\.venv\Scripts\python.exe scripts/install_paper_plugin.py --prepare-only
.\.venv\Scripts\python.exe -m pytest backend/tests qa/tests
.\.venv\Scripts\python.exe scripts/build_release.py --output releases
```

机制依据：[OpenAI 插件文档](https://developers.openai.com/plugins/build/plugins)、[Hooks 文档](https://learn.chatgpt.com/docs/hooks)。本项目是个人插件，与 OpenAI 官方产品无隶属关系。
