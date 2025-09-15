## oh-my-logo-cjk

![预览](https://raw.githubusercontent.com/Antonoko/oh-my-logo-CJK/refs/heads/main/images/preview.jpg)

该项目能将输入的中、日、韩文本渲染为 CLI 中的像素风 ASCII 图，支持多种渐变色板。

启发自只能输入 ASCII 的 [oh-my-logo](https://github.com/shinshin86/oh-my-logo)

- CJK 友好：基于像素字体采样（非预制 ASCII），支持中、日、韩等多语种
- 从 oh-my-logo 偷了多种渐变色板与方向，支持 24-bit ANSI 颜色
- 多种像素输出模式与样式（half/full、block/shade 等）
- 开箱即用，跨平台（Windows 通过 colorama 兼容 ANSI）

---

### 安装与运行

- 安装 Python >= 3.9
- 推荐使用 [`uv`](https://github.com/astral-sh/uv) 运行本地项目

不安装到全局环境，直接在仓库根目录运行：

```bash
uv run oh-my-logo-cjk "你好世界" 7px grad-blue --color
```

也可安装为包后使用：

```bash
pip install -e .
oh-my-logo-cjk "你好世界" 7px grad-blue --color
```

---

### 如何使用

在目录下运行：
```bash
uv run oh-my-logo-cjk "你好世界" 7px grad-blue --color
```

这个玩具的命令组合为：
```text
oh-my-logo-cjk <text> [font] [palette] [options]
```
- **text** - 必填，用英文双引号包裹，例如："你好世界"
- ***font*** - 项目自带了两种字体配置： 7px, 9px, 和 7px-ja (Japanese font)。你可以通过 `fonts/fonts.json` 添加更多字体和配置。
  - 7px 和 9px 的字体默认使用了 [丁卯点阵体](https://3type.cn/fonts/dinkie_bitmap/index.html) 的 demo 版，能输出的文字有限，你可以购买完整版字体进行替换、以获得完整输出字库
  - 7px-ja 使用 門真なむ (Num Kadoma) 设计的  [美咲フォント(字形)](https://littlelimit.net/misaki.htm)，该字体允许无限次使用、复制和分发。详情请参阅 [License (许可协议)] (https://littlelimit.net/font.htm#license)
- ***palette*** - 渐变调色盘：
    - 通过命令 `--gallery --color`，可以一次性预览所有色板，方便直接选一个最喜欢的：

        ```bash
        uv run oh-my-logo-cjk "你好世界" --gallery --color
        ```
| Palette | Colors | Description |
|---------|--------|-------------|
| `grad-blue` | `#4ea8ff → #7f88ff` | Blue gradient (default) |
| `sunset` | `#ff9966 → #ff5e62 → #ffa34e` | Warm sunset colors |
| `dawn` | `#00c6ff → #0072ff` | Cool morning blues |
| `nebula` | `#654ea3 → #eaafc8` | Purple space nebula |
| `ocean` | `#667eea → #764ba2` | Deep ocean blues |
| `fire` | `#ff0844 → #ffb199` | Intense fire colors |
| `forest` | `#134e5e → #71b280` | Natural green tones |
| `gold` | `#f7971e → #ffd200` | Luxurious gold gradient |
| `purple` | `#667db6 → #0082c8 → #0078ff` | Royal purple to blue |
| `mint` | `#00d2ff → #3a7bd5` | Fresh mint colors |
| `coral` | `#ff9a9e → #fecfef` | Soft coral pink |
| `matrix` | `#00ff41 → #008f11` | Classic matrix green |
| `mono` | `#f07178 → #f07178` | Single coral color |

- ***option*** - 值得一试的额外可选项
    - `-s, --style <style>`：字符画样式
      - `none` | `simpleBlock` | `shade` | `block`（默认）
    - `-pw, --pixel-width <h|f|hf>`：像素宽度模式
        - `h`：半角（默认，扁扁的）
        - `f`：全角（方块字）
        - `hf`：两个半角营造全角视觉（兼容性更佳）
    - `-d, --direction <dir>`：颜色渐变方向
        - `vertical`（默认）| `horizontal` | `diagonal`
    - `--reverse-gradient`：反转渐变
    
    没那么值得一试的选项：
    - `--letter-spacing <n>`：字符间距（像素网格单位，整数，默认 1）
    - `-l, --list-palettes`：列出色板
    - `--color/--no-color`：强制开/关颜色（管道场景有用）
    - `--color-space <rgb|oklab>`：插值色彩空间（默认 `rgb`）

获得满意的结果后，可以将输出结果重定向保存，使用到其他 CLI 项目之中（保留颜色的终端可还原）：

```bash
uv run oh-my-logo-cjk run "你好世界" 7px grad-blue --color > art.txt

cat art.txt
```



---

### 字体与采样

- 像素字体及配置位于 `fonts/` 目录，文件 `fonts.json` 示例：

```json
{
  "fonts": [
    {
      "name": "7px",
      "path": "DinkieBitmap-7pxDemo.ttf",
      "font_size": 8,
      "grid_size": [8, 8],
      "offset": [0, -1]
    },
    {
      "name": "9px",
      "path": "DinkieBitmap-9pxDemo.ttf",
      "font_size": 10,
      "grid_size": [10, 10],
      "offset": [0, -1]
    },
    ... // 更多字体配置
  ]
}
```

- `grid_size` 定义单字在像素网格中的宽高；渲染与输出严格按该网格采样。
- 若字符在字体中缺失，将以“豆腐框”边框替代；空白字符输出为空网格。

---

### 其他示例

- 竖向渐变 + block 样式（默认）：

```bash
uv run oh-my-logo-cjk run "你好世界" 7px grad-blue --color
```

- 横向渐变 + `hf` 宽度 + shade 样式：

```bash
uv run oh-my-logo-cjk run "你好世界" 9px ocean -d horizontal -pw hf -s shade --color
```

- 反转渐变 + OKLab 插值：

```bash
uv run oh-my-logo-cjk run "你好世界" sunset --reverse-gradient --color-space oklab --color
```

- 日本語を出力するには (输出日语的方法)：

```bash
uv run oh-my-logo-cjk "こんにちは世界" 7px-ja mint --color
```

![Preview](https://raw.githubusercontent.com/Antonoko/oh-my-logo-CJK/refs/heads/main/images/preview-ja.png)


---

### 开发

- 查看帮助：`uv run oh-my-logo-cjk --help`
- 调试渐变：加 `--debug-gradient` 输出包围盒/轴信息到 stderr

