# 基金实时技术分析面板

一个**单文件**的基金看板：净值曲线 / K 线、MACD·KDJ·RSI、综合评分与白话建议、定投回测、重仓股联动、板块资金流。

没有构建步骤、没有依赖、没有后端 —— 只有 `index.html` 一个文件，浏览器直接打开就能跑。

## 手机上怎么用

部署到 GitHub Pages 后，用手机浏览器打开链接，然后：

- **iPhone Safari**：分享 → 「添加到主屏幕」
- **Android Chrome**：右上角菜单 → 「添加到主屏幕」

加完之后是全屏的 App 样式（页面已带 `apple-mobile-web-app-*` 和 `theme-color`）。

> 手机端是适配过的：手机上指数卡两列、图表自适应宽度、表格横向滑动，页面本身不会横向溢出。

## 部署到 GitHub Pages（3 分钟）

### 方式一：网页上传（不用装任何工具）

1. 在 GitHub 新建一个仓库，比如 `fund-panel`，可见性选 **Public**
2. 进入仓库 → `Add file` → `Upload files` → 把本目录的 `index.html` 和 `README.md` 拖进去 → Commit
3. 仓库 → `Settings` → 左侧 `Pages` → **Source** 选 `Deploy from a branch`，Branch 选 `main`，目录选 `/ (root)` → Save
4. 等 1-2 分钟，访问 `https://<你的用户名>.github.io/fund-panel/`

### 方式二：命令行

```bash
cd 基金面板-github
git init
git add .
git commit -m "fund panel"
git branch -M main
git remote add origin https://github.com/<你的用户名>/fund-panel.git
git push -u origin main
```

然后在仓库 Settings → Pages 里把 Source 设成 `main` / `/ (root)`。

## 数据来源

| 内容 | 来源 |
|---|---|
| 净值 / 阶段涨幅 / 全市场排行 | 东方财富数据中心 `RPT_FUND_RANK` 报表 |
| 历史净值、规模、经理、费率、重仓股代码 | 天天基金 `pingzhongdata` |
| 基金搜索 | 天天基金 `fundsuggest`（回调式 JSONP） |
| 场内 ETF/LOF 行情、K 线、分时 | 腾讯财经 |
| 板块资金流 | 东方财富数据中心 `RPT_FUNDFLOW_BOARD`（概念板块） |

页面不产生任何模拟数据：接口失败就留空并在顶部标「异常」。

## 几个必须知道的口径

- **场外基金没有盘中实时净值**（监管后第三方估值已停供）。页面的「分时」在场外基金上是用**前十大重仓股等权估算**的，会明确标注「估算」，不是净值。
- **技术分析默认用复权净值**。单位净值在分红/份额折算日会跳空，直接算波动率会得到翻倍的假数据；但止损/目标这些数字会自动换算回单位净值，和你 App 里看到的一致。
- **重仓股没有权重**：能给出占净值比的两个接口（网页版校验 Referer、移动端按 UA 限流）在浏览器里都拿不到，所以只能做等权方向判断。
- 所有指标与建议都是按固定规则算的，**不构成投资建议**。

## 常见问题

**顶部显示某个数据源「异常」？**
把鼠标停在状态点上会列出具体接口。多半是网络波动或临时限流，点「立即刷新」重试即可。

**GitHub Pages 打开很慢或打不开？**
`github.io` 在国内访问不稳定。可以让页面托管到 Cloudflare Pages / Vercel（同样是静态托管，免费），或者先在电脑上跑一个本地服务，手机连同一个 Wi-Fi 访问。

**想改成自己的默认基金？**
在 `index.html` 里搜索 `DEFAULT_FUND`，把 `'161725'` 换成你想要的 6 位基金代码即可；`QUICK_FUNDS` 是快捷入口列表。
