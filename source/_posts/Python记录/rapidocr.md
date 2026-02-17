---
title: 使用rapidocr识别截图文本
date: 2026-02-17
categories: Python
icon: Python
desc: 利用 RapidOCR 实时检测 Android/iOS 权限弹窗
---

# 基于 RapidOCR 的跨平台弹窗检测方案

## 1. 方案简介

本方案采用视觉识别（OCR）技术，将 Android/iOS 设备视为“图像源”，通过识别屏幕截图中的关键词（如“允许”、“始终允许”、“权限请求”）来判定系统权限弹窗。

* **优点**：完全跨平台（Android/iOS 通用）、无需系统底层权限、本地离线运行（隐私安全）。
* **核心工具**：`rapidocr_onnxruntime` (基于 ONNX 的轻量化推理引擎)。

## 2. 环境配置

```bash
# 安装核心库
pip install rapidocr_onnxruntime opencv-python

```

## 3. 核心检测逻辑 (Python)

以下代码包含：**图像预处理（提速）**、**关键词匹配**、**判定弹窗**。

```python
import cv2
import time
from rapidocr_onnxruntime import RapidOCR

class DialogDetector:
    def __init__(self):
        # 初始化引擎，不使用方向分类器以提升速度
        self.engine = RapidOCR()
        # 定义需要监控的弹窗关键词
        self.keywords = ["允许", "始终允许", "仅在使用中允许", "权限", "Access", "Allow"]

    def detect(self, img_path):
        """
        输入：图片路径或字节流
        输出：是否发现弹窗 (True/False), 关键词坐标, 识别文字
        """
        # 1. 读取并进行灰度化（可选，提升对比度）
        img = cv2.imread(img_path)
        if img is None: return False, None, ""

        # 2. 性能优化：裁剪屏幕中心区域 (ROI)
        # 弹窗通常出现在屏幕 20% - 80% 的高度之间
        h, w = img.shape[:2]
        roi_img = img[int(h*0.2):int(h*0.8), :]

        # 3. OCR 推理
        start_time = time.time()
        result, _ = self.engine(roi_img)
        cost = time.time() - start_time

        if result:
            for item in result:
                box, text, score = item
                # 4. 关键词过滤
                if any(kw in text for kw in self.keywords):
                    # 坐标还原（加上裁剪掉的 top 偏移）
                    actual_box = box[0]
                    actual_box[1] += int(h*0.2)
                    return True, actual_box, text
        
        return False, None, ""

# 使用示例
detector = DialogDetector()
found, pos, content = detector.detect(r"C:\Users\chenx\Downloads\IMG_4181.PNG")
if found:
    print(f"检测到弹窗！文字: {content}, 坐标: {pos}")

```

## 4. 性能调优心得

1. **降低频率**：没必要每秒识别 60 次。对于权限弹窗，**1秒 2-3 次**轮询（Polling）已足够。
2. **降低分辨率**：将截图缩放到长边 960px 左右，识别速度可提升 100%，且不丢失准确率。
3. **多线程处理**：将“截图获取”和“OCR识别”放在两个线程中，防止截图过程阻塞识别逻辑。