# ♟️ 中国象棋 Pro17 - 典藏神擎版 (V10)

![HTML5](https://img.shields.io/badge/HTML5-Canvas-E34F26?style=flat-square&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JS-Vanilla-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Zero Dependencies](https://img.shields.io/badge/Dependencies-0-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

**一个纯净、极致、算力爆炸的单文件前端象棋 AI 引擎。**

本项目旨在挑战纯前端环境下的算力与渲染极限。没有笨重的后端框架，没有繁杂的外部资源，仅用一个不到 1500 行的 `.html` 文件，实现了一个具备大师级棋力、支持多核并发、且拥有典藏级 2D/3D 拟真视觉体验的中国象棋游戏。

非常适合作为 **NAS (如飞牛、群晖) / HomeLab** 的极客小玩具，随时随地打开浏览器即可对弈。

---

## ✨ 核心特性 (Features)

* 📦 **极致的“万剑归一” (单文件架构)**
  * **零外部依赖**：没有图片、没有 CSS 文件、没有外部 JS 库。
  * 棋盘、木纹、棋子高光阴影全部由 HTML5 Canvas 代码实时渲染。
  * **程序化音效 (Procedural Audio)**：舍弃 `.mp3` 文件，采用 Web Audio API (`AudioContext`) 的底层波形振荡器实时合成落子与吃子音效。

* 🧠 **硬核 AI 战术大脑**
  * **知识图谱**：摒弃死板的查表，AI 具备真实的战术认知。支持“车霸开放线”、“连环兵保护”、“马腿防卡死预判”以及“兵临城下”等高级动态评估。
  * **内建开局库**：自带中炮、飞相、仙人指路、起马等 8 种正统大师开局。
  * **绝杀与规则引擎**：精准识别“绝杀无解”、“困毙”，并严格执行“长将判负”、“三次重复局面作和”的正规象棋规则。

* 🚀 **多核性能榨汁机 (Web Worker)**
  * 突破浏览器单线程瓶颈，利用 `Blob URL` 技术在 HTML 内部动态生成 Web Worker 线程。
  * **狂暴模式 (L4)** 下可瞬间唤醒多核心（专为 Apple M系列 / 高端 PC 优化）并发推演，算力爆炸的同时保持 UI 丝滑不卡顿。

* 🎨 **典藏级视觉美学**
  * **动态拟真光影**：基于 `shadowBlur` 和径向渐变实现的硬件加速 3D 立体木质棋子。
  * **排版强迫症**：黄金视觉比例 `10.4 : 11.4`，精准校准的防遮挡坐标轴，以及绝对居中的繁简双修棋子字体。
  * **物理级动画**：引入 `Cubic Ease-out` 缓出算法，棋子移动拥有真实的“减速轻放”物理落地感。

---

## ⚙️ AI 底层架构解析 (Architecture)

本引擎的 AI 模块复刻了现代顶级传统棋类引擎的经典路线：

1. **PVS (Principal Variation Search)** + **Alpha-Beta 剪枝** 核心搜索树。
2. **Zobrist Hashing** + 1048576 容量的 **置换表 (Transposition Table, TT)** 极速查表。
3. **启发式排序 (Move Ordering)**：MVV-LVA (高价值换低价值) + Killer Moves (杀手步) + History Heuristic (历史启发)。
4. **静止搜索 (Quiescence Search)**：防范水平线效应，算透一切连环吃子危机。

---

## 🚀 快速开始与部署 (Deployment)

由于是纯静态单文件，部署极其简单：

### 方案 A：本地直接运行
1. 下载 `index.html`。
2. 双击在任意现代浏览器（Chrome / Edge / Safari）中打开即可开玩。

### 方案 B：NAS / Docker Nginx 部署 (推荐)
极其适合部署在你的家庭服务器上作为 Web 服务：
1. 在 NAS 中创建一个文件夹（例如 `/volume1/docker/chess`），将下载的 HTML 文件放入并重命名为 `index.html`。
2. 确保该文件及文件夹具有外部读取权限（`chmod 755`）。
3. 使用 Docker 启动 Nginx 容器，将该目录挂载至 Nginx 默认目录：
   ```bash
   docker run -d -p 8080:80 \
     -v /volume1/docker/chess:/usr/share/nginx/html:ro \
     --name chinese-chess nginx
