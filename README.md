# 终端配置备份使用说明

## 图标预览

### Ghostty 图标
<img src="ghostty/ghostty.png" alt="Ghostty Icon" height="200">

### Kitty 图标

<div>
  <img src="kitty/kitty-light.png" alt="Kitty Light" height="200" style="margin-right: 20px;">
  <img src="kitty/kitty-dark.png" alt="Kitty Dark" height="200">
</div>

> Kitty 图标使用自: [DinkDonk/kitty-icon](https://github.com/DinkDonk/kitty-icon)

---

## Ghostty 配置恢复

### 1. 恢复配置文件
```bash
mkdir -p ~/.config/ghostty
cp ghostty/config ~/.config/ghostty/
```

### 2. 恢复图标
```bash
mkdir -p ~/Pictures/icons
cp ghostty/ghostty.png ~/Pictures/icons/
cp ghostty/ghostty.icns ~/Pictures/icons/
```

### 3. 配置说明
- 图标已设置为 85% 比例，保持原始宽高比（427x512）
- 周围有透明填充，不会变形
- 配置文件中已设置自定义图标路径

## Kitty 配置恢复

### 1. 恢复配置文件
```bash
mkdir -p ~/.config/kitty
cp -r kitty/* ~/.config/kitty/
```

### 2. 恢复图标
```bash
mkdir -p ~/Pictures/icons
cp kitty/kitty-*.icns ~/Pictures/icons/
```

## 注意事项

- 恢复后需要重启对应的终端应用
- 如需刷新图标，运行：`killall Dock`
- 如果图标路径不同，需要修改配置文件中的路径

## 图标调整脚本

如果需要重新调整 Ghostty 图标大小，使用以下 Python 脚本：

```python
from PIL import Image
import os

SCALE_FACTOR = 0.85  # 修改这个值调整大小

source_img = Image.open("ghostty.png")
original_width, original_height = source_img.size

output_iconset = "ghostty_custom.iconset"
os.makedirs(output_iconset, exist_ok=True)

sizes = {
    "icon_16x16.png": 16,
    "icon_16x16@2x.png": 32,
    "icon_32x32.png": 32,
    "icon_32x32@2x.png": 64,
    "icon_128x128.png": 128,
    "icon_128x128@2x.png": 256,
    "icon_256x256.png": 256,
    "icon_256x256@2x.png": 512,
    "icon_512x512.png": 512,
    "icon_512x512@2x.png": 1024,
}

for filename, canvas_size in sizes.items():
    icon_height = int(canvas_size * SCALE_FACTOR)
    aspect_ratio = original_width / original_height
    icon_width = int(icon_height * aspect_ratio)

    resized_icon = source_img.resize((icon_width, icon_height), Image.Resampling.LANCZOS)
    canvas = Image.new('RGBA', (canvas_size, canvas_size), (0, 0, 0, 0))

    offset_x = (canvas_size - icon_width) // 2
    offset_y = (canvas_size - icon_height) // 2
    canvas.paste(resized_icon, (offset_x, offset_y), resized_icon)

    canvas.save(os.path.join(output_iconset, filename), 'PNG')

# 然后运行：iconutil -c icns ghostty_custom.iconset -o ghostty.icns
```
