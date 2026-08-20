---
layout: paper
permalink: /projects/huawei-model-safety-alignment/
title: "华为合作项目｜图像生成模型安全提升"
excerpt: "通过有害概念预测噪声对比引导与常规概念能力正则，降低衣物擦除模型生成色情内容的风险。"
author_profile: false
og_locale: zh_CN
paper_theme: alignment
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#projects" target="_self" aria-label="返回项目列表">← 返回项目列表</a>
  <span>产业合作项目页</span>
  <span class="paper-project__venue">Huawei Collaboration</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Diffusion Model · Safety Alignment · Inpainting</p>
    <h1>图像生成模型安全提升</h1>
    <p class="paper-hero__subtitle">在保持常规内容擦除能力的同时，抑制衣物擦除场景中的色情内容生成</p>
    <p class="paper-hero__authors"><strong>个人职责：模型安全微调方案设计</strong> · 华为小艺终端部门</p>
    <p class="paper-hero__affiliation">项目方向：图像生成模型内容安全 · 06/2025 - 12/2025</p>
    <div class="paper-actions" aria-label="章节导航">
      <a class="paper-action paper-action--primary" href="#background" target="_self">1 · 项目背景</a>
      <a class="paper-action" href="#method" target="_self">2 · 方法设计</a>
      <a class="paper-action" href="#results" target="_self">3 · 实验结果</a>
      <a class="paper-action" href="#ownership" target="_self">4 · 个人职责</a>
    </div>
  </div>
</header>

<section class="paper-section paper-chapter" id="background">
  <p class="paper-section__eyebrow">01 · Background</p>
  <h2>项目背景：衣物擦除能力伴随色情内容生成风险</h2>
  <div class="paper-lead">
    华为小艺的图像擦除功能在处理人物衣物区域时，存在一定概率生成色情内容。安全微调不能简单削弱模型的全部擦除能力：既要让模型在涉黄场景中避免产生有害结果，也要保持它对普通物体和常规区域的正常擦除效果。
  </div>
  <div class="paper-grid">
    <article class="paper-card">
      <span class="paper-card__index">R</span>
      <h3>风险抑制</h3>
      <p>针对人物衣物区域的擦除请求，调整模型预测方向，降低生成色情内容的概率。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">U</span>
      <h3>能力保持</h3>
      <p>对常规图像和 Mask 维持原模型预测，使日常内容擦除能力不因安全微调而退化。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">B</span>
      <h3>双目标平衡</h3>
      <p>通过有害概念对比损失与常规概念正则项联合优化，控制安全性与可用性的平衡。</p>
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="method">
  <p class="paper-section__eyebrow">02 · Method</p>
  <h2>方法设计：风险概念对比引导 + 常规概念能力正则</h2>
  <p class="paper-section__intro">冻结原始扩散模型作为教师，通过两组预测噪声约束微调模型：在有害概念上改变模型的擦除方向，在常规概念上复现原模型行为，从而只修正高风险场景。</p>

  <div class="alignment-formulas">
    <div class="paper-formula">
      <p class="paper-formula__label">有害概念安全对齐</p>
      <div class="paper-formula__equation" role="math" aria-label="有害概念擦除损失">
        <span>ℒ<sub>erase</sub> =</span>
        <span>‖ε<sub>θ</sub>(x<sub>t</sub>) ⊙ m − η(ε<sub>porn</sub><sup>*</sup> ⊙ m − ε<sub>null</sub><sup>*</sup> ⊙ m)‖<sub>2</sub><sup>2</sup></span>
      </div>
      <p class="paper-formula__note"><strong>作用：</strong>对有害概念的预测噪声进行对比引导，使微调模型远离会产生色情内容的擦除方向，逼近安全的“不擦除”行为，迫使高风险擦除请求失效。</p>
    </div>

    <div class="paper-formula">
      <p class="paper-formula__label">常规概念能力保持</p>
      <div class="paper-formula__equation" role="math" aria-label="常规概念预测正则项">
        <span>ℒ<sub>reg</sub> =</span>
        <span>‖(ε<sub>θ</sub>(x<sub>t</sub><sup>reg</sup>) − ε<sup>*</sup>(x<sub>t</sub><sup>reg</sup>)) ⊙ m‖<sub>2</sub><sup>2</sup></span>
      </div>
      <p class="paper-formula__note"><strong>作用：</strong>在普通概念和常规 Mask 上对齐微调模型与冻结原模型的预测输出，限制模型行为漂移，保持基础擦除能力。</p>
    </div>
  </div>

  <div class="paper-formula">
    <p class="paper-formula__label">联合训练目标</p>
    <div class="paper-formula__equation" role="math" aria-label="联合损失等于有害概念擦除损失与常规概念正则项之和">
      <span>ℒ<sub>total</sub> = ℒ<sub>erase</sub> + λℒ<sub>reg</sub></span>
    </div>
    <p class="paper-formula__note">第一项负责安全方向修正，第二项负责通用能力保持；λ 控制安全增强与原始功能保真的权衡。</p>
  </div>

  <figure class="paper-figure paper-figure--contained paper-figure--source">
    <img src="{{ '/images/projects/huawei-model-safety-alignment/method-overview.png' | relative_url }}" alt="有害概念预测噪声对比引导和常规概念预测输出对齐的双目标安全微调框架">
    <figcaption><strong>方法框架。</strong>上半部分由冻结模型提供有害与无害预测方向，下半部分训练目标模型；另一组常规样本约束微调前后的预测一致性，防止安全增强破坏普通擦除能力。</figcaption>
  </figure>
</section>

<section class="paper-section paper-chapter" id="results">
  <p class="paper-section__eyebrow">03 · Experiments</p>
  <h2>实验结果：涉黄安全指标达到 96%</h2>
  <p class="paper-section__intro">实验仅关注人物衣物擦除中的色情内容风险。安全对齐后，模型对普通擦除请求仍保持有效，同时显著降低高风险 Mask 产生色情内容的概率。</p>
  <div class="paper-stats" aria-label="涉黄安全实验结果">
    <div class="paper-stat"><strong>80%</strong><span>合同要求</span></div>
    <div class="paper-stat"><strong>96%</strong><span>实际涉黄安全指标</span></div>
    <div class="paper-stat"><strong>+16pp</strong><span>超过合同要求</span></div>
    <div class="paper-stat"><strong>近 50%</strong><span>色情内容抑制成功率提升</span></div>
  </div>

  <figure class="paper-figure paper-figure--contained paper-figure--source">
    <img src="{{ '/images/projects/huawei-model-safety-alignment/results-sexual-only.png' | relative_url }}" alt="人物衣物擦除输入图像、输入 Mask、原始输出和安全对齐输出的对比">
    <figcaption><strong>涉黄场景可视化。</strong>同一组人物图像和衣物 Mask 下，原模型输出存在色情内容风险；经过安全对齐后，结果避免生成对应有害内容。此处仅展示项目 PPT 中的涉黄实验区域。</figcaption>
  </figure>
</section>

<section class="paper-section paper-chapter" id="ownership">
  <p class="paper-section__eyebrow">04 · Ownership</p>
  <h2>个人职责：从风险定位到安全微调方案验证</h2>
  <div class="paper-steps">
    <div class="paper-step"><strong>1 · 问题建模</strong><span>将衣物擦除产生色情内容的问题转化为扩散模型预测噪声方向的安全对齐任务。</span></div>
    <div class="paper-step"><strong>2 · 损失设计</strong><span>设计有害概念对比损失与常规概念正则项，分别负责风险抑制和能力保持。</span></div>
    <div class="paper-step"><strong>3 · 效果验证</strong><span>围绕涉黄安全性和常规擦除能力进行对比实验，验证方案满足交付要求。</span></div>
  </div>
</section>

<section class="paper-section" id="public-note">
  <p class="paper-section__eyebrow">Public Note</p>
  <h2>公开说明</h2>
  <p class="paper-section__intro">本页面仅介绍衣物擦除场景中的色情内容安全实验。示例来自项目 PPT 汇总图，原始高分辨率数据、训练样本及内部系统实现不公开。</p>
</section>
