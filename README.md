# interactive-mv-skill

**Turn any song into a highly interactive web music video — an open-source skill for AI coding agents.**

An open-source skill in the Agent Skills (`SKILL.md`) format that teaches an AI coding agent how to build a complete interactive MV web page from a song: a full-screen canvas stage with code-drawn characters, karaoke lyrics synced to the exact audio timeline, tap / drag / particle interactions, and branching endings. No image assets — everything is drawn and scheduled in code.

🎬 **Live demo:** [《爱错》 (Love Wrong) — interactive MV](https://muse.ai/s/mv-xpxo6xhi1xacxlex0) — a full 3:59 song, 14 shots, 38 timestamped lyric lines, every interaction probe-verified before delivery.

## What it does

Give the agent a song (audio + lyric timeline) and it produces a single-page interactive MV with:

- A full-screen canvas stage; characters are pure draw functions, so they stay visually consistent across every shot
- Karaoke lyrics with per-character fill, plus a tappable lyric list — tap any line and the song seeks there
- Tap the stage to burst particles; press and drag to create wind that bends particles, scarves and a live wind meter
- Tap characters for dialogue bubbles; collectible items with counters; a weather switcher; full-screen particle rain; floating hearts; danmaku (bullet comments) flying across the stage
- A branching ending near the end of the song (e.g. "forgive him" vs. "let it drift away"), with a replay option to try the other branch
- Live interaction stats (wind count, hearts sent, …) that make the page feel alive

## Why it works — the hard parts are already solved

1. **Audio first.** Re-encode the song to 128 kbps (≈10 MB → 4 MB) so mobile networks can actually stream it, then verify the duration and embedded lyric tags survive — a one-second drift breaks all lyric sync.
2. **One clock.** Shots, lyric highlighting, karaoke fill and particle systems all derive from a single `requestAnimationFrame` loop reading `audio.currentTime`, so seeking stays in sync for free.
3. **Playback never fails silently.** A loading state on the play button, auto-start when the audio is ready, automatic re-fetch on stall. (A silent `play()` catch was the real bug that once made taps feel dead.)
4. **Probe-verified delivery.** Before handoff, an automated probe taps play, seeks via the lyric list and triggers the ending — interactions are verified, not assumed.

## Case study

**Wang Leehom — 《爱错》:** 3 min 59 s of original audio driving 14 shots and 38 precisely timestamped lyric lines. Play/pause, lyric seeking, both ending branches and the interaction counters all passed probe verification. Delivered both as a public web page (the live demo above) and as a single self-contained HTML file for sharing inside chat apps.

Companion skills by the same author: [cartoon_mv](https://github.com/wdu496132-rgb/cartoon_mv) — pure-code 2D cartoon MVs rendered to MP4 — and a pixel-art MV skill (publication in progress). Together they cover the interactive, cartoon and pixel-art styles of code-generated music video.

## Install

Clone this repo into your agent's skills directory, e.g. for Muse:

```bash
git clone https://github.com/wdu496132-rgb/interactive-mv-skill ~/workspace/custom-skills/interactive-mv
```

`SKILL.md` follows the open Agent Skills format used by Muse, Claude Code, Codex and other compatible agents. Then just ask your agent: *"Make an interactive MV for this song."*

## Files

- `SKILL.md` — the skill itself: the full build procedure the agent follows
- `skill.yaml` — skill metadata (name, description, category)
- `LICENSE` — MIT License

## License

MIT © 2026 Du Wei ([@wdu496132-rgb](https://github.com/wdu496132-rgb))

---

## 中文说明

一个可复用的开源 Skill：**根据一首歌，做出可交互的网页版 MV**。

给一首歌（音频 + 歌词时间轴），AI 按本 Skill 的流程生成一个高交互网页 MV：

- 全屏 Canvas 舞台，角色全部由代码绘制，不用任何图片素材
- 大字卡拉 OK 歌词逐字跟唱；歌词列表可点击，点哪句跳哪句
- 点画面惊起粒子、按住拖拽生风（粒子 / 围巾 / 风力条联动）
- 点角色弹出台词、收集金叶子、换天气、落叶雨、送心动、发弹幕
- 结尾双分支结局（如"原谅他" vs "随风错过"），可重选重玩

**实测案例：** 王力宏《爱错》交互 MV（演示链接见顶部）——3 分 59 秒原音频驱动，14 个镜头、38 句精确时间轴，全部交互经探针实测验证（播放 / 暂停、歌词跳转、双结局分支均通过）。

**做法要点（已沉淀在 SKILL.md 中）：**

1. 音频先压到 128kbps（10MB → 4MB），手机网络才带得动；时长与歌词标签必须分毫不差
2. 播放失败不能静默吞掉：显示"加载中"，到位自动开播，卡住自动重拉
3. 全站由 `audio.currentTime` 驱动的单循环调度，跳转天然同步
4. 交付前探针验证：点播放时间走、点歌词能跳、结局能进

**安装：** 把本仓库放到 Muse 的 custom skills 目录（见上方英文命令），之后对 AI 说"做个交互 MV"即可按这套方法开工。

**许可证：** MIT
