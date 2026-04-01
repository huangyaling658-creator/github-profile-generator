# 🌸 GitHub Profile Generator

一键生成精美的 GitHub 个人主页，只需改配置文件，即可生成完整的 Banner、作品卡片、技能图谱等所有视觉资产和 README。

<div align="center">
<img src="https://raw.githubusercontent.com/huangyaling658-creator/huangyaling658-creator/main/assets/header-banner.png" width="90%"/>
</div>

## ✨ 效果预览

| 板块 | 说明 |
|------|------|
| 🎨 顶部 Banner | 渐变背景 + 花体字名字 + 人物角色 + 爱心装饰 |
| 📝 个人介绍 | 原生 Markdown，职位/方向/座右铭 |
| 🃏 作品卡片 | 粉绿主题宝藏卡，圆角渐变 + 首字图标 |
| 📊 技能图谱 | 进度条可视化 + 人物背景 |
| 🗺️ 成长路线 | 时间轴节点，里程碑记录 |
| 📈 活动足迹 | GitHub 贡献活动图 |

## 🚀 快速开始

### 1. 创建 Profile 仓库

在 GitHub 上创建一个**与你用户名同名**的仓库（如用户名 `alice`，则创建 `alice` 仓库）。

### 2. 克隆并复制文件

```bash
# 克隆你的 Profile 仓库
git clone https://github.com/你的用户名/你的用户名.git
cd 你的用户名

# 复制工具文件
cp /path/to/github-profile-generator/generate_profile.py .
cp /path/to/github-profile-generator/profile_config.yaml .
```

### 3. 安装依赖

```bash
pip3 install Pillow pyyaml
```

### 4. 准备人物图片

将你的人物图片放入 `assets/` 目录：
- **Banner 人物图** — 推荐 PNG 透明背景，1024×1024 以上
- **技能背景图** — 推荐 PNG，1200×650 以上

### 5. 修改配置文件

编辑 `profile_config.yaml`，替换成你自己的内容：

```yaml
github:
  username: "你的用户名"

banner:
  name: "YourName"                          # 英文名
  subtitle: "你的一句话介绍"                  # 副标题
  character_image: "assets/你的人物.png"      # 人物图

about_me:
  - emoji: "🍁"
    label: "Position"
    value: "你的职位"

projects:
  cards:
    - name: "项目名"
      desc: "一句话描述"
      tech: "Python"
      repo: "仓库名"
      color_top: [245, 180, 195]
      color_bottom: [235, 155, 175]

skills:
  items:
    - name: "技能名"
      percent: 80
      color: [232, 130, 154]
      desc: "关键词描述"
```

### 6. 一键生成

```bash
python3 generate_profile.py
```

脚本会自动生成：
- `assets/header-banner.png` — 顶部横幅
- `assets/projects/card-*.png` — 作品卡片
- `assets/skills-timeline.png` — 技能图谱
- `README.md` — 完整的个人主页

### 7. 推送

```bash
git add -A
git commit -m "update profile"
git push origin main
```

## 🎨 自定义指南

### 颜色配置

所有颜色均为 `[R, G, B]` 格式（0~255）：

| 位置 | 配置项 | 说明 |
|------|--------|------|
| Banner 背景 | `banner.gradient_start/end` | 渐变起止色 |
| 作品卡片 | `cards[].color_top/bottom` | 每张卡片渐变色 |
| 技能进度条 | `skills.items[].color` | 进度条颜色 |
| 时间节点 | `timeline.items[].color` | 节点和标题颜色 |
| 技能蒙层 | `skills.overlay_color` | RGBA 蒙层色 |
| 活动图 | `activity.*_color` | 十六进制色值 |

### 尺寸配置

```yaml
style:
  banner_width: 1200    # Banner 宽
  banner_height: 300    # Banner 高
  card_width: 340       # 卡片宽
  card_height: 150      # 卡片高
  skills_width: 1200    # 技能图宽
  skills_height: 650    # 技能图高
```

### 增减作品

在 `projects.cards` 列表中增删项目即可，脚本会自动生成对应数量的卡片，每行 3 个。

### 更换人物

1. 新图片放入 `assets/`
2. 修改 `profile_config.yaml` 中的路径
3. 重新运行 `python3 generate_profile.py`

## ⚠️ 注意事项

- 人物图片推荐 **PNG 透明背景**，效果最佳
- GitHub 会剥离 CSS/style 属性，所以所有样式都通过图片生成实现
- 字体自动匹配系统字体，macOS / Linux / Windows 均兼容
- 作品卡片圆形图标会自动取项目名首字（中英文均支持）
- 依赖：`Python 3.7+`、`Pillow`、`PyYAML`

## 📄 License

MIT License — 随意使用和修改 🎉
