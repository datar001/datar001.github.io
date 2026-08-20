---
layout: paper
permalink: /projects/t2i-riskyprompt/
title: "T2I-RiskyPrompt｜文生图安全评测基准"
excerpt: "面向文生图模型安全评测、攻击与防御的层次化风险提示词基准。"
author_profile: false
og_locale: zh_CN
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#publications" target="_self" aria-label="返回论文列表">← 返回论文列表</a>
  <span>论文项目页</span>
  <span class="paper-project__venue">AAAI 2026</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Text-to-Image Safety · Benchmark</p>
    <h1>T2I-RiskyPrompt</h1>
    <p class="paper-hero__subtitle">面向文生图模型安全评估、攻击与防御的高质量风险提示词基准</p>
    <p class="paper-hero__authors"><strong>Chenyu Zhang</strong> · Tairen Zhang · Lanjun Wang · Ruidong Chen · Wenhui Li · Anan Liu</p>
    <p class="paper-hero__affiliation">Tianjin University · AAAI 2026 · 36039–36047</p>
    <div class="paper-actions" aria-label="论文资源">
      <a class="paper-action paper-action--primary" href="https://ojs.aaai.org/index.php/AAAI/article/view/40920">AAAI 正式论文</a>
      <a class="paper-action" href="https://arxiv.org/abs/2510.22300">arXiv</a>
      <a class="paper-action" href="https://github.com/datar001/T2I-RiskyPrompt">代码与提示词</a>
      <a class="paper-action" href="https://huggingface.co/datasets/datarr/T2I-RiskyPrompt-ImageDataset">图像数据集</a>
      <a class="paper-action" href="#citation" target="_self">引用</a>
    </div>
  </div>
  <figure class="paper-hero__visual">
    <img src="{{ '/images/projects/t2i-riskyprompt/Taxonomy.png' | relative_url }}" alt="T2I-RiskyPrompt 的六类风险体系与代表性样例">
    <figcaption>六类一级风险、十四类细粒度子类及代表性样例。页面涉及安全研究中的敏感内容，示例已经过模糊处理。</figcaption>
  </figure>
</header>

<section class="paper-section paper-chapter" id="motivation">
  <p class="paper-section__eyebrow">01 · Motivation</p>
  <h2>研究动机：现有数据和评估工具都不够完整</h2>
  <div class="paper-insights">
    <article class="paper-insight">
      <h3>风险提示词数据集存在三类缺陷</h3>
      <p>已有数据集通常面临风险类别有限、标注粒度粗、有效性不足三个问题，因而难以覆盖真实世界中多样的风险表达。</p>
    </article>
    <article class="paper-insight">
      <h3>缺少可靠的风险图像检测器</h3>
      <p>已有基准往往缺乏专门的风险图像检测工具，无法稳定量化不同文生图模型实际生成内容的安全风险。</p>
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="dataset">
  <p class="paper-section__eyebrow">02 · Dataset & Detector</p>
  <h2>核心方案：高质量风险提示词数据集 + 风险图像检测器</h2>
  <p class="paper-section__intro">T2I-RiskyPrompt 不只是一个提示词列表，而是一套贯穿“风险体系设计—数据构建—风险识别”的评估底座。</p>
  <div class="paper-steps">
    <div class="paper-step"><strong>1 · 层次化风险体系</strong><span>使用 6 个一级风险类别和 14 个细粒度子类组织风险，为跨类别分析提供统一坐标系。</span></div>
    <div class="paper-step"><strong>2 · 更高质量的数据</strong><span>最终保留 6,432 条有效风险提示词，并为每条样本同时提供类别标签和风险理由。</span></div>
    <div class="paper-step"><strong>3 · 原因驱动的检测</strong><span>利用风险理由对多模态模型进行对齐，使检测器不仅判断风险，还能理解风险产生的原因。</span></div>
  </div>

  <figure class="paper-figure paper-figure--source paper-figure--poster-flow">
    <img src="{{ '/images/projects/t2i-riskyprompt/Data_Pipeline.png' | relative_url }}?v=20260818" width="2017" height="957" alt="T2I-RiskyPrompt 的层次化风险体系和六阶段数据构建流程">
    <figcaption><strong>数据构建流程。</strong>首先依据主流平台和企业安全政策建立层次化风险体系；随后依次进行提示词收集、提示词润色、多样性过滤、类别标注、有效性过滤和风险理由标注，以保证数据的有效性、多样性与细粒度。</figcaption>
  </figure>

  <div class="paper-result-block" id="detector">
    <p class="paper-result-block__index">Reason-driven Detector</p>
    <h3>风险理由让检测器获得更全面的细粒度识别能力</h3>
    <p class="paper-section__intro">通用多模态模型在不同风险类别上的识别能力并不均衡。加入基于风险理由的对齐后，多个模型在暴力、违法活动、版权、扰人内容和政治敏感等类别上获得显著提升。</p>
    <figure class="paper-figure paper-figure--source paper-figure--poster-table">
      <img src="{{ '/images/projects/t2i-riskyprompt/Risk_Detector_Evaluation.png' | relative_url }}?v=20260818" width="2500" height="927" alt="不同多模态模型在加入原因驱动方法前后的风险图像检测结果">
      <figcaption><strong>风险图像检测结果。</strong>原因驱动方法能够系统提升不同多模态模型的细粒度风险检测能力；其中 Qwen2.5-VL-3B 结合本文方法后平均准确率达到 91.8%，明显优于现有检测器。</figcaption>
    </figure>
  </div>
</section>

<section class="paper-section paper-chapter" id="experiments">
  <p class="paper-section__eyebrow">03 · Experiments</p>
  <h2>实验结果：从四类安全环节得到八条关键观察</h2>
  <p class="paper-section__intro">实验统一评估 8 个文生图模型、9 种防御方法、5 个安全过滤器和 5 种越狱攻击，揭示现有文生图安全机制的能力边界。</p>
  <div class="paper-results">
    <article class="paper-result">
      <div class="paper-result__copy">
        <h3>文生图模型评估</h3>
        <ol class="paper-result__insights" start="1">
          <li>生成能力越强的模型，往往也呈现出更高的安全风险。</li>
        </ol>
      </div>
      <img src="{{ '/images/projects/t2i-riskyprompt/T2I_Model_Evaluation.png' | relative_url }}" alt="文生图模型评测图表">
    </article>
    <article class="paper-result">
      <div class="paper-result__copy">
        <h3>文生图防御方法评估</h3>
        <ol class="paper-result__insights" start="2">
          <li>具有多样视觉表现形式的风险内容很难被统一防御。</li>
          <li>无需微调的方法难以同时防御多种 NSFW 风险类别。</li>
          <li>防御强度与图像生成质量之间存在显著权衡。</li>
        </ol>
      </div>
      <img src="{{ '/images/projects/t2i-riskyprompt/Defense_Evaluation.png' | relative_url }}" alt="文生图防御方法评测图表">
    </article>
    <article class="paper-result">
      <div class="paper-result__copy">
        <h3>安全过滤器评估</h3>
        <ol class="paper-result__insights" start="5">
          <li>提示词层面的风险通常比图像层面的风险更容易识别。</li>
          <li>基于特征的安全过滤器在不同风险类别上各有优势。</li>
        </ol>
      </div>
      <img src="{{ '/images/projects/t2i-riskyprompt/Filter_Evaluation.png' | relative_url }}" alt="安全过滤器评测图表">
    </article>
    <article class="paper-result">
      <div class="paper-result__copy">
        <h3>越狱攻击评估</h3>
        <ol class="paper-result__insights" start="7">
          <li>基于关键词的过滤器容易受到伪词攻击。</li>
          <li>基于特征的过滤器容易受到大语言模型生成攻击的影响。</li>
        </ol>
      </div>
      <img src="{{ '/images/projects/t2i-riskyprompt/Attack_Evaluation.png' | relative_url }}" alt="越狱攻击评测图表">
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="conclusion">
  <p class="paper-section__eyebrow">04 · Conclusion</p>
  <h2>工作总结</h2>
  <div class="paper-insights">
    <article class="paper-insight"><h3>层次化风险体系</h3><p>提出包含 6 个一级风险类别和 14 个细粒度子类的层次化风险分类体系。</p></article>
    <article class="paper-insight"><h3>高质量提示词数据</h3><p>构建包含 6,432 条风险提示词的 T2I-RiskyPrompt，每条样本同时带有类别标签和风险理由。</p></article>
    <article class="paper-insight"><h3>原因驱动风险检测</h3><p>提出原因驱动的风险图像检测方法，在细粒度类别上显著优于现有检测器。</p></article>
    <article class="paper-insight"><h3>全面的攻防评估</h3><p>系统评估模型、防御、过滤器和攻击策略，并总结出文生图安全能力与局限性的八条关键观察。</p></article>
  </div>
</section>

<section class="paper-section" id="responsible-use">
  <p class="paper-section__eyebrow">Responsible Use</p>
  <h2>研究边界与负责任使用</h2>
  <div class="paper-note">
    本项目面向文生图安全研究与受控评测，数据可能包含暴力、色情等敏感类别。请遵守所在地法律、机构伦理规范与数据许可要求，避免将相关内容用于公开生成服务、伤害性用途或不当传播。
  </div>
</section>

<section class="paper-section" id="citation">
  <p class="paper-section__eyebrow">Citation</p>
  <h2>引用</h2>
  <pre class="paper-citation">@inproceedings{zhang2026t2iriskyprompt,
  title     = {T2I-RiskyPrompt: A Benchmark for Safety Evaluation,
               Attack, and Defense on Text-to-Image Model},
  author    = {Zhang, Chenyu and Zhang, Tairen and Wang, Lanjun and
               Chen, Ruidong and Li, Wenhui and Liu, Anan},
  booktitle = {Proceedings of the AAAI Conference on Artificial Intelligence},
  volume    = {40},
  number    = {42},
  pages     = {36039--36047},
  doi       = {10.1609/aaai.v40i42.40920},
  year      = {2026}
}</pre>
</section>

<footer class="paper-footer">
  T2I-RiskyPrompt · AAAI 2026 · 36039–36047 · DOI: 10.1609/aaai.v40i42.40920
</footer>
