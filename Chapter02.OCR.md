# OCR

<!-- TOC -->

- [OCR](#ocr)
    - [Tesseract](#tesseract)
    - [pytesseract](#pytesseract)
        - [tesseract training preprocessing](#tesseract-training-preprocessing)
        - [picture processing](#picture-processing)
    - [BaiduAI](#baiduai)

<!-- /TOC -->

## Tesseract

download [Tesseract](https://github.com/UB-Mannheim/tesseract/wiki), from [Tesseract Data-file](https://github.com/tesseract-ocr/tesseract/wiki/Data-Files) download [chi_sim and chi_sim_vert](https://github.com/tesseract-ocr/tessdata_best)

```bash
# result
tesseract-ocr-w32-setup-v4.0.0-rc3.20181014.exe
chi_sim.traineddata
chi_sim_vert.traineddata
```

- Install tesseract
- move **chi_sim.traineddata** & **chi_sim_vert.traineddata** to **InstallationDir/tessdata/**
- Add **InstallationDir** to environment
- run in comand line: 
    - english: `tesseract chn.png result`
    - chinese: `tesseract chn.png result -l chi_sim`

```python
# English OCR
import subprocess

p=subprocess.Popen(['C:\\Program Files (x86)\\Tesseract-OCR\\tesseract.exe','c:\\3.jpg','c:\\result'], 
                    stdout=subprocess.PIPE,
                    stderr=subprocess.PIPE)
p.wait()
with open('c\\result.txt', 'r') as file:
    print(file.read())
```

```python
# Chinese OCR
import subprocess

p=subprocess.Popen(['C:\\Program Files (x86)\\Tesseract-OCR\\tesseract.exe','c:\\3.jpg','c:\\result', '-l', 'chi_sim'], 
                    stdout=subprocess.PIPE,
                    stderr=subprocess.PIPE)
p.wait()
with open('c\\result.txt', 'r') as file:
    print(file.read())
```

> tesseract训练数据, [example](https://blog.csdn.net/business122/article/details/79174774)

## pytesseract

```python
# English OCR
from PIL import Image
import pytesseract

pytesseract.pytesseract.tesseract_cmd = 'C:/Program Files (x86)/Tesseract-OCR/tesseract.exe'
tessdata_dir_config = '--tessdata-dir "C:/Program Files (x86)/Tesseract-OCR/tessdata"'

img=Image.open('eng.png')
pytesseract.image_to_string(img, config=tessdata_dir_config)
```

```python
# Chinese OCR
from PIL import Image
import pytesseract

pytesseract.pytesseract.tesseract_cmd = 'C:/Program Files (x86)/Tesseract-OCR/tesseract.exe'
tessdata_dir_config = '--tessdata-dir "C:/Program Files (x86)/Tesseract-OCR/tessdata"'

img=Image.open('chn.png')
pytesseract.image_to_string(img, config=tessdata_dir_config, lang='chi_sim')
```

### tesseract training preprocessing

采用tesseract训练captcha需要预处理图片方便识别，并且为tesseract生成`.box`文件

example1: preprocessing pictures

```python
from PIL import Image, ImageFilter, ImageEnhance
img=Image.open('temp.jpg').convert('L') # convert to greyscale
img=img.filter(ImageFilter.MaxFilter) # filter noise
img=ImageEnhance.Contrast(img).enhance(5) # increase contrast
img.show()
```

```python
from PIL import Image
img=Image.open('temp.jpg').convert('L') # convert to greyscale
img=img.point(lambda x:0 if x<150 else 255) # increase contrast
img.show()
```

```python
# 清除部分干扰线
from PIL import Image

def denoise(img, threshold):
    data=img.load() # 这样才能用下标
    w, h=img.size
    for y in range(1, h-1):
        for x in range(1, w-1):
            count=0
            if data[x, y-1]>threshold:
                count+=1
            if data[x, y+1]>threshold:
                count+=1
            if data[x-1, y]>threshold:
                count+=1
            if data[x+1, y]>threshold:
                count+=1
            if count>2:
                data[x,y]=255
    return img

img=Image.open('myx.png').convert('L')
img=denoise(img, 245)
img.show()
```

example2: generate `.box` file
> 采用`tesseract mjorcen.normal.exp.tif mjorcen.normal.exp batch.nochop makebox`生成的box文件不准确，矫正的过程相当于修改box文件，所以采用直接写box文件

```python
import string
import random
from PIL import Image, ImageDraw, ImageFont


def get_random_string(character_number):
    string_list = [random.choice(string.ascii_letters+string.digits)
                   for _ in range(character_number)]
    return ''.join(string_list)


def draw_random_line(draw, width, height):
    begin = (random.randint(0, width), random.randint(0, height))
    end = (random.randint(0, width), random.randint(0, height))
    draw.line([begin, end], fill=get_random_color(), width=3)


def get_random_color():
    return (random.randint(30, 100), random.randint(30, 100), random.randint(30, 100))


def get_verify_code():
    width, height = 240, 60
    im = Image.new('RGB', (width, height), (180, 180, 180))
    draw = ImageDraw.Draw(im)

    # draw text
    text = get_random_string(4)
    font = ImageFont.truetype('arial.ttf', height)
    for i, c in enumerate(text):
        w, h = font.getsize(c)
        draw.text((60*i+(60-w)/2, 0), c, font=font, fill=get_random_color())

    # draw lines
    line_numbers = random.randint(1, 6)
    for _ in range(line_numbers):
        draw_random_line(draw, width, height)

    # add noise
    for _ in range(random.randint(1500, 3000)):
        draw.point((random.randint(0, width), random.randint(
            0, height)), fill=get_random_color())

    return im, text


def main():
    img_list = []
    with open('mjorcen.normal.exp.box', 'w') as file:
        for i in range(100): # 1tif, 100 frames
            im, text = get_verify_code()
            img_list.append(im)
            for j, c in enumerate(text):
                line = f'{c} {60*j} 0 {60*(j+1)} 60 {i}\n'
                file.write(line)
    img_list[0].save('mjorcen.normal.exp.tif', compression="tiff_deflate",
                     save_all=True, append_images=img_list[1:], dpi=(150, 150))


if __name__ == '__main__':
    main()
```

example3: tesseract only detect digits, `tesseract gogogo.png out digits`

### picture processing

```python
# rotate image
from PIL import Image

src_im = Image.open("rabbit.jpg")
im = src_im.rotate(30,fillcolor='white', expand=True)
im.show()
```

```python
# draw text
from PIL import Image, ImageDraw, ImageFont

font=ImageFont.truetype('arial.ttf', 60)
src_im = Image.open("rabbit.jpg")
draw=ImageDraw.Draw(src_im)
draw.text((0, 0), 'This is text', font=font, fill=(255, 0, 0))
src_im.show()
```

```python
# transpareny watermark
from PIL import Image, ImageDraw, ImageFont

image = Image.open("rabbit.jpg").convert("RGBA")
txt = Image.new('RGBA', image.size, (255,255,255,0))

font = ImageFont.truetype("arial.ttf", 60)
d = ImageDraw.Draw(txt)    

d.text((0, 0), "opacity=50%", fill=(0, 0, 0, 255//2), font=font)
combined = Image.alpha_composite(image, txt)    

combined.show()
```

```python
# rotate transpareny watermark
from PIL import Image, ImageDraw, ImageFont

image = Image.open("rabbit.jpg").convert("RGBA")
txt_layer = Image.new('RGBA', image.size, (255,255,255,0))

font = ImageFont.truetype("arial.ttf", 60)
d = ImageDraw.Draw(txt_layer)    
text="opacity=50%"
w, h=font.getsize(text)
W,H=image.size

d.text(((W-w)/2, (H-h)/2), text, fill=(0, 0, 0, 255//2), font=font)
txt_layer=txt_layer.rotate(30)
combined = Image.alpha_composite(image, txt_layer)    

combined.show()
```

## BaiduAI

[BaiduSDK](https://ai.baidu.com/sdk) download or `pip install baidu-aip`

去[BaiduAI](https://ai.baidu.com)在线创建对应的应用，baidu会提供

```bash
AppName
AppID
API Key
```

根据[API文档](https://ai.baidu.com/docs#/OCR-Python-SDK/32034d46)写代码

```python
from aip import AipOcr

APP_ID = '666666666'
API_KEY = 'xxxxxxxxx'
SECRET_KEY = 'yyyyyyyyy'

client = AipOcr(APP_ID, API_KEY, SECRET_KEY)

# read image
def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()

image = get_file_content('chn.png')

options = {}
options["language_type"] = "CHN_ENG"
options["detect_direction"] = "true"
options["detect_language"] = "true"
options["probability"] = "true"

client.basicGeneral(image, options)
```

```bash
# output
{'log_id': 7829085531619740665,
 'direction': 0,
 'words_result_num': 1,
 'words_result': [{'words': '这里有一份不裁员公司名单',
   'probability': {'variance': 1.5e-05,
    'average': 0.997777,
    'min': 0.987008}}],
 'language': 3}
```

