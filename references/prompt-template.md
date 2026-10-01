# 单词卡标注图 Prompt 模板骨架

生成时把下面骨架展开成一段连贯中文（不要换行、不要列表、不要标题）。所有卡片文字必须逐字写进 prompt，不要用「其余类似」省略。

## 开头（固定）

这是一张英语单词学习标注信息图，竖版，完整保留图1的书页照片作为画面中央栏主体，书页上所有彩色高亮单词的颜色、位置和正文英文文字保持原样，背景为干净的米白色学习纸质感。画面分三栏：中央栏是书页原文段落，左栏和右栏各竖向等距排列一列白色圆角小卡片；每张卡片用一条细的、与该单词原高亮色相同的平滑弧线箭头连接到中央段落里对应的高亮单词，弧线箭头末端有一个小箭头指向该高亮单词，弧线不穿过正文文字。

## 左列卡片（按从上到下顺序逐个写）

左栏从上到下共 N 张小卡片，每张卡片为白色圆角矩形、左侧有一条与单词高亮色一致的细色条，卡片内第一行是加粗黑色英文单词，第二行是小号灰色中文释义，第三行是更小号深灰色英文例句：第1张【颜色】色卡片写"【word】"，下一行"【词性+中文释义】"，再下一行"【English example.】"；第2张……（逐个写完）。

## 右列卡片（按从上到下顺序逐个写）

右栏从上到下共 M 张同样式卡片：第1张【颜色】色卡片写"【word】"，下一行"【词性+中文释义】"，再下一行"【English example.】"；……（逐个写完）。

## 结尾（固定）

所有卡片大小一致、竖向等距排列，弧线箭头从卡片内缘中点平滑弯向中央段落对应高亮单词并以箭头指向该词，整体干净整洁、文字清晰可读，不要出现其他多余文字。

## 引号规则

- 中文释义用中文双引号：`"让出、放弃"`。
- 英文单词和英文例句用英文双引号：`"cede"`、`"You can cede control."`。

## 工具参数

- 工具：`image_edit`
- `image_reference_url_list`：[用户上传照片的 URL]
- `model_version`：`seedream_5.0_flash`
- `width`：1536，`height`：2048

## 常见词参考（可复用释义与例句）

| 单词 | 中文释义 | 英文例句 |
|---|---|---|
| cede | v. 让出、放弃（控制权） | You can cede control and execute a plan. |
| worse | adj. 更糟的 | Procrastination only makes things worse. |
| slight | v. 怠慢、轻视 | He felt slighted when no one said hello. |
| lash out | phr. 猛击、怒斥 | We lash out with angry words. |
| angry | adj. 愤怒的 | Don't respond with angry words. |
| cut off | phr. 打断、切断 | When someone cuts us off, we assume the worst. |
| malice | n. 恶意、敌意 | Don't assume malice on their part. |
| slower | adj. 更慢的 | Progress feels slower than we want. |
| frustrated | adj. 沮丧的、受挫的 | She felt frustrated by the delays. |
| impatient | adj. 不耐烦的 | Waiting makes us impatient. |
| passive-aggressive | adj. 被动攻击的 | Passive-aggressive behavior hides anger. |
| bait | n. 诱饵、诱惑 | Don't take the bait and escalate. |
| hijack | v. 劫持、把持 | Our brains are hijacked by emotion. |
| biology | n. 生物本能、生理机制 | Our reactions come from biology. |
| hoard | v. 囤积、暗藏 | Hoarding information hurts the team. |
| advantage | n. 优势、好处 | Share information to gain an advantage. |
| hurt | v. 伤害、损害 | Withholding knowledge is hurting the team. |
| ourselves | pron. 我们自己 | Think for yourselves, not just conform. |
| emotion | n. 情绪、情感 | Our emotions can cloud our judgment. |
| react | v. 反应、回应 | We react in ways that cause problems. |
| train | v. 训练、培养 | Train yourself to pause first. |
| identify | v. 识别、辨认 | Identify when judgment is needed. |
| judgment | n. 判断、判断力 | Use good judgment in the moment. |
| pause | v./n. 暂停、停顿 | Pause before you react. |
| space | n. 空间、余地 | Create space to think clearly. |
| counterbalance | v. 抵消、平衡 | Counterbalancing our hardwired instincts. |
| hardwired | adj. 天生的、固有的 | Fear responses are hardwired in the brain. |
| default | n. 默认模式 | These are our brain's defaults. |
| success | n. 成功 | Mastery is the ingredient to success. |
