<div align="center">

**English　|　[简体中文 →](README.zh-CN.md)**

# Wodex: An Autonomous Agent for WoW

**Plan → Act → Observe → Learn**

</div>

Wodex is an autonomous agent that interacts with World of Warcraft **like a human player**, through **Plan → Act → Observe → Learn**—for exploration, questing, combat, commerce and more. It decides where to go, works out what to do, and learns from what happens.

**It observes the screen and uses ordinary player controls.** Wodex does not read or write game memory, or reverse-engineer game network protocols to control the character.

With each exploration, the agent becomes more practiced. It saves useful discoveries as knowledge and methods it can use again. Finding an unfamiliar doorway may take minutes; returning through it can take seconds. **Experience becomes something the agent can execute.**

## In-game demos

### ⚔️ Demo 01 · Level 1 → 5

**The AI can accept quests, navigate, fight and level without step-by-step gameplay guidance.** In my early orc experiment, nobody had to tell it which quest to take, where to turn or which monster to attack. Basic computer use—looking at the screen and operating a simple keyboard controller—was already enough to complete the gameplay loop. It was slow, but autonomous.

These key screenshots capture a later undead paladin's **level 1–5 development run**:

| Create a character | Fight | Reach level 5 |
| --- | --- | --- |
| ![A new undead paladin](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/leveling-start.png) | ![The paladin fighting a spider](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/leveling-combat.png) | ![The in-game level 5 milestone](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/leveling-complete.png) |
| A fresh character starts its journey. | Target, approach, attack and loot become a local feedback loop. | The character reaches level 5. |

The next challenge is efficiency: letting the AI make meaningful decisions while the Runtime handles repeated actions locally.

---

### 🧭 Demo 02 · Free exploration, then fast reuse

The task: **go from the mailbox to the innkeeper in Thunder Bluff, then reuse what you learned.**

| Autonomous exploration · about 8 minutes | Reusing the experience · 24.1 seconds |
| --- | --- |
| The AI reads maps, tries approaches, takes a detour and encounters two doorway obstacles before finding the entrance. | It submits the learned approach as one five-step Plan. The Runtime completes the trip and opens the innkeeper's dialogue. |
| ![Real-time excerpt of autonomous exploration](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/exploration.gif) | ![Real-time excerpt of the learned trip](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/reuse.gif) |
| [Full exploration recording](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/exploration.mp4) | [Full reuse recording](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/reuse.mp4) |

The AI saved entrance waypoints and an interaction procedure. On the next trip, the Runtime carried out all five steps locally. No human took over the controls in either sequence. Existing street knowledge supplied the starting segment; the inn approach was discovered during this run.

*Same character, same starting area, same destination. GIFs are real-time excerpts. Exploration time includes reasoning, tools and waiting; 24.1 seconds is the second trip's Runtime execution time.*

---

### 🏙️ Demo 03 · A 29-step trip around Thunder Bluff

Once familiar with the area, the agent can visit a general goods vendor, repair vendor, auctioneer and banker, open their service windows, and return to the start—all in **one Plan, 29 steps, 149 seconds**.

![The complete 29-step city trip at 4x playback speed](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/city-tour.gif)

*Full trip at 4× speed. [Watch at original speed](https://github.com/simpx/wodex/releases/download/showcase-2026-10-07/city.mp4).*

## What's going on?

Wodex gives the AI a **WoW Runtime** with two fundamental capabilities: **Act and Observe**. The AI plans; the Runtime executes, checks live feedback and reports the result; the AI learns from it and decides what comes next.

The Runtime's interface to the game is **screen recognition and ordinary input**. It reads screenshots and visible on-screen state, then operates the game through keyboard and mouse input—the same player-facing interface used during normal play. It does not read or write the game's memory, or intercept, parse or construct game network packets.

| Part | Responsibility |
| --- | --- |
| **AI** | **Plan and Learn**: choose goals, interpret unfamiliar situations, revise plans and turn discoveries into useful knowledge. |
| **Runtime** | **Act and Observe**: read visible screen output, send ordinary input, and complete navigation, movement, interaction and combat loops; return structured results and screenshots when needed. |
| **Skills** | Reusable methods for operating in the world and handling recurring tasks. |
| **Knowledge** | The agent's accumulated sources, analysis and experience. Its navigation knowledge lives in an **Atlas** of paths, areas and points of interest. |

Exploration leaves a trail: original observations, analysis, then reusable knowledge. A map suggests a road; walking it reveals whether the approach works; the result improves the Atlas. Failed attempts remain useful evidence for the next plan.

Learning happens in this growing body of knowledge and methods. Familiar trips can then run as complete local sequences, without another model decision at every turn. That is how slow exploration becomes practiced action.

## Setup

| Component | Environment |
| --- | --- |
| **Game** | Steam Deck, desktop mode; WoW Forever test client. Most recently tested: **1.60.1, build 70245**. |
| **Runtime** | Running locally on the Steam Deck. |
| **Agent** | Codex in Ubuntu on WSL, usually **GPT-6 Astra at High / XHigh**. |

Ubuntu WSL is the best Linux distribution. 😎

I started with 6.1 at Max/Ultra, but those sessions spawned too many subagents and felt slow overall. Astra has been plenty for this project. That is my experience building and running it.

## Source and implementation

**Wodex is a work in progress.** This repository presents the concept and in-game demonstrations; the implementation and data remain private for now.

Blizzard's rules prohibit unauthorized automated control of the game. **Having an AI control the character is still automation and falls under that prohibition.** This is a personal experiment made for fun, without Blizzard's authorization or endorsement. See the [Blizzard EULA](https://www.blizzard.com/en-us/legal/simple/bfbbb648-bcc6-4b78-a5c7-e2fe20c135df/blizzard-end-user-license-agreement).

World of Warcraft and its game content belong to Blizzard and their respective rights holders. See [LICENSE](LICENSE) for the rights notice.
