---
layout: paper
permalink: /projects/targeted-attacks-stable-diffusion/
title: "Targeted Attacks｜用定向生成揭示 Stable Diffusion 漏洞"
excerpt: "将文生图攻击从非定向语义破坏推进到目标对象与目标风格控制，并从输入空间、CLIP 文本编码器和早期去噪注意力解释漏洞机制。"
author_profile: false
og_locale: zh_CN
paper_theme: targeted
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#publications" target="_self" aria-label="返回论文列表">← 返回论文列表</a>
  <span>论文项目页</span>
  <span class="paper-project__venue">Preprint · 2024</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Targeted Red Teaming · Model Interpretability</p>
    <h1>Targeted Attacks</h1>
    <p class="paper-hero__subtitle">不只让生成结果“出错”，而是检验攻击者能否把它稳定推向指定对象或指定风格</p>
    <p class="paper-hero__authors"><strong>Chenyu Zhang</strong><sup>†</sup> · Lanjun Wang<sup>†</sup> · Anan Liu</p>
    <p class="paper-hero__affiliation">Revealing Vulnerabilities in Stable Diffusion via Targeted Attacks · † Equal contribution · arXiv 2024</p>
    <div class="paper-actions" aria-label="论文资源">
      <a class="paper-action paper-action--primary" href="https://arxiv.org/abs/2401.08725">阅读论文</a>
      <a class="paper-action" href="https://github.com/datar001/Revealing-Vulnerabilities-in-Stable-Diffusion-via-Targeted-Attacks">代码仓库</a>
      <a class="paper-action" href="#method" target="_self">方法概览</a>
      <a class="paper-action" href="#citation" target="_self">引用</a>
    </div>
  </div>
  <figure class="paper-hero__visual paper-hero__visual--contained">
    <img src="{{ '/images/projects/targeted-attacks-stable-diffusion/figure1-targeted-example.png' | relative_url }}" alt="干净提示经过轻微扰动后稳定生成指定目标对象的论文示意图">
    <figcaption>论文 Fig. 1：原提示描述街角、密码锁、帆船和纸箱；加入不显眼的词语后，生成结果却稳定出现同一个指定对象。</figcaption>
  </figure>
</header>

<section class="paper-section" id="motivation">
  <p class="paper-section__eyebrow">01 · Motivation</p>
  <h2>研究动机：随机生成错误，不等于能够定向操纵</h2>
  <p class="paper-section__intro">非定向攻击只要让输出与原提示不一致即可成功；定向攻击则必须把生成结果推到预先指定的类别。后者成功条件更严格，也更能暴露模型是否存在可被稳定控制的隐藏路径。</p>
  <div class="paper-grid">
    <article class="paper-card">
      <span class="paper-card__index">01</span>
      <h3>目标必须命中</h3>
      <p>生成任意错误对象并不算成功，输出必须被分类为攻击者预先指定的对象或风格。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">02</span>
      <h3>文本是离散变量</h3>
      <p>图像像素可连续优化，而自然语言需要落回词表中的真实 token，梯度难以直接使用。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">03</span>
      <h3>攻击意图要隐藏</h3>
      <p>对抗提示不能直接包含与目标类别相关的词，同时还要与原提示保持足够相似。</p>
    </article>
  </div>
  <div class="paper-lead">
    <strong>灰盒威胁模型：</strong>攻击者可以访问公开且广泛复用的 CLIP 文本编码器，但将去噪网络和图像解码器视为黑盒；输出图像分类器用于判断目标是否命中，目标词检测器用于约束提示的隐蔽性。
  </div>
  <div class="paper-insights">
    <div class="paper-insight"><h3>目标对象攻击</h3><p>让与目标无关的干净提示生成指定对象。论文覆盖 10 个对象类别，并用 acc-K / acc-avg 衡量多次生成中的命中稳定性。</p></div>
    <div class="paper-insight"><h3>目标风格攻击</h3><p>把图像推向 animation、oil painting、sketch、watercolor 四种风格，同时尽量保留原始对象语义。</p></div>
  </div>
</section>

<section class="paper-section" id="method">
  <p class="paper-section__eyebrow">02 · Method</p>
  <h2>核心方法：在连续嵌入中优化，再投影回真实词表</h2>
  <p class="paper-section__intro">核心困难是“梯度能优化向量，却不能直接生成可用单词”。论文通过 proxy embedding 将连续梯度与最近邻离散 token 连接起来。</p>
  <figure class="paper-figure paper-figure--poster-flow">
    <img src="{{ '/images/projects/targeted-attacks-stable-diffusion/figure3-framework.png' | relative_url }}" alt="论文中的定向攻击方法框图，包括提示扰动优化、最近邻投影和目标图文匹配">
    <figcaption>论文 Fig. 3：下方参考图像定义目标语义，上方在提示嵌入中执行词替换或后缀添加；最近邻模块保证前向传播始终使用真实词表 token。</figcaption>
  </figure>
  <div class="paper-steps">
    <div class="paper-step"><strong>1 · 构造目标参考</strong><span>使用安全的类别模板生成参考图像，再由 CLIP 图像编码器提取目标特征。</span></div>
    <div class="paper-step"><strong>2 · 注入可学习扰动</strong><span>替换非名词 token，或在原提示末尾添加少量 suffix embedding，形成连续对抗表示。</span></div>
    <div class="paper-step"><strong>3 · Proxy Embedding</strong><span>前向传播使用词表最近邻向量，反向传播更新连续扰动，最终可逆查询为真实 token。</span></div>
  </div>
  <div class="paper-formula">
    <p class="paper-formula__label">目标语义引导 · Eq. 2</p>
    <div class="paper-formula__equation">
      <span>ℒ<sub>match</sub> =</span>
      <span><sup>1</sup>⁄<sub>U</sub> ∑<sub>u=1</sub><sup>U</sup></span>
      <span>[1 − cos(f<sub>ref</sub><sup>u</sup>, f<sub>x̃</sub>)]</span>
    </div>
    <p class="paper-formula__note">最小化对抗提示特征与多张目标参考图像特征之间的余弦距离，使生成语义稳定靠近指定对象或风格，而非依赖单张参考图。</p>
  </div>
  <div class="paper-transfer-grid">
    <div class="paper-formula">
      <p class="paper-formula__label">风格攻击的一致性约束 · Eq. 5–6</p>
      <div class="paper-formula__equation paper-formula__equation--stack">
        <span>ℒ<sub>mse</sub> = <sup>1</sup>⁄<sub>d</sub> ‖(f<sub>x</sub> ⊙ mask) − (f<sub>x̃</sub> ⊙ mask)‖<sub>2</sub><sup>2</sup></span>
        <span>ℒ<sub>sty</sub> = ℒ<sub>match</sub> + ℒ<sub>mse</sub></span>
      </div>
      <p class="paper-formula__note"><strong>作用：</strong>只推动风格接近目标，同时约束原始对象的显著语义，缓解“风格改对了、内容却丢了”的问题。</p>
    </div>
    <div class="paper-formula">
      <p class="paper-formula__label">Proxy Embedding · Eq. 8–9</p>
      <div class="paper-formula__equation paper-formula__equation--stack">
        <span>E<sub>p</sub><sup>k</sup> = arg max<sub>e∈E<sub>S</sub></sub> sim(ẽ<sub>x</sub><sup>k</sup>, e)</span>
        <span>x̃ = Emb<sup>−1</sup>(E<sub>p</sub>)</span>
      </div>
      <p class="paper-formula__note"><strong>作用：</strong>前向时选取允许词表中最接近的 token，反向时继续更新连续向量，从而连接“可求梯度”与“可读文本”。</p>
    </div>
  </div>
  <div class="paper-lead">
    <strong>隐蔽性约束：</strong>搜索前先移除与目标类别直接相关的词及其语义近邻；因此优化目标不是把类别名塞进提示，而是在受限词表中寻找能够激活相同生成语义的替代表达。
  </div>
</section>

<section class="paper-section" id="results">
  <p class="paper-section__eyebrow">03 · Experiments</p>
  <h2>实验结果：对象与风格两类定向攻击均显著优于基线</h2>
  <p class="paper-section__intro">对象攻击中，词替换策略达到 44.8% acc-10；风格攻击同样取得更高命中率，但 Semantic Consistency 表明，更强的风格操纵也可能牺牲部分原对象语义。</p>
  <div class="paper-transfer-grid">
    <article class="paper-result-panel">
      <h3 class="paper-result-subtitle">目标对象攻击</h3>
      <div class="paper-table-wrap">
        <table class="paper-table paper-table--compact">
          <thead><tr><th>扰动</th><th>方法</th><th>acc-5</th><th>acc-10</th><th>acc-avg</th><th>FID ↓</th></tr></thead>
          <tbody>
            <tr><td>替换</td><td>ATM</td><td>1.2%</td><td>2.2%</td><td>0.5%</td><td>96.69</td></tr>
            <tr><td>替换</td><td>RIATIG</td><td>3.3%</td><td>4.6%</td><td>1.3%</td><td>96.95</td></tr>
            <tr class="is-highlight"><td>替换</td><td>Ours</td><td>40.0%</td><td>44.8%</td><td>25.7%</td><td>81.41</td></tr>
            <tr><td>后缀</td><td>ATM</td><td>2.9%</td><td>4.8%</td><td>0.8%</td><td>96.72</td></tr>
            <tr><td>后缀</td><td>RIATIG</td><td>2.1%</td><td>2.9%</td><td>0.8%</td><td>96.25</td></tr>
            <tr class="is-highlight"><td>后缀</td><td>Ours</td><td>29.8%</td><td>35.1%</td><td>17.9%</td><td>87.29</td></tr>
          </tbody>
        </table>
      </div>
    </article>
    <article class="paper-result-panel">
      <h3 class="paper-result-subtitle">目标风格攻击</h3>
      <div class="paper-table-wrap">
        <table class="paper-table paper-table--compact">
          <thead><tr><th>扰动</th><th>方法</th><th>acc-10</th><th>acc-avg</th><th>SC ↑</th><th>FID ↓</th></tr></thead>
          <tbody>
            <tr><td>替换</td><td>ATM</td><td>26.2%</td><td>5.4%</td><td>88.5%</td><td>92.32</td></tr>
            <tr><td>替换</td><td>RIATIG</td><td>24.5%</td><td>6.8%</td><td>87.7%</td><td>90.20</td></tr>
            <tr class="is-highlight"><td>替换</td><td>Ours</td><td>29.8%</td><td>10.6%</td><td>85.7%</td><td>88.77</td></tr>
            <tr><td>后缀</td><td>ATM</td><td>24.0%</td><td>5.4%</td><td>88.8%</td><td>92.49</td></tr>
            <tr><td>后缀</td><td>RIATIG</td><td>23.5%</td><td>6.2%</td><td>90.9%</td><td>89.80</td></tr>
            <tr class="is-highlight"><td>后缀</td><td>Ours</td><td>28.3%</td><td>10.5%</td><td>87.8%</td><td>88.37</td></tr>
          </tbody>
        </table>
      </div>
    </article>
  </div>
  <h3 class="paper-result-subtitle">定性结果：一个后缀即可把生成结果推向指定目标</h3>
  <div class="paper-transfer-grid">
    <figure class="paper-figure paper-figure--source">
      <img src="{{ '/images/projects/targeted-attacks-stable-diffusion/figure9-object-results.png' | relative_url }}" alt="论文中的单后缀目标对象攻击可视化结果">
      <figcaption>论文 Fig. 9（节选）：上行为干净生成，下行为仅添加一个后缀 token 后的结果；原始描述仍基本保留，但目标对象已经进入图像。</figcaption>
    </figure>
    <figure class="paper-figure paper-figure--source">
      <img src="{{ '/images/projects/targeted-attacks-stable-diffusion/figure10-style-results.png' | relative_url }}" alt="论文中的单后缀目标风格攻击可视化结果">
      <figcaption>论文 Fig. 10（节选）：animation、sketch、oil painting、watercolor 四类目标风格均可由一个后缀触发，同时大体维持原对象内容。</figcaption>
    </figure>
  </div>
  <h3 class="paper-result-subtitle">后缀长度消融：更长的后缀带来更强攻击，也扩大文本偏离</h3>
  <div class="paper-table-wrap paper-figure--poster-table">
    <table class="paper-table paper-table--compact">
      <thead><tr><th>后缀 token 数</th><th>acc-5 ↑</th><th>acc-10 ↑</th><th>acc-avg ↑</th><th>FID ↓</th></tr></thead>
      <tbody>
        <tr><td>0</td><td>0.3%</td><td>0.3%</td><td>0.1%</td><td>—</td></tr>
        <tr><td>1</td><td>4.3%</td><td>4.8%</td><td>1.6%</td><td>93.05</td></tr>
        <tr><td>2</td><td>12.4%</td><td>14.6%</td><td>5.6%</td><td>91.27</td></tr>
        <tr><td>3</td><td>21.8%</td><td>24.4%</td><td>11.0%</td><td>88.72</td></tr>
        <tr><td>4</td><td>26.8%</td><td>30.3%</td><td>14.9%</td><td>87.18</td></tr>
        <tr class="is-highlight"><td>5（论文默认）</td><td>29.8%</td><td>35.1%</td><td>17.9%</td><td>87.29</td></tr>
      </tbody>
    </table>
  </div>
  <p class="paper-table-note">论文 Table 5。只加入一个后缀就能获得与既有基线相当的攻击效果；增加后缀数会继续提高命中率，但也更容易改变原提示语义。词替换消融还显示，动词和介词等关系词对攻击影响明显。</p>
</section>

<section class="paper-section" id="findings">
  <p class="paper-section__eyebrow">04 · Mechanism Findings</p>
  <h2>最后三个发现：目标语义如何从文本一路传到图像</h2>
  <p class="paper-section__intro">论文不只报告攻击成功率，还沿 Stable Diffusion 的生成链路分析成功样本，依次定位输入空间、CLIP 文本编码器和早期去噪网络中的漏洞机制。</p>

  <div class="paper-result-block paper-result-block--first">
    <p class="paper-result-block__index">Finding 01 · Input Space</p>
    <h3>屏蔽目标词，不等于屏蔽目标语义</h3>
    <p>当目标类别名称及其直接近义词不可用时，优化仍会找到四类替代表达：跨语言词、带有隐藏语义的词、缩写，以及与目标间接关联的实体。这些词表面上并未直接暴露攻击目标，却能把生成结果推向相同概念。</p>
    <figure class="paper-figure paper-figure--poster-flow paper-figure--source">
      <img src="{{ '/images/projects/targeted-attacks-stable-diffusion/figure4-input-space.png' | relative_url }}" alt="论文展示跨语言词、隐藏语义词、缩写和关联词触发目标对象的示例">
      <figcaption>论文 Fig. 4：pollo、spitfire、pizz、deere 分别通过跨语言、历史语义、缩写和品牌关联，触发鸡、战斗机、披萨与拖拉机。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">Finding 02 · CLIP Text Space</p>
    <h3>对抗提示越接近目标类别的 CLIP 语义，攻击越容易成功</h3>
    <p>论文使用零样本 CLIP 分类器，把与目标类别语义一致的攻击提示单独组成 SC 组。该组在不同后缀长度上的平均 acc-10 达到 66.9%，而全部攻击提示的平均值只有 21.8%。</p>
    <div class="paper-stats">
      <div class="paper-stat"><strong>66.9%</strong><span>SC 提示平均 acc-10</span></div>
      <div class="paper-stat"><strong>21.8%</strong><span>全部提示平均 acc-10</span></div>
      <div class="paper-stat"><strong>+45.1pp</strong><span>语义一致带来的提升</span></div>
      <div class="paper-stat"><strong>CLIP</strong><span>目标语义的中间通道</span></div>
    </div>
    <div class="paper-lead">
      <strong>含义：</strong>攻击并非随机碰撞生成器；它首先在 CLIP 文本空间建立与目标类别一致的全局表示，再由后续网络把这一隐藏语义解码成图像。
    </div>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">Finding 03 · Latent Denoising</p>
    <h3>目标轮廓在第一步去噪时就已经进入注意力图</h3>
    <p>论文观察第一扩散步、首个下采样块中的交叉注意力。即使此时的中间潜变量仍近似噪声，注意力图中往往已经出现最终目标对象的轮廓，说明定向语义在生成早期便完成空间布局。</p>
    <figure class="paper-figure paper-figure--poster-flow paper-figure--source">
      <img src="{{ '/images/projects/targeted-attacks-stable-diffusion/figure5-attention.png' | relative_url }}" alt="第一步去噪注意力图已经显现目标战斗机轮廓的论文示意图">
      <figcaption>论文 Fig. 5：第一步的潜变量尚未显现目标，但首个下采样块的注意力图已经勾勒出战斗机轮廓。</figcaption>
    </figure>
    <div class="paper-lead">
      <strong>83.6%：</strong>在成功的单后缀攻击样本中，研究者可以仅依据第一张注意力图识别出最终目标类别。这为在生成早期检测并阻断定向操纵提供了直接线索。
    </div>
  </div>
</section>

<section class="paper-section" id="summary">
  <p class="paper-section__eyebrow">05 · Summary</p>
  <h2>工作总结：定向攻击揭示了从文本输入到早期去噪的完整风险链路</h2>
  <div class="paper-steps">
    <div class="paper-step"><strong>更严格的任务</strong><span>从“让模型出错”推进到稳定命中指定对象或风格，更直接地衡量可控操纵风险。</span></div>
    <div class="paper-step"><strong>可优化的离散提示</strong><span>Proxy Embedding 连接连续梯度与真实词表，使对象和风格两类目标共享同一优化框架。</span></div>
    <div class="paper-step"><strong>可解释的漏洞链路</strong><span>输入空间、CLIP 文本表征与第一步去噪注意力共同解释定向语义如何传导到图像。</span></div>
  </div>
  <div class="paper-insights">
    <div class="paper-insight"><h3>当前公开版本</h3><p>本页按 arXiv v1（2024）整理，图号、公式与实验数据均以该版本为准。</p></div>
    <div class="paper-insight"><h3>实验范围</h3><p>主要实验基于 Stable Diffusion 2.1 与可访问的 CLIP 文本编码器；结论不应直接外推到所有闭源或新架构模型。</p></div>
  </div>
</section>

<section class="paper-section" id="responsible-use">
  <p class="paper-section__eyebrow">Responsible Use</p>
  <h2>研究边界与负责任使用</h2>
  <div class="paper-note">
    本工作用于在受控环境中理解定向操纵风险，并帮助开发者改进提示检测、文本表示鲁棒性和生成早期监测。本页不展示可直接复用的对抗提示词；论文代码应仅用于获得授权的安全研究、复现实验和防御验证，不得用于绕过真实服务的安全机制。
  </div>
</section>

<section class="paper-section" id="citation">
  <p class="paper-section__eyebrow">Citation</p>
  <h2>引用</h2>
  <pre class="paper-citation">@article{zhang2024revealing,
  title   = {Revealing Vulnerabilities in Stable Diffusion via
             Targeted Attacks},
  author  = {Zhang, Chenyu and Wang, Lanjun and Liu, Anan},
  journal = {arXiv preprint arXiv:2401.08725},
  year    = {2024}
}</pre>
</section>

<footer class="paper-footer">
  Revealing Vulnerabilities in Stable Diffusion via Targeted Attacks · arXiv:2401.08725 · Preprint
</footer>
