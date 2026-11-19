# SLAM Gauss-Newton Localization Calculator

A fully offline, zero-dependency educational tool that solves the **"where am I"** single-point localization problem with the **Gauss-Newton (GN)** method, and extends it to multi-robot (swarm) and multi-time-step (trajectory) scenarios.

## Features

- **Single-file HTML** — runs by double-click; no installation, no CDN, works fully offline.
- **Two independent switches** — the swarm switch sets the robot count `R`, the trajectory switch sets the time-step count `T`. Each `robot × time` unit runs an independent GN localization.
- **Covers Q1–Q4** — single-point, trajectory, swarm, and the combined swarm+trajectory case with three built-in presets and a random Q4 generator.
- **Bilingual UI** — one button toggles between Chinese and English; both languages live in the same file.
- **Notebooks (Chinese & English)** — all code is visible, using only `numpy` + `matplotlib`, so they render directly on GitHub.
- **Analysis views** — reverse validation, pairwise distance heatmaps, 3D trajectories with depth fog, start/end markers and direction arrows, and cosine direction similarity.

## Files

| File | Description |
| --- | --- |
| `SLAM高斯牛顿定位计算器.html` | Main offline calculator (single file, bilingual, zero dependency). Recommended for daily use — easier to customize and more polished visuals; its purpose differs substantially from the notebooks |
| `SLAM高斯牛顿定位计算器.ipynb` | Chinese notebook with visible code (Q1–Q4) |
| `SLAM_Gauss_Newton_Localization.ipynb` | English notebook with visible code (Q1–Q4) |
| `build_nb.py`, `build_nb_en.py` | Generators that build the two notebooks (optional, for reproducibility) |

## Method

- **Predicted distance** from the model: `h = ||c − X||`, where `c` is a landmark and `X` the current pose estimate; the **measured distance** `d` is the LiDAR observation.
- **Residual** `r = h − d`; the GN update solves `Δ = −(JᵀJ)⁻¹ JᵀR` and iterates `X ← X + Δ`.
- **Objective** `SSR = ½ Σ r²`; a value close to `0` means the pose explains the measurements.
- Each `robot × time` unit is independent (3 unknowns, 4 landmark constraints), analogous to GPS single-point positioning. The time interval only generates time-stamp labels and does not enter the solve.
- For Q4, landmarks are placed on a tetrahedron so the geometry is never degenerate, and a warm start (previous solution seeds the next step) keeps the recovered track accurate.

## Usage

- **HTML:** open the file in any browser, use a preset or enter data, then click **Build and Solve**.
- **Notebook:** run `jupyter notebook`, open the notebook and execute the cells top to bottom. Adjust parameters only in the parameter cell and re-run.

## Requirements

- HTML: none (plain HTML/CSS/vanilla JavaScript).
- Notebooks: Python with `numpy` and `matplotlib`.

## Future Work & Problems

After adding direction arrows to the 3D trajectories, users can visually identify at a glance the two trajectories closest in direction. It remains hard, however, to judge the **distance** between them intuitively, so charts are still needed. Suggestions for more intuitive distance-comparison approaches are very welcome.

## License

Released under the MIT License.

---

# SLAM 高斯-牛顿定位计算器

一个完全离线、零依赖的教学工具，用**高斯-牛顿法**求解「我在哪儿」的单点定位问题，并扩展到多机器人（群体）与多时刻（轨迹）场景。

## 功能特性

- **单文件 HTML**：双击即可运行，无需安装、不依赖 CDN、完全离线。
- **两个独立开关**：群体开关决定机器人数量 `R`，轨迹开关决定时刻数量 `T`；每个「机器人 × 时刻」单元独立 GN 定位。
- **覆盖 Q1–Q4**：单点、轨迹、群体及群体+轨迹综合，内置三套示例与 Q4 随机生成。
- **中英双语界面**：一个按钮切换，两套语言都在同一文件内。
- **中英两份 notebook**：代码可见，仅依赖 `numpy` 与 `matplotlib`，可在 GitHub 直接渲染。
- **分析视图**：反验证、两两距离热图、带深度雾的 3D 轨迹、起终点与方向箭头、cosθ 方向相似度。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `SLAM高斯牛顿定位计算器.html` | 主离线计算器（单文件、双语、零依赖）。日常使用推荐：自定义更方便、可视化更完善；用途与 notebook 有明显区别 |
| `SLAM高斯牛顿定位计算器.ipynb` | 中文 notebook，代码可见（Q1–Q4） |
| `SLAM_Gauss_Newton_Localization.ipynb` | 英文 notebook，代码可见（Q1–Q4） |
| `build_nb.py`、`build_nb_en.py` | 生成两份 notebook 的脚本（可选，用于复现） |

## 方法

- 模型给出的**预测距离** `h = ||c − X||`（`c` 为 landmark，`X` 为当前位姿）；**实测距离** `d` 来自 LiDAR 观测。
- **残差** `r = h − d`；GN 更新求解 `Δ = −(JᵀJ)⁻¹ JᵀR`，并迭代 `X ← X + Δ`。
- 目标函数 `SSR = ½ Σ r²`，接近 `0` 表示位姿能解释观测。
- 每个「机器人 × 时刻」单元相互独立（3 个未知量、4 个 landmark 约束），类比 GPS 单点定位；时间间隔只生成时间戳标签，不参与定位。
- Q4 中 landmark 按四面体布置以保证几何不退化，并用暖启动（上一时刻解作为下一步初值）保证反解轨迹准确。

## 使用方法

- **HTML**：用浏览器打开，选择示例或输入数据，点击「构建并求解」。
- **Notebook**：运行 `jupyter notebook`，打开后从上到下依次执行；只在参数 cell 调整参数后重新运行。

## 依赖

- HTML：无（纯 HTML/CSS/原生 JavaScript）。
- Notebook：Python 及 `numpy`、`matplotlib`。

## 后续工作与问题

在 3D 轨迹中加入方向箭头后，用户可以通过目视大致判断方向上最接近的两条轨迹；但仍难以直观判断它们之间的距离，因此仍然需要借助图表。欢迎大家提供让距离对比更直观的方案。

## 许可

基于 MIT 协议发布。
