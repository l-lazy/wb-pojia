# wb-pojia

WorkBuddy 配置打包 / 还原工具（Windows）。把 `~/.workbuddy` 下的注入点文件一次性备份、应用与还原。

## 下载

| 文件 | 大小 | 说明 |
|---|---|---|
| [`wb-pojia.zip`](./wb-pojia.zip) | 2,641,711 字节 | 完整工具包（Release 里也有同名附件） |

直链：

```
https://github.com/l-lazy/wb-pojia/raw/main/wb-pojia.zip
```

## 校验

```
SHA256  c70a1e849f5afb52d461e39e4952a5b210fd0a5cbe9a5cb54b17bc02b7788ddf
```

本地校验：

```bash
# Linux / macOS / Git Bash
sha256sum wb-pojia.zip
# 期望输出
# c70a1e849f5afb52d461e39e4952a5b210fd0a5cbe9a5cb54b17bc02b7788ddf  wb-pojia.zip
```

```powershell
# Windows PowerShell
Get-FileHash .\wb-pojia.zip -Algorithm SHA256
```

## 包内结构

```
wb-pojia/
├── 一键启动.bat        应用全部注入（双击）
├── 查看状态.bat        查看各通道状态
├── 还原.bat            从 backup\ 还原所有改动文件
├── 停止.bat            撤销 settings.json 的 hooks 持久注入
├── 检测.bat            诊断 + 修复，定位失败环节
├── 使用说明.txt        详细文档
├── .state.json         当前状态
├── lib/
│   └── engine.mjs      引擎（Node 脚本）
├── payload/
│   ├── contract.md     主契约模板
│   ├── soul.md         身份契约
│   └── skill-entry.md  技能入口（验证词握手 + 任务路由）
└── backup/             应用前自动备份的原文件（用于还原，31 个 .bak）
```

## 使用

1. 解压到任意目录（路径不要含空格以外的特殊字符）。
2. 双击 `一键启动.bat`，看到全部通道 `✓` 即完成。
3. 完全退出并重新打开 WorkBuddy。
4. 在对话框单独发送 `CR` 验证。

失败时运行 `检测.bat` → 选 `[1]` 完整诊断。

## 还原

双击 `还原.bat`：从 `backup\` 恢复全部被改文件，并剥离文本里的覆盖层。
双击 `停止.bat`：撤销 `settings.json` 中的 hooks 持久注入。

## 依赖

- Windows 10/11
- Node.js >= 18（脚本会自动探测 `.workbuddy` 自带 node 或 PATH 中的 node）

## 目录（8 类注入通道）

```
wb-cache            ~/.workbuddy/cache/acc-product-config-v3-*.json → system prompt
wb-codebuddy-home   ~/.workbuddy/CODEBUDDY.md
wb-lskill-rules     ~/.workbuddy/rules/*.mdc
wb-plugins-tpl      各插件 prompt.tpl
wb-memory           ~/.workbuddy/memory/*_memory.md
wb-soul              ~/.workbuddy/user-*-personal/SOUL.md
skill               ~/.workbuddy/skills/cr-reverse-skill/SKILL.md
wb-hooks            ~/.workbuddy/settings.json（持久自动注入）
```

## 说明

`backup/` 内为打包时本机 `~/.workbuddy` 的原始文件快照，含本机用户目录路径。
`还原.bat` 只按文件名映射回原路径，不依赖路径前缀。
