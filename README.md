# Soil Ecosystem ANT Simulator

### Actor-Network Theory-Based Real-Time Simulation of Multi-Species Interactions in Soil Ecosystems

（土壌生態系アクターネットワーク・リアルタイムシミュレーション）

---

## 🌱 Overview

（概要）

**Soil Ecosystem ANT Simulator** is a real-time, browser-based simulation that visualizes multi-species interactions in soil ecosystems using Actor-Network Theory (ANT) and graph theory as its theoretical foundations.

（本プロジェクトは、アクターネットワーク理論（ANT）とグラフ理論を理論的基盤として、土壌生態系における多種アクターの相互作用をリアルタイムに可視化するブラウザベースのシミュレーターです。）

Nine types of actors — bacteria, mycorrhizal fungi, nematodes, protozoa, earthworms, plant roots, organic matter, water, and oxygen — interact through five relationship types (decomposition, predation, symbiosis, competition, transport), producing emergent network dynamics that are quantified in real time.

（9種のアクター——細菌・菌根菌・線虫・原生動物・ミミズ・植物根・有機物・水分・酸素——が5種の相互作用（分解・捕食・共生・競争・輸送）を通じてネットワークを形成し、その動態がリアルタイムに定量化されます。）

> This is the second iteration of the [Soil Microcosm](https://github.com/mitsuaki1/Soil-Microcosm) project, advancing from scene-based visualization to agent-based simulation with quantitative graph metrics.
>
> （本プロジェクトは [Soil Microcosm](https://github.com/mitsuaki1/Soil-Microcosm) の第2弾であり、シーン遷移型の可視化からエージェントベースシミュレーション＋定量的グラフ指標へと発展させたものです。）

---

## 🎯 Purpose

（目的）

Conventional agriculture treats soil as an object to be controlled. In contrast, organic farming, regenerative land management, and fermentation practices are grounded in the principle of **designing conditions for self-organizing ecological networks**.

（慣行農業は土壌を「制御対象」として扱いますが、有機農業・大地の再生・発酵の実践は「自律的な生態系ネットワークが成立する条件をデザインする」という原理に基づいています。）

This simulator aims to:

（本シミュレーターの目的：）

- **Visualize** the hidden network dynamics of soil ecosystems in real time
  （土壌生態系の隠れたネットワーク動態をリアルタイムに可視化する）
- **Quantify** ecological states using graph-theoretic metrics (density, clustering, Shannon diversity, system stability)
  （グラフ理論指標（密度・クラスタリング・Shannon多様性・系全安定性）で生態系の状態を定量評価する）
- **Bridge** ecological practice knowledge and computational science
  （生態学的実践知と計算科学を架橋する）
- **Demonstrate** that stability emerges not from control but from relational design
  （安定性は制御からではなく関係性のデザインから創発することを示す）

---

## 🧬 Simulation Architecture

（シミュレーション設計）

### Actors (9 types)（アクター 9種）

| Actor | Japanese（日本語） | Role（役割） | Color（色） |
|-------|----------|------|-------|
| Bacteria | 細菌 | Primary decomposers（主要分解者） | 🔵 `#00e8ff` |
| Mycorrhizal Fungi | 菌根菌 | Symbiotic mediators, hyphal networks（共生仲介者・菌糸ネットワーク） | 🟠 `#ff8c00` |
| Nematodes | 線虫 | Bacterial/fungal predators（細菌・菌根菌の捕食者） | 🟢 `#b8ff00` |
| Protozoa | 原生動物 | Fast bacterial grazers（高速細菌捕食者） | 🟣 `#e844ff` |
| Earthworms | ミミズ | Ecosystem engineers（生態系エンジニア） | 🔴 `#ff3d3d` |
| Plant Roots | 植物根 | Carbon suppliers, rhizosphere effect（炭素供給・根圏効果） | 💚 `#44ff88` |
| Organic Matter | 有機物 | Decomposition substrate, energy source（分解基質・エネルギー源） | 🟤 `#9a7244` |
| Water | 水分 | Transport medium / OPP（輸送媒体・義務的通過点） | 💧 `#3db8ff` |
| Oxygen | 酸素 | Aerobic metabolism prerequisite（好気的代謝の前提条件） | ⚪ `#dde8ff` |

### Interaction Edges (5 types)（相互作用エッジ 5種）

| Type（種別） | Description（内容） | ANT Concept（ANT概念との対応） |
|------|-------------|-------------|
| **Decomposition**（分解） | Bacteria/Fungi/Earthworms → Organic Matter（細菌・菌根菌・ミミズ → 有機物） | Translation / resource conversion（翻訳・資源変換） |
| **Predation**（捕食） | Nematodes/Protozoa → Bacteria/Fungi（線虫・原生動物 → 細菌・菌根菌） | Energy transfer via Lotka-Volterra（Lotka-Volterra方程式によるエネルギー移動） |
| **Symbiosis**（共生） | Fungi ↔ Plant Roots（菌根菌 ↔ 植物根） | Mutual enrollment（相互的登録） |
| **Competition**（競争） | Same-type resource competition（同種間の資源競合） | Rivalry among actants（アクタント間の対抗関係） |
| **Transport**（輸送） | Water/Oxygen mediation（水分・酸素による媒介） | Obligatory Passage Point / OPP（義務的通過点） |

### Lotka-Volterra Discrete Model（Lotka-Volterra離散モデル）

Energy exchange between predator `i` and prey `j`:

（捕食者 `i` と被食者 `j` のエネルギー交換：）

```
ΔEᵢ = +αᵢⱼ · s · Eⱼ     (predator gains / 捕食者のエネルギー獲得)
ΔEⱼ = -βᵢⱼ · s · Eⱼ     (prey loses / 被食者のエネルギー損失)
s = 1 - d/r               (distance-dependent interaction strength / 距離依存の相互作用強度)
```

| Predator（捕食者） | Prey（被食者） | α | β |
|----------|------|---|---|
| Nematode（線虫） | Bacteria（細菌） | 5.0 | 1.5 |
| Protozoa（原生動物） | Bacteria（細菌） | 6.0 | 2.0 |
| Nematode（線虫） | Fungi（菌根菌） | 3.0 | 0.8 |
| Bacteria（細菌） | Organic（有機物） | 3.0 | 0.7 |
| Fungi（菌根菌） | Organic（有機物） | 2.0 | 0.6 |
| Earthworm（ミミズ） | Organic（有機物） | 6.0 | 0.9 |

---

## 📊 Real-Time Graph Metrics

（リアルタイムグラフ理論指標）

| Metric（指標） | Symbol（記号） | Formula（数式） | Ecological Meaning（生態学的意味） |
|--------|--------|---------|-------------------|
| Network Density（ネットワーク密度） | ρ | `2m / n(n-1)` | Interaction richness（相互作用の豊かさ） |
| Mean Degree（平均次数） | k̄ | `2m / n` | Average connectivity（平均的接続密度） |
| Clustering Coefficient（クラスタリング係数） | C | `(1/n) Σ Cᵢ` | Local cooperation triads（局所的三者共存の密度） |
| Shannon Diversity（Shannon多様性指数） | H' | `-Σ pᵢ ln pᵢ` | Species evenness（種の均等度） |
| Evenness（均等度） | J' | `H' / ln(s)` | Normalized diversity（正規化された多様性） |
| System Stability（系全安定性） | S | `0.4·J' + 0.3·min(20ρ,1) + 0.3·B` | Composite stability index（複合安定性指標） |

Where `B = 1 - max(nᵢ)/N` represents dominance-based evenness.

（ここで `B = 1 - max(nᵢ)/N` は種優占度からみた均等性を表す。）

---

## 🔄 Ecological Phases

（生態フェーズ）

The simulation self-organizes into three emergent phases:

（シミュレーションは以下の3つのフェーズに自律的に遷移します：）

1. **Phase 1: Bacterial Bloom** (t ≈ 0–100 ticks)
   — Rapid bacterial/fungal growth on organic matter. ρ and H' increase.
   （初期増殖期 — 細菌・菌根菌が有機物を活発に分解し爆発的に増殖。ρ と H' が上昇。）

2. **Phase 2: Predator Surge** (t ≈ 100–250 ticks)
   — Nematodes and protozoa proliferate. Lotka-Volterra oscillations emerge. H' temporarily decreases.
   （捕食者台頭期 — 線虫・原生動物が急増。Lotka-Volterra型の個体数振動が出現し、H' が一時的に低下。）

3. **Phase 3: Symbiotic Equilibrium** (t ≈ 250+ ticks)
   — Mycorrhizal-root symbiosis stabilizes. System stability S reaches local maximum. Small-world network structure observed.
   （共生安定期 — 菌根菌と植物根の共生ネットワークが安定化。系全安定性 S が局所最大値に到達。小世界ネットワーク構造が観察される。）

---

## ⚙️ Technical Implementation

（技術実装）

- **Single HTML file**: HTML5 Canvas + Vanilla JavaScript, zero dependencies
  （単一HTMLファイル：HTML5 Canvas + バニラJS、外部ライブラリ不使用）
- **Spatial Grid optimization**: Cell-based hashing reduces neighbor search from O(n²) to O(n·k̄_cell)
  （空間グリッド最適化：セルベースハッシュにより近傍探索を O(n²) → O(n·k̄_cell) に削減）
- **60fps rendering**: `requestAnimationFrame` loop with configurable speed (1×–10×)
  （60fps描画：requestAnimationFrame ループ、速度は1×〜10×で調整可能）
- **Time scale**: 1 tick = 60 seconds (real-time correspondence based on literature values)
  （時間スケール：1 tick = 60秒、文献値に基づく実時間対応）
- **Max agents**: 450 concurrent entities with dynamic population management
  （最大エージェント数：450個体、動的な個体数管理）
- **Periodic events**: Rainfall (200 ticks), organic input (150 ticks), rhizosphere effect (300 ticks)
  （周期イベント：降雨（200 tick）、有機物投入（150 tick）、根圏効果（300 tick））

### Module Structure（モジュール構成）

```
┌─────────────────────────────────────────────────────────────┐
│  Agent Management     エージェント管理                         │
│  (lifecycle: spawn / update / death / reproduction)          │
│  （ライフサイクル：生成・更新・死亡・複製）                      │
├─────────────────────────────────────────────────────────────┤
│  SpatialGrid          空間グリッド                             │
│  (O(1) cell-based neighbor search, 60px cells)               │
│  （O(1) セルベース近傍探索、60pxセル分割）                      │
├─────────────────────────────────────────────────────────────┤
│  Interaction Engine    相互作用エンジン                         │
│  (5 edge types, ANT rules, Lotka-Volterra exchange)          │
│  （5種エッジ、ANTルール、Lotka-Volterra方程式）                │
├─────────────────────────────────────────────────────────────┤
│  Graph Metrics         グラフ指標計算                           │
│  (ρ, k̄, C, H', S computed every 20 ticks)                   │
│  （ρ・k̄・C・H'・S を20 tickごとに算出）                       │
├─────────────────────────────────────────────────────────────┤
│  Rendering Engine      レンダリングエンジン                     │
│  (Canvas 2D: nodes, edges, charts, UI panels)                │
│  （Canvas 2D：ノード・エッジ・グラフ・UIパネルの描画）           │
└─────────────────────────────────────────────────────────────┘
```

---

## ▶️ How to Run

（実行方法）

Open the HTML file in any modern browser:

（HTMLファイルを任意のブラウザで開くだけで実行できます：）

```bash
# Clone the repository（リポジトリをクローン）
git clone https://github.com/mitsuaki1/Soil-Ecosystem-ANT-Simulator.git

# Open in browser（ブラウザで開く）
open Soil_ecosystem_with_ANT_claude_v1_.html
```

No installation, no dependencies, no build step required.

（インストール不要、依存ライブラリなし、ビルドステップなし。）

### Controls（操作方法）

| Control（操作） | Action（動作） |
|---------|--------|
| ⏸ 停止 / ▶ 再開 | Pause / Resume simulation（シミュレーションの一時停止・再開） |
| ↺ リセット | Reset all agents and metrics（全エージェントと指標をリセット） |
| OPP 表示 | Toggle Obligatory Passage Point visualization（義務的通過点の表示切替） |
| 速度スライダー | Simulation speed 1×–10×（シミュレーション速度 1×〜10×） |
| マウスホバー | Display individual agent info tooltip（個体情報のツールチップ表示） |
| クリック | Track a specific agent（特定個体の追跡） |

---

## 🖥️ Recommended Browsers

（推奨ブラウザ）

- Google Chrome（推奨）
- Microsoft Edge
- Firefox

---

## 🌍 Theoretical Background

（理論的背景）

### Actor-Network Theory / ANT（アクターネットワーク理論）

This project applies Latour, Callon, and Law's ANT framework to soil ecology. Key concepts operationalized in the simulation:

（本プロジェクトは、ラトゥール・カロン・ローによるANTの枠組みを土壌生態学に適用しています。シミュレーション上で操作化された主要概念：）

- **Actant**（アクタント）: Every entity (biological or non-biological) is modeled as an equally capable network participant
  （生物・非生物を問わず、すべての存在が等価なネットワーク参与者としてモデル化される）
- **Translation**（翻訳）: Five interaction edge types represent the process of mutual transformation between actants
  （5種の相互作用エッジがアクタント間の相互変容プロセスを表現する）
- **Obligatory Passage Point / OPP**（義務的通過点）: Water and mycorrhizal fungi emerge as critical mediators whose removal destabilizes the entire network
  （水分と菌根菌が重要な仲介者として浮上し、その除去はネットワーク全体を不安定化させる）
- **Non-human agency**（非人間的行為者性）: Organic matter distributions actively guide bacterial movement and shape network topology
  （有機物の空間分布が細菌の移動方向を能動的に誘導し、ネットワーク構造全体を形成する）

### Implications for Practice（実践への示唆）

- **Diversity sustains resilience**（多様性がレジリエンスを支える）: High H' correlates with greater recovery from disturbance
  （高い H' は攪乱からの高い回復力と相関する）
- **Stability through relational design**（関係性のデザインによる安定性）: Maximum S emerges from combined conditions (rainfall + organic input + rhizosphere), not from controlling individual actors
  （系全安定性 S の最大値は、個別アクターの制御ではなく、降雨・有機物投入・根圏効果の複合条件から自律的に達成される）
- **Water/air pathways as graph-theoretic OPPs**（水脈・空気脈はグラフ理論的にOPPである）: The "reconnecting water and air veins" principle from regenerative land management corresponds to restoring OPP node connectivity
  （「大地の再生」が強調する「水脈と空気脈のつなぎ直し」は、OPPノードの連結性回復として解釈できる）

---

## 📄 Associated Paper

（関連論文）

A full research paper is included in this repository:

（本リポジトリには研究論文の全文が同梱されています：）

**"Actor-Network Theory-Based Real-Time Simulation of Multi-Species Interactions in Soil Ecosystems"**

（「土壌生態系における多種アクター相互作用のANTベース・リアルタイムシミュレーション」）

- Author（著者）: mitsulab
- Fields（分野）: Ecological Informatics / Agricultural Systems Science / Complex Systems
  （生態情報工学 / 農業システム科学 / 複雑系科学）
- File（ファイル）: `soil_ant_paper.docx`

---

## 🚧 Limitations & Future Work

（限界と今後の課題）

### Current Limitations（現在の限界）

- 2D simulation (real soil has 3D pore/aggregate structure)
  （二次元シミュレーション：実際の土壌は三次元の孔隙・団粒構造を持つ）
- Averaged parameters (no soil type, temperature, or pH variation)
  （パラメータは文献の平均値を使用：土壌タイプ・温度・pHの違いを未反映）
- No quantitative validation against field data
  （フィールドデータとの定量的検証が未実施）

### Future Directions（今後の展開）

- SOFIX / NGS field data comparison and validation
  （SOFIX・次世代シーケンシング（NGS）によるフィールドデータとの比較検証）
- 3D soil structure extension (Unity / Blender)
  （Unity / Blender を活用した三次元土壌構造モデルへの拡張）
- IoT sensor integration (real-time digital twin)
  （IoTセンサー（土壌温度・水分・CO₂）連携によるデジタルツイン構築）
- Disturbance scenarios (frost, drought, pesticide application)
  （降霜・干ばつ・農薬投入などの攪乱シナリオの実装）
- WebGL rendering for larger agent populations
  （大規模エージェントに対応するWebGLレンダリング）

---

## 🔗 Related Projects

（関連プロジェクト）

- [Soil Microcosm (v1)](https://github.com/mitsuaki1/Soil-Microcosm) — Scene-based soil microbiome visualization
  （シーン遷移型の土壌微生物ネットワーク可視化）
- [note article (v1)](https://note.com/mitsu32_lab/n/nd04af75e5c24) — Soil Microcosm: Visualization and Programming of Soil Microbial Networks
  （note記事：土壌微生物ネットワークの可視化とプログラミングの試み）

---

## 🌿 Author

（著者）

**mitsulab**

Organic agriculture × Regenerative land management × Fermentation
（有機農業 × やわらかい土木 × 発酵）

Exploring how soil ecosystems self-organize through microbial, hydrological, and ecological interactions — designing conditions for life, not controlling it.
（微生物・水文・生態相互作用による土地の自己組織化を探究——生命を制御するのではなく、生命が育つ条件をデザインする。）

---

## 📝 License

（ライセンス）

MIT License

---

## 🏷️ Keywords

（キーワード）

`actor-network-theory` `soil-ecology` `agent-based-model` `graph-theory` `lotka-volterra` `shannon-diversity` `real-time-simulation` `organic-farming` `regenerative-agriculture` `digital-nature` `complex-systems` `soil-microbiome`
