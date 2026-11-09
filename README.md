# 科研 AI 工具包

一句话用途：把组会要用的科研 AI Skill、软件 MCP 和绘图参考打成一页清单，扫码打开、复制提示词、交给自己的智能体去装。

- 二维码指向本页：**https://github.com/undyne13b/research-ai-toolkit**
- 本页内容对应的核对日期：**2026-11-10**
- 本包**不含任何自动安装脚本**，也不要求你输入任何密码
- 国内用浏览器打开 `github.com` 一般没问题（偏慢），但 `raw.githubusercontent.com` 常被墙。所以第 2 步让智能体先把仓库 `git clone` 到本地再读文件，不依赖 raw 链接。

---

## 一、怎么用（3 步）

### 第 1 步：扫码

扫组会 PPT 上的二维码，会打开本页。打不开就用这个链接：https://github.com/undyne13b/research-ai-toolkit

### 第 2 步：复制下面整段提示词

```text
请帮我安装「科研 AI 工具包」。仓库地址：
https://github.com/undyne13b/research-ai-toolkit

先把整个仓库拉到本地，再读里面的文件（不要用 raw.githubusercontent.com 的链接，国内常不通）：
git clone https://github.com/undyne13b/research-ai-toolkit.git
没有 git 或者 clone 失败，就直接打开上面那个仓库页，读 README.md、INSTALL_PROMPT.md、manifest.json 三个文件的全文。

严格按 INSTALL_PROMPT.md 执行：先读每个仓库的 README 和 LICENSE，列出安装计划等我确认；不要删除或覆盖我已有的 Skill 和 MCP 配置；不要把任何密码、API key、推送凭据写进文件或命令行；只装清单里的项目；每装完一项做一次最小测试，最后用表格汇报名称、是否成功、失败原因、下一步。
```

### 第 3 步：发给自己的智能体

把上面整段发给你的智能体（Claude Code、Codex、Cursor 等），它会先列安装计划，**你确认后**才开始装。

> 注意：`git clone` 走的是 `github.com`，国内能连上，只是慢（本包只有几十 KB，几秒就好）。如果智能体所在的机器完全上不了 GitHub，就用手动方式：打开仓库页复制 `INSTALL_PROMPT.md` 和 `manifest.json` 的全文粘贴给它。

---

## 二、清单

### 2.1 Skill（装进智能体的技能目录）

| 名称 | 链接 | 一句话用途 | 许可证 | 密钥 |
|---|---|---|---|---|
| bioinformatics-god-skill | https://github.com/zwbao/bioinformatics-god-skill | 用 50 位生信学者的思维框架审查分析方案、工具选择和结论 | MIT | 不需要 |
| scientific-agent-skills（只装 3 个） | https://github.com/K-Dense-AI/scientific-agent-skills | 只装 `scientific-visualization`、`scientific-critical-thinking`、`cellprofiler`：投稿级绘图、证据批判、显微图像分割 | MIT | 不需要 |
| nature-skills（先装 1 个） | https://github.com/Yuan1z0825/nature-skills | 先装 `nature-figure`：按 Nature 系排版规范出图；其余按需 | Apache-2.0 | 不需要 |
| anti-defensive-writing-skill（中文版） | https://github.com/Adkid-Zephyr/anti-defensive-writing-Skill | 改掉论文里「遗憾的是」「效果有限」这类防御性写法 | MIT | 不需要 |
| research-paper-daily-push | https://github.com/caofan85-lab/research-paper-daily-push | 每天检索并中文总结新论文，可推到微信 | MIT | **需要**（推送凭据自己填） |

四个 Skill 的安装范围（不要多装）：

- `scientific-agent-skills` 仓库里有 **177** 个技能，本包只装名单里的 3 个。
- `nature-skills` 有 24 个技能，本包只装 `nature-figure`，但**必须**同时装 `nature-shared`（它是被其他技能读取的共享包）。
- `anti-defensive-writing-skill` 有中英两版，本包装中文版 `skills/anti-defensive-writing`。
- `research-paper-daily-push` 需要 Python 3.10+，推送用 WxPusher / Server酱 / 企业微信机器人，**凭据由你自己填**，不要发给任何人。

### 2.2 软件 MCP（本机要先装好对应软件）

| 名称 | 链接 | 一句话用途 | 许可证 | 前置软件 |
|---|---|---|---|---|
| illustrator-mcp-server | https://github.com/ie3jp/illustrator-mcp-server | 让智能体直接读改导出 Illustrator 设计稿（66 个工具） | MIT | Adobe Illustrator CC2024+、Node.js 20+ |
| Fiji_imageJ_mcp | https://github.com/surajinacademia/Fiji_imageJ_mcp | 让智能体打开/保存图像、跑 ImageJ 命令、执行 IJM/Groovy 脚本 | BSD-3-Clause | 本机 Fiji、Python 3.10+ |
| scientific-illustrator | https://github.com/icebird1998/scientific-illustrator | 把参考图在 PowerPoint / WPS / draw.io 里重绘成**可编辑对象** | MIT | PowerPoint 或 WPS 演示或 draw.io Desktop |

**Fiji 两个仓库二选一的比较**（你原来的清单里有 `SteffenPL/fiji-mcp` 和 `surajinacademia/Fiji_imageJ_mcp`）：

| | SteffenPL/fiji-mcp | surajinacademia/Fiji_imageJ_mcp |
|---|---|---|
| 许可证 | MIT | BSD-3-Clause |
| 安装 | `git clone` + `uv sync`，然后**往你的 Fiji 里写入一个 Java 桥接插件 JAR** | 一条 `pip install fiji-mcp-server==0.2.0`，**不改动 Fiji 安装** |
| 通信方式 | MCP ↔ WebSocket ↔ Fiji 插件；能自动拉起 Fiji | MCP ↔ PyImageJ；默认 headless |
| 独有能力 | 能记录你在 Fiji GUI 里的操作，并导出成 ImageJ macro | 9 个聚焦工具，截图核验结果 |
| 前置 | Fiji(Java 8+)、Python 3.11+、uv | Fiji、Python 3.10+ |
| 最后提交 | 2026-05-27 | 2026-08-27 |
| Star | 9 | 6 |

**推荐 `Fiji_imageJ_mcp`**：它不改动你的 Fiji 安装，只往 Python 环境装一个包，出问题能干净卸掉；如果你要的正是「把我手点 Fiji 的过程记下来变成 macro」，才选 `SteffenPL/fiji-mcp`。

> 两个都是个人小项目（star 都不到 10），建议先在非关键数据上试。

**关于 `drawio-scientific-illustrator`**：你原清单里列的是 `icebird1998/drawio-scientific-illustrator`，但作者已在该仓库顶部声明**项目已迁移、停止更新**，继任仓库是 `icebird1998/scientific-illustrator`（能力是其超集：PowerPoint / WPS / draw.io 三个都能控制）。本包用继任仓库。

### 2.3 参考资料（不是 Skill，不需要「安装」）

| 名称 | 链接 | 一句话用途 | 许可证 |
|---|---|---|---|
| scientific-visualization-book | https://github.com/rougier/scientific-visualization-book | 免费的 Matplotlib 科学绘图教材 + 全部示例代码 | 代码 BSD-2-Clause；书 CC BY-NC-SA 4.0 |
| SciencePlots | https://github.com/garrettj403/SciencePlots | `pip install SciencePlots`，一行切换成 IEEE/Nature 等期刊样式 | MIT |
| ggsci | https://github.com/nanxstats/ggsci | R 里 ggplot2 的期刊配色（NPG、Lancet、JAMA 等） | GPL-3.0 |
| figures4papers | https://github.com/ChenLiu-1996/figures4papers | 作者发在 Nature MI / ICML 等论文的绘图脚本，可直接改 | **CC BY-NC 4.0（禁止商用）** |

---

## 三、许可证提醒

| 项目 | 限制 |
|---|---|
| figures4papers | CC BY-NC 4.0：**只能非商业用途**。论文里用它的代码出图没问题，但不能用于商业产品或收费服务。 |
| scientific-visualization-book | 书的正文是 CC BY-NC-SA 4.0（非商业 + 相同方式共享），代码是 BSD-2-Clause。**书里的图和正文不要直接搬进商业稿件**。 |
| ggsci | GPL-3.0：修改分发时要注意传染性。正常 `install.packages()` 使用不受影响。 |
| SciencePlots | MIT，但 latex 系列样式要求本机装了 LaTeX。 |

---

## 四、二维码

- 二维码只编码本仓库链接，不编码提示词正文。
- 规格：纠错等级 M、四周留白 4 个模块、黑码白底、PNG 1230×1230 像素（33 模块 × 30px + 4 模块留白 × 2）。
- 文件：`qr.png`、`qr.svg`（都在本仓库根目录）。
- **建议放置尺寸**：16:9 幻灯片上，二维码边长**不小于幻灯片高度的 20%**（13.33×7.5 英寸的画布上，边长 ≥ 1.5 英寸 / ≈ 3.8 cm），四周再留出二维码自身宽度约 1/8 的空白。投影仪亮度低或教室大时，放大到高度的 25%～30% 更稳。

---

## 五、核对记录

核对日期 **2026-11-10**。逐个用 GitHub API 确认了存在性、`archived` 状态、许可证、最后提交时间，并读了每个仓库的 README 与 LICENSE。

发现的问题：

1. **`icebird1998/drawio-scientific-illustrator` 已被作者标记迁移**，后续开发在 `icebird1998/scientific-illustrator`。本包按你的确认改用继任仓库。
2. **两个仓库 license 字段是 `NOASSERTION`**：`rougier/scientific-visualization-book` 和 `ChenLiu-1996/figures4papers`。已读 LICENSE 原文，实际分别是「代码 BSD-2-Clause + 书 CC BY-NC-SA 4.0」和「CC BY-NC 4.0」。注意非商业限制。
3. **`caofan85-lab/research-paper-daily-push` 需要用户自填推送凭据**（WxPusher / Server酱 / 企业微信机器人），OpenAlex API Key 可选。
4. **`ie3jp/illustrator-mcp-server`、`scientific-illustrator`、`Fiji_imageJ_mcp` 都要求本机已装商业/桌面软件**，没装软件时 MCP 装上也调不通。
5. 其余 9 个仓库均为正常公开仓库，未归档，许可证明确（MIT / Apache-2.0 / BSD-3-Clause / GPL-3.0）。

### 核对快照（2026-11-10）

| 仓库 | 归档 | 许可证（API 判定） | Star | 最后提交 |
|---|---|---|---|---|
| zwbao/bioinformatics-god-skill | 否 | MIT | 27 | 2026-04-11 |
| K-Dense-AI/scientific-agent-skills | 否 | MIT | 56602 | 2026-11-09 |
| Yuan1z0825/nature-skills | 否 | Apache-2.0 | 52906 | 2026-11-07 |
| Adkid-Zephyr/anti-defensive-writing-Skill | 否 | MIT | 5600 | 2026-10-27 |
| caofan85-lab/research-paper-daily-push | 否 | MIT | 104 | 2026-09-04 |
| ie3jp/illustrator-mcp-server | 否 | MIT | 210 | 2026-09-29 |
| SteffenPL/fiji-mcp（备选） | 否 | MIT | 9 | 2026-05-27 |
| surajinacademia/Fiji_imageJ_mcp（推荐） | 否 | BSD-3-Clause | 6 | 2026-08-27 |
| icebird1998/drawio-scientific-illustrator（旧，已迁移） | 否 | MIT | 1467 | 2026-07-30 |
| icebird1998/scientific-illustrator（新，采用） | 否 | MIT | 985 | 2026-09-08 |
| rougier/scientific-visualization-book | 否 | NOASSERTION → 代码 BSD-2-Clause / 书 CC BY-NC-SA 4.0 | 11679 | 2026-01-04 |
| garrettj403/SciencePlots | 否 | MIT | 9389 | 2026-06-23 |
| nanxstats/ggsci | 否 | GPL-3.0 | 751 | 2026-07-30 |
| ChenLiu-1996/figures4papers | 否 | NOASSERTION → CC BY-NC 4.0 | 10503 | 2026-10-19 |

**没有编造任何仓库、Skill 名称、版本号或功能**；每条描述都来自对应仓库的 README。

---

## 六、更新记录

| 日期 | 变化 |
|---|---|
| 2026-11-10 | 首次核对并发布 |
