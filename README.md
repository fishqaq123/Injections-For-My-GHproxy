
# Injections-For-My-GHproxy

这个仓库用于存放 [gh-proxy](https://github.com/hunshcn/gh-proxy) 的远程注入配置，通过 Cloudflare Workers 动态拉取（私用）。

## 📖 工作原理

1. Cloudflare Worker 接收首页请求
2. Worker 从本仓库拉取 `injections.json`
3. 将配置中的 HTML/CSS/JS 注入到 gh-proxy 页面的指定位置
4. 如果拉取失败，自动回退到内置的悬浮返回按钮

## 🗂️ 配置文件格式

`injections.json` 是唯一生效的配置文件。

```json
{
    "version": "1.0",
    "injections": [
        {
            "position": "afterBody",
            "html": "<!-- 你的注入内容 -->"
        }
    ]
}
```

### 字段说明

| 字段 | 类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| `version` | string | ✅ | 配置版本号，建议按日期递增 |
| `injections` | array | ✅ | 注入规则列表，按顺序执行 |
| `injections[].position` | string | ✅ | `afterBody` 或 `beforeHeadEnd` |
| `injections[].html` | string | ✅ | 要注入的 HTML 内容 |

### 注入位置说明

| 位置 | 含义 |
| :--- | :--- |
| `afterBody` | 在 `<body>` 标签之后插入 |
| `beforeHeadEnd` | 在 `</head>` 标签之前插入 |

## 📝 示例

```json
{
    "version": "1.0",
    "injections": [
        {
            "position": "afterBody",
            "html": "<style>body { background: #f6f8fa; }</style>"
        }
    ]
}
```

## 🔧 部署流程

1. **修改配置**：编辑 `injections.json` 并推送
2. **自动生效**：Cloudflare Worker 会在下次请求时自动拉取最新配置
3. **无需重新部署 Worker**

## ⚙️ 版本历史

- `2026-08-13`：初始版本，支持 `afterBody` 和 `beforeHeadEnd` 注入位置
