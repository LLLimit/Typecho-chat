# typecho chat

一款适用于 Typecho 的轻量聊天室插件。访客可以查看消息，登录用户可以发送文字、表情、图片和录制语音。

| 项目 | 信息 |
| --- | --- |
| 当前版本 | 1.3.3 |
| Typecho | 1.3.0 |
| 数据库 | MySQL / MariaDB、SQLite、PostgreSQL |
| 作者 | [LLLimit](https://github.com/LLLimit) |

## 功能

- 深色与浅色主题，可手动切换并跟随 Handsome 主题初始外观。
- 登录用户发送文字、内置表情、图片和录音语音；访客可阅读聊天内容。
- 录音可试听后发送，聊天消息提供语音播放条。
- 兼容 AdminBeautifyAvatar 的用户头像设置，并回退到 Typecho Gravatar。
- 使用随包提供的 ip2region 数据库解析 IP 粗略归属地。
- 新消息未读计数和提示音。
- 消息保留最近 30 天；后台可手动清空消息和附件。

## 安装

1. 从 GitHub Releases 下载插件压缩包。
2. 将压缩包里的 `TypechoChat` 文件夹放到 Typecho 的 `usr/plugins/` 下。
3. 在 Typecho 后台的“插件”页面启用 **typecho chat**。

如果从 GitHub 仓库下载源码 ZIP，请将解压出的目录重命名为 `TypechoChat` 后再放入 `usr/plugins/`。主题模板需要执行 `$this->footer()` 来加载聊天室；Handsome 主题已支持。

数据库账号需要有创建表的权限。首次启用或接口首次运行时，插件会按需创建地区、图片和语音元数据表。

## 使用说明

### 图片和语音

图片支持 JPG、PNG、WebP、GIF，单张最大 5 MB。录音最长 2 分钟、最大 8 MB。语音录制需要 HTTPS（`localhost` 除外）、浏览器麦克风权限以及浏览器的 MediaRecorder 支持。服务器 `upload_max_filesize` 与 `post_max_size` 应允许 8 MB 上传。

图片和语音保存在 `usr/uploads/anime-chat/YYYY/MM/`。管理员删除消息、自动过期清理或手动清屏时，会一并移除相关文件。

### 消息保留和清屏

插件按滚动 30 天保留消息。聊天室接口收到请求时会检查是否需要清理，每天最多清理一次；若网站没有聊天室访问，清理会等到下一次接口请求时执行。后台插件设置中的“手动清屏”会删除全部消息及图片、语音附件，且无法撤销。

### 头像和 IP 归属地

启用 [AdminBeautifyAvatar](https://github.com/lhl77/Typecho-Plugin-AdminBeautifyAvatar) 后，插件会使用它提供的头像地址；否则使用 Typecho Gravatar。聊天接口不返回用户邮箱。

IP 地区由本地 ip2region 数据库查询。插件只保存地区文字，不保存原始 IP，也不会将 IP 发送到在线查询服务。插件优先读取 `CF-Connecting-IP`，否则使用 `REMOTE_ADDR`。若站点使用反向代理，应限制源站只接受可信代理转发的请求，避免客户端伪造代理请求头。地区数据可能过期或无法识别某些地址。

## 开发与依赖

插件入口为 `Plugin.php`，前端资源为 `chat.js` 和 `chat.css`。`vendor/ip2region/` 包含 ip2region 的 PHP 查询器和离线 IPv4/IPv6 数据库，其上游许可为 Apache-2.0 或 MIT，许可文本随文件提供。

## 许可证

本项目主插件代码采用 [MIT License](LICENSE)。`vendor/ip2region/` 下的第三方文件不属于本项目许可证范围，请按该目录中附带的上游许可文件使用。

## 作者

[LLLimit](https://github.com/LLLimit)
