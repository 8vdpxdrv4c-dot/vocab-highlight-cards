# HTML 单词注记页构建规格

本文件是 vocab-highlight-cards skill 的 HTML 构建蓝图。按此规格写单文件 HTML。

## 1. 页面骨架

```
<body>
  <div class="stage-wrap" id="stageWrap">
    <div class="stage" id="stage">
      <svg class="svg-overlay"> <defs> 三个颜色的 marker 箭头 </defs> </svg>
      <div class="row">
        <div class="col"> ...左列卡片... </div>
        <div class="center"> <img src="data:image/png;base64,..."> </div>
        <div class="col"> ...右列卡片... </div>
      </div>
    </div>
  </div>
</body>
```

- 无标题、无导航、无页脚。
- 图片 base64 内嵌，单文件自包含。
- viewport meta 必须有：`<meta name="viewport" content="width=device-width, initial-scale=1.0">`。

## 2. 布局尺寸（设计基准）

- 左列宽 320px，右列宽 320px，中央图宽 880px，舞台总宽 1520px。
- 卡片宽 300px，在列内自然排布。
- 图片按 880px 宽显示，高度按原图比例自动。
- 列用 `display:flex; flex-direction:column; gap:10px;` 卡片不重叠。

## 3. 卡片（手账风：彩色顶栏 + 浅色底）

参考用户认可的样式：每张卡片 = 彩色顶栏（单词+词性+喇叭按钮）+ 浅色底（音标→中文释义→例句）。

```html
<div class="card" style="--c:#e8503a;--t:#fde8e4;"
     data-wx="80" data-wy="207" data-color="#e8503a" data-side="left">
  <div class="head">
    <span class="w">supplies</span><span class="p">v.</span>
    <button class="play" onclick="speak('supplies', this)">喇叭svg</button>
  </div>
  <div class="body">
    <div class="ph">/səˈplaɪz/</div>
    <div class="cn">供应、提供</div>
    <div class="ex"><span class="badge">例</span><span class="en">The farm supplies milk.</span><span class="excn">农场供应鲜奶。</span></div>
  </div>
</div>
```

CSS 要点（用 CSS 变量 --c=主色、--t=浅色底）：
- `.card`：width 300px，圆角 12px，overflow hidden，背景 var(--t)，阴影柔和。
- `.head`：背景 var(--c)，白色字，padding 8px 14px，flex baseline。
  - `.w` Georgia 粗体 22px；`.p` 13px 半透明白。
  - `.play`：顶栏右上角 26px 圆形半透明白底按钮，speaking 时反白。
- `.body`：padding 10px 14px 12px。
  - `.ph`：14px 等宽字体灰色，音标前可加小喇叭 svg。
  - `.cn`：17px 粗体黑色，中文释义。
  - `.ex`：flex，`.badge` 18px 圆形 var(--c) 底白字「例」；`.en` 14px 斜体；`.excn` 13px 灰色例句翻译（display block 换行）。

颜色映射（按原文下划线色）：红 `#e8503a`/浅 `#fde8e4`，绿 `#7bc043`/浅 `#eaf5dd`，蓝 `#3b9df0`/浅 `#e3f0fd`，黄 `#f5c542`/浅 `#fdf3d6`，粉 `#f06292`/浅 `#fde4ee`，紫 `#9c27b0`/浅 `#f1e0f5`。每个新词补一组对应色。

卡片内容必须包含：单词、词性、音标、中文释义、英文例句、例句中文翻译。音标用 DJ 音标，例句短（6–10 词）自然。

## 4. 箭头 SVG

- `<svg class="svg-overlay">` 绝对定位 `top:0;left:0;pointer-events:none;overflow:visible`。
- `<defs>` 里三个 marker（蓝/红/绿，或按实际颜色扩展）：
  ```html
  <marker id="arr-blue" viewBox="0 0 10 10" refX="9" refY="5"
          markerWidth="11" markerHeight="11" orient="auto-start-reverse">
    <path d="M0,0 L10,5 L0,10 z" fill="#3b9df0"/>
  </marker>
  ```
- 路径由 JS `drawArrows()` 动态生成，不要手写固定坐标。

## 5. JS 逻辑

### 发音
```js
function speak(word, btn) {
  speechSynthesis.cancel();
  const u = new SpeechSynthesisUtterance(word);
  u.lang = 'en-US'; u.rate = 0.85;
  u.onend = () => btn.classList.remove('speaking');
  btn.classList.add('speaking');
  speechSynthesis.speak(u);
}
```

### 自适应缩放（CSS zoom，不要用 transform: scale）
```js
function fitStage() {
  const stage = document.getElementById('stage');
  const scale = Math.min(1, (window.innerWidth - 8) / 1520);
  stage.style.zoom = scale;
}
```
- 用 zoom 而非 transform，因为 zoom 真正重排布局，offsetLeft 测量才准，手机端不空白。

### 自动画箭头
```js
function posInStage(el) {
  let x=0, y=0, node=el;
  while (node && node !== document.getElementById('stage')) {
    x += node.offsetLeft; y += node.offsetTop; node = node.offsetParent;
  }
  return {x, y};
}
function drawArrows() {
  const svg = document.querySelector('.svg-overlay');
  [...svg.querySelectorAll('path')].forEach(p => p.remove());
  const NS = 'http://www.w3.org/2000/svg';
  const markers = { '#3b9df0':'arr-blue', '#e8503a':'arr-red', '#7bc043':'arr-green' };
  document.querySelectorAll('.card').forEach(card => {
    const p = posInStage(card);
    const cw = card.offsetWidth, ch = card.offsetHeight;
    const side = card.dataset.side;
    const wx = 320 + parseFloat(card.dataset.wx); // 左列宽320
    const wy = parseFloat(card.dataset.wy);
    const edgeX = side === 'left' ? p.x + cw : p.x;
    const edgeY = p.y + ch/2;
    const dx = Math.abs(wx - edgeX);
    const c1x = edgeX + (side==='left' ? -dx*0.4 : dx*0.4);
    const c2x = wx + (side==='left' ? -dx*0.4 : dx*0.4);
    const path = document.createElementNS(NS, 'path');
    path.setAttribute('d', `M ${edgeX},${edgeY} C ${c1x},${edgeY} ${c2x},${wy} ${wx},${wy}`);
    path.setAttribute('stroke', card.dataset.color);
    path.setAttribute('stroke-width', '3.5');
    path.setAttribute('fill', 'none');
    path.setAttribute('stroke-linecap', 'round');
    path.setAttribute('marker-end', `url(#${markers[card.dataset.color]})`);
    svg.appendChild(path);
  });
}
function layout(){ fitStage(); drawArrows(); }
window.addEventListener('resize', layout);
window.addEventListener('load', layout);
layout();
```

### data-wx / data-wy 怎么定
- 原图显示宽度 = 880，原图实际宽度 = W。缩放比 = 880/W。
- data-wx = 单词中心在原图中的 x 坐标 × (880/W)。
- data-wy = 单词中心在原图中的 y 坐标 × (880/W)。
- 估计不准没关系，JS 自动连到卡片边缘，箭头大致对准即可。

## 6. 自检

```bash
python3 <html-skill>/scripts/shot.py <html> --only desktop
python3 <html-skill>/scripts/shot.py <html> --only mobile
```

桌面确认：箭头对准单词、卡片不重叠、无 consoleError。
手机确认：三栏等比缩小后仍可见，不空白。

## 7. 交付

`present_files` 交付单个 .html 文件，文件名用语义化中文名（如「国富论单词注记.html」）。
