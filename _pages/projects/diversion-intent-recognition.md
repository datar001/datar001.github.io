---
layout: paper
permalink: /projects/diversion-intent-recognition/
title: "多模态内容安全项目｜风险用户导流意图识别"
excerpt: "将人工审核偏好沉淀为多点位证据链标注规范，并通过蒸馏与强化学习训练可解释的导流意图识别模型。"
author_profile: false
og_locale: zh_CN
paper_theme: intent
---

<nav class="paper-project__nav" aria-label="页面导航">
  <a class="paper-project__back" href="{{ '/' | relative_url }}#projects" target="_self" aria-label="返回项目列表">← 返回项目列表</a>
  <span>多模态内容安全项目页</span>
  <span class="paper-project__venue">Intent Recognition</span>
</nav>

<header class="paper-hero">
  <div class="paper-hero__copy">
    <p class="paper-hero__eyebrow">Multimodal Reasoning · Data Agent · RL</p>
    <h1>风险用户导流意图识别</h1>
    <p class="paper-hero__subtitle">从“检测到导流点位”进一步理解用户真正想把他人导向什么主题</p>
    <p class="paper-hero__authors"><strong>个人职责：数据基准与后训练方案设计</strong></p>
    <p class="paper-hero__affiliation">多点位用户理解 · 可回溯证据链 · Qwen3.6-35B-A3B · Teacher Distillation / GRPO</p>
    <div class="paper-actions" aria-label="章节导航">
      <a class="paper-action paper-action--primary" href="#task" target="_self">1 · 任务与案例</a>
      <a class="paper-action" href="#labeler" target="_self">2 · 标注智能体</a>
      <a class="paper-action" href="#training" target="_self">3 · 训练方案</a>
      <a class="paper-action" href="#results" target="_self">4 · 阶段结果</a>
    </div>
  </div>
</header>

<section class="paper-section paper-chapter" id="task">
  <p class="paper-section__eyebrow">01 · Task & Cases</p>
  <h2>任务定义：有导流行为，不等于知道导流意图</h2>
  <div class="paper-lead">
    本项目联合账号资料、投稿、评论、关注/喜欢/收藏页、群聊、私信与行为记录进行多点位推理，明确用户最终导流意图并回溯真实证据。
  </div>

  <article class="intent-full-case">
    <div class="intent-full-case__head">
      <div>
        <span>TEST SET CASE · UID 7644583021509051449</span>
        <h3>账号 xxx：从分散点位中识别“博彩主题”导流意图</h3>
      </div>
      <div class="intent-full-case__badges"><b>6 张原始图片</b><b>多点位文本</b><b>博彩主题</b></div>
    </div>

    <div class="intent-full-case__overview">
      <figure class="intent-full-case__media">
        <div class="intent-case-gallery">
          <div class="intent-case-gallery__primary">
            <img src="{{ '/images/projects/diversion-intent-recognition/case-7644583021509051449-post.jpg' | relative_url }}" alt="测试集案例 7644583021509051449 的原始投稿封面">
            <span>投稿封面 · 强支撑证据</span>
          </div>
          <div class="intent-case-gallery__thumbs">
            <div><img loading="lazy" src="{{ '/images/projects/diversion-intent-recognition/case-7644583021509051449-avatar.jpg' | relative_url }}" alt="测试集案例的原始用户头像"><span>头像</span></div>
            <div><img loading="lazy" src="{{ '/images/projects/diversion-intent-recognition/case-7644583021509051449-background.png' | relative_url }}" alt="测试集案例的原始背景图"><span>背景图</span></div>
            <div><img loading="lazy" src="{{ '/images/projects/diversion-intent-recognition/case-7644583021509051449-likes.jpg' | relative_url }}" alt="测试集案例的原始喜欢页截图"><span>喜欢页</span></div>
            <div><img loading="lazy" src="{{ '/images/projects/diversion-intent-recognition/case-7644583021509051449-favorites.jpg' | relative_url }}" alt="测试集案例的原始收藏页截图"><span>收藏页</span></div>
            <div><img loading="lazy" src="{{ '/images/projects/diversion-intent-recognition/case-7644583021509051449-following.jpg' | relative_url }}" alt="测试集案例的原始关注页头像拼接图"><span>关注页</span></div>
          </div>
        </div>
        <figcaption>测试集中的 6 张原始多点位图片；投稿封面是主题判断的关键视觉证据，其余图片作为联合推理上下文。</figcaption>
      </figure>
      <div class="intent-full-case__fields">
        <div><strong>账号资料</strong><p>昵称：<b>xxx</b><br>个签：“想看大哥纸波怎么办？薇心搜（电子村）”</p></div>
        <div><strong>投稿内容</strong><p>原始投稿封面文案：“大哥正在纸波百拉中！！四毛仔看过来”。其中“纸波百拉”体现博彩 / 赌博语境。</p></div>
        <div><strong>个人页与行为</strong><p>输入同时包含喜欢页、收藏页、关注页截图，以及 3 次点赞和 1 次“猫咪代码冲突名场面”搜索记录。</p></div>
        <div><strong>导流识别信息</strong><p>导流入口定位为<b>个签</b>，“薇心搜（电子村）”提供站外承接；测试集标签中的导流类型为<b>微信公众号</b>。</p></div>
      </div>
    </div>

    <div class="intent-reasoning-map" aria-label="完整用户案例的导流意图推理流程">
      <div class="intent-reasoning-map__stage">
        <span>01 · 输入</span><strong>多点位联合建模</strong>
        <small>6 张图片 + 账号资料 + 投稿 + 评论 + 行为记录</small>
      </div>
      <i aria-hidden="true">→</i>
      <div class="intent-reasoning-map__stage">
        <span>02 · 候选</span><strong>比较可能主题</strong>
        <div class="intent-reasoning-map__chips"><em>博彩主题</em><em>泛娱乐</em><em>其他</em></div>
      </div>
      <i aria-hidden="true">→</i>
      <div class="intent-reasoning-map__stage">
        <span>03 · 证据</span><strong>归属与支撑性核验</strong>
        <small>“纸波百拉”投稿 + “赌狗”昵称 + “纸波 / 薇心搜”个签</small>
      </div>
      <i aria-hidden="true">→</i>
      <div class="intent-reasoning-map__stage intent-reasoning-map__stage--result">
        <span>04 · 输出</span><strong>博彩主题</strong>
        <small>入口：个签 · 类型：微信公众号</small>
      </div>
    </div>

    <div class="intent-full-case__evidence">
      <div><span>强支撑 · 投稿内容</span><strong>“大哥正在纸波百拉中！！四毛仔看过来”</strong><p>“纸波百拉”具有明显博彩黑话特征，招徕语义直接指向赌博 / บาคาร่า类内容。</p></div>
      <div><span>辅助支撑 · 昵称</span><strong>“赌狗不值得可怜”</strong><p>“赌狗”是与赌博人群强相关的直接指称，提供账号所处博彩语境的辅助证据。</p></div>
      <div><span>强支撑 · 个签</span><strong>“想看大哥纸波怎么办？薇心搜（电子村）”</strong><p>“纸波”体现赌博直播类语义，“薇心搜”同时给出站外导流承接方式。</p></div>
    </div>
  </article>
</section>

<section class="paper-section paper-chapter" id="labeler">
  <p class="paper-section__eyebrow">02 · Semi-automatic Labeling Agent</p>
  <h2>核心一：把人工审核偏好沉淀为可执行的证据链规范</h2>
  <div class="intent-pain-grid">
    <article><span>Challenge 01</span><h3>信息量大、点位多</h3><p>单个账号可能同时包含资料、投稿、评论、关注/喜欢/收藏页、群聊、私信和行为序列；人工需要反复跨点位核验，审核速度慢。</p></article>
    <article><span>Challenge 02</span><h3>审核逻辑复杂</h3><p>主题判断涉及信息归属、导流入口、主题语义和跨点位证据关系，大量隐性经验难以用固定规则自动化标注。</p></article>
  </div>

  <p class="intent-pipeline-title">解决方案：从主题共识、证据抽取到人工偏好的持续沉淀</p>
  <div class="intent-annotation-pipeline" aria-label="半自动数据标注智能体流程">
    <article class="intent-annotation-pipeline__stage">
      <span>Stage 01</span>
      <div class="intent-annotation-pipeline__icon">3×</div>
      <h3>三模型投票</h3>
      <p>由三个独立模型判断最终导流主题，通过多数投票获得高置信主题标签，并将分歧样本送入后续复核。</p>
      <small>输出：最终主题</small>
    </article>
    <i aria-hidden="true">→</i>
    <article class="intent-annotation-pipeline__stage">
      <span>Stage 02</span>
      <div class="intent-annotation-pipeline__icon">G</div>
      <h3>Gemini 证据抽取</h3>
      <p>在最终主题约束下扫描全量点位，抽取证据来源、原始内容、支撑说明与证据强度，形成可回溯标签。</p>
      <small>输出：结构化证据链</small>
    </article>
    <i aria-hidden="true">→</i>
    <article class="intent-annotation-pipeline__stage intent-annotation-pipeline__stage--evolving">
      <span>Stage 03 · Validated</span>
      <div class="intent-annotation-pipeline__icon">↻</div>
      <h3>自进化标注智能体</h3>
      <p>将人工复核中的主题边界、证据归属和排除逻辑持续写入标注技能，通过“审核反馈—规则更新—回放评估”沉淀人工偏好。</p>
      <small>输出：持续演进的审核规范</small>
    </article>
  </div>

  <div class="paper-stats intent-labeling-stats" aria-label="半自动标注智能体评估结果">
    <div class="paper-stat"><strong>7,500</strong><span>前两阶段处理 Case</span></div>
    <div class="paper-stat"><strong>7,080</strong><span>最终主题正确</span></div>
    <div class="paper-stat"><strong>94.4%</strong><span>前两阶段主题正确率</span></div>
    <div class="paper-stat"><strong>97.5%</strong><span>融合第三阶段后准确率</span></div>
  </div>

  <div class="intent-schema" aria-label="导流意图识别结构化标签">
    <div><span>Final Theme</span><strong>最终意图主题</strong><small>12 类业务与风险主题</small></div>
    <div><span>Diversion</span><strong>导流入口与类型</strong><small>联系方式 · 二维码 · 网址 · 诱导话术</small></div>
    <div><span>Evidence</span><strong>证据链列表</strong><small>来源 · 内容 · 说明 · 支撑强度</small></div>
    <div><span>Rationale</span><strong>主题判断依据</strong><small>候选比较与排除理由</small></div>
  </div>

  <div class="paper-note">
    <strong>半自动而非全自动：</strong>模型负责高置信主题投票与证据链生成，人工审核聚焦模型分歧、复杂边界和错误案例；审核反馈再反向驱动标注技能更新，在效率与标签可靠性之间取得平衡。
  </div>
</section>

<section class="paper-section paper-chapter" id="training">
  <p class="paper-section__eyebrow">03 · Post-training</p>
  <h2>核心二：先蒸馏审核规则，再用强化学习优化决策</h2>
  <p class="paper-section__intro">单个 Case 的审核并非普通分类：模型需要同时遵守点位归属、跨点位承接、高风险特殊规则、12 类主题边界和严格输出 Schema。本项目先把超长人工规则蒸馏为高质量教师响应，让 Qwen 在短 System Prompt 下学习审核逻辑；再通过 GRPO 优化最终主题与证据推理。</p>

  <div class="intent-distillation" aria-label="长审核规则的教师数据蒸馏流程">
    <div class="intent-distillation__problem">
      <div>
        <span>WHY DISTILLATION</span>
        <h3>审核规则太长，不能直接作为 Qwen 的常驻训练提示词</h3>
        <p>完整规则需要覆盖“信息属于谁、点位如何跳转、哪些证据能够支撑主题、哪些候选必须排除”等大量人工审核逻辑。在当前训练配置下，直接把整份规则塞进每条训练样本，会显著挤占 Case 与推理响应的上下文空间，并使小模型同时承担“理解规则”和“解决任务”两层负担，训练更难稳定。</p>
      </div>
      <div class="intent-distillation__stats">
        <div><strong>2,057</strong><span>长规则行数</span></div>
        <div><strong>28,480</strong><span>规则字符数</span></div>
        <div><strong>74.8 KB</strong><span>文本体积</span></div>
      </div>
    </div>

    <div class="intent-distillation__flow">
      <article>
        <span>01 · Rule Induction</span>
        <strong>超长审核规则 + 原始 Case</strong>
        <small>完整描述归属判断、证据链、主题边界与输出约束</small>
      </article>
      <i aria-hidden="true">→</i>
      <article class="is-teacher">
        <span>02 · Teacher</span>
        <strong>Gemini 生成合规审核响应</strong>
        <small>让强教师模型先执行全部规则，产出推理过程与结构化答案</small>
      </article>
      <i aria-hidden="true">→</i>
      <article>
        <span>03 · Distilled Data</span>
        <strong>筛选高质量教师响应</strong>
        <small>过滤主题、Schema 与空推理错误，保留可回溯的优质样本</small>
      </article>
      <i aria-hidden="true">→</i>
      <article class="is-student">
        <span>04 · Student</span>
        <strong>短 Prompt 训练 Qwen</strong>
        <small>以教师响应作为监督目标，把复杂审核逻辑压缩进模型行为</small>
      </article>
    </div>

    <div class="intent-distillation__equation" role="math" aria-label="教师数据蒸馏过程">
      <span>y<sup>teacher</sup> = Gemini(P<sub>long</sub>, x)</span>
      <i>→</i>
      <span>Qwen(P<sub>short</sub>, x) ≈ y<sup>teacher</sup></span>
    </div>

    <div class="intent-distillation__footer">
      <p><strong>这里是教师数据蒸馏，不是 RSFT。</strong>训练阶段使用 Gemini 生成的审核响应作为监督目标；页面后续统一使用“教师数据蒸馏”表述。</p>
      <a href="{{ '/projects/diversion-intent-recognition/teacher-distillation/' | relative_url }}" target="_self">查看 2,057 行审核规则与蒸馏样例 →</a>
    </div>
  </div>

  <div class="intent-training-roadmap">
    <article>
      <span>Stage 01 · Teacher Distillation</span>
      <h3>长规则生成，短提示词学习</h3>
      <p>利用超长规则诱导 Gemini 生成符合审核逻辑的响应，再以短 System Prompt 训练 Qwen，使模型学习主题边界、证据归属与结构化输出。</p>
      <div class="intent-training-roadmap__flow"><b>长规则</b><i>→</i><b>教师响应</b><i>→</i><b>短 Prompt</b></div>
    </article>
    <article class="is-current">
      <span>Stage 02 · Implemented</span>
      <h3>标准 GRPO：约束最终输出</h3>
      <p>围绕最终主题正确性、输出格式合法性和推理长度设计奖励，先建立稳定、可部署的强化学习基线。</p>
      <div class="intent-training-roadmap__flow"><b>主题奖励</b><i>+</i><b>格式奖励</b><i>+</i><b>长度奖励</b></div>
    </article>
    <article class="is-next">
      <span>Stage 03 · Next</span>
      <h3>证据感知 GRPO：监督推理过程</h3>
      <p>进一步奖励证据可回溯性、单条支撑性、证据集合充分性与候选主题比较，使模型不仅“答对”，还能够“依据正确的证据答对”。</p>
      <div class="intent-training-roadmap__flow"><b>中间证据</b><i>+</i><b>候选主题</b><i>+</i><b>最终主题</b></div>
    </article>
  </div>

  <div class="paper-formula intent-reward-formula">
    <p class="paper-formula__label">当前：标准 GRPO 奖励</p>
    <div class="paper-formula__equation" role="math" aria-label="当前强化学习奖励由主题、格式和长度奖励组成">
      <span>R<sub>current</sub> = R<sub>theme</sub> + 0.3R<sub>format</sub> + 0.2R<sub>length</sub></span>
    </div>
    <p class="paper-formula__note">主题奖励保证最终分类正确；格式奖励约束 JSON Schema 与字段完整性；长度奖励减少过短、缺少分析的推理结果。</p>
  </div>

  <div class="paper-formula intent-reward-formula intent-reward-formula--next">
    <p class="paper-formula__label">规划：证据与候选主题过程奖励</p>
    <div class="paper-formula__equation" role="math" aria-label="规划中的奖励在当前奖励上加入证据和候选主题奖励">
      <span>R<sub>next</sub> = R<sub>current</sub> + λ<sub>e</sub>R<sub>evidence</sub> + λ<sub>c</sub>R<sub>candidate</sub></span>
    </div>
    <div class="intent-reward-components">
      <div><strong>Traceability</strong><span>引用的内容能否在原始输入中定位</span></div>
      <div><strong>Item-support</strong><span>每条证据是否真正支持最终主题</span></div>
      <div><strong>Set-sufficiency</strong><span>证据集合能否充分推出最终结论</span></div>
      <div><strong>Candidate Theme</strong><span>是否召回并正确比较关键候选主题</span></div>
    </div>
    <p class="paper-formula__note"><strong>目标：</strong>降低“答案碰巧正确但证据错误”、忽略关键候选主题和跨点位错误拼接等问题。该部分为下一阶段训练方案，尚不作为现有结果主张。</p>
  </div>
</section>

<section class="paper-section paper-chapter" id="results">
  <p class="paper-section__eyebrow">04 · Current Results</p>
  <h2>阶段结果：自研模型超过 Gemini 闭源基线</h2>
  <p class="paper-section__intro">在当前导流意图识别基准上，经过数据蒸馏与标准 GRPO 训练的模型取得 88.62% 的最终主题识别准确率。后续将重点验证过程奖励能否继续提高困难边界、证据质量与高风险主题表现。</p>
  <div class="paper-stats" aria-label="项目阶段结果">
    <div class="paper-stat"><strong>12 类</strong><span>细粒度导流主题</span></div>
    <div class="paper-stat"><strong>2,152</strong><span>当前测试基准样本</span></div>
    <div class="paper-stat"><strong>88.62%</strong><span>GRPO 模型准确率</span></div>
    <div class="paper-stat"><strong>≈ +3pp</strong><span>相对 Gemini 提升</span></div>
  </div>

  <div class="paper-grid intent-result-grid">
    <article class="paper-card">
      <span class="paper-card__index">D</span>
      <h3>数据能力</h3>
      <p>把分散的人工判断经验转化为统一、可执行、可版本化的多点位证据链标注规范。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">M</span>
      <h3>模型能力</h3>
      <p>通过蒸馏与强化学习，让开源多模态模型学习复杂业务边界、结构化输出和可解释推理。</p>
    </article>
    <article class="paper-card">
      <span class="paper-card__index">R</span>
      <h3>研究增量</h3>
      <p>从只监督最终主题扩展到监督证据与候选主题，为进一步提升可靠性建立训练路线。</p>
    </article>
  </div>
</section>

<section class="paper-section" id="public-note">
  <p class="paper-section__eyebrow">Public Note</p>
  <h2>公开说明</h2>
  <p class="paper-section__intro">任务案例取自测试集样本 7644583021509051449：仅将账号名显示为“xxx”，其余文字、图片与测试集标签按原始数据呈现。页面不提供原始数据下载或训练服务；88.62% 为当前版本测试基准上的阶段结果，证据与候选主题奖励属于后续训练规划。公开发布前仍需再次确认该样本的展示授权与合规边界。</p>
</section>
