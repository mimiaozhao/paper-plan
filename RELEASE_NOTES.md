# Paper Plan 2.0.1 — 使用说明与全文显示修复

- 新增完整中文 [使用说明](https://github.com/mimiaozhao/paper-plan/blob/main/USAGE.md)，覆盖安装信任、计划讨论、目录编辑、全局要求、再修改、确认执行、停止、更新与排错。
- 修复全局目标和约束框高度过小的问题，自动按全文增高，显示目标字数；折叠展开和锁定状态下也可完整查看。
- 安装包和完整源码包均附带 `USAGE.md` 及测试示例截图。
- 延续 2.0.0 的目录编辑、正式计划同步、精确版本执行和再修改功能；已有计划数据格式不变。

下载 `paper-plan-2.0.1-windows.zip`，完整解压后运行 `Install.cmd`。需要 Windows Codex 和 Python 3.11+，首次安装依赖需要联网；首次安装需在 Codex 中审阅并信任钩子。

从旧版更新：先结束任务、停止插件服务并备份 `data`，将新包内容覆盖到原安装目录，保留 `data` 和 `.venv`，再运行 `Install.cmd`。
