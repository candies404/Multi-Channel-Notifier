# Notify 通知推送使用文档

## 一、功能概述

notify.py 是一个多平台消息推送工具，支持多种推送渠道，包括钉钉机器人、企业微信、Telegram、SMTP邮件等。

## 二、支持的推送渠道

1. Bark
2. 控制台输出
3. 钉钉机器人
4. 飞书机器人
5. Go-CQHTTP
6. Gotify
7. iGot
8. Server酱
9. PushDeer
10. Chat
11. PushPlus
12. 微加机器人
13. Qmsg
14. 企业微信应用
15. 企业微信机器人
16. Telegram机器人
17. 智能微秘书
18. SMTP邮件
19. PushMe
20. Chronocat
21. WPUSH
22. 自定义通知

## 三、基本使用方法

### 基础示例

```yaml
name: 发送通知
on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: 发送通知
        uses: candies404/Multi-Channel-Notifier@latest
        with:
          title: '构建通知'
          content: '项目有新的提交！'
          # Telegram 配置示例
          tg_bot_token: ${{ secrets.TG_BOT_TOKEN }}
          tg_user_id: ${{ secrets.TG_USER_ID }}
          # 钉钉机器人配置示例
          dd_bot_token: ${{ secrets.DD_BOT_TOKEN }}
          dd_bot_secret: ${{ secrets.DD_BOT_SECRET }}
```

### 多渠道推送示例

```yaml
name: 多渠道通知
on:
  release:
    types: [published]

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: 发送发布通知
        uses: candies404/Multi-Channel-Notifier@latest
        with:
          title: '新版本发布通知'
          content: |
            🎉 版本 ${{ github.event.release.tag_name }} 已发布！

            更新内容：
            ${{ github.event.release.body }}

          # 企业微信应用
          qywx_am: ${{ secrets.QYWX_AM }}

          # Telegram
          tg_bot_token: ${{ secrets.TG_BOT_TOKEN }}
          tg_user_id: ${{ secrets.TG_USER_ID }}

          # 钉钉
          dd_bot_token: ${{ secrets.DD_BOT_TOKEN }}
          dd_bot_secret: ${{ secrets.DD_BOT_SECRET }}
          dd_msg_type: 'markdown'

          # 飞书
          fskey: ${{ secrets.FSKEY }}

          # 是否启用一言
          hitokoto: 'true'
```

## 四、主要推送渠道配置说明

### 1. 钉钉机器人

#### 必需配置
```
DD_BOT_SECRET=your_secret_here
DD_BOT_TOKEN=your_token_here
```

#### 可选配置

```
DD_MSG_TYPE=text  # 消息类型：text/markdown/actionCard
```
### 2. Telegram机器人

#### 必需配置

```
TG_BOT_TOKEN=your_bot_token
TG_USER_ID=your_user_id
```

#### 可选配置
```
TG_API_HOST=api_host  # 自定义API
TG_PROXY_HOST=proxy_host  # 代理服务器
TG_PROXY_PORT=proxy_port  # 代理端口
TG_PROXY_AUTH=proxy_auth  # 代理认证
```
### 3. SMTP邮件

#### 必需配置
```
SMTP_SERVER=smtp_server
SMTP_SSL=true/false
SMTP_EMAIL=your_email
SMTP_PASSWORD=your_password
SMTP_NAME=sender_name
```
### 4. WPUSH

#### 必需配置

```WPUSH_KEY=your_api_key```

#### 可选配置 
推送通道

```WPUSH_CHANNEL=wechat```  

## 五、高级功能

### 1. 一言功能

启用/禁用一言功能（在消息末尾添加随机句子）

```HITOKOTO=true/false```

### 2. 跳过特定标题的推送

使用换行分隔多个标题
```
SKIP_PUSH_TITLE=标题1
标题2
标题3
```

### 3. 自定义通知

#### 必需配置
```
WEBHOOK_URL=your_webhook_url
WEBHOOK_METHOD=POST/GET
```
#### 可选配置
```
WEBHOOK_CONTENT_TYPE=content_type

WEBHOOK_BODY=request_body

WEBHOOK_HEADERS=request_headers
```

## 六、注意事项

1. 环境变量可以通过系统设置或在运行时通过参数传入
2. 不同推送渠道的配置项互相独立
3. 可以同时启用多个推送渠道
4. 推送失败会有相应的错误提示
5. 建议在正式使用前进行测试



## 七、常见问题

1. 推送失败：检查配置参数是否正确

2. 环境变量无效：检查变量名称和格式

3. 消息格式错误：参考对应渠道的消息格式要求
