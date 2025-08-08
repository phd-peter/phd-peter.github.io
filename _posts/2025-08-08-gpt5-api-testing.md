---
title: "gpt5 api calling의 성과 비교. 프롬프트의 중요성"
date: 2025-08-08 09:15:00 +0900
categories: [Python, 코드공부]
tags: [python, 코드공부, OpenAI]
--- 

## Introduction
- GPT5 가 공개되었다.
- api calling에서 reasoning_effort = ["low", "medium", "high", "minimal"] 가 있다.
- 과연 얼마나 결과물에서 차이가 발생할지 테스트 해보았다.

## Simple prompt
- [docs에서 제공하는 simple prompt](https://platform.openai.com/docs/guides/latest-model)를 적용했다. 
- 프롬프트는 자유의 여신상을 1mm 두께로 구리로 도금하는 경우, 얼마나 많은 양의 구리가 필요한가?
- effort를 바꿔가며 결과물을 출력했다. 

```python
client = OpenAI()
reasoning_effort = ["low", "medium", "high", "minimal"]
for i in reasoning_effort:
    response = client.responses.create(
        model="gpt-5",
        input="How much gold would it take to coat the Statue of Liberty in a 1mm layer?",
    reasoning={
        "effort": i
    }
    )
    print(f"Effort: {i}")
    print(response.output[1].content[0].text)
    print("--------------------------------")
```

### 결과물
- 결과물을 [gpt-4o에게 비교](https://chatgpt.com/share/68953cac-6620-8003-839f-5bf84f043bab)해달라고 요청했다.
- medium (default값)이 가장 합리적이며, 오히려 reasoning effort = high 가 과대평가하는 결과를 보였다.
- 

## 정교화된 프롬프트
- OpenAI에서는 [gpt-5에 대한 프롬프트 가이드](https://cookbook.openai.com/examples/gpt-5/gpt-5_prompting_guide)를 제공한다.
- 아마 GPT-5의 성능을 최대화하기 위해서는 프롬프트의 정교화가 필수적이기 때문이라고 생각한다.
- 그래서 프롬프트 정교화를 도와주는 [optimizer](https://platform.openai.com/chat/edit?models=gpt-5&optimize=true)를 제공한다
- 기존 simple prompt를 입력해서 optimized된, 정교화된 프롬프트를 받았다.

```
Developer: # Role and Objective
- Calculate the total amount of gold required to coat the entire surface of the Statue of Liberty with a uniform 1 millimeter thick layer.

# Process Checklist
- Begin with a concise checklist (3–7 bullets) of the steps to complete before performing calculations.

# Instructions
- Assume full coverage of the statue, excluding the pedestal.
- Use a reasonable estimate for the overall surface area of the Statue of Liberty.
- Use standard gold properties, such as density (approximately 19.32 g/cm³).
- Show and explain all calculations in detail, including unit conversions.
- Output the final answer in both kilograms and troy ounces.

# Output Format
- Provide the answer as a step-by-step calculation, followed by a concise summary table displaying the final amounts in kilograms and troy ounces.

# Reasoning Effort
- Set reasoning_effort = medium to ensure concise yet explanatory steps appropriate to the task's complexity level.

# Verbosity
- Use clear, explanatory steps, but keep explanations concise.

# Stop Conditions
- Stop after providing the calculations and final answer summary.
```

- 기존 코드에서 input을 정교화한 프롬프트로 수정하였다.
- reasoning effort에 대한 결과물을 다시 한번 시도했다.

### 결과물
- 결과물을 다시 한 번 [gpt-4o에게 비교](https://chatgpt.com/share/6895431b-ac48-8003-b72c-9587b198977a)해달라고 요청했다.
- minimal 값의 추정이 너무 러프하였고, low, medium, high의 추정값은 바람직하였다.
- reasoning effort = high는 가장 정교하게 추정하였다.

## 결론
- GPT-5 API를 사용할 때는 프롬프팅에 대한 충분한 고민이 필요하다.
- 어려울 경우에는 OpenAI가 제공하는 optimizer 사이트를 활용하자.

https://platform.openai.com/chat/edit?models=gpt-5&optimize=true
