---
layout: paper
permalink: /projects/reason2attack/
title: "Reason2Attack｜让大模型学会推理式安全红队测试"
excerpt: "将文生图越狱攻击建模为大语言模型推理任务，通过 Frame Semantics、监督微调和强化学习提高攻击有效性与查询效率。"
author_profile: false
og_locale: zh_CN
paper_theme: r2a
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#publications" target="_self" aria-label="返回论文列表">← 返回论文列表</a>
  <span>论文项目页</span>
  <span class="paper-project__venue">AAAI 2026</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">LLM Reasoning · Generative AI Safety</p>
    <h1>Reason2Attack</h1>
    <p class="paper-hero__subtitle">将文生图越狱测试建模为大语言模型的推理与后训练任务</p>
    <p class="paper-hero__authors"><strong>Chenyu Zhang</strong> · Lanjun Wang · Yiwen Ma · Wenhui Li · Guoqing Jin · Anan Liu</p>
    <p class="paper-hero__affiliation">AAAI 2026 · Vol. 40(42) · 36030–36038</p>
    <div class="paper-actions" aria-label="论文资源">
      <a class="paper-action paper-action--primary" href="https://ojs.aaai.org/index.php/AAAI/article/view/40919">AAAI 正式论文</a>
      <a class="paper-action" href="https://arxiv.org/abs/2503.17987">arXiv</a>
      <a class="paper-action" href="#method" target="_self">方法概览</a>
      <a class="paper-action" href="#citation" target="_self">引用</a>
    </div>
  </div>
</header>

<section class="paper-section paper-chapter" id="motivation">
  <p class="paper-section__eyebrow">01 · Motivation</p>
  <h2>研究动机：现有方法缺少统一框架，攻击效率也偏低</h2>
  <div class="paper-insights">
    <article class="paper-insight">
      <h3>缺少统一的生成框架</h3>
      <p>已有方法依赖人工设计不同策略，引导 LLM 生成对抗提示词；这种方式耗时、彼此割裂，也无法系统探索开放世界中的多样表达。</p>
    </article>
    <article class="paper-insight">
      <h3>黑盒攻击效率较低</h3>
      <p>LLM 不理解目标文生图模型及其安全机制，往往需要大量查询才能获得成功样例，限制了真实红队评估中的可用性。</p>
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="method">
  <p class="paper-section__eyebrow">02 · Method</p>
  <h2>核心方案：统一生成框架 + 两阶段推理训练</h2>
  <div class="paper-steps">
    <div class="paper-step"><strong>1 · Frame Semantics 生成</strong><span>围绕风险概念检索关联词、生成语境描述、筛选有效表达，并将过程组织为可解释的 CoT 训练样例。</span></div>
    <div class="paper-step"><strong>2 · CoT 监督微调</strong><span>使用合成样例训练 LLM，使其内化从风险概念到隐蔽表达的逐步对抗提示词生成过程。</span></div>
    <div class="paper-step"><strong>3 · 在线强化学习</strong><span>把真实越狱测试接入在线训练，使 LLM 根据黑盒文生图模型及其安全机制的反馈持续优化。</span></div>
  </div>
  <figure class="paper-figure paper-figure--source paper-figure--poster-flow">
    <img src="{{ '/images/projects/reason2attack/framework.png' | relative_url }}?v=20260818" width="1592" height="492" alt="Reason2Attack 的 Frame Semantics 数据生成与两阶段推理训练框架">
    <figcaption><strong>R2A 总体框架。</strong>左侧利用 Frame Semantics 统一生成并筛选 CoT 对抗样例；右侧先通过监督微调学习推理过程，再利用攻击过程奖励进行在线强化学习。</figcaption>
  </figure>

  <div class="r2a-core-formulas">
    <article class="paper-formula">
      <p class="paper-formula__label">Frame Semantics · 统一样本构造</p>
      <div class="paper-formula__equation paper-formula__equation--stack" role="math" aria-label="基于 Frame Semantics 生成关联词、语境、对抗提示和思维链样例">
        <span>𝒲<sub>rel</sub> = {w<sub>rel</sub><sup>i</sup>}<sub>i=1</sub><sup>N</sup> = ConceptNet(w<sub>risk</sub>)</span>
        <span>𝒳<sub>context</sub> = {x<sub>context</sub><sup>i</sup>}<sub>i=1</sub><sup>N</sup> = LLM(w<sub>risk</sub>, w<sub>rel</sub><sup>i</sup>)</span>
        <span>𝒳<sub>adv</sub> = {x<sub>adv</sub><sup>i</sup>}<sub>i=1</sub><sup>N</sup> = LLM(w<sub>risk</sub>, w<sub>rel</sub><sup>i</sup>, x<sub>frame</sub><sup>i</sup>)</span>
        <span>𝒳<sub>cot</sub> = {x<sub>cot</sub><sup>i</sup>}<sub>i=1</sub><sup>N</sup> = LLM(w<sub>risk</sub>, w<sub>rel</sub><sup>i</sup>, x<sub>frame</sub><sup>i</sup>, x<sub>adv</sub><sup>i</sup>)</span>
      </div>
      <p class="paper-formula__note"><strong>构造逻辑：</strong>从风险概念 w<sub>risk</sub> 出发，检索关联词并生成框架语境，再筛选有效测试表达，最终合成包含完整推理过程的 CoT 样例。</p>
    </article>

    <article class="paper-formula r2a-training-formula">
      <p class="paper-formula__label">Two-stage Training · 两阶段推理训练</p>
      <div class="paper-formula__equation paper-formula__equation--stack" role="math" aria-label="Reason2Attack 的监督微调与在线强化学习目标">
        <span>𝓛<sub>SFT</sub> = −𝔼<sub>o∈D<sub>CoT</sub></sub>[log π<sub>θ</sub>(x<sub>cot</sub> | x<sub>sen</sub>)]</span>
        <span>𝓛<sub>RL</sub> = 1/G · Σ<sub>i=1</sub><sup>G</sup> min(α<sub>i</sub>A<sub>i</sub>, α<sub>i</sub><sup>clip</sup>A<sub>i</sub>) − βD<sub>KL</sub>(π<sub>θ</sub> ∥ π<sub>ref</sub>)</span>
      </div>
      <div class="r2a-reward-inline" id="reward">
        <p class="paper-reward__label">Attack Process Reward · 攻击过程奖励</p>
        <div class="paper-reward__formula" aria-label="攻击过程奖励等于长度奖励、隐蔽性奖励和有效性奖励的加权组合">
          <i>R</i><sub>AP</sub> = γ · <i>R</i><sub>length</sub> + (1 − γ) · <i>R</i><sub>stealth</sub> + <i>R</i><sub>effect</sub>
        </div>
        <div class="paper-reward__terms">
          <span><strong>R<sub>length</sub> · 长度</strong>约束提示词长度。</span>
          <span><strong>R<sub>stealth</sub> · 隐蔽性</strong>衡量能否绕过安全机制。</span>
          <span><strong>R<sub>effect</sub> · 有效性</strong>判断是否保留测试目标。</span>
        </div>
      </div>
      <p class="paper-formula__note"><strong>训练逻辑：</strong>第一阶段利用 D<sub>CoT</sub> 让模型内化逐步生成过程；第二阶段将真实黑盒反馈接入在线强化学习，并由长度、隐蔽性与有效性奖励共同引导策略优化。</p>
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="results">
  <p class="paper-section__eyebrow">03 · Experiments</p>
  <h2>实验结果：高成功率、低查询成本，并具备跨模型迁移性</h2>

  <div class="paper-result-block" id="blackbox">
    <p class="paper-result-block__index">3.1 · Black-box Attack</p>
    <h3>在四类风险上均取得最佳黑盒攻击表现</h3>
    <figure class="paper-figure paper-figure--source paper-figure--poster-flow">
      <img src="{{ '/images/projects/reason2attack/blackbox-results.png' | relative_url }}?v=20260818" width="2640" height="800" alt="R2A 在 Stable Diffusion 1.4 和安全过滤器上的黑盒攻击结果">
      <figcaption><strong>SD1.4 黑盒结果。</strong>在性内容、暴力、扰人内容和违法活动四类风险上，R2A 的平均攻击成功率达到 90%，平均仅需 2.5 次查询，显著优于现有方法。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block" id="transfer">
    <p class="paper-result-block__index">3.2 · Transferability</p>
    <h3>无需针对每个目标系统重新训练</h3>
    <div class="paper-transfer-grid">
      <div>
        <h4 class="paper-result-subtitle">迁移到不同开源模型</h4>
        <figure class="paper-figure paper-figure--source">
          <img src="{{ '/images/projects/reason2attack/transfer-open-source.png' | relative_url }}?v=20260818" width="1580" height="300" alt="R2A 在 SDV3 和 FLUX 开源模型上的迁移攻击结果">
          <figcaption><strong>开源模型迁移。</strong>R2A 在 SDV3 和 FLUX 上分别达到 78% 和 68% 的平均成功率，均优于现有对比方法。</figcaption>
        </figure>
      </div>
      <div>
        <h4 class="paper-result-subtitle">迁移到商业模型</h4>
        <figure class="paper-figure paper-figure--source">
          <img src="{{ '/images/projects/reason2attack/transfer-commercial.png' | relative_url }}?v=20260818" width="1580" height="320" alt="R2A 在 DALL-E 3 和 Midjourney 商业模型上的迁移攻击结果">
          <figcaption><strong>商业模型迁移。</strong>R2A 可直接迁移到 DALL·E 3 与 Midjourney，平均成功率分别达到 66% 和 62%。</figcaption>
        </figure>
      </div>
    </div>
  </div>

  <div class="paper-result-block" id="examples">
    <p class="paper-result-block__index">3.3 · Qualitative Examples</p>
    <h3>推理过程能够保持目标语义，同时生成更隐蔽的测试表达</h3>
    <figure class="paper-figure paper-figure--source paper-figure--poster-flow">
      <img src="{{ '/images/projects/reason2attack/attack-example.png' | relative_url }}?v=20260818" width="2650" height="790" alt="R2A 从风险提示词生成推理过程和对抗测试提示词的示例">
      <figcaption><strong>R2A 推理示例。</strong>模型先分析原始风险语义，再通过艺术化语境和关联概念生成更隐蔽的测试表达，同时尽可能保持目标语义。</figcaption>
    </figure>
  </div>
</section>

<section class="paper-section paper-chapter" id="conclusion">
  <p class="paper-section__eyebrow">04 · Conclusion</p>
  <h2>工作总结</h2>
  <div class="paper-grid">
    <article class="paper-card"><span class="paper-card__index">01</span><h3>推理式安全测试</h3><p>将文生图越狱问题形式化为 LLM 推理任务，提出 R2A 以更高效率发现模型的安全薄弱点。</p></article>
    <article class="paper-card"><span class="paper-card__index">02</span><h3>统一生成框架</h3><p>基于 Frame Semantics 统一组织关联词、语境描述、有效表达和 CoT 样例，实现逐步生成。</p></article>
    <article class="paper-card"><span class="paper-card__index">03</span><h3>攻击过程引导训练</h3><p>结合监督微调与在线强化学习，并综合隐蔽性、有效性和长度反馈，提升 LLM 的红队测试能力。</p></article>
  </div>
</section>

<section class="paper-section" id="responsible-use">
  <p class="paper-section__eyebrow">Responsible Use</p>
  <h2>研究边界与负责任使用</h2>
  <div class="paper-note">
    R2A 的目标是帮助开发者在受控环境中发现文生图系统的安全薄弱点，并推动过滤器、模型对齐和评测协议改进。论文与本页不提供可直接部署的攻击服务；相关方法和样例应仅用于获得授权的安全研究与红队测试。
  </div>
</section>

<section class="paper-section" id="citation">
  <p class="paper-section__eyebrow">Citation</p>
  <h2>引用</h2>
  <pre class="paper-citation">@article{zhang2026reason2attack,
  title   = {Reason2Attack: Jailbreaking Text-to-Image Models via LLM Reasoning},
  author  = {Zhang, Chenyu and Wang, Lanjun and Ma, Yiwen and
             Li, Wenhui and Jin, Guoqing and Liu, Anan},
  journal = {Proceedings of the AAAI Conference on Artificial Intelligence},
  volume  = {40},
  number  = {42},
  pages   = {36030--36038},
  year    = {2026},
  doi     = {10.1609/aaai.v40i42.40919}
}</pre>
</section>

<footer class="paper-footer">
  Reason2Attack · AAAI 2026 · DOI: 10.1609/aaai.v40i42.40919 · 联系：zcy@tju.edu.cn
</footer>
