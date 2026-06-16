---
name: github-profile
description: 对话式生成 / 美化 GitHub 个人主页。通过提问收集信息，自动生成顶部 Banner、作品卡片、技能图谱、成长路线和完整 README，一键产出全部视觉资产并可推到用户的 profile 仓库。当用户说"美化 GitHub 主页"、"优化我的 github 主页"、"把 GitHub 首页弄漂亮"、"做个 github profile"、"生成 GitHub 个人主页"等时触发。
---

# GitHub 主页生成器 SOP

把原来"用户自己装 Python + 手动改 YAML + 跑脚本"的工具，变成**对话式一键生成**：你（agent）负责问、填、跑、交付，用户只管回答几个问题。

`SKILL_DIR` = 本 skill 所在目录（含 `generate_profile.py` 和 `profile_config.template.yaml`）

## 工具依赖
- Python3 + Pillow + PyYAML。执行生成前先跑 `pip3 install -q Pillow pyyaml`（已装会秒过）。
- 中文渲染靠系统中文字体（macOS 自带 PingFang，通常没问题）。

---

## 执行流程

### Step 1：开场 + 分组提问（关键：像聊天一样，分组问，别一次性把所有问题砸给用户）
逐组问，每组确认后再进下一组。用户没想法的字段，**主动给建议默认值**让他确认即可。

**A. 基本信息**
- GitHub 用户名？（profile 仓库一般与用户名同名）
- 显示的名字（英文，会用花体渲染）？打招呼语（默认 `Hi, I'm`）？一句话副标题？

**B. 个人介绍（About Me，约 3 条）**
- 职位 / 方向 / 座右铭，每条配一个 emoji。

**C. 作品卡片（最重要）**
- 要展示哪些项目？每个：名字、一句话描述、技术栈、GitHub 仓库名（用于链接）。
- **颜色不要问用户**，你按"粉 / 绿交替"的宝藏卡风格自动分配（示例 RGB：粉 top[245,180,195]/bottom[235,155,175]，绿 top[190,215,120]/bottom[165,200,95]，交替循环）。
- 可主动帮用户拉取仓库列表参考：`gh repo list <用户名> --limit 50 --json name,description`。

**D. 技能图谱（4-6 项）**
- 技能名、熟练度百分比(0-100)、一句话描述。进度条颜色你自动配（粉/绿/橙/紫系）。

**E. 成长路线（时间轴，4-5 个里程碑）**
- 日期、标题、1-2 行说明。

**F. 主题 & 人物图（可选）**
- 整体色调偏好（默认深紫 → 粉紫渐变 `gradient_start:[75,45,95]` → `gradient_end:[120,75,115]`）。
- 有没有一张人物 / 插画 PNG 想放在 Banner 上？
  - 有 → 要文件路径（透明背景 PNG 最佳），技能图背景也可放一张。
  - **没有 → 明确告诉用户"可以不放，Banner 会用纯渐变背景，一样好看"**，把 `character_image` 和 `background_image` 设为空字符串即可（脚本会自动跳过，不会报错）。

### Step 2：写配置
以 `SKILL_DIR/profile_config.template.yaml` 为模板，把用户答案逐字段替换，写到一个工作目录的 `profile_config.yaml`。作品卡片/技能颜色按上面的配色规则自动填。

### Step 3：生成
1. `pip3 install -q Pillow pyyaml`
2. 把 `SKILL_DIR/generate_profile.py` 和填好的 `profile_config.yaml` 放在同一个工作目录，`cd` 过去
3. 跑 `python3 generate_profile.py`
4. 产出：`assets/header-banner.png`、`assets/skills-timeline.png`、`assets/projects/card-*.png`、`README.md`

### Step 4：交付 + 上线
- 给用户看生成的 `README.md` 和图片预览。
- 问要不要直接推到他的 GitHub profile 仓库（与用户名同名的仓库；没有就帮他建 `gh repo create <用户名> --public`）：
  - 把 `assets/` 和 `README.md` commit + push 到该仓库 `main`。
  - 推完给出主页链接 `https://github.com/<用户名>`，让他刷新查看。

---

## 铁律
- **不要让用户碰 YAML 或命令行**——所有配置由你根据对话自动写，所有命令由你执行。
- 人物图 / 背景图缺失要优雅降级（纯渐变背景），生成前先检查文件是否存在。
- 作品卡片保持"粉绿交替宝藏卡"风格；README 由脚本自动生成，不要手写。
- 一组一组问，给默认值，降低用户决策负担。
