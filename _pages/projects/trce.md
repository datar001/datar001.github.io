---
layout: paper
permalink: /projects/trce/
title: "TRCE｜兼顾可靠擦除与知识保留的文生图安全方法"
excerpt: "通过文本语义擦除与去噪轨迹引导两阶段协作，提高概念擦除对隐式和对抗提示的可靠性，同时保留模型的正常生成能力。"
author_profile: false
og_locale: zh_CN
paper_theme: trce
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#publications" target="_self" aria-label="返回论文列表">← 返回论文列表</a>
  <span>论文项目页</span>
  <span class="paper-project__venue">ICCV 2025</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Concept Erasure · Diffusion Model Safety</p>
    <h1>TRCE</h1>
    <p class="paper-hero__subtitle">先擦除提示词里的隐式风险语义，再把早期生成轨迹拨向安全方向</p>
    <p class="paper-hero__authors">Ruidong Chen · Honglin Guo · Lanjun Wang · <strong>Chenyu Zhang</strong> · Weizhi Nie · An-An Liu</p>
    <p class="paper-hero__affiliation">ICCV 2025 · 18927–18936 · Tianjin University</p>
    <div class="paper-actions" aria-label="论文资源">
      <a class="paper-action paper-action--primary" href="https://openaccess.thecvf.com/content/ICCV2025/html/Chen_TRCE_Towards_Reliable_Malicious_Concept_Erasure_in_Text-to-Image_Diffusion_Models_ICCV_2025_paper.html">ICCV 正式论文</a>
      <a class="paper-action" href="https://arxiv.org/abs/2503.07389">arXiv</a>
      <a class="paper-action" href="https://github.com/ddgoodgood/TRCE">代码仓库</a>
      <a class="paper-action" href="#citation" target="_self">引用</a>
    </div>
  </div>
  <figure class="paper-hero__visual">
    <img src="{{ '/images/projects/trce/figure1-overview.png' | relative_url }}" alt="TRCE 与现有概念擦除方法的效果和知识保留对比">
    <figcaption>论文 Fig. 1：TRCE 在正常与对抗提示下均能抑制目标概念，并在擦除效果与模型知识保留之间取得更优平衡。图中敏感区域已由原论文遮挡。</figcaption>
  </figure>
</header>

<section class="paper-section" id="motivation">
  <p class="paper-section__eyebrow">01 · Motivation</p>
  <h2>研究动机：擦掉一个关键词，不等于忘掉一个概念</h2>
  <p class="paper-section__intro">文生图模型中的概念并不只存在于关键词嵌入里，还分散在整句上下文、注意力映射与去噪轨迹中。只做关键词映射，往往无法覆盖隐式表达；扩大参数修改范围，又容易出现“过度擦除”。</p>
  <div class="paper-grid">
    <article class="paper-card">
      <span class="paper-card__index">01</span>
      <h3>隐式语义残留</h3>
      <p>提示词可以通过隐喻、关联或优化后的表达传递目标概念，即使不出现被擦除的关键词。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">02</span>
      <h3>视觉轨迹复现</h3>
      <p>风险语义可能在生成早期形成视觉轮廓，后续去噪会沿既定轨迹继续补全相关细节。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">03</span>
      <h3>知识保留冲突</h3>
      <p>更激进的参数编辑虽能增强擦除，却可能误伤无关概念、改变构图或降低图像质量。</p>
    </article>
  </div>
  <figure class="paper-figure paper-figure--poster-table">
    <img src="{{ '/images/projects/trce/figure2-embedding.png' | relative_url }}" alt="关键词和 EoT 特殊嵌入对显式与隐式风险语义的注意力差异">
    <figcaption>论文 Fig. 2：显式风险词可以直接定位关键词注意力，但隐式表达不包含可直接删除的关键词；[EoT] 聚合整句语义，因此更适合处理隐喻、联想和对抗提示。将 [EoT] 注意力直接置零会破坏整体内容，TRCE 因而选择把它映射到语境相近的安全语义。</figcaption>
  </figure>
</section>

<section class="paper-section" id="method">
  <p class="paper-section__eyebrow">02 · Method</p>
  <h2>核心方法：先擦除文本语义，再校正早期视觉轨迹</h2>
  <p class="paper-section__intro">TRCE 不把可靠性压力集中在一次模型编辑上，而是让两个阶段各自解决最适合的问题：先减少提示词影响，再小幅修正早期生成方向。</p>
  <figure class="paper-figure paper-figure--poster-flow">
    <img src="{{ '/images/projects/trce/figure3-framework.png' | relative_url }}" alt="TRCE 文本语义擦除与去噪轨迹引导两阶段方法框图">
    <figcaption>论文 Fig. 3：第一阶段用闭式解将风险提示的 [EoT] 表征映射到安全语义；第二阶段比较安全与风险预测，通过对比学习修正早期去噪方向。</figcaption>
  </figure>
  <div class="paper-insights">
    <div class="paper-insight"><h3>阶段一 · Textual Semantic Erasure</h3><p>扩展目标概念及其安全对应表达，提取整句 [EoT] 语义，并以闭式解更新交叉注意力的 Key / Value 矩阵。</p></div>
    <div class="paper-insight"><h3>阶段二 · Denoising Trajectory Steering</h3><p>缓存早期采样轨迹，用对比损失让预测靠近安全方向、远离风险方向，同时用正则项保持原模型知识。</p></div>
  </div>
  <h3 class="paper-result-subtitle">为什么选择 [EoT] 作为映射目标？</h3>
  <div class="paper-grid">
    <article class="paper-card">
      <span class="paper-card__index">S</span>
      <h3>[SoT] · 整体构图</h3>
      <p>对视觉内容和整体构图影响很强，直接修改容易造成大范围图像变化。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">K</span>
      <h3>[KEY] · 局部概念</h3>
      <p>更偏向词本身的独立含义，难以覆盖整句隐式语义，反复映射也容易导致知识遗忘。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">E</span>
      <h3>[EoT] · 上下文语义</h3>
      <p>聚合整句信息并关注显著区域，既能改变目标概念，又更有机会保留提示词的整体语境。</p>
    </article>
  </div>
  <div class="paper-formula">
    <p class="paper-formula__label">阶段一 · 文本语义擦除目标</p>
    <div class="paper-formula__equation" role="math" aria-label="L TSE 等于风险语义映射误差与知识保持误差的加权和">
      <span>ℒ<sub>TSE</sub>(W′) =</span>
      <span>Σ<sub>i</sub> ‖W′e<sub>i</sub><sup>m</sup> − We<sub>i</sub><sup>s</sup>‖<sub>2</sub><sup>2</sup></span>
      <span>+ η Σ<sub>j</sub> ‖W′e<sub>j</sub><sup>k</sup> − We<sub>j</sub><sup>k</sup>‖<sub>2</sub><sup>2</sup></span>
    </div>
    <p class="paper-formula__note"><strong>第一项负责擦除：</strong>把风险提示的 [EoT] 表征 e<sub>i</sub><sup>m</sup> 映射到语境对应的安全表征 e<sub>i</sub><sup>s</sup>；<strong>第二项负责保留：</strong>约束无关知识表征 e<sub>j</sub><sup>k</sup> 在更新前后保持一致。该二次目标可以直接求得闭式解，无需长时间迭代训练。</p>
  </div>
  <h3 class="paper-result-subtitle">只在生成早期“轻推一下”</h3>
  <figure class="paper-figure paper-figure--poster-table">
    <img src="{{ '/images/projects/trce/figure4-trajectory.png' | relative_url }}" alt="TRCE 在扩散早期改变一次预测后引导后续安全生成轨迹">
    <figcaption>论文 Fig. 4：扩散模型早期先确定整体轮廓。TRCE 在转折点之前将一次风险预测替换为安全方向，之后即使使用空文本继续推理，轨迹仍会沿安全内容逐步细化。</figcaption>
  </figure>
  <div class="paper-formula">
    <p class="paper-formula__label">阶段二 · 轨迹引导与知识保持</p>
    <div class="paper-formula__equation paper-formula__equation--stack" role="math" aria-label="轨迹引导损失由擦除损失和知识保持正则项组成">
      <span>ℒ<sub>erase</sub> = 𝔼[max(‖ε̂ − f<sub>safe</sub>‖<sub>2</sub><sup>2</sup> − ‖ε̂ − f<sub>unsafe</sub>‖<sub>2</sub><sup>2</sup> + δ, 0)]</span>
      <span>ℒ<sub>DTS</sub> = ℒ<sub>erase</sub> + λ‖ε̂(z<sub>t</sub><sup>u</sup>, ∅, t) − ε(z<sub>t</sub><sup>u</sup>, ∅, t)‖<sub>2</sub><sup>2</sup></span>
    </div>
    <p class="paper-formula__note">Triplet margin loss 让当前预测 ε̂ 靠近安全方向 f<sub>safe</sub>、远离风险方向 f<sub>unsafe</sub>；无条件预测正则项则限制模型行为偏移，避免安全增强破坏正常生成能力。</p>
  </div>
</section>

<section class="paper-section" id="results">
  <p class="paper-section__eyebrow">03 · Experiments</p>
  <h2>实验结果：可靠擦除与正常知识保留取得更优平衡</h2>
  <p class="paper-section__intro">ASR 越低，说明攻击越难让被擦除概念重新出现。TRCE 两阶段协作在 I2P 与四个对抗基准上均不超过 1.99%，同时保持接近原模型的图文一致性。</p>
  <div class="paper-grid">
    <article class="paper-card"><span class="paper-card__index">931</span><h3>真实网络提示</h3><p>使用 I2P 中 931 条相关风险提示，测试直接但多样的用户表达。</p></article>
    <article class="paper-card"><span class="paper-card__index">4×</span><h3>红队对抗提示</h3><p>覆盖 MMA、P4D、Ring-A-Bell 与 UnlearnDiff 四种攻击来源。</p></article>
    <article class="paper-card"><span class="paper-card__index">3M</span><h3>三类评价指标</h3><p>ASR 衡量可靠性，FID<sub>gen</sub> 衡量行为偏移，CLIP-S 衡量正常图文一致性。</p></article>
  </div>
  <h3 class="paper-result-subtitle">单概念擦除</h3>
  <div class="paper-table-wrap">
    <table class="paper-table paper-table--compact">
      <thead><tr><th>方法</th><th>I2P ↓</th><th>MMA ↓</th><th>P4D ↓</th><th>Ring ↓</th><th>UnDiff ↓</th><th>FID<sub>gen</sub> ↓</th><th>CLIP-S ↑</th></tr></thead>
      <tbody>
        <tr><td>RECE</td><td>6.34%</td><td>23.10%</td><td>32.00%</td><td>6.33%</td><td>15.49%</td><td>14.21</td><td>30.79</td></tr>
        <tr><td>MACE</td><td>7.09%</td><td>10.60%</td><td>7.95%</td><td>10.13%</td><td>11.27%</td><td>17.44</td><td>28.84</td></tr>
        <tr><td>AdvUnlearn</td><td>1.71%</td><td>0.30%</td><td>1.99%</td><td>6.33%</td><td>3.52%</td><td>13.84</td><td>28.93</td></tr>
        <tr><td>TRCE · Stage 1</td><td>5.05%</td><td>7.80%</td><td>7.95%</td><td>11.39%</td><td>11.97%</td><td>11.94</td><td>30.69</td></tr>
        <tr class="is-highlight"><td>TRCE · Stage 1 + 2</td><td>1.29%</td><td>1.40%</td><td>1.99%</td><td>1.27%</td><td>0.70%</td><td>12.08</td><td>30.71</td></tr>
      </tbody>
    </table>
  </div>
  <p class="paper-table-note">数据来自 ICCV 2025 论文 Table 1。TRCE 的优势是把低 ASR 与较好的 FID<sub>gen</sub> / CLIP-S 放在同一折中点，而非只优化单一指标。</p>
  <div class="paper-transfer-grid">
    <article class="paper-result-panel">
      <h3 class="paper-result-subtitle">七类风险联合擦除</h3>
      <p class="paper-table-note">总体不当内容率由原始 SD1.4 的 35.6% 降至 2.0%。</p>
      <div class="paper-table-wrap">
        <table class="paper-table paper-table--compact">
          <thead><tr><th>方法</th><th>风险率 ↓</th><th>FID<sub>gen</sub> ↓</th><th>FID<sub>real</sub> ↓</th><th>CLIP-S ↑</th></tr></thead>
          <tbody>
            <tr><td>SD1.4</td><td>35.6%</td><td>—</td><td>27.18</td><td>30.97</td></tr>
            <tr><td>Safree</td><td>8.8%</td><td>13.72</td><td>30.90</td><td>26.57</td></tr>
            <tr><td>MACE</td><td>5.6%</td><td>19.31</td><td>26.20</td><td>28.13</td></tr>
            <tr class="is-highlight"><td>TRCE</td><td>2.0%</td><td>12.11</td><td>27.23</td><td>30.48</td></tr>
          </tbody>
        </table>
      </div>
    </article>
    <article class="paper-result-panel">
      <h3 class="paper-result-subtitle">[EoT] 映射消融</h3>
      <p class="paper-table-note">只优化 [EoT] 在擦除可靠性与知识保留之间取得最佳折中。</p>
      <div class="paper-table-wrap">
        <table class="paper-table paper-table--compact">
          <thead><tr><th>映射目标</th><th>I2P ↓</th><th>对抗 ASR ↓</th><th>FID<sub>gen</sub> ↓</th><th>CLIP-S ↑</th></tr></thead>
          <tbody>
            <tr class="is-highlight"><td>[EoT]</td><td>5.05%</td><td>9.79%</td><td>11.94</td><td>30.69</td></tr>
            <tr><td>[KEY]</td><td>22.80%</td><td>53.09%</td><td>11.32</td><td>30.71</td></tr>
            <tr><td>[EoT]+[KEY]</td><td>6.34%</td><td>10.27%</td><td>12.34</td><td>30.35</td></tr>
            <tr><td>[EoT]+[SoT]</td><td>6.56%</td><td>11.22%</td><td>12.31</td><td>30.36</td></tr>
          </tbody>
        </table>
      </div>
    </article>
  </div>
  <p class="paper-table-note">消融仅比较第一阶段；加入轨迹引导后，对抗平均 ASR 由 9.79% 进一步降至约 1.33%，而 FID<sub>gen</sub> 仅由 11.94 变化至 12.08。</p>
</section>

<section class="paper-section" id="summary">
  <p class="paper-section__eyebrow">04 · Summary</p>
  <h2>工作总结：用两个轻量阶段化解“擦得干净”与“保留知识”的冲突</h2>
  <div class="paper-steps">
    <div class="paper-step"><strong>文本层面更完整</strong><span>利用 [EoT] 聚合整句上下文，覆盖关键词之外的隐式、关联和对抗语义。</span></div>
    <div class="paper-step"><strong>视觉层面更可靠</strong><span>只在生成早期校正去噪方向，阻断风险轮廓继续演化，同时减少全程干预。</span></div>
    <div class="paper-step"><strong>综合效果更均衡</strong><span>在单概念、多概念与四类红队提示上保持低 ASR，并维持正常内容的一致性与质量。</span></div>
  </div>
  <div class="paper-insights">
    <div class="paper-insight"><h3>公开实现</h3><p>代码仓库提供基于 SD1.4 的单概念和多概念两阶段训练、生成与评测脚本，以及预训练擦除模型链接。</p></div>
    <div class="paper-insight"><h3>补充材料</h3><p>论文进一步讨论艺术风格、名人概念擦除，以及向 SDXL、SD3、FLUX 等更新架构迁移时的兼容性问题。</p></div>
  </div>
</section>

<section class="paper-section" id="responsible-use">
  <p class="paper-section__eyebrow">Responsible Use</p>
  <h2>研究边界与负责任使用</h2>
  <div class="paper-note">
    TRCE 面向生成模型安全加固、合规治理和经授权的鲁棒性评估。概念擦除也可能误伤正常表达或放大数据偏差，因此部署前应进行分类别安全测试、知识保留评估与人工审查。本页不展示敏感生成样例，论文和代码中的评测材料应在受控环境中使用。
  </div>
</section>

<section class="paper-section" id="citation">
  <p class="paper-section__eyebrow">Citation</p>
  <h2>引用</h2>
  <pre class="paper-citation">@inproceedings{chen2025trce,
  title     = {TRCE: Towards Reliable Malicious Concept Erasure in
               Text-to-Image Diffusion Models},
  author    = {Chen, Ruidong and Guo, Honglin and Wang, Lanjun and
               Zhang, Chenyu and Nie, Weizhi and Liu, An-An},
  booktitle = {Proceedings of the IEEE/CVF International Conference
               on Computer Vision (ICCV)},
  pages     = {18927--18936},
  year      = {2025}
}</pre>
</section>

<footer class="paper-footer">
  TRCE · ICCV 2025 · 18927–18936 · Official code: github.com/ddgoodgood/TRCE
</footer>
