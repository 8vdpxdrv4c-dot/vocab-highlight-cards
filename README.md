# vocab-highlight-cards

识别书页/文章照片里被彩色下划线或荧光笔高亮的英文单词，自动生成一个自包含 HTML 单词注记页：

![demo](https://aka.doubaocdn.com/s/Czv8x3IRV2)

- 中央保留原照片，左右两列排单词卡
- 手账风卡片：彩色顶栏（单词+词性+喇叭发音）+ 音标 + 中文释义 + 英文例句（附中文翻译）
- 用与高亮色一致的贝塞尔曲线箭头从卡片连回原图划线词
- 点击喇叭按钮用浏览器 Web Speech API 朗读单词
- 手机端 CSS zoom 等比缩放，三栏不变
- 单文件 HTML，图片 base64 内嵌，可离线打开

## 用法

把这个目录放到豆包/Agent 工作区的 `.user_skills/` 下，然后上传一张带划线单词的书页照片，说「识别单词做单词卡」即可自动触发。

## 文件结构

```
vocab-highlight-cards/
├── SKILL.md                    # 触发描述与执行流程
└── references/
    ├── html-spec.md            # HTML 页面构建蓝图（卡片样式/箭头/自适应）
    └── prompt-template.md       # 兜底的 image_edit 生图方案 prompt 骨架
```
