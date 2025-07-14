---
title: "영수증 이미지에서 데이터 추출하기: OpenAI API 완벽 가이드.  OpenAI Python Snippets for image processing and Function calling"
date: 2025-07-11 18:00:00 +0900
categories: [Python, 코드공부]
tags: [python, 코드공부, OpenAI]
--- 

## 1. Introduction
- 영수증 이미지를 OPENAI의 Vision api를 통해서 원하는 항목만을 추출하는 코드를 작성해야함.
- 구현 방식은 2가지가 가능함
    1. 이미지 전체를 OCR 한 이후에 원하는 항목을 추출
    2. 이미지를 OPENAI의 Vison api 사용할 때 function calling을 통해서 원하는 항목만을 추출하기
- GPT의 답변으로는 1은 토큰수가 많아지고 비효율적이고 부정확할 가능성 있음. 2가 훨씬 효율적
> 1-OCR+LLM 매핑은 빠른 프로토타이핑·유연성이 장점이지만, 결과의 일관성과 정확도를 보장하기 위해 프롬프트·파싱 로직을 지속해서 다듬어야 합니다.

> 2-Function Calling은 “LLM ↔ 내 시스템 간 계약”을 명확히 설정해 안정적·일관적 결과를 얻고, 유지보수도 쉬운 반면, 초기 스키마 정의 작업이 필요합니다.

## 2. 코드 작성
- chatGPT에 학습된 정보가 구식 정보여서 생각만큼 원활하게 작동하지 않음 (혹은 그냥 과거의 내가 지식이 없었던가). Cookbook에도 구식 정보만 있음.
- [OpenAI docs function calling](https://platform.openai.com/docs/guides/function-calling?api-mode=responses&example=get-weather)를 직접 읽어가면서 하나씩 구현함. 특히 예제 코드가 있어서 참고함.
- openai api를 호출하는 방식은 (구식의) Chat Completions 방식과, (최신식의) Responses 방식이 있음.

### 환경설정
```python
import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv()
client = OpenAI(api_key=os.getenv("OPENAI_API_KEY")) # 그냥 고정이다.
```
> api key 설정은 항상 고정이다. 복붙하면된다. 언제나 가져다가 쓰기

### Response 방식
```python
response = client.responses.create(
    model="gpt-4o",
    input=input,
    tools=tools
)
```
> very simple 하다.
- body에 들어갈 수 있는 목록은 [OpenAI api reference](https://platform.openai.com/docs/api-reference/responses/create)에서 확인 가능하다. 여기서는 input과 tools를 사용했음.
- Prompt라는 body 목록이 있는데. 이건 미리 openai dashboard에 프롬프트를 {{customer_id}} 형식으로 저장해놓고. 코드에서는 {{customer_id}} 만 정의하는 그런거도있다. 자세한건 추후 공부.

- input 설정과 tools 설정에서 고생을 좀 했다. 정해진 값들이 있고 그것을 무조건 써야한다.


#### input 설정 (이미지 처리)
```python
input=[
        {
            "role": "system",
            "content": "You are a helpful assistant that extracts structured data from Korean taxi receipts."
        },
        {
            "role": "user",
            "content": [
                {
                    "type": "input_image",
                    "image_url": f"data:image/jpeg;base64,{b64(front_img)}"
                }
            ]
        },
    ]
```
- 이미지 input을 할 경우에 4o, 4.1, 4.5 / o1, o1-pro, o3 모델 중 하나를 선택해야 한다.

- [이 형태 자체가 고정이다. 외워야한다.](https://platform.openai.com/docs/guides/images-vision?api-mode=responses&format=base64-encoded)
- "type": "input_image" 이라고 명시해줘야 한다.(그냥 고정이다)
- "image_url" 키에 이미지 데이터를 넣어준다. 키 값까지는 그냥 고정이다.
  - input은 리스트안에 딕셔너리로 설정한다.
  - image를 넣게 될 경우 content 키에 대응하는 내용을 리스트안에 딕셔너리로 넣어준다.

#### 이미지를 base64 변환
```python
# 이미지 → Base64 인코딩
import base64
def b64(path: str) -> str:
    with open(path, "rb") as f:
        return base64.b64encode(f.read()).decode("utf-8")
```
- 이거도 그냥 암기과목이다.
- 그래도 조금 분해해보자면 아래와 같다.

```python
with open("img/KakaoTalk_20250711_132102437.jpg", "rb") as f:
    # with 구문은 파일을 열고 자동으로 닫아주는 안전한 방식입니다.
    # "rb" = read binary. 바이너리(이진)방식으로 열기
    
    # 1단계: 이미지 파일을 바이너리로 읽어오기
    binary_data=f.read() 

    # 2단계: Base64 인코딩 (bytes)
    binary_encoded_data=base64.b64encode(binary_data)

    # 3단계: 문자열로 디코딩 (str)
    binary_string_data=binary_encoded_data.decode("utf-8")
    
    print(binary_data[:100])
    print(binary_encoded_data[:100])
    print(binary_string_data[:100])
```

### Functions Calling (tools 설정)
```python
tools = [{
    "type": "function",
    "name": "parse_taxi_receipt",
    "description": "Extract key fields from Korean taxi receipts",
    "parameters": {
        "type": "object",
        "properties": {
            "date": {
                "type": "string",
                "description": "YYYY-MM-DD"
            },
            "applicant": {
                "type": "string",
                "description": "Hand-written name if present"
            },
            "purpose": {
                "type": "string",
                "description": "업무내용 필드, 예: '리사→집'"
            },
            "route": {
                "type": "string",
                "description": "출발지 - 도착지"
            },
            "fare": {
                "type": "integer",
                "description": "총 결제 금액 (원)"
            },
            "paid_at": {
                "type": "string",
                "description": "YYYY-MM-DD HH:MM"
            }
        },
        "required": ["date", "fare", "paid_at"],
        "additionalProperties": False
    }
}]
```

- function은 [playground](https://platform.openai.com/playground/prompts)에서 자동 생성할 수 있다. 그렇게 생성된 tools는 맨 앞 줄에 "type": "function"를 까먹지 말고 추가해야한다.
- 함수호출은 다이어그램은 천천히 보면 이해간다. 특히 3번은 tool을 제공하는 developer를 의미한다. (나 일수도있지만 아닐수도 있음)
![함수호출 다이어그램](https://cdn.openai.com/API/docs/images/function-calling-diagram-steps.png)
- function 의 내부내용은 [docs](https://platform.openai.com/docs/guides/function-calling?api-mode=responses&strict-mode=enabled#defining-functions)를 확인하시라
- 필수 요소는 5가지

| Field         |                                                                                                  Description |
| :------------ | -----------------------------------------------------------------------------------------------------------: |
| `type`        |                                                                             This should always be `function` |
| `name`        |                                                                     The function's name (e.g. `get_weather`) |
| `description` |                                                                  Details on when and how to use the function |
| `parameters`  | JSON schema defining the function's input arguments. 내부에 `"additionalProperties": false` 를 꼭 넣어줘야함 |
| `strict`      |                                                         Whether to enforce strict mode for the function call |

- function calling에 대해서는 fine tuning이 가능하다.
- 의외로 논문 등 발전 가능성이 있을지도..[관련 독스](https://cookbook.openai.com/examples/fine_tuning_for_function_calling)




---