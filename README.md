# Kimi API 逆向研究手册

Kimi Android 客户端 API 静态逆向工程成果。覆盖 59 个 proto 服务、212 个 RPC 方法、609 个 message。

## 数据来源

- 目标：Kimi App Android
- 反编译：APKTool M（smali）
- 方法：Connect-RPC 字符串常量提取 + 启发式类型还原

## 文件导航

| 文件 | 内容 |
|------|------|
| index.html | 总览、协议架构、导航入口 |
| methodology.html | 研究方法论、工具链、调用方式、验证方法 |
| business-map.html | 59 服务按业务域归类 |
| services.html | 服务详解、方法列表 |
| fields.html | 字段级调用详解 |
| style.css | 全站样式 |

## 协议概览

| 协议 | 路径 | 用途 |
|------|------|------|
| REST | /api/* | 账户、设备、文件等外围能力 |
| Connect-RPC | /apiv2/* | 聊天、社区、IM、会员等全部核心业务 |

核心业务全部走 Connect-RPC，Content-Type 为 application/connect+json，纯 JSON 无二进制 protobuf。

## 调用示例

curl -X POST https://www.kimi.com/apiv2/kimi.gateway.chat.v1.ChatService/ListChats \
  -H "Content-Type: application/connect+json" \
  -H "Authorization: Bearer $TOKEN" \
  -H "x-msh-platform: android" \
  -d '{"pageSize": 20}'

流式方法（Chat / SubscribeMessageStream / ResumeChat）响应为按行分隔的 JSON 事件流，靠递增 eventOffset 断点续传。

## 工具链

- 反编译：APKTool M / jadx
- 提取：grep -rhoE / Python 正则
- 还原：R8 mapping.txt + 混淆类清单
- 抓包：mitmproxy / Charles
- Hook：Frida

## 贡献

欢迎 PR 修正字段类型、补充服务定义。请先阅读 methodology.html 了解提取与还原方法。

## 免责声明

本项目仅用于技术研究与学习，不得用于违反服务条款或法律法规的用途。所有定义来自公开反编译分析，不含用户数据或私密凭证。

## 协议

MIT
