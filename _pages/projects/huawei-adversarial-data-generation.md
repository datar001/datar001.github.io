---
layout: paper
permalink: /projects/huawei-adversarial-data-generation/
title: "华为合作项目｜自动化对抗数据生成工具构建"
excerpt: "针对五类内容安全风险组合提示词逆向、Inpainting、DreamBooth、LoRA 与大语言模型，构建自动化对抗数据生产工具。"
author_profile: false
og_locale: zh_CN
paper_theme: industry
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#projects" target="_self" aria-label="返回项目列表">← 返回项目列表</a>
  <span>产业合作项目页</span>
  <span class="paper-project__venue">Huawei Collaboration</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Generative AI · Data Engine · Content Safety</p>
    <h1>自动化对抗数据生成工具构建</h1>
    <p class="paper-hero__subtitle">针对不同风险场景设计定制化生成与评估链路，规模化生产内容审核所需的高难样本</p>
    <p class="paper-hero__authors"><strong>个人角色：项目负责</strong> · 华为合作项目</p>
    <p class="paper-hero__affiliation">技术路线：提示词逆向 · Inpainting · DreamBooth · LoRA · LLM 场景扩写</p>
    <div class="paper-actions" aria-label="章节导航">
      <a class="paper-action paper-action--primary" href="#background" target="_self">1 · 项目背景</a>
      <a class="paper-action" href="#method" target="_self">2 · 技术方案</a>
      <a class="paper-action" href="#responsibility" target="_self">3 · 个人职责</a>
      <a class="paper-action" href="#results" target="_self">4 · 项目成果</a>
    </div>
  </div>
</header>

<section class="paper-section paper-chapter" id="background">
  <p class="paper-section__eyebrow">01 · Background</p>
  <h2>项目背景：为内容审核补充长尾与高难风险数据</h2>
  <div class="paper-lead">
    内容审核模型需要覆盖大量细粒度、长尾且不断变化的风险表达，但真实数据采集和人工构造的成本较高。项目没有用单一 LoRA 方案覆盖所有场景，而是依据每类数据的视觉属性、可控条件和验收方式，分别设计生成与评估链路，最终形成可重复、可规模化的数据生产能力。
  </div>
  <div class="paper-stats" aria-label="项目目标概览">
    <div class="paper-stat"><strong>2 类</strong><span>重点风险大类</span></div>
    <div class="paper-stat"><strong>5 类</strong><span>细分数据场景</span></div>
    <div class="paper-stat"><strong>10 万</strong><span>合同交付目标</span></div>
    <div class="paper-stat"><strong>规模化</strong><span>自动数据生产</span></div>
  </div>
</section>

<section class="paper-section paper-chapter" id="method">
  <p class="paper-section__eyebrow">02 · Technical Solution</p>
  <h2>技术方案：五类风险，五条定制数据生成链路</h2>
  <p class="paper-section__intro">每条链路都遵循“锚点数据 → 条件构造 → 图像生成 → 质量评估”的基本结构，但根据风险类型选用不同技术。以下示例图均裁切自项目 PPT 的对应结果区域。</p>

  <div class="industry-methods">
    <article class="industry-method">
      <div class="industry-method__media">
        <div class="industry-method__image industry-method__image--borderline" style="--project-image: url('{{ '/images/projects/huawei-adversarial-data-generation/ppt-overview.png' | relative_url }}')" role="img" aria-label="PPT 中的擦边色情数据生成示例"></div>
        <span>PPT 示例 · 擦边色情</span>
      </div>
      <div class="industry-method__content">
        <p class="industry-method__index">01 · 3.04 万张</p>
        <h3>擦边色情：真实图片引导的提示词逆向生成</h3>
        <div class="industry-method__flow" aria-label="擦边色情数据生成流程">
          <span>网络图片抓取</span><i>→</i><span>Prompt 逆向</span><i>→</i><span>文生图生成</span><i>→</i><span>NudeNet</span>
        </div>
        <p>使用开源 <a href="https://github.com/alex000kim/nsfw_data_scraper">NSFW Data Scraper</a> 从公开网络来源收集大规模 sexy 图片；随后利用我们在 <a href="{{ '/projects/targeted-attacks-stable-diffusion/' | relative_url }}" target="_self">Targeted Attacks</a> 工作中的提示词逆向技术，从图像恢复对应文本提示，再驱动图像生成模型构造同类样本。</p>
        <div class="industry-method__evaluation"><strong>评估方式</strong><span>使用 <a href="https://pypi.org/project/nudenet/">NudeNet</a> 对生成结果进行自动化评估。</span></div>
      </div>
    </article>

    <article class="industry-method">
      <div class="industry-method__media">
        <div class="industry-method__image industry-method__image--deformed" style="--project-image: url('{{ '/images/projects/huawei-adversarial-data-generation/ppt-overview.png' | relative_url }}')" role="img" aria-label="PPT 中的畸形色情数据生成示例"></div>
        <span>PPT 示例 · 畸形色情</span>
      </div>
      <div class="industry-method__content">
        <p class="industry-method__index">02 · 0.5 万张</p>
        <h3>畸形色情：利用早期 Inpainting 模型的擦除缺陷</h3>
        <div class="industry-method__flow" aria-label="畸形色情数据生成流程">
          <span>人物图片</span><i>+</i><span>服装 Mask</span><i>→</i><span>Inpainting</span><i>→</i><span>人工评估</span>
        </div>
        <p>针对早期 Stable Diffusion Inpainting 模型擦除与补全能力不足、容易产生人体结构异常的特性，设计人物 sexy 图片及对应服装区域 Mask，通过批量局部重绘自然产生畸形样本。</p>
        <div class="industry-method__evaluation"><strong>评估方式</strong><span>由人工检查样本是否呈现目标畸形特征，并排除无效生成结果。</span></div>
      </div>
    </article>

    <article class="industry-method">
      <div class="industry-method__media">
        <div class="industry-method__image industry-method__image--portrait" style="--project-image: url('{{ '/images/projects/huawei-adversarial-data-generation/ppt-overview.png' | relative_url }}')" role="img" aria-label="PPT 中的敏感人物肖像生成示例"></div>
        <span>PPT 示例 · 敏感人物</span>
      </div>
      <div class="industry-method__content">
        <p class="industry-method__index">03 · 1.55 万张</p>
        <h3>敏感人物：肖像锚点与多样化场景联合生成</h3>
        <div class="industry-method__flow" aria-label="敏感人物数据生成流程">
          <span>肖像锚点</span><i>+</i><span>LLM 场景扩写</span><i>→</i><span>DreamBooth</span><i>→</i><span>人脸识别</span>
        </div>
        <p>人工收集目标人物肖像作为身份锚点，并由大语言模型批量扩写不同背景、事件和人物活动等场景提示；随后使用 DreamBooth 进行肖像定制化生成，在保持人物身份的同时扩大场景覆盖。</p>
        <div class="industry-method__evaluation"><strong>评估方式</strong><span>使用人脸识别模型计算身份一致性，筛选能够正确保留目标人物特征的图像。</span></div>
      </div>
    </article>

    <article class="industry-method">
      <div class="industry-method__media">
        <div class="industry-method__image industry-method__image--slogan" style="--project-image: url('{{ '/images/projects/huawei-adversarial-data-generation/ppt-overview.png' | relative_url }}')" role="img" aria-label="PPT 中的涉政标语图片生成示例"></div>
        <span>PPT 示例 · 涉政标语</span>
      </div>
      <div class="industry-method__content">
        <p class="industry-method__index">04 · 1 万张</p>
        <h3>涉政标语：锚点文本与 FLUX 场景化文字生成</h3>
        <div class="industry-method__flow" aria-label="涉政标语数据生成流程">
          <span>标语锚点</span><i>+</i><span>LLM 场景扩写</span><i>→</i><span>FLUX 文本渲染</span><i>→</i><span>MLLM Judge</span>
        </div>
        <p>人工整理需要生成的标语锚点，由大语言模型批量构造包含不同背景、事件和载体的场景提示，再利用 FLUX 文本渲染模型生成包含目标标语的多样化图片。</p>
        <div class="industry-method__evaluation"><strong>评估方式</strong><span>采用 MLLM-as-a-Judge，判断目标文字是否正确出现以及图像场景是否满足生成要求。</span></div>
      </div>
    </article>

    <article class="industry-method">
      <div class="industry-method__media">
        <div class="industry-method__image industry-method__image--symbol" style="--project-image: url('{{ '/images/projects/huawei-adversarial-data-generation/ppt-overview.png' | relative_url }}')" role="img" aria-label="PPT 中的涉政标识图片生成示例"></div>
        <span>PPT 示例 · 涉政标识</span>
      </div>
      <div class="industry-method__content">
        <p class="industry-method__index">05 · 5.5 万张</p>
        <h3>涉政标识：标识 LoRA 过拟合与场景组合生成</h3>
        <div class="industry-method__flow" aria-label="涉政标识数据生成流程">
          <span>标识锚点</span><i>+</i><span>LLM 场景扩写</span><i>→</i><span>标识 LoRA</span><i>→</i><span>MLLM Judge</span>
        </div>
        <p>人工收集目标标识作为视觉锚点，通过大语言模型扩写多样化背景和事件提示；针对每类标识直接训练专用 LoRA，使模型充分拟合目标视觉概念，并将标识组合到不同生成场景中。</p>
        <div class="industry-method__evaluation"><strong>评估方式</strong><span>采用 MLLM-as-a-Judge，验证目标标识是否正确呈现以及生成场景是否合理。</span></div>
      </div>
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="responsibility">
  <p class="paper-section__eyebrow">03 · Ownership</p>
  <h2>个人职责：从技术框架到规模交付</h2>
  <div class="paper-grid">
    <article class="paper-card">
      <span class="paper-card__index">01</span>
      <h3>多技术链路设计</h3>
      <p>根据五类风险场景的不同数据特征，分别选择提示词逆向、Inpainting、DreamBooth、FLUX 文本渲染和 LoRA 定制方案。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">02</span>
      <h3>自动化工具构建</h3>
      <p>将网络数据获取、LLM 场景扩写、模型训练、批量推理与自动评估组织为规模化数据生产流程。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">03</span>
      <h3>交付目标推进</h3>
      <p>围绕合同目标推进多类别数据生成与交付，最终完成 11.6 万张高质量图像生产。</p>
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="results">
  <p class="paper-section__eyebrow">04 · Results</p>
  <h2>项目成果：超额完成交付，并转化为审核效果提升</h2>
  <p class="paper-section__intro">项目覆盖色情低俗、政治敏感两类重点风险，包括低俗擦边、畸形低俗、敏感人物、涉政标语和涉政标识等 5 个细分类别。</p>
  <div class="paper-stats" aria-label="项目核心成果">
    <div class="paper-stat"><strong>11.6 万</strong><span>累计交付图像</span></div>
    <div class="paper-stat"><strong>116%</strong><span>合同目标完成度</span></div>
    <div class="paper-stat"><strong>93% → 96%</strong><span>关键审核指标</span></div>
    <div class="paper-stat"><strong>+3pp</strong><span>审核效果提升</span></div>
  </div>
  <div class="paper-note">
    <strong>业务价值：</strong>生成数据补充了审核系统在长尾与高难风险场景中的训练样本，使关键审核指标由 93% 提升至 96%，验证了生成式数据生产链路对实际内容治理任务的增益。
  </div>
</section>

<section class="paper-section" id="public-note">
  <p class="paper-section__eyebrow">Public Note</p>
  <h2>公开说明</h2>
  <p class="paper-section__intro">本页面中的示例来自项目 PPT 汇总缩略图，仅用于说明五类数据的视觉范围。出于内容安全与合作信息保护考虑，不公开原始高清数据、内部训练集和系统实现细节。</p>
</section>
