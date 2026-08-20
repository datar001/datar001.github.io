---
layout: paper
permalink: /projects/metaphor-jailbreak/
title: "MJA｜面向未知防御的隐喻式文生图安全红队测试"
excerpt: "无需预先知道安全防御类型，通过多智能体生成与贝叶斯优化，高效评测文生图系统在隐喻表达下的安全边界。"
author_profile: false
og_locale: zh_CN
paper_theme: mja
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#publications" target="_self" aria-label="返回论文列表">← 返回论文列表</a>
  <span>论文项目页</span>
  <span class="paper-project__venue">arXiv v2 · Preprint</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Black-box Red Teaming · T2I Safety</p>
    <h1>MJA</h1>
    <p class="paper-hero__subtitle">不知道防御类型，也能用隐喻暴露文生图系统的跨机制安全漏洞</p>
    <p class="paper-hero__authors"><strong>Chenyu Zhang</strong> · Lanjun Wang · Yiwen Ma · Wenhui Li · Yi Tu · An-An Liu</p>
    <p class="paper-hero__affiliation">Metaphor-based Jailbreak Attacks on Text-to-Image Models · arXiv v2 · 2026 修订</p>
    <div class="paper-actions" aria-label="论文资源">
      <a class="paper-action paper-action--primary" href="https://arxiv.org/abs/2512.10766">阅读论文</a>
      <a class="paper-action" href="https://github.com/datar001/metaphor-based-jailbreaking-attack">代码仓库</a>
      <a class="paper-action" href="#method" target="_self">方法概览</a>
      <a class="paper-action" href="#citation" target="_self">引用</a>
    </div>
  </div>
</header>

<section class="paper-section paper-chapter" id="motivation">
  <p class="paper-section__eyebrow">01 · Motivation</p>
  <h2>研究动机：在未知防御下评估含蓄表达的安全风险</h2>
  <div class="paper-insights">
    <article class="paper-insight">
      <h3>真实防御机制通常不可知</h3>
      <p>文本过滤、图像过滤和生成过程防御作用位置不同，针对单一机制设计的测试方法难以直接迁移到真实黑盒服务。</p>
    </article>
    <article class="paper-insight">
      <h3>有效性、隐蔽性与查询成本相互制约</h3>
      <p>表达过于直接容易被拦截，过于模糊又可能丢失视觉语义；逐个尝试大量候选则会显著增加黑盒评测成本。</p>
    </article>
  </div>
  <div class="paper-note">
    <strong>核心直觉 · Taboo 猜词游戏。</strong>安全过滤器像“裁判”，生成模型像“猜词者”：隐喻和语境可能避开显式文本风险特征，但生成模型仍能补全隐藏语义，由此暴露跨机制理解差异。
  </div>
</section>

<section class="paper-section paper-chapter" id="method">
  <p class="paper-section__eyebrow">02 · Method</p>
  <h2>核心方案：多智能体隐喻生成 + 黑盒主动优化</h2>
  <div class="paper-steps">
    <div class="paper-step"><strong>1 · LMAG 多智能体生成</strong><span>Metaphor、Context 与 Prompt Agent 依次完成隐喻检索、语境匹配和自然提示生成，构建多样候选池。</span></div>
    <div class="paper-step"><strong>2 · APO 代理建模</strong><span>利用文本特征、PCA 与高斯过程回归，近似候选提示与黑盒测试结果之间的未知关系。</span></div>
    <div class="paper-step"><strong>3 · 主动查询与更新</strong><span>Expected Improvement 平衡预测收益与不确定性，选择最值得测试的候选，并用真实反馈持续更新。</span></div>
  </div>
  <figure class="paper-figure paper-figure--source paper-figure--poster-flow">
    <img src="{{ '/images/projects/metaphor-jailbreak/framework.png' | relative_url }}?v=20260818" width="1482" height="669" alt="MJA 多智能体生成和对抗提示词优化方法图">
    <figcaption><strong>MJA 总体框架。</strong>左侧 LMAG 通过三个智能体生成多样、自然的隐喻候选；右侧 APO 使用代理模型和采集策略，把有限查询集中到最有希望的候选上。</figcaption>
  </figure>

  <div class="mja-method-formulas">
    <article class="paper-formula">
      <p class="paper-formula__label">LMAG · 三智能体协作生成</p>
      <div class="paper-formula__equation paper-formula__equation--stack" role="math" aria-label="LMAG 三个智能体依次生成隐喻、语境和对抗提示词">
        <span>𝒳<sub>met</sub> = {x<sub>met</sub><sup>i</sup>}<sub>i=1</sub><sup>N</sup> = A<sub>met</sub>(x<sub>sen</sub>, I<sub>met</sub>, E<sub>met</sub>)</span>
        <span>𝒳<sub>con</sub><sup>i</sup> = {x<sub>con</sub><sup>ij</sup>}<sub>j=1</sub><sup>M</sup> = A<sub>con</sub>(x<sub>sen</sub>, x<sub>met</sub><sup>i</sup>, I<sub>con</sub>, E<sub>con</sub>)</span>
        <span>x<sub>adv</sub><sup>ij</sup> = A<sub>adv</sub>(x<sub>sen</sub>, x<sub>met</sub><sup>i</sup>, x<sub>con</sub><sup>ij</sup>, I<sub>adv</sub>, E<sub>adv</sub>)</span>
      </div>
      <p class="paper-formula__note"><strong>生成逻辑：</strong>Metaphor Agent 先检索隐喻，Context Agent 为每个隐喻匹配语境，Prompt Agent 再组合二者，得到 N × M 个候选提示。任务示例 E 来自共享记忆与相似样例检索。</p>
    </article>

    <article class="paper-formula">
      <p class="paper-formula__label">APO · 代理建模与候选采集</p>
      <div class="paper-formula__equation paper-formula__equation--stack" role="math" aria-label="APO 通过高斯过程代理模型和期望改进采集函数选择候选提示词">
        <span>O(x<sub>adv</sub>) = Sim(ℳ(x<sub>adv</sub>), x<sub>sen</sub>) · 𝟙<sub>Query(x<sub>adv</sub>) ≠ None</sub></span>
        <span>h(x<sub>adv</sub>) = PCA(CLIP(x<sub>adv</sub>))</span>
        <span>𝒢(x<sub>adv</sub>) ∼ 𝒩(μ(x<sub>adv</sub>), σ<sup>2</sup>(x<sub>adv</sub>))</span>
        <span>EI(x<sub>adv</sub>) = (μ − O<sub>best</sub>)Φ(Z) + σφ(Z), &nbsp; Z = (μ − O<sub>best</sub>) / σ</span>
      </div>
      <p class="paper-formula__note"><strong>优化逻辑：</strong>先用 CLIP + PCA 表征提示词，再以 GPR 拟合候选提示与真实测试结果之间的映射；EI 同时考虑预测效果 μ 与不确定性 σ，自适应选择下一条黑盒查询。</p>
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="results">
  <p class="paper-section__eyebrow">03 · Experiments</p>
  <h2>实验结果：跨防御有效、查询更省，并可迁移到不同模型</h2>

  <div class="paper-result-block paper-result-block--first" id="defenses">
    <p class="paper-result-block__index">3.1 · Cross-defense Evaluation</p>
    <h3>在外部过滤与内部防御上保持均衡表现</h3>
    <div class="paper-table-wrap">
      <table class="paper-table">
        <thead><tr><th>防御设置</th><th>类型</th><th>BR ↑</th><th>ASR-C ↑</th><th>ASR-MLLM ↑</th></tr></thead>
        <tbody>
          <tr><td>text-cls + image-clip</td><td>多模态外部过滤</td><td>94%</td><td>79%</td><td>81%</td></tr>
          <tr><td>Latent Guard</td><td>对抗训练过滤器</td><td>100%</td><td>91%</td><td>93%</td></tr>
          <tr><td>GuardT2I</td><td>提示词改写检测</td><td>91%</td><td>82%</td><td>82%</td></tr>
          <tr><td>MACE</td><td>内部概念擦除</td><td>—</td><td>81%</td><td>85%</td></tr>
          <tr><td>Safree</td><td>内部安全引导</td><td>—</td><td>79%</td><td>87%</td></tr>
        </tbody>
      </table>
    </div>
    <p class="paper-table-note">四类风险共 400 条独立测试提示词。BR 表示是否绕过外部过滤器；内部防御直接干预生成过程，因此不单独报告 BR。</p>
  </div>

  <div class="paper-result-block" id="efficiency-transfer">
    <p class="paper-result-block__index">3.2 · Efficiency & Transferability</p>
    <h3>减少黑盒查询，并直接迁移到不同开源模型</h3>
    <div class="paper-transfer-grid">
      <div class="paper-result-panel">
        <h4 class="paper-result-subtitle">强防御下的查询效率</h4>
        <div class="paper-table-wrap">
          <table class="paper-table paper-table--compact">
            <thead><tr><th>防御</th><th>Sneaky ↓</th><th>MJA ↓</th><th>减少</th></tr></thead>
            <tbody>
              <tr><td>text-cls + image-clip</td><td>24 ± 18</td><td>8 ± 5</td><td>约 67%</td></tr>
              <tr><td>GuardT2I</td><td>20 ± 24</td><td>6 ± 5</td><td>约 70%</td></tr>
              <tr><td>SLD-Strong</td><td>18 ± 12</td><td>5 ± 5</td><td>约 72%</td></tr>
            </tbody>
          </table>
        </div>
        <p class="paper-table-note">多数设置仅需 3–8 次查询，且波动更小。</p>
      </div>
      <div class="paper-result-panel">
        <h4 class="paper-result-subtitle">迁移到不同开源模型</h4>
        <div class="paper-table-wrap">
          <table class="paper-table paper-table--compact">
            <thead><tr><th>模型</th><th>方式</th><th>BR ↑</th><th>ASR-C ↑</th><th>ASR-MLLM ↑</th></tr></thead>
            <tbody>
              <tr><td>SD1.4</td><td>黑盒优化</td><td>94%</td><td>78%</td><td>81%</td></tr>
              <tr><td>SD3</td><td>直接迁移</td><td>88%</td><td>69%</td><td>73%</td></tr>
              <tr><td>FLUX</td><td>直接迁移</td><td>89%</td><td>69%</td><td>70%</td></tr>
            </tbody>
          </table>
        </div>
        <p class="paper-table-note">无需重新执行黑盒优化，仍保持较高成功率。</p>
      </div>
    </div>
  </div>

  <div class="paper-result-block" id="commercial-ablation">
    <p class="paper-result-block__index">3.3 · Commercial Service & Ablation</p>
    <h3>商业系统验证与关键组件分析</h3>
    <div class="paper-transfer-grid">
      <div class="paper-result-panel">
        <h4 class="paper-result-subtitle">DALL·E 3 商业系统验证</h4>
        <div class="paper-table-wrap">
          <table class="paper-table paper-table--compact">
            <thead><tr><th>方法</th><th>BR ↑</th><th>ASR-C ↑</th><th>ASR-MLLM ↑</th><th>PPL ↓</th><th>Q ↓</th></tr></thead>
            <tbody>
              <tr><td>Sneaky</td><td>73%</td><td>40%</td><td>55%</td><td>1,000</td><td>28 ± 6</td></tr>
              <tr class="is-highlight"><td>MJA</td><td>95%</td><td>63%</td><td>78%</td><td>49</td><td>7 ± 6</td></tr>
            </tbody>
          </table>
        </div>
        <p class="paper-table-note">MJA 提高成功率，并将平均查询从 28 次降至 7 次。</p>
      </div>
      <div class="paper-result-panel">
        <h4 class="paper-result-subtitle">LMAG 多智能体消融</h4>
        <div class="paper-table-wrap">
          <table class="paper-table paper-table--compact">
            <thead><tr><th>设置</th><th>BR ↑</th><th>ASR-C ↑</th><th>ASR-MLLM ↑</th><th>PPL ↓</th></tr></thead>
            <tbody>
              <tr><td>仅 Prompt</td><td>83%</td><td>62%</td><td>66%</td><td>98</td></tr>
              <tr><td>+ Metaphor</td><td>83%</td><td>67%</td><td>71%</td><td>62</td></tr>
              <tr><td>+ Context</td><td>89%</td><td>71%</td><td>78%</td><td>75</td></tr>
              <tr class="is-highlight"><td>三智能体</td><td>93%</td><td>79%</td><td>81%</td><td>48</td></tr>
            </tbody>
          </table>
        </div>
        <p class="paper-table-note">隐喻、语境与提示生成协作共同提升成功率和语言自然度。</p>
      </div>
    </div>
  </div>

  <div class="paper-result-block" id="analysis">
    <p class="paper-result-block__index">3.4 · Why It Works</p>
    <h3>隐喻放大文本检测与视觉生成之间的理解差异</h3>
    <div class="mja-analysis-grid">
      <figure class="mja-analysis-card">
        <div class="mja-analysis-card__head">
          <span>Text-side Evidence</span>
          <h4>文本分类器难以识别隐含风险语义</h4>
        </div>
        <div class="mja-analysis-card__media mja-analysis-card__media--tsne">
          <img src="{{ '/images/projects/metaphor-jailbreak/semantic-analysis.png' | relative_url }}?v=20260820" width="876" height="384" alt="风险、安全和 MJA 对抗提示词在两个文本分类器中的 t-SNE 特征分布">
        </div>
        <figcaption><strong>论文 Fig. 7。</strong>在两个 NSFW 文本分类器的 t-SNE 可视化中，MJA 提示形成区别于直接风险与安全提示的分布外区域，说明分类器难以从隐喻表达中恢复稳定的风险特征。</figcaption>
      </figure>

      <figure class="mja-analysis-card">
        <div class="mja-analysis-card__head">
          <span>Generation-side Evidence</span>
          <h4>不同 seed 触发概率性语义补全</h4>
        </div>
        <div class="mja-analysis-card__media mja-analysis-card__media--seeds">
          <img src="{{ '/images/projects/metaphor-jailbreak/seed-analysis.png' | relative_url }}?v=20260820" width="1140" height="825" alt="同一隐喻提示在 SD1.4 和 FLUX 不同随机种子下的生成结果">
        </div>
        <figcaption><strong>论文 Fig. 9。</strong>同一隐喻提示在不同 seed 下产生不同视觉结果：绿色边框表示未出现显式风险内容，红色边框表示隐含风险语义被显化。文生图模型会沿不同采样轨迹补全含混上下文，并在部分轨迹中实现被隐藏的风险语义。</figcaption>
      </figure>
    </div>
    <div class="paper-note mja-analysis-conclusion">
      <strong>跨机制理解差异。</strong>隐喻表达使文本侧难以形成稳定的风险判别，但生成模型仍可能依据上下文与随机采样补全隐含语义，从而形成“文本侧漏检、生成侧触发”的安全缺口。
    </div>
  </div>
</section>

<section class="paper-section paper-chapter" id="conclusion">
  <p class="paper-section__eyebrow">04 · Conclusion</p>
  <h2>工作总结</h2>
  <div class="paper-grid">
    <article class="paper-card"><span class="paper-card__index">01</span><h3>面向未知防御</h3><p>无需预先知道安全机制类型，即可评估外部过滤器、内部生成防御和商业黑盒服务。</p></article>
    <article class="paper-card"><span class="paper-card__index">02</span><h3>LMAG + APO</h3><p>多智能体生成多样隐喻候选，主动优化把有限查询集中到更有希望的测试样本。</p></article>
    <article class="paper-card"><span class="paper-card__index">03</span><h3>跨模型有效</h3><p>方法在多类防御下保持均衡表现，并可直接迁移到 SD3、FLUX 与商业文生图系统。</p></article>
  </div>
</section>

<section class="paper-section" id="responsible-use">
  <p class="paper-section__eyebrow">Responsible Use</p>
  <h2>研究边界与负责任使用</h2>
  <div class="paper-note">
    MJA 用于在获得授权的环境中评估文生图系统对含蓄表达和未知防御的鲁棒性，目标是帮助开发者改进文本检测、生成过程监测和跨模态安全对齐。本页不展示可直接复用的攻击提示词或敏感生成样例；论文代码与实验材料也应仅用于合规的安全研究和红队测试。
  </div>
</section>

<section class="paper-section" id="citation">
  <p class="paper-section__eyebrow">Citation</p>
  <h2>引用</h2>
  <pre class="paper-citation">@article{zhang2025metaphor,
  title   = {Metaphor-based Jailbreak Attacks on Text-to-Image Models},
  author  = {Zhang, Chenyu and Wang, Lanjun and Ma, Yiwen and
             Li, Wenhui and Tu, Yi and Liu, An-An},
  journal = {arXiv preprint arXiv:2512.10766},
  year    = {2025}
}</pre>
</section>

<footer class="paper-footer">
  MJA · arXiv:2512.10766 · Preprint · 联系：zcy@tju.edu.cn
</footer>
