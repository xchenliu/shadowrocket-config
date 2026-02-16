# Shadowrocket 配置

> **sliu's config — Shadowrocket Edition** — 一份开箱即用的 Shadowrocket 配置，涵盖 DNS 分流、AI 服务隔离、流媒体/社交独立策略、广告拦截与电竞优化。

## 功能特性

### DNS 分流
- 国内站点（淘宝、京东、QQ、B站等）走阿里/腾讯 DNS（`223.5.5.5` / `119.29.29.29`）
- 海外站点（Google、AI 服务、社交平台）走 Google DNS（`8.8.8.8`）

### AI 服务独立路由
每个 AI 服务拥有独立策略组，互不干扰：

| 服务 | 策略组 | 默认地区 |
|------|--------|----------|
| ChatGPT / OpenAI | AI-US | 新加坡 |
| Claude / Anthropic | Claude | 新加坡 |
| Gemini | AI-US | 美国 |
| Grok | AI-US | 美国 |
| DeepSeek | AI-US | 新加坡 |
| Perplexity | AI-US | 新加坡 |
| Copilot | AI-US | 新加坡 |

### 流媒体 & 社交平台
每个服务独立策略组，可单独选择地区节点：
- **流媒体**：YouTube、Netflix、Spotify
- **社交**：Twitter/X、Facebook、Instagram、Reddit
- **通讯**：Telegram、WhatsApp、Discord
- **开发**：GitHub

### 广告拦截
- 远程规则列表：来自 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)
- 本地关键词拦截：远程列表失效时自动兜底

### 其他
- **Apple / Microsoft**：默认直连，可手动切换代理
- **电竞优化**：亚洲低延迟自动选择（港/日/新）
- **国内流量**：GeoIP + ChinaMax 规则直连
- **兜底规则**：未匹配流量走 Global 代理

---

## 策略组架构

```
Global（Auto / US / HK / JP / SG / TW / DIRECT）
  |
  |-- AI-US ........... ChatGPT、Gemini、Grok、DeepSeek、Perplexity、Copilot
  |-- AI-API .......... OpenAI API 调用
  |-- Claude .......... Claude / Anthropic
  |
  |-- YouTube ......... YouTube + CDN
  |-- Netflix ......... Netflix
  |-- Spotify ......... Spotify
  |-- Media ........... 其他海外流媒体
  |
  |-- Twitter ......... Twitter / X
  |-- Facebook ........ Facebook
  |-- Instagram ....... Instagram
  |-- Reddit .......... Reddit
  |-- Discord ......... Discord
  |-- GitHub .......... GitHub
  |
  |-- Telegram ........ Telegram
  |-- WhatsApp ........ WhatsApp + IP 段
  |-- Google .......... Google 服务
  |-- Apple ........... Apple（默认直连）
  |-- Microsoft ....... Microsoft（默认直连）
  |
  |-- Asia-LowLatency . 电竞低延迟（自动测速）
  |-- DIRECT .......... 国内直连
  |-- REJECT .......... 广告拦截 / 隐私追踪
  |-- Global .......... 兜底
```

---

## 导入教程

### 第一步：复制配置链接

复制以下远程配置 URL：

```
https://raw.githubusercontent.com/xchenliu/shadowrocket-config/master/Shadowrocket.conf
```

### 第二步：添加远程配置

打开 Shadowrocket → 底部点击 **配置** 标签 → 点击右上角 **+** → 粘贴上面的 URL → 点击 **下载**。

### 第三步：启用配置

下载完成后，在配置列表中点击刚下载的配置文件 → 选择 **使用配置**（配置前会出现黄色圆点表示已激活）。

### 第四步：添加订阅节点

回到 Shadowrocket **首页** → 点击右上角 **+** → 类型选择 **Subscribe** → 粘贴你的机场订阅链接 → 保存。

> **注意**：Shadowrocket 的节点和配置是分开管理的。配置文件只包含规则和策略组，节点需要单独通过订阅添加。添加后策略组会自动按正则匹配到对应地区的节点。

### 第五步：开启代理

回到首页，打开顶部的 **连接开关**，即可开始使用。

---

## 规则优先级

规则按以下顺序匹配（优先级从高到低）：

1. **广告拦截** — REJECT 规则最先匹配
2. **本地精确规则** — AI、WhatsApp、YouTube、社交平台防抢规则
3. **远程规则列表** — blackmatrix7 完整规则（RULE-SET）
4. **广告关键词备份** — 本地 DOMAIN-KEYWORD 兜底
5. **GeoIP CN** — 国内流量直连
6. **FINAL** — 未匹配流量走 Global 代理

---

## 与 Quantumult X 版本的区别

本配置与 [Quantumult X 版本](https://github.com/xchenliu/quantumultx-config) 规则完全一致，仅语法格式不同：

| 内容 | Quantumult X | Shadowrocket |
|------|-------------|--------------|
| 域名后缀 | `host-suffix` | `DOMAIN-SUFFIX` |
| 精确域名 | `host` | `DOMAIN` |
| 关键词 | `host-keyword` | `DOMAIN-KEYWORD` |
| 远程规则 | `[filter_remote]` + URL | `RULE-SET,URL,Policy` |
| 策略组 | `[policy]` | `[Proxy Group]` |
| 节点管理 | 写在配置文件内 | 单独通过订阅添加 |

---

## 致谢

- 规则列表：[blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)
- Quantumult X 版本：[xchenliu/quantumultx-config](https://github.com/xchenliu/quantumultx-config)

## 许可证

MIT
