# ♟️ Chinese Chess Pro - V41 (云边协同修罗版)

[![Vanilla JS](https://img.shields.io/badge/JavaScript-Vanilla-f7df1e?logo=javascript&logoColor=black)](#)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-0-success)](#)
[![Single File](https://img.shields.io/badge/Architecture-Single_HTML-blue)](#)
[![Hybrid AI](https://img.shields.io/badge/AI-Cloud_Edge_Hybrid-ff69b4)](#)

这是一个纯 Vanilla JavaScript 编写的**“单文件、零依赖”**中国象棋极客引擎。在极其精简的代码体积内，不仅实现了基于 Canvas 的现代化赛博极简 UI，更从零开始构建了对标现代超级引擎（如 Pikafish / Stockfish）的底层搜索架构，并成功接入云库,实现了前端算力的终极压榨。

## ✨ 核心特性 (Features)

### ☁️ 1. 云边协同双擎架构 (Hybrid AI)
* **智能离合系统**：引擎具备动态环境感知能力。
  * **开局段 (≤15回合)**：直连 云库，1000ms 极限 Fetch 获取特级大师实战名谱。
  * **残局段 (≤7子)**：直连云端残局库 (Tablebase)，获取“绝对胜负”上帝视角。
  * **中局肉搏/断网脱谱**：瞬间切断网络依赖，0 延迟唤醒本地 V41 多核修罗引擎接管比赛，实现 100% 高可用。
* **FEN 实时编译**：内置极速翻译官，将二维矩阵实时编码为国际标准 FEN 字符串，并将云端 UCCI 协议无缝映射为本地物理坐标。

### ⚡ 2. 极限物理并发 (Multi-threading & Memory)
* **Web Workers 算力矩阵**：走法生成后，瞬间拉起 6 个独立 Worker 线程进行高并发深度搜索。
* **无锁共享物理内存 (Lock-free TT)**：开辟 100MB `SharedArrayBuffer`，基于 `Int32Array` 和 `BigInt64Array` 构建跨线程置换表（Transposition Table）。一核踩坑，多核瞬间共享极窄窗口经验。

### 🧠 3. 现代引擎搜索树 (Search Architecture)
* **极速 PVS (主要变例搜索)**：取代传统 Alpha-Beta 剪枝，利用零窗口 (Zero Window) 试探，极大缩减无效分支。
* **迭代加深时间管理 (Iterative Deepening)**：打破固定层数限制。引擎按“时间阈值”（如 5 秒）疯狂向下深挖（1层、2层... N层），时间耗尽瞬间触发物理级熔断 (`AbortController`)，最大化利用 CPU 空闲周期。
* **静止搜索 (Quiescence Search)**：看破“地平线效应”。在主搜索停止后，继续推演所有激烈的“吃子”与“将军”变化，直至局面彻底归于平静。

### 🛡️ 4. 暴躁的启发式剪枝 (Heuristic Pruning)
* **SEE-Lite (轻量级静态交换评估)**：纯手工打造的全向火力雷达！在第 0 层提前拦截“用大子吃被保护的小子”以及“主动将高价值棋子送入敌方火力网”的低智行为，强打 `-10000` 分执行物理硬剪枝，彻底终结“送子买命”的 AI 顽疾。
* **带白名单的 LMR (晚期移动缩减)**：对非战术移动强行降维减层；但对吃子、将军、杀手步实施绝对保护。
* **NMP (空步剪枝) + 动态残局熔断**：依据 `phaseMat` (子力繁荣度) 动态评估。中局通过“停一手”快速确认优势；进入残局则自动熔断 NMP 功能，全速死算绝杀路径。

### ⚖️ 5. 走法排序与变异 (Move Ordering)
* **MVV-LVA**：最有价值受害者-最无价值攻击者排序，优化吃子顺序。
* **History Table & Killer Heuristics**：历史表与杀手启发加持，动态记忆能引起优质剪枝的防守走位。
* **Zobrist Hashing**：64位哈希算法记录全盘指纹，极速比对长将/长捉循环。
* **Root Randomness (根节点微扰)**：底层 PVS 保持绝对纯净的整数计算，在根节点提取 Top 分数相近的走法进行智能随机，确保棋风诡谲多变且永不犯错。

### 🎨 6. 原生赛博美学 (Zero-dependency UI/UX)
* **Canvas 2D 绘图**：纯数学几何计算绘制棋盘与带有立体光影的棋子。
* **Web Audio API 合成音效**：**不包含任何音频文件**！直接通过底层的 OscillatorNode 合成正弦波和锯齿波，硬核生成落子、吃子、将军报警等全部音效。
* **单行降噪日志**：提炼极简符号流 (如 `[⚙️L8|+124]`)，在有限空间内展示最核心的赛场数据。

## 🚀 快速开始 (Getting Started)

本项目为完全独立的 HTML 单文件，**无任何 NPM 依赖，无需构建打包**。

1. 克隆或下载本仓库。
2. 双击打开 `chinese_chess.html` 即可在本地浏览器中运行体验（默认单核降级模式）。
3. **⚠️ 性能解锁指南 (解锁 100MB 共享内存)**：
   由于现代浏览器的安全策略，要激活 `SharedArrayBuffer` 实现 6 核并发性能全开，必须在具有跨域隔离 (Cross-Origin Isolation) 的本地服务器上运行。
   
   你可以使用 Python 快速启动一个带隔离头的本地服务器：
   ```bash
   # 保存以下代码为 server.py 并运行
   import http.server
   class Handler(http.server.SimpleHTTPRequestHandler):
       def end_headers(self):
           self.send_header("Cross-Origin-Opener-Policy", "same-origin")
           self.send_header("Cross-Origin-Embedder-Policy", "require-corp")
           super().end_headers()
   http.server.HTTPServer(("", 8080), Handler).serve_forever()
