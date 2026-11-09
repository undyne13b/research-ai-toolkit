# INSTALL_PROMPT.md —— 给智能体的安装提示词

> 你是要执行这份清单的智能体。以下规则是硬性的，不因为「更快」「更省事」而放宽。

---

## 0. 你要做什么

把「科研 AI 工具包」清单里的项目装到当前这台机器上，让用户能在自己的智能体里用起来。

- 清单：`manifest.json`（同目录）
- 给人看的说明：`README.md`（同目录）
- 仓库主页：https://github.com/undyne13b/research-ai-toolkit
- 取文件的唯一推荐方式：`git clone https://github.com/undyne13b/research-ai-toolkit.git` 后读本地文件；没有 git 就让用户打开仓库页复制文件全文。
- **不要要求用户为了读这几个文件去翻墙**：`raw.githubusercontent.com` 在国内常不通，别把它当成必要的下载源。

**本项目不含任何安装脚本，你也不许自己写一个「一键跑完全部」的脚本。**

---

## 1. 五条硬性规则

1. **先读再装。** 对清单里每一项，先读取该仓库的 `README` 和 `LICENSE` 原文，确认它现在的安装方式和依赖，然后**列出完整安装计划**（装哪个、装到哪、需要下载什么、会改动哪些已有文件），**等用户明确确认后**再执行第一个安装动作。
2. **不删除、不覆盖。** 用户已有的 Skill 目录、MCP 配置（`~/.claude.json`、`~/.codex/config.toml`、`mcp.json` 之类）、`settings.json` 一律不删不覆盖。同名冲突时**停下来**问用户：跳过、改名并存、还是用户自己决定。改动配置文件前先备份成 `xxx.bak.<时间戳>`。
3. **不碰凭据。** 不把密码、API key、推送 token、webhook 地址写进任何文件或命令行参数。需要凭据时**停下来**，告诉用户去哪个界面/哪个变量名自己填，然后在用户确认填好之后再继续。也不要读取、打印、截图用户的凭据文件。
4. **只装清单里的项目。** 仓库里有 177 个技能就只装点名的几个，有 24 个就只装点名的几个。不顺手装「看起来也有用」的，不自动升级用户已有的其他工具，不 `apt/pip/npm install` 清单外的包（清单里某一项自己声明的依赖除外，且要单独列进计划）。
5. **每装完一项做一次最小测试**，最后用表格汇报：**名称 / 是否成功 / 失败原因 / 下一步**。失败的不要反复重试超过 2 次，不要静默跳过，不要用「应该可以」代替实测。

补充：

- 每一步执行前告诉用户你要跑什么命令。
- 任何一步需要管理员权限、需要重启软件、需要联网下载大文件（>200 MB），先说明再问。
- 不确定的地方就停下来问，不要猜。

---

## 2. 安装顺序

建议顺序，因为前置依赖从少到多：

1. 三个纯文本 Skill（第 3.1～3.4 节）——最快见效
2. 参考资料（第 3.9～3.12 节）——多数只需 `pip` / R 包
3. 需要本机软件的 MCP（第 3.6～3.8 节）——先确认软件装了没
4. 需要用户填凭据的 `research-paper-daily-push`——放最后

用户如果只想装一部分，按用户说的来。

---

## 3. 逐项安装方式

> 下面写的是「核对时的官方方式」。你仍要先读该仓库 README 确认有没有变。

### 3.1 bioinformatics-god-skill

- 仓库：https://github.com/zwbao/bioinformatics-god-skill
- 许可证：MIT
- 方式：`git clone` 到本地临时目录，把**整个仓库目录**复制进你的技能目录，目录名用 `bioinformatics-god-skill`，确保 `SKILL.md` 和 `references/` 都在该目录根下。
- 依赖：无
- 最小测试：让用户问一句「评估一个在我自建 benchmark 上全面超越 Seurat 的单细胞 pipeline」，确认技能能被触发并给出方法学质疑。

### 3.2 scientific-agent-skills —— 只装 3 个

- 仓库：https://github.com/K-Dense-AI/scientific-agent-skills
- 许可证：MIT（仓库另有 `LICENSE.md`，先读）
- 方式：`git clone` 后，**只**把这三个目录复制进技能目录：
  - `skills/scientific-visualization`
  - `skills/scientific-critical-thinking`
  - `skills/cellprofiler`
- 不要装仓库里其余 174 个技能。
- 依赖：按各 `SKILL.md` 里的说明装（`cellprofiler` 需要本机 CellProfiler，`scientific-visualization` 主要是 matplotlib 系）。**装依赖前先列给用户确认。**
- 最小测试：让用户随便给一组数据，确认 `scientific-visualization` 被触发并给出画图建议。

### 3.3 nature-skills —— 先装 nature-figure

- 仓库：https://github.com/Yuan1z0825/nature-skills
- 许可证：Apache-2.0
- 方式：`git clone` 后复制**两个**目录进技能目录：
  - `skills/nature-figure`
  - `skills/nature-shared`（共享依赖，**必须一起装**，否则 nature-figure 读不到 support 文件）
- 不要只复制 `SKILL.md`，要保留整个技能目录（`references/`、脚本、assets 都在里面）。
- 最小测试：给一组数据，让它出一张目标期刊风格的图。

### 3.4 anti-defensive-writing-skill —— 中文版

- 仓库：https://github.com/Adkid-Zephyr/anti-defensive-writing-Skill
- 许可证：MIT
- 方式：`git clone` 后复制 `skills/anti-defensive-writing`（**中文版**）进技能目录。英文版 `skills/anti-defensive-writing-en` 不装。
- 依赖：无（零依赖）
- 最小测试：给一段含「遗憾的是」「效果有限」的论文摘要，确认它改成发布会式表述。

### 3.5 research-paper-daily-push —— 需要用户填凭据

- 仓库：https://github.com/caofan85-lab/research-paper-daily-push
- 许可证：MIT
- 方式：
  1. `git clone`
  2. `python -m pip install -r requirements.txt`（Python 3.10+，用到 `pypdf`）
  3. 按仓库内 `references/installation_guide.md` 完成研究画像配置
- **凭据**：推送选 WxPusher / Server酱 / 企业微信机器人之一。**你不要代填、不要写进任何文件、不要放进命令行**；停下来告诉用户去哪填，用户填好并确认后再继续。
- 未配推送时应能照常生成本地报告——用这一点做最小测试。
- 最小测试：跑一次只出本地报告、不推送的流程，确认能产出结果。

### 3.6 illustrator-mcp-server

- 仓库：https://github.com/ie3jp/illustrator-mcp-server
- 许可证：MIT
- 前置：本机已装 **Adobe Illustrator CC2024+**、**Node.js 20+**。先检查，没装就告诉用户，不要自己装 Illustrator。
- 方式（MCP 配置，不是 Skill）：在用户的 MCP 配置里加一个 stdio 服务，`command` 用 `npx`，参数 `illustrator-mcp-server`。改配置前备份。
- 最小测试：让智能体读一次当前 Illustrator 打开的文档（只读操作），确认工具能列出来并返回成功。

### 3.7 Fiji_imageJ_mcp（推荐）／ SteffenPL/fiji-mcp（备选）

**推荐方案**

- 仓库：https://github.com/surajinacademia/Fiji_imageJ_mcp
- 许可证：BSD-3-Clause
- 前置：本机 Fiji、Python 3.10+
- 方式：
  1. `python -m pip install fiji-mcp-server==0.2.0`
  2. MCP 配置：stdio 启动 `fiji-mcp-server`，设环境变量 `FIJI_PATH` 指向**本机 Fiji 根目录**（该目录下必须直接有 `jars/` 和 `plugins/`），`FIJI_MODE=headless`
- 不改动用户的 Fiji 安装目录。
- 最小测试：打开一张测试图，读一下图像尺寸，确认返回正确。

**备选方案**（只有用户明确要「记录我手点 Fiji 的操作并导出 macro」时才用）

- 仓库：https://github.com/SteffenPL/fiji-mcp
- 许可证：MIT
- 前置：Fiji(Java 8+)、Python 3.11+、`uv`
- 方式：`git clone` → `uv sync` → `uv run fiji-mcp install --fiji-home <Fiji 路径>`
- **注意：这一步会往用户的 Fiji `plugins/` 目录写入一个 JAR**，属于改动用户已有安装，执行前必须单独告知并取得确认。

### 3.8 scientific-illustrator

- 仓库：https://github.com/icebird1998/scientific-illustrator
- 许可证：MIT
- 前置：Codex 桌面端或支持插件的 Codex CLI；本机已装 PowerPoint / WPS 演示 / draw.io Desktop 之一
- 方式：`git clone` 后注册本地 marketplace，再装插件：
  - `codex plugin marketplace add <本地 clone 路径>`
  - `codex plugin add scientific-illustrator@scientific-illustrator-tools`
- 装完需要重启 Codex 才生效，记得告诉用户。
- 最小测试：让它连接已打开的 PowerPoint / draw.io，查一次连接状态，确认 `connected` 为真。

### 3.9 scientific-visualization-book —— 参考资料，不安装

- 仓库：https://github.com/rougier/scientific-visualization-book
- 许可证：**代码 BSD-2-Clause；书的正文 CC BY-NC-SA 4.0**
- 方式：只需 `git clone` 或直接在线读，**没有任何安装步骤**。示例代码在 `code/` 下。
- 最小测试：不需要。

### 3.10 SciencePlots

- 仓库：https://github.com/garrettj403/SciencePlots
- 许可证：MIT
- 方式：`pip install SciencePlots`；用 `scienceplots` 的 latex 样式时，本机还需装 LaTeX（没装就只用非 latex 样式）
- 最小测试：`import matplotlib.pyplot as plt; import scienceplots; plt.style.use(['science']); plt.plot([1,2,3]); plt.savefig('t.png')` 能出图。

### 3.11 ggsci

- 仓库：https://github.com/nanxstats/ggsci
- 许可证：GPL-3.0
- 方式：R 里 `install.packages("ggsci")`
- 最小测试：`library(ggsci)` 能加载，`pal_npg("nrc")(10)` 能返回 10 个颜色。

### 3.12 figures4papers

- 仓库：https://github.com/ChenLiu-1996/figures4papers
- 许可证：**CC BY-NC 4.0 —— 禁止商业用途**
- 方式：`git clone` 参考 `figure_*/` 下的脚本，**不安装**。
- 最小测试：不需要。

---

## 4. 最后要交的汇报表

装完（或部分失败）后，用这张表汇报，一行一项，不要合并、不要省略失败项：

| 名称 | 是否成功 | 失败原因 | 下一步 |
|---|---|---|---|
| bioinformatics-god-skill | ✅ / ❌ | 具体报错原文 | 用户要做什么 |
| scientific-agent-skills（3 个） | | | |
| nature-skills（nature-figure + nature-shared） | | | |
| anti-defensive-writing-skill | | | |
| research-paper-daily-push | | | |
| illustrator-mcp-server | | | |
| Fiji_imageJ_mcp | | | |
| scientific-illustrator | | | |
| scientific-visualization-book | 不适用（参考） | — | — |
| SciencePlots | | | |
| ggsci | | | |
| figures4papers | 不适用（参考） | — | — |

表格后面再补三句话：

1. 这次**动过**用户已有的哪些文件（列路径）。
2. 哪些项需要用户自己填凭据、填在哪。
3. 哪些项因为缺软件没装成，缺的是什么。
