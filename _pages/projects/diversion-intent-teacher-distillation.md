---
layout: paper
permalink: /projects/diversion-intent-recognition/teacher-distillation/
title: "教师数据蒸馏｜将超长审核规则压缩进 Qwen"
excerpt: "使用超长审核规则诱导 Gemini 生成合规响应，再以短 System Prompt 训练 Qwen 学习复杂审核逻辑。"
author_profile: false
og_locale: zh_CN
paper_theme: intent
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/projects/diversion-intent-recognition/' | relative_url }}#training" target="_self" aria-label="返回导流意图识别项目页">← 返回核心二</a>
  <span>风险用户导流意图识别</span>
  <span class="paper-project__venue">Teacher Distillation</span>
</nav>

<header class="paper-hero distillation-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Long Rules · Gemini Teacher · Short Prompt</p>
    <h1>将 2,057 行审核规则<br>蒸馏进 Qwen</h1>
    <p class="paper-hero__subtitle">不要求学生模型在每条样本中重新阅读整本“审核手册”，而是让教师先执行规则，再让学生学习合规响应。</p>
    <div class="paper-actions" aria-label="章节导航">
      <a class="paper-action paper-action--primary" href="#problem" target="_self">1 · 为什么不能直接训练</a>
      <a class="paper-action" href="#pipeline" target="_self">2 · 蒸馏流程</a>
      <a class="paper-action" href="#prompt" target="_self">3 · 长规则全文</a>
      <a class="paper-action" href="#response" target="_self">4 · 响应示例</a>
    </div>
  </div>
</header>

<section class="paper-section paper-chapter" id="problem">
  <p class="paper-section__eyebrow">01 · The Bottleneck</p>
  <h2>为什么需要蒸馏</h2>
  <p class="paper-section__intro">模型需要先判断内容归属，再识别导流入口和跨点位关系，比较候选主题、执行高风险特殊规则，最后输出可回溯的证据链。规则之间还存在大量例外、优先级和排除条件。</p>

  <div class="paper-stats distillation-stats" aria-label="长审核规则规模">
    <div class="paper-stat"><strong>2,057</strong><span>规则行数</span></div>
    <div class="paper-stat"><strong>28,480</strong><span>字符数</span></div>
    <div class="paper-stat"><strong>74.8 KB</strong><span>纯文本体积</span></div>
    <div class="paper-stat"><strong>12 类</strong><span>最终意图主题</span></div>
  </div>

  <div class="distillation-rule-map">
    <article><span>01</span><h3>判断对象与归属</h3><p>区分当前用户自有内容、行为对象、辅助点位中的他人信息与举报上下文。</p></article>
    <article><span>02</span><h3>还原导流链路</h3><p>识别“看主页 → 个签”“评论 → 私信”“行为 → 关联内容”等真实承接关系。</p></article>
    <article><span>03</span><h3>比较主题边界</h3><p>处理场所推广与性主题、普通招聘与高收益兼职盘等细粒度冲突。</p></article>
    <article><span>04</span><h3>构造证据输出</h3><p>证据必须可定位、能支撑最终主题，并满足固定 JSON Schema 与强度定义。</p></article>
  </div>

  <div class="distillation-compare" aria-label="直接训练与教师数据蒸馏对比">
    <article class="distillation-compare__direct">
      <span>Direct Training</span>
      <h3>长规则直接进入每条 Qwen 样本</h3>
      <div><b>超长 System Prompt</b><i>+</i><b>多模态 Case</b><i>+</i><b>长推理响应</b></div>
      <p>规则反复占用上下文；学生模型需要同时理解规则、处理 Case 并学习输出格式。在当前训练配置下，有效样本信息受到挤压，训练更难稳定。</p>
    </article>
    <i aria-hidden="true">VS</i>
    <article class="distillation-compare__teacher">
      <span>Teacher Distillation</span>
      <h3>教师执行长规则，学生学习合规响应</h3>
      <div><b>短 System Prompt</b><i>+</i><b>多模态 Case</b><i>→</i><b>教师响应</b></div>
      <p>Gemini 负责读取完整审核手册并生成示范；Qwen 的训练输入保持简洁，将优化容量集中在业务推理和规范输出上。</p>
    </article>
  </div>
</section>

<section class="paper-section paper-chapter" id="pipeline">
  <p class="paper-section__eyebrow">02 · Distillation Pipeline</p>
  <h2>蒸馏流程：长规则只交给教师，短提示词留给学生</h2>
  <div class="distillation-pipeline" aria-label="教师数据蒸馏四阶段流程">
    <article><span>01 · Input</span><strong>P<sub>long</sub> + Case</strong><p>把完整审核逻辑与原始多模态 Case 输入 Gemini。</p></article>
    <i aria-hidden="true">→</i>
    <article class="is-teacher"><span>02 · Rollout</span><strong>Gemini 教师响应</strong><p>生成包含分析过程、最终主题、导流信息与证据链的合规答案。</p></article>
    <i aria-hidden="true">→</i>
    <article><span>03 · Quality</span><strong>质量过滤与选优</strong><p>过滤主题错误、Schema 错误和空推理，并比较证据、导流与推理质量。</p></article>
    <i aria-hidden="true">→</i>
    <article class="is-student"><span>04 · Train</span><strong>P<sub>short</sub> + Case → Qwen</strong><p>以筛选后的教师响应为监督目标，让 Qwen 内化审核逻辑。</p></article>
  </div>

  <div class="paper-formula distillation-formula">
    <p class="paper-formula__label">教师生成</p>
    <div class="paper-formula__equation" role="math" aria-label="教师模型根据长规则和案例生成监督响应">
      <span>y<sub>i</sub><sup>G</sup> = Gemini(P<sub>long</sub>, x<sub>i</sub>)</span>
    </div>
    <p class="paper-formula__label">学生学习</p>
    <div class="paper-formula__equation" role="math" aria-label="学生模型使用短提示词学习教师响应">
      <span>ℒ<sub>distill</sub> = −∑<sub>t</sub> log p<sub>Qwen</sub>(y<sub>i,t</sub><sup>G</sup> | P<sub>short</sub>, x<sub>i</sub>, y<sub>i,&lt;t</sub><sup>G</sup>)</span>
    </div>
    <p class="paper-formula__note"><strong>关键点：</strong>长规则没有消失，而是从“每条训练样本必须显式携带的上下文”转化为“教师响应中可被学生模仿的决策行为”。这一阶段在本项目中统一称为教师数据蒸馏，不表述为 RSFT。</p>
  </div>
</section>

<section class="paper-section paper-chapter" id="prompt">
  <p class="paper-section__eyebrow">03 · The Long Rulebook</p>
  <h2>长规则全文</h2>

  <div class="distillation-prompt-viewer">
    <div class="distillation-prompt-viewer__head">
      <div><span>LONG SYSTEM PROMPT</span><strong>2,057 lines · 28,480 characters · 74.8 KB</strong></div>
      <em>向下滚动查看全文 ↓</em>
    </div>
    {% capture long_review_system_prompt %}{% include projects/diversion-intent-recognition/long-review-system-prompt.txt %}{% endcapture %}
    <pre class="distillation-prompt-viewer__body" aria-label="导流意图识别长审核规则全文"><code>{{ long_review_system_prompt | escape }}</code></pre>
  </div>

</section>

<section class="paper-section paper-chapter" id="response">
  <p class="paper-section__eyebrow">04 · Distilled Response</p>
  <h2>学生同时学习审核 CoT 与结构化 Response</h2>

  <div class="distillation-response">
    <div class="distillation-response__cot">
      <div class="distillation-response__head">
        <span>&lt;think&gt; · TEACHER COT</span>
        <em>Gemini CoT 摘录</em>
      </div>
      <pre class="distillation-response__cot-body" aria-label="Gemini 0817 原始审核 CoT 摘录"><code>## 1. 全量点位扫描、候选证据与关系线索标记

- 用户资料·个签：“🛰️wx_xxx”
  - 导流信息：是。入口为个签，类型为变体联系方式；卫星表情通常指代微信。
  - 主题语义：无。它是主要导流承接点，需要寻找其他点位解释导流目的。

- 用户资料·昵称：“F is”
  - 导流信息：否；主题语义：无；作为背景信息处理。

- 投稿封面（img_003）
  - 画面为车内仪表盘前的手机麻将游戏界面。
  - 用户主动发布，属于潜在博彩主题证据，需要与个签入口关联分析。

- 评论内容与评论图片（img_007）
  - 多条评论包含“[足球]”，在导流语境下可能指向体育博彩。
  - 评论图片展示“双天至尊”麻将游戏界面，与投稿主题一致，强化博彩内容传播意图。

- 喜欢页与收藏页（img_004 / img_005）
  - 多张截图出现同类棋牌游戏界面，属于辅助点位。
  - 用户的喜欢、收藏行为进一步确认其对博彩游戏内容的持续关注。

- 关注页（img_006）
  - 出现“大发”“财神就在三层住，上楼就是发财路”等博彩文化黑话。
  - 虽然来自辅助点位，但属于高风险主题证据，不能忽略。

## 2. 用户整体导流意图关联推理

### 2.1 导流入口分析
用户主要导流入口位于个签，内容为“🛰️wx_xxx”，类型为变体联系方式（微信）。该入口本身不带主题，其目的需由账号其他内容确定。

### 2.2 用户自有内容主题分析
用户本人发布的内容高度聚焦：投稿与评论图片均展示麻将类游戏界面，多条评论反复出现足球内容。二者分别指向棋牌博彩与体育博彩，说明用户持续主动传播博彩主题内容。

### 2.3 行为关联展开
用户从高频评论、图片评论到独立投稿，围绕棋牌游戏与足球形成连续传播行为，而不是孤立兴趣点。这些内容用于强化账号的博彩属性并吸引同好。

### 2.4 跨点位链路分析
- 点位 A（个签）：提供微信变体联系方式，完成站外承接。
- 点位 B（投稿、评论）：用户本人通过棋牌游戏截图与足球内容明确博彩主题。
- 点位 C（喜欢、收藏、关注）：同类游戏与“发财路”等内容构成生态佐证。

链路形成典型的“内容引流 → 主页承接”模式：以博彩内容吸引目标用户，再通过个签联系方式完成下一步导流。

### 2.5 辅助点位核验与高风险判断
关注页出现明确博彩文化黑话，喜欢页和收藏页的大量棋牌游戏截图又与用户自有内容一致。这些辅助证据排除了偶然兴趣的解释，进一步证明用户主动聚合并传播博彩内容。

## 3. 最终导流链、主题比较与证据过滤

- 博彩主题：从个签入口、投稿与评论主题，到关注／喜欢／收藏佐证，证据链完整且存在明确高风险证据。
- 游戏陪玩或其他主题：无法解释发财、足球与棋牌游戏内容在多个点位的共同出现。

最终选择“博彩主题”。保留个签作为入口证据，保留用户投稿与评论作为核心主题证据，并保留关注、喜欢页中的高风险与一致性证据。</code></pre>
      <div class="distillation-response__foot">&lt;/think&gt;</div>
    </div>
    <div class="distillation-response__answer">
      <div class="distillation-response__head">
        <span>&lt;answer&gt; · FINAL RESPONSE</span>
        <em>结构化监督目标</em>
      </div>
      <pre aria-label="教师数据蒸馏的结构化响应示例"><code>{
  "最终意图主题": "博彩主题",
  "导流信息": {
    "导流入口": "个签",
    "导流类型": "变体联系方式"
  },
  "意图识别证据": [
    {
      "证据强度": "强支撑",
      "证据点位来源": "个签",
      "证据点位内容": "🛰️wx_xxx",
      "证据点位说明": "卫星表情指代微信，构成账号主要的站外导流承接方式。"
    },
    {
      "证据强度": "强支撑",
      "证据点位来源": "投稿内容",
      "证据点位内容": "[图片ID:img_003]",
      "证据点位说明": "用户本人发布手机麻将游戏界面，直接明确账号的棋牌博彩主题。"
    },
    {
      "证据强度": "强支撑",
      "证据点位来源": "评论内容",
      "证据点位内容": "[图片ID:img_007]",
      "证据点位说明": "用户在评论中主动发布麻将游戏界面，与投稿内容一致。"
    },
    {
      "证据强度": "强支撑",
      "证据点位来源": "评论内容",
      "证据点位内容": "[激情开麦][足球]",
      "证据点位说明": "多条评论反复出现足球内容，在导流背景下高度指向体育博彩。"
    },
    {
      "证据强度": "强支撑",
      "证据点位来源": "关注页",
      "证据点位内容": "[图片ID:img_006] 财神就在三层住，上楼就是发财路",
      "证据点位说明": "辅助点位出现明确博彩文化黑话，构成博彩主题的高风险证据。"
    },
    {
      "证据强度": "辅助支撑",
      "证据点位来源": "喜欢页",
      "证据点位内容": "[图片ID:img_004]",
      "证据点位说明": "同类棋牌游戏界面证明用户持续关注并聚合博彩内容。"
    }
  ],
  "导流主题判断依据": "用户以个签微信变体联系方式承接流量，并持续发布棋牌与足球博彩内容；关注、喜欢和收藏页中的同类高风险内容进一步形成跨点位博彩证据闭环。"
}</code></pre>
      <div class="distillation-response__foot">&lt;/answer&gt;</div>
    </div>
  </div>

  <div class="distillation-next-step">
    <div><span>NEXT</span><strong>教师数据蒸馏之后</strong><p>标准 GRPO 继续优化最终主题、格式与长度；下一阶段再加入中间证据和候选主题奖励，约束模型如何得到答案。</p></div>
    <a href="{{ '/projects/diversion-intent-recognition/' | relative_url }}#training" target="_self">返回核心二查看 GRPO 方案 →</a>
  </div>
</section>
