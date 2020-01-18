---
home: true
heroImage: /koishi.png
heroText: Koishi
tagline: 不仅仅是一个 QQ 机器人
actionText: 快速上手 →
actionLink: /guide/getting-started
features:
- title: 开箱即用
  details: 高度便利的 CLI 和 API 可以让你无需基础在几分钟之内搭建自己的机器人。
- title: 功能强大
  details: 监听器，发送器，中间件，指令系统，插件系统，数据库，多机器人……它们可以让你顺利实现任何需求。
- title: 生态丰富
  details: 官方提供了大量插件和解决方案，在满足各种需求的同时，也为编写插件提供了绝佳的范例。
footer: MIT Licensed | Copyright © 2019-present Shigma
---

```sh
# 进入文件夹
cd my-bot

# 安装 Koishi
npm i koishi -g

# 初始化配置文件
koishi init

# 运行你的 Bot
koishi run
```

现在可以对你的机器人说话了：

<chat-panel>
<chat-message nickname="Alice" color="#cc0066">echo 你好</chat-message>
<chat-message nickname="Koishi" avatar="/koishi.png">你好</chat-message>
</chat-panel>

::: tip 提示
Koishi 要求您的 Node.js 的版本不小于 10，CQHTTP 的版本不小于 3.0。如果使用 WebSocket，Koishi 要求 CQHTTP 版本不小于 4.6。
:::
