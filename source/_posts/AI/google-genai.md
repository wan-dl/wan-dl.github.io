---
title: python使用google genai
date: 2025-10-10
categories: AI
icon: AI
desc: 人工智能
---

```
import os
from google import genai
from google.genai.errors import APIError

PROXY_ADDRESS = 'http://127.0.0.1:7890'
print(f"--- 尝试通过代理 {PROXY_ADDRESS} 连接 ---")


def google_main(send_content: str):
    """
    使用 Gemini API 进行专业翻译。
    """
    client = genai.Client(api_key="")

    # 构造带有系统指令的 Prompt
    system_instruction = (
        "你是一名专业的Android开发人员，后面我会给你发送一些android错误进行分析。"
    )
    prompt = f"[{send_content}]"
    
    try:
        # 生成内容
        response = client.models.generate_content(
            model="gemini-2.5-flash",
            contents=prompt,
            config={
                "system_instruction": system_instruction
            }
        )
        return response.text.strip()
    except APIError as e:
        return f"Gemini API 错误: {e}"
    except Exception as e:
        return f"发生错误: {e}"


if __name__ == "__main__":
    # 手动设置代理环境变量（如果之前没有设置）
    # os.environ["HTTP_PROXY"] = PROXY_ADDRESS

    content = """
"""

    result = google_main(content)

    print("\n原始内容:")
    print(content)
    print("\nAI结果:")
    print(result)

```