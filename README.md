**简体中文** · [English](README.en.md)

# 方块主义 BLOCKISM
> 一款致敬《Thomas Was Alone》的构成主义 2D 平台跳跃游戏。极简几何、锐利棱角、红黑纸三色,用 Godot 4.7 / GDScript 搭起来。

仓库名 `blockism`(原 `geometric-construct`)。游戏原名《几何构成 / Geometric Construct》。

## 这是什么
《方块主义 BLOCKISM》是一款单人为主、附带双人竞速的 2D 平台跳跃游戏。玩家在名册中的几何体之间切换,每个几何体只有一个主动词——速度、弹性、置换。16 个战役关卡分四幕推进,另配一个新手教程关与一个双人竞速专关。

- 引擎:Godot 4.7(标准版)/ GDScript 2.0
- 视口:1280×720,`canvas_items` 拉伸 + `expand` 宽高比;桌面最小窗口 960×540;支持移动端横竖屏
- 平台:PC(键鼠 / 手柄)与 Android(触屏)
- 美术:全程序化绘制(`_draw` 配方 + 少量 `canvas_item` shader),取色只经 `data/palette.tres`

## 美术方向
极简主义 + 构成主义 + 几何 + 锐利:

- 禁圆角、禁渐变、禁柔影
- 平面色块、细线、硬边直角投影、大号编号
- 红 / 黑 / 纸三色主视觉

色板(`data/palette.tres`):`ink #101216`、`paper #EDEAE0`、`red #E0492F`、`yellow #E8B33A`、`blue #4E86D8`、`orange #E07E2E`;另有机关语义槽 `buff #4E9E58`(增益 / 安全)与 `cool #45B0BE`(冷环境)。

## 四个几何体
设计上有四个几何体,属性标尺 0.0–2.0,重量关系为 黄 > 红 > 橙 > 蓝。当前名册实装三个(橙球已在开发中退役)。

| 名 | 形 | 特性 | 重量 |
|---|---|---|---|
| 疾(今名「红」) | 红色正方形 | 速度型:冲刺(按住 Shift,目标速度 1.5× ≈ 4.5 格/秒)、二段跳(每跳 2.0 格)、贴墙攀爬(单次至多 2.0 格) | 1.0 |
| 簧(今名「黄」) | 黄色竖长方形 | 弹性型:反弹率 100%(按住跳跃主动发力弹更高)、同伴踩其头顶起跳跳高 ×2 | 1.5 |
| 坠(今名「蓝」) | 蓝色倒悬正方形 | 置换型:按跳跃键在天与地之间翻转,水平惯性守恒;基础速度 0.5,过加速门后上限 2.0× | 0.2 |
| 转(橙球) | 橙色圆球 | 滚动型:以滚动前进 —— **已退役,不在当前名册** | — |

> 名称沿革:疾 / 簧 / 坠 / 转 → 疾 / 跃 / 逆 / 圆 → 红 / 黄 / 蓝(三体定册)。圆形与第五体(双体三角形)先后清退,现名册为三个几何体。

## 机制
机关(源码 slug):

- 加速门 `speed_gate` —— 门内冲刺上限提升
- 限时桥 `timed_bridge` —— 踩过即塌,定时回归
- 共鸣踏板 `resonance_pedal` —— 非活跃同伴驻板,为活跃体显形一条纸桥
- 钢琴砖 `piano_tile` —— 踩踏 / 滚过发声,音级随高度反向映射
- 滑雪带 `ski_patch` —— 加速滑行带
- 移动平台 `mover`
- 记录点 / 信标 `checkpoint_beacon`
- 出口门 `exit_door`

另有世界道具:提示牌 `hint_marker`、出生标记 `spawn_marker`。

## 幕与关卡
四幕,每幕 4 关,共 16 个战役关卡;另有新手教程关、双人竞速专关、以及开发用门禁探针关。

| 幕 | 题 | 主题色 | 关卡目录 |
|---|---|---|---|
| 第一幕 · 各自的路上 | 红与黄的入门四场,终场三扇门并立 | blue | `levels_native/act1/` |
| 第二幕 · 分岔 | 三条独路,在路口并拢 | yellow | `levels_native/act3/` |
| 第三幕 · 蜕变 | 三次反着来的路 | orange | `levels_native/act4/` |
| 第四幕 · 刻度的真相 | 三门归位,落幕 | red | `levels_native/act5/` |

> 关卡目录号与幕序不一致:旧 `act2/` 已退役、幕档前移(第一幕 = `act1/`、第二幕 = `act3/`、第三幕 = `act4/`、第四幕 = `act5/`)。`levels_native/` 下另有 `tutorial/`、`duel/`、`dev/`,以及 `_template.tscn` 作关模板。

音乐 `data/music/`:prologue + act1–4 共五条音轨。背景 `data/backdrop/`:act1–4 + menu。

## 操作
| 动作 | 键盘 | 手柄 | 触屏 |
|---|---|---|---|
| 移动 | A / D、← / → | 左摇杆 / 十字键 | 左下轮盘(轻推慢走、拉满加速) |
| 跳跃 · 二段跳 | 空格 / W / ↑(轻点小跳、长按全高) | A 键 | 点按小跳、长按全高 |
| 置换(蓝) | 空格 | A 键 | 点按屏幕 |
| 冲刺 | Shift(按住) | X 键 | — |
| 贴墙攀爬 | 空格(贴墙按住) | — | 贴墙长按屏幕 |
| 切换几何体 | Tab / E / Q | RB / LB | 点按左上队伍芯片 |
| 名册直达 | 1–3 | — | — |
| 召回 | R | Back(View / Select) | 右上按钮行 |
| 暂停 | Esc / P | Start | 右上按钮行 |

界面导航:主菜单 `1–4` 选剧目,`K` 键位指南、`C` 档案几何、`S` 设置、`Esc` 退出;`Enter` / `空格` 确认,`Esc` 返回(手柄 A 确认、B 返回)。触屏全部界面均有可点按的按钮 / 页签,无键盘依赖。轮盘布局可在设置面板切换「固定 / 浮动」。

## 运行
前置:Godot 4.7(标准版)。下面命令中的 `godot` 请替换为你的 Godot 4.7 可执行文件。

```bash
# 1) 首次导入资源(注册 class_name、生成 .godot/ 缓存)
godot --headless --path . --import

# 2) 运行游戏(主场景 res://scenes/Main.tscn)
godot --path .

# 3) 无头冒烟测试(以退出码为准)
godot --headless --path . res://tests/native_check.tscn
```

> 注意:项目在 `project.godot` 中注册了自动加载 `_mcp_game_helper` → `res://addons/godot_ai/runtime/game_helper.gd`。该插件是本地 AI / MCP 辅助工具,**未纳入版本库**(见 `.gitignore` 的「第三方插件」段)。新克隆需自行补装该插件,或先删除该自动加载行,游戏才能正常启动。

## 项目结构
```
addons/editor_kit/     第一方编辑器插件(机关坞)
assets/
  brand/               应用图标(192 / 432,前景 / 背景 / 单色)
  editor/              编辑器用机关 SVG 图标
  fonts/               Noto Sans SC 可变字体 + OFL 许可
  tiles/               ground / platform / decor / special 四张分类图集
data/
  backdrop/            act1–4 + menu 背景资源
  characters/          dash / spring / fall 三个几何体 .tres
  music/               prologue + act1–4
  sfx/                 音效 .tres
  tiles/               native_tileset.tres
  tuning/              movement_default.tres
  palette.tres         色板
levels_native/         关卡 .tscn(act1 / act3 / act4 / act5 + tutorial + duel + dev)+ _template
scenes/                Main.tscn 与 core / entities / world / ui / net / fx / tutorial / art 场景
scripts/               GDScript(art / core / data / dev / entities / fx / net / tutorial / ui / world)
shaders/               light_streaks / speed_lines / tile_theme
tests/                 无头门禁场景
tools/                 审计与生成脚本
project.godot  export_presets.cfg  icon.png  CHANGELOG.md
```

## 编辑器插件
`addons/editor_kit` —— 随库发布的第一方 Godot 编辑器插件(`@tool EditorPlugin`):

- 停靠面板:机关摆放表(`dock_panel.gd` + `placement_table.gd`)
- 视口内叠加绘制:经 `_forward_canvas_draw_over_viewport` 绘制移动平台行程箭头与加速门感应虚线框

项目已在 `project.godot` 注册并启用该插件。

另:`addons/godot_ai` 是本地 AI / MCP 辅助插件(自动加载 `_mcp_game_helper`),仅供开发使用,未纳入版本库,也不随游戏发行。

## 测试与门禁
仓库**没有 CI 流水线**(无 `.github/` 工作流)。质量门禁是一组可无头运行的 Godot 场景,以文本断言输出、退出码为准:

- `tests/`:`flow_check`(过关链)、`native_check`(逐关装载)、`stats_check`、`focus_check`、`trait_check`、`ambience_check`、`transition_check`、`replay_transition_check`、`mech_resonance`、`tutorial_check`,以及截图工具 `shot_all` / `win_shot`
- `tools/`:关卡审计、门位审计、热路径审计、键位对账、图集物理检查、字体覆盖检查、瓦片 / 品牌图标生成、废物清理等

## 已知限制
- 名册为三个几何体;设计上的第四个「橙球 · 转」已退役。
- 游戏标题在 `project.godot` / `scripts/core/version.gd` 中仍为「几何构成 / GEOMETRIC CONSTRUCT」;本仓库正在更名为 `blockism`(《方块主义 BLOCKISM》)。
- 同屏双人 / 竞速的分区操作需要键鼠或双手柄;触屏设备不支持同屏双人。
- 剧情 / 对白系统、肉鸽模式、震动反馈均已在早期版本退役,不在当前构建中。
- 唯一已发布的构建物是调试用 Android APK(见 Releases,最新标签 `v0.70.0`);没有正式签名的商店发布。
- 运行依赖未入库的第三方插件 `addons/godot_ai`(自动加载),新克隆需补装或移除该自动加载行。
- 仓库根目录没有 `LICENSE` 文件(见下)。

## 许可证
仓库根目录没有 `LICENSE` 文件,因此默认「保留所有权利」(all rights reserved),除非仓库所有者另行声明。

随库字体 `assets/fonts/NotoSansSC-VF.ttf`(Noto Sans SC Variable)按 **SIL Open Font License 1.1** 授权,详见 `assets/fonts/LICENSE.txt`。
