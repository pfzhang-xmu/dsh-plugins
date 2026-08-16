# DSH 插件集合

[English](./README.md)

这是一个面向 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 的插件集合仓库。

## 当前插件

| 插件 | 说明 |
| --- | --- |
| [`dsh-github-workspace`](./dsh-github-workspace) | 基于本机 GitHub CLI 的工作区，可在 DSH Web GUI 中浏览仓库并提交文本文件修改。 |
| [`dsh-image-tools`](./dsh-image-tools) | Host 端插件，为 DSH agent 注册 OpenAI Images 兼容的 `generate_image` / `edit_image` 工具，输出保存到项目目录。 |
| [`dsh-cowart`](./dsh-cowart) | 嵌入 DSH Web GUI 的 tldraw 无限画布，项目存储 + 常驻悬浮窗 + 「生成 → 标注 → 按标注精修」agent 工作流。 |

每个插件都位于独立目录中，并应提供安装、配置、安全边界和验证说明。后续新增插件请作为根目录下的同级目录添加，并同步更新本索引。

## 文档要求

每个插件必须同时提供以下两份文档：

- 英文文档：`README.md`
- 中文文档：`README.zh.md`

当插件的功能、安装方式、配置、安全边界或验证步骤发生变化时，两份文档必须同步更新。
