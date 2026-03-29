# ♟️ 中国象棋 Pro17 - 典藏神擎版 (V17)

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



# ♟️ 中国象棋 Pro17 (Chinese Chess Pro17)

一个基于纯前端（HTML/JS/CSS）的极限性能中国象棋 AI 引擎。
本项目挑战了浏览器环境下的性能物理极限，通过结合 **Zobrist 哈希、置换表 (TT)、Web Workers 集群** 以及硬核的 **Nginx 跨域隔离 + SharedArrayBuffer 共享内存**，在网页端实现了国家大师级开局库与极速的多核 Lazy SMP 并发推演。

---

## 🚀 核心进化史 (Changelog: V10 ➡️ V17)

从基础的单核搜索，到无锁并发的内存共享集群，以下是引擎核心的演进纪要：

### [V17 Ultimate] 最终奥义：坚若磐石的硬锁与优雅降级
- **🛡️ 绝对防穿梭硬锁**：引入 `isAnimating`、`isInterrupted` 等底层状态锁，彻底杜绝了动画飞行途中或 AI 思考时狂点界面导致的“时空穿梭”与引擎崩溃。
- **✨ 优雅降级 (Graceful Degradation)**：新增探测探针，在脱离 Nginx/HTTPS 环境（如纯本地 `file:///` 双击运行）时，能自动放弃请求 `SharedArrayBuffer`，并通过 Blob 内存态注入技术绕过跨域限制，无缝降级为“独立多核模式”，实现开箱即用。

### [V16] 算力提纯：消除木桶效应与 Lazy SMP 强化
- **⚡ 主线程 0 毫秒光速首步**：将 `checkOpeningBook` 开局命中逻辑提升至主线程。解决了此前 Worker 0 命中开局库后，仍需等待 Worker 1~5 深度搜索结束的“木桶效应”。
- **🎲 协同扰动算法 (Noise)**：在 Lazy SMP 算法中为不同核心引入微小随机扰动，迫使 6 个核心的搜索树产生分化，极大提升了共享置换表的剪枝复用效率。
- **🐛 阻断拦截重写**：修复了“悔棋”操作强制杀死线程时，因 Promise 返回 `null` 导致的“AI 冤枉认输”误判 Bug。

### [V15] 史诗级架构跃迁：物理共享内存池解封
- **🔥 解锁 SharedArrayBuffer**：配合 Nginx 的 `COOP/COEP` 跨域隔离响应头，成功突破浏览器安全封印，激活高达 104MB 的物理共享内存。
- **🧠 真正的 Lazy SMP 协同**：彻底废弃低效的静态根节点分割。6 个 Web Worker 核心通过 Lock-free 的 `Int16Array/BigInt64Array` 视图疯狂读写同一个置换表（TT），实现跨线程“抄作业”式的 0 毫秒剪枝，NPS (每秒节点数) 几何级暴涨。

### [V14] 棋谱拓展：全境响应大师开局库
- **📖 状态侦测法 (State-Pattern Detection)**：重构了单向红方开局库，引入黑方被动响应机制。当玩家执红走当头炮、仙人指路或飞相等阵型时，AI 能在 1 毫秒内瞬间使出“屏风马”、“卒底炮”等千锤百炼的名谱予以反击，终结了开局盲目耗算力的痛点。

### [V13] 赛博美学与防死锁重构
- **🎨 极客级 UI 重构**：侧边栏重绘为【战况监控】、【战术指挥】、【终端日志】三大毛玻璃拟物化模块。
- **🧹 消灭内存泄漏**：修复了高频点击导致的 Web Worker 僵尸线程堆积与 Promise 挂起导致的闭包内存泄漏。引入了极速回收列队 `pendingResolves`。

### [V12] 内核手术：修复哈希污染与“懦夫 Bug”
- **🦠 修复致命逻辑**：排查并修复了极深的 Zobrist 哈希序列还原反向 Bug（Unmake Move 污染），彻底治愈了置换表读取错误导致的“AI 不吃子 / 懦夫来回走”现象。
- **⚔️ 杀意重燃**：剥离了存在误判的早期伪 SEE 评估，回归并重构了纯粹的 MVV-LVA（高价值换低价值）静态吃子排序；修复了炮“隔空指将”的过高错误估值。

### [V11] 战术引擎基础夯实
- **⚖️ 高级兵型与空间评估**：大幅强化了对“车控开放线”、“憋马腿”、“过河连环兵”的静态评分权重。
- **🔄 异步引擎升级**：将原本会导致浏览器主线程假死的 `autoNext` 升级为 `async/await` 异步非阻塞流。
- **🪞 镜像坐标系**：底层鼠标映射与渲染层重写，完美支持玩家随时“翻转棋盘”而不错位。

---

## ⚙️ 极致性能部署指南 (Docker + Nginx)

如果想在本地解锁本引擎的最强形态（V15+ 的 104MB 共享内存），必须配置受信任的安全上下文与响应头。推荐使用 NAS 或 HomeLab 部署：

1. **部署 Nginx 容器**
2. **配置 `nginx.conf` 核心反向代理响应头**：
   ```nginx
   server {
       listen 80;
       location / {
           root /usr/share/nginx/html;
           index index.html;
           
           # 必须包含这两行以解除浏览器多核共享内存封锁
           add_header Cross-Origin-Opener-Policy "same-origin" always;
           add_header Cross-Origin-Embedder-Policy "require-corp" always;
       }
   }

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
访问 http://<你的NAS_IP>:8080 即可跨设备畅玩！

💡 踩坑提示： 如果部署后访问提示 403 Forbidden，请检查：

文件名必须是 index.html。

挂载的本地文件夹权限是否已开放（Nginx 容器内的用户需要读取权限）。

📅 版本历史 (Changelog)
V10.0 (典藏神擎版)：彻底隔离 UI 与 Worker 线程，修复极限算力下的内存泄漏与 undefined 崩溃；加入绝杀判定引擎与 Cubic 缓出动画。

V9.0：重构 UI 渲染模块，修复画布坐标文字被边缘截断的问题，优化繁简字体绝对居中，增加木纹物理光影。

V8.0：升级评价函数，加入开局库，强化兵型与车马战术动态加分。

V1.0：基础 Alpha-Beta 引擎与 Canvas 绘制。

📄 许可证 (License)
本项目采用 MIT License 开源，欢迎自由 Fork、魔改或将其集成到你的项目中。如果你喜欢这个项目，欢迎点个 ⭐️ Star！
