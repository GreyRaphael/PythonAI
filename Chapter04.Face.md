# Face

<!-- TOC -->

- [Face](#face)
    - [BaiduAI Face Recognition](#baiduai-face-recognition)

<!-- /TOC -->

## BaiduAI Face Recognition

```python
import base64
from aip import AipFace

APP_ID = '666666'
API_KEY = 'xxxxxx'
SECRET_KEY = 'yyyyyy'
client = AipFace(APP_ID, API_KEY, SECRET_KEY)

options = {}
options["face_field"] = 'age,beauty,gender,expression,face_shape'
options["max_face_num"] = 1

with open('Image/wei3.jpg', 'rb') as file:
    image=base64.standard_b64encode(file.read()).decode('utf8')
    res=client.detect(image, 'BASE64', options=options)

print(res)
```