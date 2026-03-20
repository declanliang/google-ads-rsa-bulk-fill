<img width="2880" height="1421" alt="image" src="https://github.com/user-attachments/assets/a172930c-cfa7-49cf-8c5f-32252dead099" /># google-ads-rsa-bulk-fill

一个用于 Google Ads 网页端 的小工具脚本。
适用于在 Responsive Search Ads 创建页面中，快速将：

15 个标题（Headlines）

4 个描述（Descriptions）

一次性批量填充到对应输入框中。

这个方案不依赖 Google Ads API，也不依赖 Google Ads Editor。
适合在 API 尚未申请通过前，临时提升人工建广告效率。

背景

在 Google Ads 网页端创建 RSA 广告时，通常需要手动把：

15 条标题

4 条描述

逐条粘贴到页面中。
如果每天需要处理很多广告，这会产生大量重复劳动。

这个脚本的作用是：

在 Google Ads 创建广告页面打开 Console

运行脚本

粘贴一整段文本

自动拆分成 15 个标题和 4 个描述

自动填入页面

适用场景

适用于：

无法使用 Google Ads Editor

尚未拿到 Google Ads API

需要频繁在网页端手工创建 RSA 广告

希望减少重复复制粘贴操作

功能说明

脚本会：

弹出一个输入框

让你一次性粘贴整段文本

按“前 15 行 = 标题，后 4 行 = 描述”的规则自动拆分

自动填充到 Google Ads 页面中已经展开的 RSA 字段里

使用前提

使用前请确认：

你已经进入 Google Ads 的 RSA 广告创建页面

页面中已经展开：

15 个 Headline 输入框

4 个 Description 输入框

使用 Chrome 浏览器或 Chromium 内核浏览器

可以打开开发者工具 Console

最终代码

将下面代码完整复制到浏览器 Console 中运行：

const rawText = prompt("请粘贴 15 个标题 + 4 个描述，每行一条：");

if (!rawText) {
  console.log("你没有粘贴内容，脚本停止。");
} else {
  const lines = rawText
    .split("\n")
    .map(s => s.trim())
    .filter(Boolean);

  if (lines.length < 19) {
    console.log(`文本行数不够。当前只有 ${lines.length} 行，至少需要 19 行（15 个标题 + 4 个描述）。`);
  } else {
    const headlines = lines.slice(0, 15);
    const descriptions = lines.slice(15, 19);

    function isVisible(el) {
      const r = el.getBoundingClientRect();
      return r.width > 0 && r.height > 0 && el.offsetParent !== null && !el.disabled;
    }

    function setValue(el, value) {
      el.focus();
      el.value = value;
      el.dispatchEvent(new Event("input", { bubbles: true }));
      el.dispatchEvent(new Event("change", { bubbles: true }));
      el.blur();
    }

    const allVisibleInputs = [...document.querySelectorAll('input[type="text"].input.input-area')]
      .filter(isVisible);

    const path2Index = allVisibleInputs.findIndex(el =>
      (el.getAttribute("aria-label") || "").trim() === "Path 2"
    );

    if (path2Index === -1) {
      console.log("没找到 Path 2，脚本停止。");
    } else {
      const headlineFields = allVisibleInputs
        .slice(path2Index + 1)
        .filter(el =>
          !(el.getAttribute("aria-label")) &&
          !(el.getAttribute("placeholder"))
        )
        .slice(0, 15);

      const descriptionFields = [...document.querySelectorAll("textarea")]
        .filter(isVisible)
        .filter(el => (el.getAttribute("aria-label") || "").trim() === "Description")
        .slice(0, 4);

      headlineFields.forEach((el, i) => {
        if (headlines[i] !== undefined) setValue(el, headlines[i]);
      });

      descriptionFields.forEach((el, i) => {
        if (descriptions[i] !== undefined) setValue(el, descriptions[i]);
      });

      console.log("headline fields found:", headlineFields.length);
      console.log("description fields found:", descriptionFields.length);
    }
  }
}
SOP：标准操作流程
第 1 步：打开 Google Ads 广告创建页面

进入你要新建 Responsive Search Ad 的页面。

第 2 步：确保字段已经展开

在页面中确认已经显示：

15 个 Headline 输入框

4 个 Description 输入框

这一步很重要。
如果字段没有展开，脚本可能无法正确填充。

第 3 步：打开浏览器 Console

在 Chrome 中：

Windows：按 F12

Mac：按 Command + Option + I

然后点击顶部标签页：

Console

第 4 步：首次允许在 Console 粘贴

如果 Chrome 阻止粘贴，先在 Console 输入：

allow pasting

然后按回车。

第 5 步：运行脚本

将上面的完整脚本粘贴到 Console，按回车。

第 6 步：在弹窗中粘贴广告文案

脚本运行后，会弹出一个输入框。
在这个输入框中粘贴你的广告文案。

格式要求：

每行一条

前 15 行为标题

后 4 行为描述

示例：

Levoit Official Store
{KeyWord:Levoit} Official Store
Levoit OasisMist 450S Smart
Levoit Warm and Cool Mist
Shop Levoit OasisMist 4.5L
{KeyWord:Levoit} Humidifier
Levoit 26dB Ultra Quiet
Levoit Smart App Alexa Control
Levoit Fast Symptom Relief
Trusted Levoit Quality Gear
Levoit Free Shipping in USA
Levoit 30 Day Return Policy
Levoit Easy Top Fill Design
Levoit Auto Mode Humidity
Levoit Essential Oil Diffuser
Shop Levoit OasisMist 450S. Dual warm and cool mist for fast relief and better sleep.
Levoit smart humidifiers feature 26dB quiet operation and App control. Perfect for bedrooms.
Trusted by millions. Levoit offers 4.5L top-fill tanks with free shipping and easy cleaning.
Discover Levoit OasisMist. Maintain healthy humidity levels with Auto Mode and Alexa sync.
第 7 步：点击确定

点击弹窗中的 OK / 确定。
脚本会自动将内容填入页面。

第 8 步：检查填充结果

正常情况下，Console 会输出：

headline fields found: 15
description fields found: 4

如果页面内容已经填好，就说明成功。

输入格式要求

必须满足以下规则：

1. 每行只能有一条内容

不要把两条标题或一条标题和一条描述写在同一行。

2. 前 15 行必须是标题

不能少，也不能乱序。

3. 后 4 行必须是描述

共 4 条。

4. 总行数至少 19 行

即：

15 条标题

4 条描述

推荐工作流

适合日常高频使用的流程：

用 AI 生成 15 个标题 + 4 个描述

检查是否每条都单独占一行

打开 Google Ads 广告创建页

展开 15 个标题框和 4 个描述框

打开 Console

运行脚本

在弹窗中粘贴整段文本

点击确定

完成自动填充

常见问题
1. 为什么脚本没有反应？

先检查：

是否在正确的 Google Ads 广告创建页面

Headline 和 Description 字段是否已经展开

是否把内容粘贴到了弹窗里，而不是再次粘贴到 Console

是否满足 15 行标题 + 4 行描述

2. Console 里出现“行数不够”

说明你粘贴进去的文本少于 19 行。
请检查是否有内容被写在同一行了。

3. 为什么刷新页面后又不能用了？

因为这是临时脚本，不是浏览器插件。
页面刷新后，需要重新在 Console 里运行一次。

4. 这是不是一个插件？

不是。
这是一个在浏览器 Console 中运行的临时自动填充脚本。

5. 这是不是长期方案？

不是长期最优解。
长期更好的方案仍然是：

Google Ads API

或更系统化的自动化工具

但在没有 API 的阶段，这个脚本已经能显著减少重复劳动。

局限性

这个脚本依赖当前 Google Ads 页面结构。
如果 Google Ads 后续修改了前端 DOM 结构，脚本可能失效，需要重新调整。

也就是说：

它非常实用

但不是官方支持方案

未来可能需要维护

为什么这个方案有价值

虽然它不是插件，也不是完整网站，更不是正式产品，
但它可以非常直接地解决一个高频重复动作：

把逐条手工粘贴，变成一次性批量填充。

在高频操作场景下，这种小工具往往能节省大量时间。

后续可扩展方向

后续可以继续升级成：

浏览器书签脚本

Tampermonkey 脚本

Chrome 插件

本地小工具

接入 Google Ads API 的正式投放工具
