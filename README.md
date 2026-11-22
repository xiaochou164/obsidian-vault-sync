# Vault Sync for Obsidian

Vault Sync 的插件安装文件发布仓库，支持通过 BRAT 在桌面和移动端安装。
此仓库公开可读，安装插件不需要 GitHub Token。

当前版本 `1.0.0` 为预发布测试版。构建、TypeScript 检查及 50 项插件测试
已通过；iOS / iPadOS 真机验收仍待完成。本仓库仅分发插件端文件，
同步服务需要另行配置。

## 使用 BRAT 安装

1. 在 Obsidian 的“设置 → 第三方插件”中安装并启用 BRAT。
2. 在 BRAT 中保持 GitHub Token / 密钥选择为空。如果先前配置了错误的全局
   GitHub Token，也先取消该全局密钥选择，否则公开仓库请求仍可能返回 401。
3. 打开命令面板，执行 `BRAT: Add a beta plugin for testing`，填写：

   ```text
   xiaochou164/obsidian-vault-sync
   ```

4. 选择 `1.0.0` 测试版本并添加插件。
5. 在“第三方插件”中启用 `Vault Sync`。

[查看 Release 安装文件](https://github.com/xiaochou164/obsidian-vault-sync/releases/tag/1.0.0)

插件 ID 保持为 `obsidian-universal-sync`，安装目录与已有安装版一致。

## iPhone / iPad 首次配置

1. 创建一个本地空白库，关闭“存储到 iCloud”，删除新库生成的示例笔记。
   已有笔记的其他库继续保留。
2. 安装插件后，在“设置 → Vault Sync”填写：

   | 配置项 | 内容 |
   | --- | --- |
   | 服务地址 | 你自己的同步服务 HTTPS 根地址，例如 `https://sync.example.com` |
   | Vault ID | 同步服务分配的现有 Vault ID |
   | API Token | 授权此 Vault 的 Vault Sync API Token |

   服务地址不附加 `/mcp` 或 `/api/v1`。填写输入框后点击外侧保存。
3. 首次配置时先关闭“后台自动同步”，按需开启“同步附件”，保持
   “同步选定的 .obsidian 配置”为关闭。
4. 先确认原设备已上传需要的笔记，再执行 Vault Sync 的 `Test Connection`，
   通过后执行 `Sync Now`。空白本地库会从现有云端库初始化下载。
5. 保持 Obsidian 在前台，使用 `Open Sync Status` 查看进度。队列清空、
   状态为空闲且最近同步时间更新后，再做一次手机与桌面的双向编辑测试。
   验证后可开启后台自动同步。

GitHub Token 负责下载私有 GitHub 仓库的插件；本公开仓库不需要它。
Vault Sync API Token 负责访问同步服务，仍然需要配置。

每台设备应生成自己的设备身份、同步游标和本地队列。不要从其他设备
复制插件 `data.json` 或 `.universal-sync` 目录。

## 手动安装

从 Release 分别下载 `main.js` 和 `manifest.json`，放入：

```text
你的库/.obsidian/plugins/obsidian-universal-sync/
├── main.js
└── manifest.json
```

重启 Obsidian 后启用 `Vault Sync`。GitHub 自动生成的源码 ZIP 不是 BRAT
安装所需的 Release 附件。

## 常见问题

| 问题 | 处理 |
| --- | --- |
| BRAT 添加时出现 401 | 取消本次安装的密钥选择，并清空错误的全局 GitHub 密钥选择 |
| Vault Sync 连接出现 401 / 403 | 检查同步服务 API Token 的有效期、Vault 授权和读写权限 |
| BOOTSTRAP_REQUIRES_DECISION | 本地和云端已有文件；保留原库，另建空白本地库连接 |
| 503 / 网络不可用 | 网络恢复后使用原队列重试，保留本地暂存内容和操作 ID |
| 手机锁屏后同步停止 | 回到 Obsidian 前台，执行 `Sync Now` |

当前自动同步主要由启动和本地文件变更触发。接收另一设备的修改时，
可手动执行 `Sync Now`。

## 文件完整性

仓库及 Release 中的 `SHA256SUMS` 提供 `main.js`、`manifest.json` 的 SHA-256
校验值。公开附件不包含同步 Token、笔记或设备同步状态。

参考：[BRAT 安装指南](https://tfthacker.com/brat-quick-guide)。
