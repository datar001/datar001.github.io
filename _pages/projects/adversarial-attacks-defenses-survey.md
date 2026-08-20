---
layout: paper
permalink: /projects/adversarial-attacks-defenses-survey/
title: "AD-on-T2IDM｜文生图扩散模型攻防研究地图"
excerpt: "系统梳理文生图扩散模型的鲁棒性与安全风险，建立攻击、防御、评价指标和数据集的统一分类框架，并维护持续更新的 Awesome List。"
author_profile: false
og_locale: zh_CN
paper_theme: survey
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#publications" target="_self" aria-label="返回论文列表">← 返回论文列表</a>
  <span>论文项目页</span>
  <span class="paper-project__venue">Information Fusion · 2025</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Survey · Generative AI Security</p>
    <h1>AD-on-T2IDM</h1>
    <p class="paper-hero__subtitle">把分散的文生图攻击与防御工作，整理成一张可检索、可比较、可持续更新的研究地图</p>
    <p class="paper-hero__authors"><strong>Chenyu Zhang</strong> · Mingwang Hu · Wenhui Li · Lanjun Wang</p>
    <p class="paper-hero__affiliation">Information Fusion · Vol. 114 · Article 102701 · 2025</p>
    <div class="paper-actions" aria-label="论文资源">
      <a class="paper-action paper-action--primary" href="https://www.sciencedirect.com/science/article/pii/S1566253524004792">期刊正式版</a>
      <a class="paper-action" href="https://arxiv.org/abs/2407.15861">arXiv</a>
      <a class="paper-action" href="https://github.com/datar001/Awesome-AD-on-T2IDM">Awesome List</a>
      <a class="paper-action" href="#attacks" target="_self">攻防图谱</a>
    </div>
  </div>
</header>

<section class="paper-section" id="motivation">
  <p class="paper-section__eyebrow">01 · Motivation</p>
  <h2>研究动机：快速增长的攻防工作，缺少统一的比较坐标</h2>
  <p class="paper-section__intro">文生图扩散模型的安全研究横跨计算机视觉、自然语言处理与对抗机器学习。不同工作面对的攻击目标、访问权限和防御位置并不相同，只比较“攻击成功率”很容易把本质不同的问题混在一起。</p>
  <div class="paper-grid">
    <article class="paper-card">
      <span class="paper-card__index">01</span>
      <h3>问题边界分散</h3>
      <p>鲁棒性攻击、安全越狱、概念擦除和外部过滤往往使用不同术语，缺少共同的全局视图。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">02</span>
      <h3>实验口径不一</h3>
      <p>白盒与黑盒、字符级与句子级、干净提示与风险提示之间不能直接横向比较。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">03</span>
      <h3>资源更新很快</h3>
      <p>静态综述只能覆盖截稿前的研究，还需要公开、可维护的动态索引持续补充论文、代码与数据。</p>
    </article>
  </div>
</section>

<section class="paper-section" id="attacks">
  <p class="paper-section__eyebrow">02 · Attack Landscape</p>
  <h2>攻击版图：目标、模型知识与扰动粒度共同定义威胁</h2>
  <p class="paper-section__intro">综述没有把方法简单排列成名单，而是先回答三个正交问题：攻击者想让输出发生什么变化、能够访问多少模型信息、又通过怎样的文本改动实现目标。</p>
  <div class="paper-insights">
    <div class="paper-insight"><h3>Robustness · 非定向攻击</h3><p>让生成结果偏离原提示语义，用于暴露多对象、属性绑定、拼写错误或轻微语法噪声下的不稳定性。</p></div>
    <div class="paper-insight"><h3>Safety · 定向攻击</h3><p>绕过外部、内部或黑盒安全机制，并把输出推向预设概念，用于检验系统面对隐蔽提示时的真实安全性。</p></div>
  </div>
  <figure class="paper-figure paper-figure--wide-data paper-figure--source">
    <img src="{{ '/images/projects/adversarial-attacks-defenses-survey/figure4-attack-landscape.png' | relative_url }}" alt="论文 Figure 4：现有文生图对抗攻击方法的完整分类树">
    <figcaption><strong>论文 Fig. 4 · 攻击方法总览。</strong>非定向攻击按白盒 / 黑盒划分；定向攻击则进一步按外部、内部与黑盒安全机制组织。红色 C、W、S 分别表示字符级、词级和句子级扰动。</figcaption>
  </figure>
  <div class="paper-grid">
    <article class="paper-card"><span class="paper-card__index">T</span><h3>Target</h3><p>非定向攻击检验鲁棒性，定向攻击检验安全性；二者攻击成功的定义不同，不能只用同一个成功率直接比较。</p></article>
    <article class="paper-card"><span class="paper-card__index">K</span><h3>Knowledge</h3><p>白盒方法可利用模型结构、参数与梯度，黑盒方法只能借助查询结果、替代模型或大语言模型反馈搜索提示。</p></article>
    <article class="paper-card"><span class="paper-card__index">P</span><h3>Perturbation</h3><p>字符级改动通常更小；词级替换、前后缀和句子级重写更常见，却也更容易破坏自然度并被检测。</p></article>
  </div>
  <div class="paper-lead">
    <strong>综述观察：</strong>定向攻击数量明显多于非定向攻击，说明研究重点已经从“生成是否稳定”转向“安全机制能否被绕过”；同时，词级和句子级方法多于字符级方法，攻击隐蔽性仍是核心缺口。
  </div>
</section>

<section class="paper-section" id="defenses">
  <p class="paper-section__eyebrow">03 · Defense Landscape</p>
  <h2>防御版图：模型外部拦截输入，模型内部改变生成过程</h2>
  <p class="paper-section__intro">安全防御首先按部署位置分为 External Safeguards 与 Internal Safeguards。前者不修改生成模型，便于接入闭源服务；后者直接干预参数或中间特征，控制更深入，但通常需要访问开源模型内部。</p>

  <div class="paper-result-block paper-result-block--first">
    <p class="paper-result-block__index">External Safeguards</p>
    <h3>外部防线：在提示进入生成模型之前完成检测或改写</h3>
    <p>Prompt Classifier 直接判断输入是否接近风险概念；Prompt Transformation 则先把风险提示改写为安全表达，或把不自然的对抗提示还原成可被黑名单识别的语句。它们独立于扩散模型部署，但安全上限取决于能否理解隐式语义。</p>
    <figure class="paper-figure paper-figure--poster-flow paper-figure--source">
      <img src="{{ '/images/projects/adversarial-attacks-defenses-survey/figure5-external-safeguards.png' | relative_url }}" alt="论文 Figure 5：提示分类、提示安全改写与自然语言还原三类外部安全机制">
      <figcaption><strong>论文 Fig. 5 · 外部安全机制。</strong>三条路径分别对应提示分类、风险提示改写，以及“对抗提示还原 + 黑名单过滤”。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">Internal Safeguards</p>
    <h3>内部防线：在训练或推理阶段直接改变模型行为</h3>
    <p>Model Editing 在训练阶段修改文本编码器、交叉注意力或去噪网络参数，使目标概念难以被生成；Inference Guidance 不改权重，而是在推理期间修改潜变量或中间特征，把生成方向推离风险概念。</p>
    <figure class="paper-figure paper-figure--poster-flow paper-figure--source">
      <img src="{{ '/images/projects/adversarial-attacks-defenses-survey/figure6-internal-safeguards.png' | relative_url }}" alt="论文 Figure 6：模型编辑和推理引导两类内部安全机制">
      <figcaption><strong>论文 Fig. 6 · 内部安全机制。</strong>模型编辑永久改变生成能力；推理引导仅在生成阶段施加安全方向，部署更灵活。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">Defense Taxonomy</p>
    <h3>完整防御图谱：真正同时覆盖显式风险与对抗提示的方法仍然有限</h3>
    <figure class="paper-figure paper-figure--wide-data paper-figure--source">
      <img src="{{ '/images/projects/adversarial-attacks-defenses-survey/figure7-defense-landscape.png' | relative_url }}" alt="论文 Figure 7：外部与内部安全防御方法分类树">
      <figcaption><strong>论文 Fig. 7 · 防御方法总览。</strong>M 表示能够处理直接风险提示，A 表示进一步考虑对抗提示；图中多数方法只有 M，说明“挡住显式风险词”仍不等于具备对抗鲁棒性。</figcaption>
    </figure>
  </div>
</section>

<section class="paper-section" id="evaluation">
  <p class="paper-section__eyebrow">04 · Evaluation & Data</p>
  <h2>评测体系：同时回答“攻防是否有效”与“为此付出什么代价”</h2>
  <p class="paper-section__intro">单独报告攻击成功率或风险抑制率都不完整。攻击还要衡量提示是否自然隐蔽；防御还要衡量是否误伤正常概念，以及是否降低生成质量。</p>
  <div class="paper-transfer-grid">
    <article class="paper-result-panel">
      <h3 class="paper-result-subtitle">攻击侧：效果与隐蔽性</h3>
      <div class="paper-table-wrap">
        <table class="paper-table paper-table--compact">
          <thead><tr><th>评测问题</th><th>代表性指标</th><th>理想表现</th></tr></thead>
          <tbody>
            <tr><td>是否命中攻击目标</td><td>ACC、CLIP Score</td><td>目标相关性更强</td></tr>
            <tr><td>提示是否自然隐蔽</td><td>编辑距离、BR、PPL</td><td>改动更小、表达更自然</td></tr>
          </tbody>
        </table>
      </div>
    </article>
    <article class="paper-result-panel">
      <h3 class="paper-result-subtitle">防御侧：有效、特异与保真</h3>
      <div class="paper-table-wrap">
        <table class="paper-table paper-table--compact">
          <thead><tr><th>评测问题</th><th>代表性指标</th><th>理想表现</th></tr></thead>
          <tbody>
            <tr><td>是否抑制风险概念</td><td>风险分类准确率</td><td>风险生成更少</td></tr>
            <tr><td>是否保留正常概念</td><td>CLIP Score</td><td>误伤更少</td></tr>
            <tr><td>是否保持视觉质量</td><td>FID、IS</td><td>质量退化更小</td></tr>
          </tbody>
        </table>
      </div>
    </article>
  </div>
  <h3 class="paper-result-subtitle">数据也必须与评测问题匹配</h3>
  <div class="paper-grid">
    <article class="paper-card"><span class="paper-card__index">C</span><h3>正常数据</h3><p>MSCOCO、LAION-COCO、DiffusionDB 等用于检验正常生成能力、提示保持与防御误伤。</p></article>
    <article class="paper-card"><span class="paper-card__index">M</span><h3>显式风险数据</h3><p>直接描述风险、版权或敏感概念，用于检验基础过滤和概念擦除是否有效。</p></article>
    <article class="paper-card"><span class="paper-card__index">A</span><h3>对抗数据</h3><p>由攻击方法生成隐蔽提示，用于判断安全机制面对绕过策略时是否仍然可靠。</p></article>
  </div>
  <div class="paper-lead">
    <strong>使用边界：</strong>正常数据、显式风险数据和对抗数据分别对应效用、基础安全与真实韧性，三类数据不能互相替代。
  </div>
</section>

<section class="paper-section" id="challenges">
  <p class="paper-section__eyebrow">05 · Challenges & Directions</p>
  <h2>未来方向：从识别已知提示，走向理解可迁移的攻击规律</h2>
  <div class="paper-grid">
    <article class="paper-card"><span class="paper-card__index">01</span><h3>更自然的攻击</h3><p>许多方法依赖噪声词、长后缀或整句重写，容易被语法检测发现。未来攻击评测应把语言自然度和语义隐蔽性纳入首要目标。</p></article>
    <article class="paper-card"><span class="paper-card__index">02</span><h3>细微噪声下的鲁棒性</h3><p>多对象与属性绑定已有较多修正方法，但轻微拼写、字符和语法错误仍缺乏成熟防御；修复时还要避免破坏 CLIP 的基础编码能力。</p></article>
    <article class="paper-card"><span class="paper-card__index">03</span><h3>覆盖未知攻击的防线</h3><p>现有防御常针对显式风险词或特定噪声模板。更可持续的方向，是识别不同对抗提示共享的内部模式，而非不断记忆具体提示。</p></article>
  </div>
</section>

<section class="paper-section" id="summary">
  <p class="paper-section__eyebrow">06 · Summary</p>
  <h2>工作总结：从静态综述到持续更新的研究入口</h2>
  <p class="paper-section__intro">论文提供统一术语、攻防分类、评测指标和数据边界；配套 Awesome List 则持续补充最新论文、代码、数据集与工具，让综述在截稿后仍能继续生长。</p>
  <div class="paper-steps">
    <div class="paper-step"><strong>统一威胁模型</strong><span>用目标、知识与扰动粒度描述攻击，避免把本质不同的方法直接比较。</span></div>
    <div class="paper-step"><strong>统一防御位置</strong><span>区分外部检测 / 改写与内部编辑 / 引导，明确每类方法的能力边界和部署代价。</span></div>
    <div class="paper-step"><strong>统一评测口径</strong><span>同时衡量攻击效果与隐蔽性，以及防御有效性、特异性和生成保真度。</span></div>
  </div>
  <div class="paper-actions paper-actions--section">
    <a class="paper-action paper-action--primary" href="https://github.com/datar001/Awesome-AD-on-T2IDM">浏览 Awesome List</a>
    <a class="paper-action" href="https://github.com/datar001/Awesome-AD-on-T2IDM/issues">提交资源建议</a>
    <a class="paper-action" href="#citation" target="_self">引用本文</a>
  </div>
</section>

<section class="paper-section" id="responsible-use">
  <p class="paper-section__eyebrow">Responsible Use</p>
  <h2>研究边界与负责任使用</h2>
  <div class="paper-note">
    本综述和资源库用于帮助研究者理解风险、复现防御并建立更可靠的生成系统。部分原论文可能包含令人不适的模型生成内容；本项目页不展示攻击样例或敏感图像。涉及对抗测试的论文、代码和数据应仅在获得授权、遵守法律与机构伦理要求的环境中使用。
  </div>
</section>

<section class="paper-section" id="citation">
  <p class="paper-section__eyebrow">Citation</p>
  <h2>引用</h2>
  <pre class="paper-citation">@article{zhang2025adversarial,
  title   = {Adversarial Attacks and Defenses on Text-to-Image
             Diffusion Models: A Survey},
  author  = {Zhang, Chenyu and Hu, Mingwang and Li, Wenhui and Wang, Lanjun},
  journal = {Information Fusion},
  volume  = {114},
  pages   = {102701},
  year    = {2025},
  doi     = {10.1016/j.inffus.2024.102701}
}</pre>
</section>

<footer class="paper-footer">
  AD-on-T2IDM · Information Fusion 114 (2025) 102701 · DOI: 10.1016/j.inffus.2024.102701
</footer>
