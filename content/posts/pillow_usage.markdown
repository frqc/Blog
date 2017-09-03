# install 
just pip

# basic operation
```python
from PIL import Images as img
my_image = img.open("filename")
```

# 保存

```python
my_image.save(filename)
```

# 图片基本信息

```python
width = my_image.size[0]
depth = my_image.size[1]
```

# 裁剪

```python
cropped_img = my_img.crop(x_start, y_start, x_length, y_length)
```

# 标记文字

```python
from PIL import ImageFont
font = ImageFont.truetype('Arial.ttf', 36)
my_img.text((10, 25), "world", font=font)
```

# 打框

```python
im = Image.new("RGB", (100, 200), (255,0,0))
dr = ImageDraw.Draw(im)
	
dr.rectangle(((0,0),(50,50)), fill=None, outline = "blue") # the width cannot be set 
	
im.save("rectangle.png")
```