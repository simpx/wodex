<div align="center">

**[← English](README.md)　|　简体中文**

# Wodex：面向 WoW 的自主 Agent

**Plan → Act → Observe → Learn**

</div>

Wodex 是**像人类玩家一样**，通过 **Plan → Act → Observe → Learn** 在《魔兽世界》中自主行动的 Agent，面向探索、任务、打怪和商业等活动。它自己决定去哪里、做什么，再从实际结果中学习。

**看画面，使用玩家的常规操作方式。** Wodex 不读写游戏内存，也不通过逆向游戏网络协议控制角色。

在一次次探索中，Agent 变得越来越娴熟。它把有用的发现沉淀为知识与方法，留给下一次高效复用。第一次找到陌生建筑的入口可能需要几分钟；下一次走进去，只需要几十秒。**经验变成了可以执行的能力。**

## 实机演示

### ⚔️ 演示 01 · 1 → 5 级升级

**AI 可以自主接任务、寻路、打怪、升级，无需逐步指导。** 早期的兽人实验里，没有人告诉它该接哪个任务、往哪里转弯、该打哪只怪。基础的 computer use——观察画面，再通过简单的键盘控制器操作——就已经能完整跑通这个过程。虽然很慢，但玩法决策由 AI 自己完成。

下面是后来一轮**亡灵圣骑士 1–5 级开发过程**的关键截图：

| 创建角色 | 打怪 | 升到 5 级 |
| --- | --- | --- |
| ![新建亡灵圣骑士](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/leveling-start.png) | ![圣骑士与蜘蛛战斗](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/leveling-combat.png) | ![游戏内的 5 级升级提示](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/leveling-complete.png) |
| 新角色开始冒险。 | 选怪、接近、攻击、拾取逐渐成为本地反馈流程。 | 角色达到 5 级。 |

接下来的重点是效率：让 AI 做有意义的判断，让 Runtime 在本地完成重复动作。

---

### 🧭 演示 02 · 区域内自由探索，再高效复用

目标很简单：**从雷霆崖邮箱走到旅店老板那里，然后复用这次探索的经验。**

| 自主探索 · 约 8 分钟 | 复用经验 · 24.1 秒 |
| --- | --- |
| AI 看地图、试走、绕路，两次在门口受阻，最终找到入口。 | 把学到的通路整理成一份 5 步 Plan，Runtime 连续执行，走到旅店老板面前并打开对话。 |
| ![自主探索的原速节选](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/exploration.gif) | ![经验复用的原速节选](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/reuse.gif) |
| [完整探索录像](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/exploration.mp4) | [完整复用录像](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/reuse.mp4) |

AI 留下了入口坐标和交互方法。下一次，Runtime 直接在本地执行完全部 5 步，两段过程中都没有人工键鼠接管。出发段使用了已有街道知识，旅店入口的通路由这次探索获得。

*同一角色、同一起点区域、同一目的地。GIF 为原速节选；探索耗时包含推理、工具与等待，24.1 秒为复用时 Runtime 的执行耗时。*

---

### 🏙️ 演示 03 · 29 步雷霆崖漫游

熟悉区域以后，Agent 可以连续访问杂货商、修理商、拍卖师和银行，打开服务窗口，再返回起点。**一份 Plan，29 步，149 秒。**

![完整 29 步城市漫游，4 倍速播放](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/city-tour.gif)

*完整行程，4 倍速。[观看原速录像](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/city.mp4)。*

## 发生了什么？

Wodex 为 AI 实现了一个 **WoW Runtime**，提供两项基本能力：**Act 与 Observe**。AI 制定计划，Runtime 执行、持续读取反馈并返回结果；AI 从中学习，再决定下一步。

Runtime 与游戏之间的接口是**画面识别与常规输入**：读取截图及画面中显示的状态，通过键鼠操作游戏，沿用玩家正常游玩时的交互入口。不读写游戏内存，不截获、解析或构造游戏网络报文。

| 部分 | 职责 |
| --- | --- |
| **AI** | **Plan 与 Learn**：确定目标、理解陌生环境、调整计划，把发现整理成有用的知识。 |
| **Runtime** | **Act 与 Observe**：读取可见画面、发送常规输入，完成导航、移动、交互和战斗循环；返回结构化结果及必要画面。 |
| **Skills** | 在游戏中操作和处理常见任务的可复用方法。 |
| **Knowledge** | 积累的资料、分析与经验。导航知识保存在 **Atlas** 中，包括通路、区域和兴趣点。 |

每次探索都会留下依据：原始观察、分析，再到可复用知识。地图提供候选道路，实际走一遍验证入口是否可用，结果再补充进 Atlas。失败的尝试也会保留，帮助下一次规划。

学习体现在不断增长的知识与方法中。熟悉的行程可以交给 Runtime 一次执行完成，不必每转一个弯都等待模型判断。慢慢摸索出来的路，逐渐变成了熟练动作。

## 运行环境

| 组件 | 环境 |
| --- | --- |
| **游戏本体** | Steam Deck 桌面模式，WoW Forever 测试客户端；最近实测版本为 **1.60.1，build 70245**。 |
| **Runtime** | 在 Steam Deck 本地运行。 |
| **Agent** | Ubuntu WSL 中的 Codex，通常使用 **GPT-6 Astra 高 / 极高**。 |

Ubuntu WSL 是最好的 Linux 发行版。😎

最初用过 6.1 Max / Ultra，但当时子代理太多，整体推进感觉偏慢。Astra 对这个项目已经很够用，这是我实际开发与使用的体验。

## 源码和实现

**Wodex 仍在 WIP 阶段。** 当前仓库展示理念与实机效果，源码、实现和数据暂未公开。

暴雪规则禁止未经授权的游戏自动控制。**让 AI 控制角色同样属于自动化，也在这一禁令范围内。** 这是出于好玩的个人实验，没有获得暴雪的授权或背书。参见[暴雪最终用户许可协议](https://www.blizzard.com/en-us/legal/simple/bfbbb648-bcc6-4b78-a5c7-e2fe20c135df/blizzard-end-user-license-agreement)。

《魔兽世界》及其游戏内容归暴雪与各自权利人所有。权利说明见 [LICENSE](LICENSE)。
