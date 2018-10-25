# OCR

<!-- TOC -->

- [OCR](#ocr)
    - [Tesseract](#tesseract)
    - [pytesseract](#pytesseract)
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
