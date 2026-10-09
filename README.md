<h1 align="center">typecho chat</h1>

<p align="center">一款适用于 Typecho 的轻量聊天室插件。<br>文字与表情 · 图片与录音 · 深浅主题 · 离线 IP 地区</p>

<p align="center">
  <img src="https://img.shields.io/badge/Typecho%201.3.0-E54B4B?style=flat-square" alt="Typecho 1.3.0">
  <img src="https://img.shields.io/badge/PHP-777BB4?style=flat-square&amp;logo=php&amp;logoColor=white" alt="PHP">
  <img src="https://img.shields.io/badge/JavaScript-B89A00?style=flat-square&amp;logo=javascript&amp;logoColor=white" alt="JavaScript">
  <img src="https://img.shields.io/badge/CSS-663399?style=flat-square&amp;logo=css&amp;logoColor=white" alt="CSS">
  <br>
  <img src="https://img.shields.io/badge/MySQL%20%2F%20MariaDB-4479A1?style=flat-square&amp;logo=mysql&amp;logoColor=white" alt="MySQL / MariaDB">
  <img src="https://img.shields.io/badge/SQLite%20%2F%20PostgreSQL-336791?style=flat-square&amp;logo=postgresql&amp;logoColor=white" alt="SQLite / PostgreSQL">
  <img src="https://img.shields.io/badge/ip2region-24563D?style=flat-square" alt="ip2region">
  <img src="https://img.shields.io/badge/License%20MIT-222222?style=flat-square" alt="License MIT">
</p>

<p align="center">
  <a href="https://github.com/LLLimit/Typecho-chat/releases/latest">下载插件</a> ·
  <a href="#快速开始">快速开始</a> ·
  <a href="#使用说明">使用说明</a> ·
  <a href="https://github.com/LLLimit/Typecho-chat/releases">版本记录</a> ·
  <a href="https://github.com/LLLimit/Typecho-chat/issues">反馈问题</a>
</p>

---

访客可以查看聊天内容，登录用户可以发送文字、表情、图片和录制语音。插件保留最近 30 天消息，支持未读提醒与管理员清屏。

## 技术栈与兼容性

| 项目 | 说明 |
| --- | --- |
| 当前版本 | 1.3.3 |
| 博客平台 | Typecho 1.3.0 |
| 插件与前端 | PHP · JavaScript · CSS |
| 数据库 | MySQL / MariaDB · SQLite · PostgreSQL |
| 浏览器录音 | MediaRecorder · HTTPS · 麦克风权限 |
| 地区查询 | ip2region 离线 IPv4 / IPv6 数据库 |

## 功能亮点

| 功能 | 说明 |
| --- | --- |
| 💬 多种消息 | 登录用户发送文字、表情、图片和录音，访客可阅读 |
| 🌓 深浅主题 | 手动切换，支持跟随 Handsome 主题的初始外观 |
| 🎙️ 录音语音 | 录音试听后发送，消息提供语音播放条 |
| 🖼️ 用户头像 | 兼容 AdminBeautifyAvatar，回退到 Typecho Gravatar |
| 🌍 IP 地区 | 本地 ip2region 查询粗略归属地 |
| 🔔 消息提醒 | 新消息未读计数和提示音 |
| 🧹 消息清理 | 滚动保留最近 30 天，后台可手动清空消息及附件 |

## 快速开始

1. 从 [GitHub Releases](https://github.com/LLLimit/Typecho-chat/releases/latest) 下载插件压缩包。
2. 将压缩包里的 `TypechoChat` 文件夹放到 Typecho 的 `usr/plugins/` 下。
3. 在 Typecho 后台的“插件”页面启用 **typecho chat**。

当前仓库用于文档与版本发布，仓库自动生成的源码 ZIP 不包含插件程序。请使用 Release 附件中的插件包，并确认 `usr/plugins/TypechoChat/Plugin.php` 存在。主题模板需要执行 `$this->footer()` 来加载聊天室；Handsome 主题已支持。

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

## 插件结构与依赖

插件入口为 `Plugin.php`，前端资源为 `chat.js` 和 `chat.css`。`vendor/ip2region/` 包含 ip2region 的 PHP 查询器和离线 IPv4/IPv6 数据库，其上游许可为 Apache-2.0 或 MIT，许可文本随文件提供。

## 许可证

本项目主插件代码采用 [MIT License](LICENSE)。`vendor/ip2region/` 下的第三方文件不属于本项目许可证范围，请按该目录中附带的上游许可文件使用。

## 反馈与作者

欢迎通过 [Issues](https://github.com/LLLimit/Typecho-chat/issues) 反馈问题。作者：[LLLimit](https://github.com/LLLimit)。
