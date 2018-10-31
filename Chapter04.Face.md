# Face

<!-- TOC -->

- [Face](#face)
    - [BaiduAI Face Recognition](#baiduai-face-recognition)
    - [BaiduAI NLP](#baiduai-nlp)
    - [Image recognition](#image-recognition)
    - [Knowledge Graph](#knowledge-graph)

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

## BaiduAI NLP

example1: 词法分析

```python
from aip import AipNlp

APP_ID = 'xxxxxx'
API_KEY = '666666'
SECRET_KEY = '777777'

client = AipNlp(APP_ID, API_KEY, SECRET_KEY)
res=client.lexer('我今天很高兴')
```

```bash
# output
{'log_id': 3331798020324814463,
 'text': '我今天很高兴',
 'items': [{'loc_details': [],
   'byte_offset': 0,
   'uri': '',
   'pos': 'r',
   'ne': '',
   'item': '我',
   'basic_words': ['我'],
   'byte_length': 2,
   'formal': ''},
  {'loc_details': [],
   'byte_offset': 2,
   'uri': '',
   'pos': '',
   'ne': 'TIME',
   'item': '今天',
   'basic_words': ['今天'],
   'byte_length': 4,
   'formal': ''},
  {'loc_details': [],
   'byte_offset': 6,
   'uri': '',
   'pos': 'a',
   'ne': '',
   'item': '很高兴',
   'basic_words': ['很', '高兴'],
   'byte_length': 6,
   'formal': ''}]}
```

example2: 短文本相似度

```bash
# # output
# client.simnet('英俊', '帅')
{'log_id': 2268038743521071103,
 'texts': {'text_2': '帅', 'text_1': '英俊'},
 'score': 0.760255}
```

example3: 情感分析

```bash
# # output
# client.sentimentClassify('我今天很高兴')
{'log_id': 5255506060294199327,
 'text': '我今天很高兴',
 'items': [{'positive_prob': 0.63481,
   'confidence': 0.188467,
   'negative_prob': 0.36519,
   'sentiment': 2}]}
```

example4: 评论观点

```bash
# # output
# client.commentTag('苹果很好吃')
{'log_id': 6022206784532697759,
 'items': [{'sentiment': 2,
   'abstract': '<span>苹果很好吃</span>',
   'prop': '苹果',
   'begin_pos': 0,
   'end_pos': 10,
   'adj': '好吃'}]}
```

## Image recognition

example1: general recognition

```python
from aip import AipImageClassify


APP_ID = 'xxxxxx'
API_KEY = 'yyyyyy'
SECRET_KEY = 'zzzzzz'
client = AipImageClassify(APP_ID, API_KEY, SECRET_KEY)

def get_content(filename):
    with open(filename, 'rb') as file:
        return file.read()

img=get_content('Image/fruits.jpg')
res=client.advancedGeneral(img)
```

```bash
# output
{'log_id': 228571135242883551,
 'result_num': 5,
 'result': [{'score': 0.58972, 'root': '商品-水果', 'keyword': '猕猴桃'},
  {'score': 0.436514, 'root': '植物-蔷薇科', 'keyword': '草莓'},
  {'score': 0.289548, 'root': '植物-仙人掌科', 'keyword': '仙人球'},
  {'score': 0.149522, 'root': '商品-水果', 'keyword': '新鲜水果'},
  {'score': 0.000973, 'root': '植物-果实/种子', 'keyword': '红草莓'}]}
```

example2: dish recognition

```bash
# # output
# img2=get_content('Image/sichuan.jpg')
# res2=client.dishDetect(img2, options={'top_num':3,})
{'log_id': 3607199214192999615,
 'result_num': 3,
 'result': [{'calorie': '154',
   'has_calorie': True,
   'name': '鱼香肉丝',
   'probability': '0.968383'},
  {'calorie': '240',
   'has_calorie': True,
   'name': '酱肉丝',
   'probability': '0.0270292'},
  {'calorie': '268',
   'has_calorie': True,
   'name': '肉丝饭',
   'probability': '0.00303683'}]}
```

其他应用: 车辆识别、动物识别、植物识别

## Knowledge Graph

知识图谱表示的是各种事物的关系，一般永远客服机器人。