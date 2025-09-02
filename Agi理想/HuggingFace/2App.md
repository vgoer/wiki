<center>App</center>







[toc]







## App

> 做一个图片转故事，故事转语音的app

![image-20250625163822324](.\assets\image-20250625163822324.png)







### 1. 创建key

> api_key: [url](https://platform.openai.com/settings/organization/api-keys)

> 安装langchain: [url](https://python.langchain.com/)

```shell
pip install langchain-openai
```

```python
from transformers import pipeline
from langchain_core.prompts import PromptTemplate
from langchain.chains import LLMChain
from langchain_openai import ChatOpenAI
import os
import json
import requests


# apikey
# https://github.com/chatanywhere/GPT_API_free
os.environ["OPENAI_API_KEY"] = "open_api_key"

def img2text(url):
    img_to_text = pipeline("image-to-text", model="Salesforce/blip-image-captioning-base")

    text = img_to_text(url)[0]["generated_text"]

    print(text)
    return text


def gencreate_story(scenario):
    # 引导词
    template = """
    你是一个很会故事的老人，下面Context中的内容是一个外国人说的一句英语，根据这个英文讲一个中文小故事。
    
    Context: {scenario}
    STORY:
    """

    prompt = PromptTemplate(template=template, input_variables=["scenario"])

    story_llm = LLMChain(llm=ChatOpenAI(
        model_name="gpt-3.5-turbo",
        temperature=1
    ), prompt=prompt, verbose=True)

    story = story_llm.predict(scenario=scenario)
    print(story)

    return story


def text2speech(message):
    headers = {"Authorization": f"Bearer huggingface_key"}
    API_URL = "https://router.huggingface.co/hf-inference/models/microsoft/speecht5_tts"

    payload = {
        "inputs": message,
    }

    response = requests.post(API_URL, headers=headers, json=payload)

    print(response)

    with open('audio.flac', 'wb') as file:
        file.write(response.content)

    return response


scenario = img2text("https://huggingface.co/datasets/Narsil/image_dummy/resolve/main/parrots.png")
story = gencreate_story(scenario)
text2speech(story)
```

