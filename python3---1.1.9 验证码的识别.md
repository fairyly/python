## 图形验证码识别

>利用 OCR 技术识别图形验证码

>识别图形验证码需要库 tesseroc 




## 测试

```
import tesserocr
from PIL import Image

image = Image.open('')
result = tesserpcr.image_to_text(image)
print(result)
```