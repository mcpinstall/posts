# RedNote-MCP：超好用的小红书内容访问 MCP 服务！

![cover_image](https://mmbiz.qpic.cn/mmbiz_jpg/n7yC6hUO441ekeRPweBYDlIgAkqLvRTwJOYEBt1oRswwzicAGlUAhyxpkywIEBbnSvqiasGbCrySNGgfibzOnMzqw/0?wx_fmt=jpeg)

# RedNote-MCP：超好用的小红书内容访问 MCP 服务！

Original 司木源 [MCP Server]

_2025 年 04 月 16 日 12:47_

大家应该还记得年初 1 月份的时候，TikTok 遭老美封禁，大量的国外网友疯狂涌入小红书，让小红书借力大火了一把。

![Image](https://mmbiz.qpic.cn/mmbiz_png/n7yC6hUO441ekeRPweBYDlIgAkqLvRTw1l4SXWwCSWpWyzybTwxFnnI5vV2dOhPtfPQibZhs6w5RheOECdr6BVw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

小红书在内容分享领域的爆红，让我们越来越多的内容创作人员把目光从其他自媒体平台迁移到了小红书。

所以，一项需求就产生了，尤其是企业用户，希望有一种方法能够更方便高效地获取和分析小红书上的内容。

![Image](https://mmbiz.qpic.cn/mmbiz_png/n7yC6hUO441ekeRPweBYDlIgAkqLvRTw56Yyp2YdM7EvRWbCpP4FQ97N8WJbI9e7Ruxh6Q8YWCGlvDrHibVskRg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

但是呢，直接访问小红书的内容也是限制多多，那么今天就给大家推荐一款好用的工具-RedNote-MCP。

RedNote-MCP 是一个专门为小红书内容访问设计的 MCP（Model Context Protocol）服务器。

![Image](https://mmbiz.qpic.cn/mmbiz_png/n7yC6hUO441ekeRPweBYDlIgAkqLvRTwNQ0DJuJwa6xtDjtibcicicKR1VRp2RWvtWKT6OkLsxsMwvKMmYQNHZ1tA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

它能够让我们内容创作者更方便，快捷的获取小红书笔记和评论内容，同时它还支持 Cookie 持久化和命令行工具，帮助我们开发人员快速集成和使用。

RedNote-MCP 的核心功能是通过 MCP 协议访问小红书的内容，RedNote-MCP 支持 Cookie 持久化，我们用户只需完成一次登录操作，就可以自动保存 Cookie 到本地文件中，后续使用不需要再重复登录。

![Image](https://mmbiz.qpic.cn/mmbiz_png/n7yC6hUO441ekeRPweBYDlIgAkqLvRTwQLwPZoDwjUqEKFlmSBC9exf6ibfiaHXhiax5eELbhFywD1mkGAUvOjMqQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

支持关键词搜索笔记，用户可以通过关键词搜索小红书上的笔记内容，方便快速定位目标内容。

支持命令行初始化工具，RedNote-MCP 提供了一个命令行工具，帮助用户快速完成初始化登录和配置。

通过 URL 访问笔记内容，用户可以通过小红书笔记的 URL 直接获取笔记内容，用户还可以通过笔记的 URL 获取评论内容，方便分析用户互动。


开始前确保安装了 playwright 环境：

```
npx playwright install
```

NPM 全局安装：

```
# 全局安装
```

从源码安装：

```
# 克隆项目
```

使用

## 初始化登录，首次使用需要先进行登录初始化：

```
rednote-mcp init
```

执行此命令后：

1.  会自动打开浏览器窗口；
2.  跳转到小红书登录页面；
3.  请手动完成登录操作；
4.  登录成功后会自动保存 Cookie 到 文件`~/.mcp/rednote/cookies.json       `

### 在 Cursor 中配置 MCP Server，在 Cursor 的 settings.json 中添加以下配置：

```
{
```

或者使用 npx 方式：

```
{
```

环境要求

- Node.js >= 16；

- npm >= 7；

### 开发流程

```
# 安装依赖
```

### 使用 MCP Inspector 进行调试 MCP，Inspector 是一个用于调试 MCP 服务器的工具，可以帮助我们检查和验证 MCP 服务器的状态。使用以下命令启动：

```
npx @modelcontextprotocol/inspector npx rednote-mcp --stdio
```

这个命令能够完成下面操作：

1.  启动 MCP Inspector 工具；
2.  通过 Inspector 运行 rednote-mcp 服务；
3.  提供一个交互式界面来检查请求和响应；
4.  帮助调试和验证 MCP 协议的实现；

以上就 RedNote-MCP 配置的全部内容，有任何问题欢迎大家评论区留言讨论！

相关学习链接：https://github.com/iFurySt/RedNote-MCP

友情提示：以上内容均为作者本人学习分享，旨在与大家学习交流，不代表任何官方平台观点（仅供学习参考），不构成任何工作建议、指导，请大家谨慎评估技术可行性后再做决策，感谢您对技术探索精神的理解与支持!
