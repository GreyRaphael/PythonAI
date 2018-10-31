# Face

<!-- TOC -->

- [Face](#face)
    - [BaiduAI Face Recognition](#baiduai-face-recognition)

<!-- /TOC -->

## BaiduAI Face Recognition


example1: face features

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

with open('Image/su3.jpg', 'rb') as file:
    image=base64.standard_b64encode(file.read()).decode('utf8')
    res=client.detect(image, 'BASE64', options=options)

print(res)
```

```bash
# output
{'error_code': 0,
 'error_msg': 'SUCCESS',
 'log_id': 2565750016589,
 'timestamp': 1540949077,
 'cached': 0,
 'result': {'face_num': 1,
  'face_list': [{'face_token': 'a9c94b7d5c82ef5a42f9db3e936d4e0a',
    'location': {'left': 98.39468384,
     'top': 289.56427,
     'width': 332,
     'height': 314,
     'rotation': -4},
    'face_probability': 1,
    'angle': {'yaw': 1.951372504, 'pitch': 4.122478485, 'roll': -4.783641338},
    'age': 23,
    'beauty': 49.33182144,
    'gender': {'type': 'male', 'probability': 0.9942845702},
    'expression': {'type': 'smile', 'probability': 0.9999237061},
    'face_shape': {'type': 'round', 'probability': 0.3334659934}}]}}
```

example2: face match

```python
import base64
from aip import AipFace

APP_ID = '666666'
API_KEY = 'xxxxxx'
SECRET_KEY = 'yyyyyy'
client = AipFace(APP_ID, API_KEY, SECRET_KEY)

def get_content(filename):
    file=open(filename, 'rb')
    return file.read()

res=client.match(images=[
    {
        'image':base64.b64encode(get_content('Image/xxx.jpg')).decode('utf8'),
        'image_type':'BASE64'
    },
    {
        'image':base64.b64encode(get_content('Image/yyy.jpg')).decode('utf8'),
        'image_type':'BASE64'
    }
])
```

```bash
# output
{'error_code': 0,
 'error_msg': 'SUCCESS',
 'log_id': 8915790515151,
 'timestamp': 1540948788,
 'cached': 0,
 'result': {'score': 91.41099548,
  'face_list': [{'face_token': '6ba99caeb1d27ad53700c0406416d7f0'},
   {'face_token': '7c258ef3e2815be8c655366832992423'}]}}
```