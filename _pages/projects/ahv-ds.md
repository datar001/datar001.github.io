---
layout: paper
permalink: /projects/ahv-ds/
title: "AHV-D&S｜在扩散 Transformer 内定位并抑制风险概念"
excerpt: "从模型内部注意力激活出发，在推理阶段自动检测风险 token，并依据注意力头敏感度进行动态抑制。"
author_profile: false
og_locale: zh_CN
paper_theme: ahv
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#publications" target="_self" aria-label="返回论文列表">← 返回论文列表</a>
  <span>论文项目页</span>
  <span class="paper-project__venue">ACM CCS · 2026</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Diffusion Transformer Safety · Training-free Defense</p>
    <h1>AHV-D&amp;S</h1>
    <p class="paper-hero__subtitle">在推理阶段，从模型内部注意力激活中直接识别并抑制风险内容</p>
    <p class="paper-hero__authors"><strong>Chenyu Zhang</strong> · Lanjun Wang · Yueyang Cheng · Ruidong Chen · Wenhui Li · An-An Liu</p>
    <p class="paper-hero__affiliation">What Concepts Lie Within? Detecting and Suppressing Risky Content in Diffusion Transformers · ACM CCS 2026</p>
    <div class="paper-actions" aria-label="章节导航">
      <a class="paper-action paper-action--primary" href="#motivation" target="_self">1 · 动机</a>
      <a class="paper-action" href="#findings" target="_self">2 · 核心发现</a>
      <a class="paper-action" href="#method" target="_self">3 · 方法概述</a>
      <a class="paper-action" href="#results" target="_self">4 · 实验结果</a>
    </div>
  </div>
</header>

<section class="paper-section paper-chapter" id="motivation">
  <p class="paper-section__eyebrow">01 · Motivation</p>
  <h2>动机：从“修正表征”转向“干预内部注意力激活”</h2>
  <div class="paper-lead">
    现有方法通常采用表征层面的风险擦除策略，在文本表征或图像潜在表征中去除风险。相比之下，我们聚焦模型推理阶段，从 Diffusion Transformer 内部的注意力激活中直接识别风险内容，并抑制负责风险语义注入的注意力连接。
  </div>
  <figure class="paper-figure paper-figure--contained paper-figure--source">
    <img src="{{ '/images/projects/ahv-ds/fig-01.png' | relative_url }}" alt="Figure 1：文本表征修正、AHV 检测与抑制、图像潜在表征修正三类策略对比">
    <figcaption><strong>Fig. 1 · 防御范式对比。</strong>现有方法主要在文本或图像潜在表征层面进行修正；AHV-D&amp;S 在生成过程中直接检测并抑制与风险语义相关的内部注意力激活。</figcaption>
  </figure>
</section>

<section class="paper-section paper-chapter" id="findings">
  <p class="paper-section__eyebrow">02 · Core Findings</p>
  <h2>核心发现：概念由少量高敏感注意力头主导</h2>
  <p class="paper-section__intro">我们从注意力头敏感度、功能因果性和概念可分性三个角度逐步验证：模型内部的概念表达并非均匀分布，而是集中在少量注意力头；这些响应还能组成可用于概念检测的 Attention Head Vector（AHV）。</p>

  <div class="paper-figure-grid">
    <figure class="paper-figure paper-figure--source">
      <img src="{{ '/images/projects/ahv-ds/fig-03.png' | relative_url }}" alt="Figure 3：不同注意力头对输入概念的敏感度热力图">
      <figcaption><strong>Fig. 3 · 敏感度差异。</strong>不同注意力头对同一输入概念呈现显著不同的敏感度，高响应只稀疏分布在少量头中。</figcaption>
    </figure>
    <figure class="paper-figure paper-figure--source">
      <img src="{{ '/images/projects/ahv-ds/fig-04.png' | relative_url }}" alt="Figure 4：按敏感度排序后的注意力头累计敏感度曲线">
      <figcaption><strong>Fig. 4 · 表达高度集中。</strong>前 5% 注意力头累计承载 78.2% 的概念敏感度，说明少量注意力头负责一个概念的主要表达。</figcaption>
    </figure>
    <figure class="paper-figure paper-figure--source">
      <img src="{{ '/images/projects/ahv-ds/fig-05.png' | relative_url }}" alt="Figure 5：按敏感度顺序抑制注意力头的概念抑制实验">
      <figcaption><strong>Fig. 5 · 少量头决定概念生成。</strong>仅抑制最敏感的前 5% 注意力头即可消除目标概念；即使抑制 99% 的低敏感头，目标概念仍然保留。</figcaption>
    </figure>
    <figure class="paper-figure paper-figure--source">
      <img src="{{ '/images/projects/ahv-ds/fig-06.png' | relative_url }}" alt="Figure 6：注意力头向量的 t-SNE 分布与风险安全相似度分布">
      <figcaption><strong>Fig. 6 · AHV 可检测输入概念。</strong>将各注意力头敏感度串联为 AHV 后，不同概念在向量空间中清晰可分，可直接作为输入概念的检测信号。</figcaption>
    </figure>
  </div>
</section>

<section class="paper-section paper-chapter" id="method">
  <p class="paper-section__eyebrow">03 · Method</p>
  <h2>方法概述：动态测量、风险评分、自适应抑制</h2>
  <p class="paper-section__intro">AHV-D&amp;S 是一种无需训练、无需修改模型参数的推理时防御。它不依赖风险关键词，而是依据 token 在模型内部触发的实际注意力响应判断生成风险。</p>
  <figure class="paper-figure paper-figure--contained paper-figure--source">
    <img src="{{ '/images/projects/ahv-ds/fig-07.png' | relative_url }}" alt="Figure 7：AHV-D&S 的动量式 AHV 测量与敏感度引导自适应抑制框架">
    <figcaption><strong>Fig. 7 · AHV-D&amp;S 框架。</strong>模型推理生成阶段，自动度量输入 token 的注意力头向量，并根据注意力头敏感度自适应计算 token 风险得分，进而在不同注意力头上动态抑制风险 token，实现安全内容生成。</figcaption>
  </figure>
  <div class="paper-steps">
    <div class="paper-step"><strong>1 · 动量式 AHV 测量</strong><span>跨去噪步和 Transformer Block 持续更新 token 级 AHV，捕捉风险语义在生成过程中的动态变化。</span></div>
    <div class="paper-step"><strong>2 · 风险—安全对比评分</strong><span>将输入 token 的 AHV 与风险原型库和安全原型库比较，得到由内部激活支持的风险方向。</span></div>
    <div class="paper-step"><strong>3 · 头级自适应抑制</strong><span>结合每个注意力头的敏感度调整风险分数，只削弱风险 token 在相关头中的注意力 logits。</span></div>
  </div>
</section>

<section class="paper-section paper-chapter" id="results">
  <p class="paper-section__eyebrow">04 · Experiments</p>
  <h2>实验结果：兼顾风险抑制与安全内容保持</h2>
  <p class="paper-section__intro">实验依次验证单风险抑制、多风险联合抑制、对抗鲁棒性、安全内容保持、跨模型迁移和抑制强度可控性。所有结果共同说明：AHV-D&amp;S 的优势不仅是“抑制更多”，而是更精准地干预风险相关生成路径。</p>

  <div class="paper-result-block">
    <p class="paper-result-block__index">4.1 · 单风险概念抑制</p>
    <h3>综合安全性与内容保持，取得最佳平衡</h3>
    <figure class="paper-figure paper-figure--source paper-figure--wide-data">
      <img src="{{ '/images/projects/ahv-ds/table-01.png' | relative_url }}" alt="Table 1：性内容抑制与正常内容保持定量比较">
      <figcaption><strong>Table 1 · 单风险定量结果。</strong>AHV-D&amp;S 将总风险率降至 45.9%，同时保持 LPIPS 0.07 和 CLIP 30.84，在风险抑制与安全内容保持的综合效果上最优。</figcaption>
    </figure>
    <figure class="paper-figure paper-figure--contained paper-figure--source">
      <img src="{{ '/images/projects/ahv-ds/fig-08.png' | relative_url }}?v=20260818" alt="Figure 8：不同方法在风险内容抑制上的可视化比较">
      <figcaption><strong>Fig. 8 · 局部精准抑制。</strong>我们的方法只抑制风险内容区域，对风险无关区域保持良好。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">4.2 · 多风险概念同时抑制</p>
    <h3>四类风险联合设置下仍保持最优表现</h3>
    <div class="paper-evidence-pair">
      <figure class="paper-figure paper-figure--source">
        <img src="{{ '/images/projects/ahv-ds/table-03.png' | relative_url }}" alt="Table 3：四类有害内容联合抑制定量比较">
        <figcaption><strong>Table 3 · 多风险联合抑制。</strong>在血腥、恐怖、武器和药物四类风险同时抑制时，AHV-D&amp;S 的各类别风险率均为最低，并维持具有竞争力的安全内容生成质量。</figcaption>
      </figure>
      <figure class="paper-figure paper-figure--source">
        <img src="{{ '/images/projects/ahv-ds/fig-11.png' | relative_url }}" alt="Figure 11：四类有害内容抑制可视化比较">
        <figcaption><strong>Fig. 11 · 多类风险可视化。</strong>同一套 AHV 检测与抑制机制可同时覆盖多种风险语义，并针对不同内容区域产生对应的抑制效果。</figcaption>
      </figure>
    </div>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">4.3 · 对抗提示鲁棒性</p>
    <h3>面对隐式和自动生成的对抗提示依然最稳健</h3>
    <figure class="paper-figure paper-figure--contained paper-figure--source">
      <img src="{{ '/images/projects/ahv-ds/table-04.png' | relative_url }}" alt="Table 4：I2P、RAB、MMA 与 UnlearnDiff 对抗提示鲁棒性比较">
      <figcaption><strong>Table 4 · 对抗鲁棒性。</strong>在 I2P、RAB、MMA 和 UnlearnDiff 四个对抗提示基准上，AHV-D&amp;S 均取得最低风险率，表明内部注意力激活检测比关键词过滤更能适应隐式风险表达。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">4.4 · 安全内容保持</p>
    <h3>抑制风险语义，不破坏正常图像结构</h3>
    <figure class="paper-figure paper-figure--source">
      <img src="{{ '/images/projects/ahv-ds/fig-12.png' | relative_url }}" alt="Figure 12：正常内容生成的视觉质量与结构保持比较">
      <figcaption><strong>Fig. 12 · 安全内容保持。</strong>在人物、动作交互、多元素组合、空间关系和属性绑定等正常提示中，我们的方法保持了原始模型的主体、布局与视觉风格。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">4.5 · 迁移到 Qwen-Image</p>
    <h3>不依赖单一模型结构，迁移后仍保持优势</h3>
    <figure class="paper-figure paper-figure--source paper-figure--wide-data">
      <img src="{{ '/images/projects/ahv-ds/table-05.png' | relative_url }}" alt="Table 5：AHV-D&S 在 Qwen-Image 上的风险抑制和正常内容保持结果">
      <figcaption><strong>Table 5 · 跨模型迁移。</strong>迁移到 Qwen-Image 后，总风险率由 100.0% 降至 38.0%，正常内容 CLIP 达到 31.49，证明方法可泛化到另一种先进 DiT 文生图模型。</figcaption>
    </figure>
    <figure class="paper-figure paper-figure--contained paper-figure--source">
      <img src="{{ '/images/projects/ahv-ds/fig-13.png' | relative_url }}" alt="Figure 13：Qwen-Image 上的风险内容抑制可视化">
      <figcaption><strong>Fig. 13 · Qwen-Image 可视化。</strong>在不同模型上，AHV-D&amp;S 仍能定位并抑制风险区域，同时保留人物、背景与构图等风险无关内容。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">4.6 · 抑制强度可调</p>
    <h3>按部署需求定制安全—效用平衡</h3>
    <figure class="paper-figure paper-figure--source paper-figure--wide-data">
      <img src="{{ '/images/projects/ahv-ds/fig-14.png' | relative_url }}" alt="Figure 14：抑制强度和自适应调制因子的超参数分析">
      <figcaption><strong>Fig. 14 · 可控安全—效用权衡。</strong>通过调整抑制强度 α，可按场景定制风险抑制水平；敏感度自适应调制则在相同强度下提供更好的风险抑制与安全内容保持平衡。</figcaption>
    </figure>
  </div>
</section>

<section class="paper-section" id="responsible-use">
  <p class="paper-section__eyebrow">Responsible Use</p>
  <h2>研究边界与负责任使用</h2>
  <div class="paper-note">
    本研究用于生成模型安全加固、内容治理与经授权的鲁棒性评估。页面中的风险样例沿用论文原图并保留遮挡处理。实际部署时，“风险”应由产品政策、地区法规和人工审核共同定义，同时持续评估误拦截、群体偏差与安全—效用权衡。
  </div>
</section>

<section class="paper-section" id="resources">
  <p class="paper-section__eyebrow">Resources</p>
  <h2>相关资源</h2>
  <p class="paper-section__intro">论文与代码尚未公开。公开版本上线后，会在此补充论文、代码和复现材料。</p>
  <div class="paper-actions paper-actions--section" aria-label="相关资源链接">
    <a class="paper-action" href="https://huggingface.co/black-forest-labs/FLUX.1-dev">FLUX.1-dev</a>
    <a class="paper-action" href="https://huggingface.co/Qwen/Qwen-Image">Qwen-Image</a>
    <a class="paper-action" href="mailto:zcy@tju.edu.cn">联系作者</a>
  </div>
</section>

<section class="paper-section" id="citation">
  <p class="paper-section__eyebrow">Citation</p>
  <h2>引用（正式出版信息待补充）</h2>
  <pre class="paper-citation">@inproceedings{zhang2026what,
  title     = {What Concepts Lie Within? Detecting and Suppressing
               Risky Content in Diffusion Transformers},
  author    = {Zhang, Chenyu and Wang, Lanjun and Cheng, Yueyang and
               Chen, Ruidong and Li, Wenhui and Liu, An-An},
  booktitle = {ACM SIGSAC Conference on Computer and
               Communications Security (CCS)},
  year      = {2026}
}</pre>
</section>

<footer class="paper-footer">
  AHV-D&amp;S · ACM CCS 2026 · Paper and code links will be added after public release
</footer>
