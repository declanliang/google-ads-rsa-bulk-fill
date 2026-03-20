# Google Ads RSA 批量填充工具（书签版）

一个基于浏览器书签（Bookmarklet）的轻量工具，用于在 Google Ads 的 Responsive Search Ads（RSA）页面中，一次性批量填充：

- 15 个标题（Headlines）
- 4 个描述（Descriptions）

无需 API、无需插件、无需安装环境。  
只需点击书签 + 粘贴文本，即可自动填充。

---

## 🚀 为什么需要这个工具

在 Google Ads 创建 RSA 广告时，通常需要手动填写：

- 15 条标题
- 4 条描述

如果每天需要创建大量广告，这种重复操作会非常耗时。

本工具可以：

- 一次粘贴全部文案
- 自动拆分并填充
- 显著减少重复劳动

---

## ✨ 功能特点

- 一键点击书签执行
- 自动识别并填充：
  - 15 个标题
  - 4 个描述
- 直接运行在 Google Ads 网页中
- 无需安装任何插件或软件

---

## ⚙️ 安装方法（书签版本）

### 第一步：创建书签

1. 打开任意网页
2. 按 `Ctrl + D`（Mac：`Command + D`）
3. 书签名称填写：

---

### 第二步：替换书签 URL

编辑该书签，把 URL 替换为以下代码：

```javascript
javascript:(function(){const rawText=prompt("请粘贴 15 个标题 + 4 个描述，每行一条：");if(!rawText){alert("未输入内容");return;}const lines=rawText.split("\n").map(s=>s.trim()).filter(Boolean);if(lines.length<19){alert("行数不足，至少需要 19 行（15 个标题 + 4 个描述）");return;}const headlines=lines.slice(0,15);const descriptions=lines.slice(15,19);function isVisible(el){const r=el.getBoundingClientRect();return r.width>0&&r.height>0&&el.offsetParent!==null&&!el.disabled;}function setValue(el,value){el.focus();el.value=value;el.dispatchEvent(new Event("input",{bubbles:true}));el.dispatchEvent(new Event("change",{bubbles:true}));el.blur();}const allVisibleInputs=[...document.querySelectorAll('input[type="text"].input.input-area')].filter(isVisible);const path2Index=allVisibleInputs.findIndex(el=>((el.getAttribute("aria-label")||"").trim()==="Path 2"));if(path2Index===-1){alert("未找到 Path 2，请确认已进入广告编辑页面");return;}const headlineFields=allVisibleInputs.slice(path2Index+1).filter(el=>!(el.getAttribute("aria-label"))&&!(el.getAttribute("placeholder"))).slice(0,15);const descriptionFields=[...document.querySelectorAll("textarea")].filter(isVisible).filter(el=>((el.getAttribute("aria-label")||"").trim()==="Description")).slice(0,4);headlineFields.forEach((el,i)=>{if(headlines[i]!==undefined)setValue(el,headlines[i]);});descriptionFields.forEach((el,i)=>{if(descriptions[i]!==undefined)setValue(el,descriptions[i]);});alert("填充完成：标题 "+headlineFields.length+" 条，描述 "+descriptionFields.length+" 条");})();
