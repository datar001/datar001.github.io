---
layout: paper
permalink: /projects/cmingre/
title: "CMIngre｜从一道菜走向食材级跨模态理解"
excerpt: "面向中餐场景构建细粒度跨模态食材基准，以 8,001 张图像、429 类食材与 95,290 个边界框支持食材检测和图文双向检索。"
author_profile: false
og_locale: zh_CN
paper_theme: cmingre
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#publications" target="_self" aria-label="返回论文列表">← 返回论文列表</a>
  <span>论文项目页</span>
  <span class="paper-project__venue">IEEE TMM · 2025</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Chinese Food Understanding · Multimodal Benchmark</p>
    <h1>CMIngre</h1>
    <p class="paper-hero__subtitle">把“这是什么菜”推进到“图中有哪些食材、它们在哪里、又如何与文字描述对应”</p>
    <p class="paper-hero__authors">Lanjun Wang · <strong>Chenyu Zhang</strong> · An-An Liu · Bo Yang · Mingwang Hu · Xinran Qiao · Lei Wang · Jianlin He · Qiang Liu</p>
    <p class="paper-hero__affiliation">Toward Chinese Food Understanding: A Cross-Modal Ingredient-Level Benchmark · Chenyu Zhang: First Student Author</p>
    <div class="paper-actions" aria-label="论文资源">
      <a class="paper-action paper-action--primary" href="https://ieeexplore.ieee.org/document/10496846/">IEEE 论文</a>
      <a class="paper-action" href="https://doi.org/10.1109/TMM.2024.3387735">DOI</a>
      <a class="paper-action" href="https://huggingface.co/datasets/huzimu/CMIngre">数据集</a>
      <a class="paper-action" href="#method" target="_self">任务与基线</a>
    </div>
  </div>
  <figure class="paper-hero__visual paper-hero__visual--contained">
    <img src="{{ '/images/projects/cmingre/annotation.png' | relative_url }}" alt="CMIngre 中菜名、菜谱和用户内容三类图文来源及食材边界框标注">
    <figcaption>CMIngre 同时保留文本食材与图像区域标注，数据来自菜品、菜谱和用户生成内容三类真实场景。</figcaption>
  </figure>
</header>

<section class="paper-section" id="motivation">
  <p class="paper-section__eyebrow">01 · Motivation</p>
  <h2>研究动机：识别菜名，不等于理解一道菜</h2>
  <p class="paper-section__intro">中餐图像中的食材往往经历切碎、混合、遮挡和烹饪变形。整图标签只能告诉模型“这是什么菜”，却无法建立局部视觉区域与具体食材之间的关系。</p>
  <div class="paper-grid">
    <article class="paper-card">
      <span class="paper-card__index">01</span>
      <h3>同物异名</h3>
      <p>“圣女果 / 小番茄”“松花蛋 / 皮蛋”等别名会造成标签重复，需要借助标准本体进行合并。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">02</span>
      <h3>细粒度差异</h3>
      <p>相近食材在外观上差异微弱，单靠视觉难以区分，文本描述可以提供补充线索。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">03</span>
      <h3>密集小目标</h3>
      <p>切碎食材边界模糊且互相遮挡；超过一半的边界框面积不到整图的 1%。</p>
    </article>
  </div>
  <figure class="paper-figure paper-figure--poster-flow">
    <img src="{{ '/images/pub/Food.png' | relative_url }}" alt="中餐食材同物异名与外观细微差异示例">
    <figcaption>标签规范化与细粒度外观辨识是构建中餐食材基准的两个典型难点。</figcaption>
  </figure>
</section>

<section class="paper-section" id="method">
  <p class="paper-section__eyebrow">02 · Benchmark & Method</p>
  <h2>核心方案：把食材区域标注与跨模态检索放进同一基准</h2>
  <p class="paper-section__intro">标注流程同时抽取文字中出现的食材，并为图中可见食材绘制边界框；随后清理冗余小框、合并同类相邻框，并依据国家食品成分数据表达标准规范同义标签。</p>
  <div class="paper-table-wrap">
    <table class="paper-table paper-table--compact">
      <thead><tr><th>数据来源</th><th>图像—文本对</th><th>占比</th><th>文本特点</th></tr></thead>
      <tbody>
        <tr><td>菜品（Dish）</td><td>1,719</td><td>21.5%</td><td>菜名，描述最简洁</td></tr>
        <tr><td>菜谱（Recipe）</td><td>2,330</td><td>29.1%</td><td>步骤与食材信息较完整</td></tr>
        <tr class="is-highlight"><td>用户内容（UGC）</td><td>3,952</td><td>49.4%</td><td>评论、环境与餐具噪声更多</td></tr>
      </tbody>
    </table>
  </div>

  <div class="paper-result-block paper-result-block--first">
    <p class="paper-result-block__index">Task 01 · Ingredient Detection</p>
    <h3>任务一：从菜品图像中定位并识别食材</h3>
    <p><strong>任务目标：</strong>输入一张中餐图像，输出每一种可见食材的类别与边界框，把整图级“菜品识别”细化为区域级“食材理解”。</p>
    <p><strong>方法设置：</strong>论文将该任务建模为 429 类细粒度目标检测，评测 Faster R-CNN、YOLOv5 与 DINO。模型需要同时处理切碎食材、小目标、严重遮挡和烹饪造成的外观变化，并以 AP50:95、AP50、AP75 衡量定位与分类质量。</p>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">Task 02 · Cross-modal Ingredient Retrieval</p>
    <h3>任务二：在图像与食材组合之间进行双向检索</h3>
    <p><strong>任务目标：</strong>同时支持“给定菜品图像，检索对应食材组合”和“给定一组食材，检索匹配菜品图像”，用 medR、Recall@K 与 Rsum 评价跨模态排序质量。</p>
    <p><strong>方法设置：</strong>图像侧使用检测器提取食材区域，文本侧编码单个食材及其组合；分层 Transformer 建模食材之间的共现关系，APS 再自适应聚合不同区域与食材的权重，将两种模态对齐到共享空间完成双向检索。</p>
  </div>
</section>

<section class="paper-section" id="results">
  <p class="paper-section__eyebrow">03 · Experiments</p>
  <h2>实验结果：细粒度区域建模同时提升检测与双向检索</h2>

  <div class="paper-result-block paper-result-block--first">
    <p class="paper-result-block__index">Task 01 · Detection Results</p>
    <h3>任务一结果：领域数据显著提升细粒度食材检测</h3>
    <p>DINO 在 7 个与 COCO 重合的食材类别上使用 CMIngre 微调后，AP50:95、AP50、AP75 分别提升 18.3、25.2、21.0，说明通用目标检测知识不足以覆盖切碎、混合和烹饪变形后的食材外观，领域级区域标注能够提供关键监督。</p>
    <div class="paper-stats">
      <div class="paper-stat"><strong>+18.3</strong><span>AP50:95</span></div>
      <div class="paper-stat"><strong>+25.2</strong><span>AP50</span></div>
      <div class="paper-stat"><strong>+21.0</strong><span>AP75</span></div>
      <div class="paper-stat"><strong>3 Sources</strong><span>Dish · Recipe · UGC</span></div>
    </div>
    <figure class="paper-figure paper-figure--poster-flow">
      <img src="{{ '/images/projects/cmingre/food-detection.jpg' | relative_url }}" alt="CMIngre 在菜品、菜谱和用户内容上的食材检测示例">
      <figcaption><strong>任务一 · 食材检测：</strong>同一套检测协议覆盖 Dish、Recipe 与 UGC，验证模型面对画质变化、密集小目标和遮挡时的定位能力。</figcaption>
    </figure>
  </div>

  <div class="paper-result-block">
    <p class="paper-result-block__index">Task 02 · Retrieval Results</p>
    <h3>任务二结果：区域特征与组合关系共同改善双向检索</h3>
    <p>在图像到食材组合检索中，分层 Transformer 将 ResNet 基线的 medR 从 62 降至 40；进一步使用检测器区域特征与 APS，在论文报告的各项检索指标上均优于端到端整图方案，其中 DINO 区域特征整体优于 Faster R-CNN。</p>
    <div class="paper-lead">
      <strong>核心结论：</strong>跨模态检索的提升并不只来自更强的整图编码器，而是来自两类细粒度信息——图像中的食材区域，以及文本中多种食材之间的组合关系。
    </div>
    <div class="paper-transfer-grid">
      <figure class="paper-figure paper-result-panel">
        <img src="{{ '/images/projects/cmingre/image-to-ingredient.jpg' | relative_url }}" alt="CMIngre 图像到食材组合的 Top-5 检索示例">
        <figcaption><strong>图像 → 食材组合：</strong>以菜品图像为查询，正确的食材组合排在检索列表首位。</figcaption>
      </figure>
      <figure class="paper-figure paper-result-panel">
        <img src="{{ '/images/projects/cmingre/ingredient-to-image.jpg' | relative_url }}" alt="CMIngre 食材组合到图像的 Top-5 检索示例">
        <figcaption><strong>食材组合 → 图像：</strong>以若干食材为查询，匹配菜品图像同样排在首位。</figcaption>
      </figure>
    </div>
  </div>
</section>

<section class="paper-section" id="summary">
  <p class="paper-section__eyebrow">04 · Summary</p>
  <h2>工作总结：把中餐理解从整图分类推进到食材级对齐</h2>
  <div class="paper-steps">
    <div class="paper-step"><strong>更细粒度的数据</strong><span>8,001 组图像—文本对、429 类标准化食材与 95,290 个区域框，为局部理解提供监督。</span></div>
    <div class="paper-step"><strong>更完整的任务</strong><span>在一套数据上同时支持食材检测、图像到食材组合和食材组合到图像的双向检索。</span></div>
    <div class="paper-step"><strong>更真实的场景</strong><span>菜品、菜谱与 UGC 三类来源覆盖开放环境中的长尾类别、遮挡、小目标和文本噪声。</span></div>
  </div>
  <div class="paper-insights">
    <div class="paper-insight"><h3>长尾与域偏差</h3><p>数据聚焦中餐且类别分布长尾，UGC 接近一半；模型结果不应直接外推到所有菜系、拍摄环境或营养判断。</p></div>
    <div class="paper-insight"><h3>数据许可</h3><p>公开数据卡标注为 CC BY-NC 4.0。使用者应以数据仓库中的最新许可与说明为准，并尊重非商业使用和署名要求。</p></div>
  </div>
  <div class="paper-actions paper-actions--section">
    <a class="paper-action paper-action--primary" href="https://huggingface.co/datasets/huzimu/CMIngre">查看数据集卡</a>
    <a class="paper-action" href="https://tech.meituan.com/2024/05/17/cross-modal-ingredient-level-dataset.html">中文技术解读</a>
  </div>
</section>

<section class="paper-section" id="citation">
  <p class="paper-section__eyebrow">Citation</p>
  <h2>引用</h2>
  <pre class="paper-citation">@article{wang2025toward,
  title   = {Toward Chinese Food Understanding: A Cross-Modal
             Ingredient-Level Benchmark},
  author  = {Wang, Lanjun and Zhang, Chenyu and Liu, An-An and
             Yang, Bo and Hu, Mingwang and Qiao, Xinran and
             Wang, Lei and He, Jianlin and Liu, Qiang},
  journal = {IEEE Transactions on Multimedia},
  volume  = {27},
  pages   = {2863--2874},
  year    = {2025},
  doi     = {10.1109/TMM.2024.3387735}
}</pre>
</section>

<footer class="paper-footer">
  Toward Chinese Food Understanding · IEEE Transactions on Multimedia 27: 2863–2874 · 2025
</footer>
