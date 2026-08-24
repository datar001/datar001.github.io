---
permalink: /
title: ""
excerpt: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

{% if site.google_scholar_stats_use_cdn %}
{% assign gsDataBaseUrl = "https://cdn.jsdelivr.net/gh/" | append: site.repository | append: "@" %}
{% else %}
{% assign gsDataBaseUrl = "https://raw.githubusercontent.com/" | append: site.repository | append: "/" %}
{% endif %}
{% assign url = gsDataBaseUrl | append: "google-scholar-stats/gs_data_shieldsio.json" %}

<span class='anchor' id='about-me'></span>

I am a third-year Ph.D. student at the School of New Media and Communication, Tianjin University (2023–present), supervised by [Dr.Lanjun Wang (Research Scientist)](https://wanglanjun-academic.github.io/) and [Prof.An-an Liu](https://seea.tju.edu.cn/info/1014/1508.htm).

Previously, I received my M.S. degree from the School of Electrical and Information Engineering, Tianjin University (2020–2023), under the supervision of [Prof.An-an Liu](https://seea.tju.edu.cn/info/1014/1508.htm) and [Dr.Wenhui Li](https://seea.tju.edu.cn/info/1014/1462.htm). I earned my B.S. degree from the School of Information and Communication Engineering, Hainan University (2016-2020).

**Research Interests**

In the past, I mainly focused on investigating safety vulnerabilities in image generation models through practical red-teaming methods. I am now studying defense strategies that adjust the model’s internal behavior to better align generated outputs with our expectations. In addition, I am interested in controllable mechanisms for image and video generation.

If you are interested in my work or have any questions, please feel free to contact me at **zcy@tju.edu.cn**.


<span class='anchor' id='publications'></span>

# 📝 Publications 

<!-- * Corresponding Author, #Equal Contribution -->



<!-- 2026-AHV-DS -->

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">ACM CCS 2026 · CCF-A</div><img src='images/pub/AHV-DS-figure1.png' alt="Figure 1: AHV-D&amp;S compared with text and latent representation correction" width="100%"></div></div>
<div class='paper-box-text' markdown="1">




<a href="{{ '/projects/ahv-ds/' | relative_url }}" target="_self">What Concepts Lie Within? Detecting and Suppressing Risky Content in Diffusion Transformers</a>

**Chenyu Zhang**, Lanjun Wang, Yueyang Cheng, Ruidong Chen, Wenhui Li, An-An Liu

*ACM CCS, 2026, CCF-A*
</div>
</div>



<!-- 2026-T2I-RiskyPrompt -->

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">AAAI 2026 · CCF-A</div><img src='images/pub/T2I-RiskyPrompt.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">




<a href="{{ '/projects/t2i-riskyprompt/' | relative_url }}" target="_self">T2I-RiskyPrompt: A Benchmark for Safety Evaluation, Attack, and Defense on Text-to-Image Model</a>

**Chenyu Zhang**, Tairen Zhang, Lanjun Wang, Ruidong Chen, Wenhui Li, Anan Liu

*AAAI, 2026, CCF-A*
</div>
</div>



<!-- 2026-R2A -->

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">AAAI 2026 · CCF-A</div><img src='images/pub/R2A.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">




<a href="{{ '/projects/reason2attack/' | relative_url }}" target="_self">Reason2Attack: Jailbreaking Text-to-Image Models via LLM Reasoning</a>

**Chenyu Zhang**, Lanjun Wang, Yiwen Ma, Wenhui Li, Guoqing Jin, An-An Liu

*AAAI, 2026, CCF-A*
</div>
</div>



<!-- 2026-MJA -->

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">IJCV · Under Review · CCF-A</div><img src='images/pub/MJA.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">




<a href="{{ '/projects/metaphor-jailbreak/' | relative_url }}" target="_self">Metaphor-based Jailbreak Attacks on Text-to-Image Models</a>

**Chenyu Zhang**, Lanjun Wang, Yiwen Ma, Wenhui Li, Yi Tu, An-An Liu

*IJCV, under review, CCF-A*
</div>
</div>



<!-- 2025-AD-on-T2I -->

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">Information Fusion · IF 15.9</div><img src='images/pub/AD-on-T2I.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">




<a href="{{ '/projects/adversarial-attacks-defenses-survey/' | relative_url }}" target="_self">Adversarial Attacks and Defenses on Text-to-Image Diffusion Models: A Survey</a>

**Chenyu Zhang**, Mingwang Hu, Wenhui Li, Lanjun Wang

*Information Fusion (JCR-1, IF=15.9), 2025*
</div>
</div>



<!-- 2025-TRCE -->

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">ICCV 2025 · CCF-A</div><img src='images/pub/TRCE.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">




<a href="{{ '/projects/trce/' | relative_url }}" target="_self">TRCE: Towards Reliable Malicious Concept Erasure in Text-to-Image Diffusion Models</a>

Ruidong Chen, Honglin Guo, Lanjun Wang, **Chenyu Zhang**, Weizhi Nie, An-An Liu

*ICCV, 2025, CCF-A*
</div>
</div>




<!-- 2025-Reveal -->

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">Preprint 2024</div><img src='images/pub/Reveal.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">




[Revealing Vulnerabilities in Stable Diffusion via Targeted Attacks]({{ '/projects/targeted-attacks-stable-diffusion/' | relative_url }})

**Chenyu Zhang**, Lanjun Wang, Anan Liu

*Preprint, 2024*
</div>
</div>



<!-- 2024-Food -->

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">IEEE TMM · CCF-A · 2025</div><img src='images/pub/Food.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">




[Toward Chinese Food Understanding: A Cross-Modal Ingredient-Level Benchmark]({{ '/projects/cmingre/' | relative_url }})

Lanjun Wang, **Chenyu Zhang (First Student Author)**, An-An Liu, Bo Yang, Mingwang Hu, Xinran Qiao, Lei Wang, Jianlin He, Qiang Liu

*IEEE Transactions on Multimedia, CCF-A, 2025*
</div>
</div>


<span class='anchor' id='projects'></span>

# 💼 Projects

<div class="resume-project-list">
  <article class="resume-project-card">
    <div class="resume-project-card__meta">
      <span>华为小艺终端部门</span>
      <span>图像生成模型内容安全</span>
      <time>06/2025 - 12/2025</time>
    </div>
    <h2><a href="{{ '/projects/huawei-adversarial-data-generation/' | relative_url }}" target="_self">风险图像检测困难数据构建</a></h2>
    <p>针对边缘色情、畸形色情、涉政标语、涉政标识、涉政人物等风险类别，构建风险图像自动化生成框架，为华为提供超 10W 张风险图像，促使华为风险图像识别准确性从 93% 提升到 96%。</p>
    <a class="resume-project-card__link" href="{{ '/projects/huawei-adversarial-data-generation/' | relative_url }}" target="_self">查看项目详情 <span aria-hidden="true">→</span></a>
  </article>
  <article class="resume-project-card">
    <div class="resume-project-card__meta">
      <span>华为小艺终端部门</span>
      <span>图像生成模型内容安全</span>
      <time>06/2025 - 12/2025</time>
    </div>
    <h2><a href="{{ '/projects/huawei-model-safety-alignment/' | relative_url }}" target="_self">图像生成模型安全提升</a></h2>
    <p>针对华为小艺擦除衣物有概率生成色情内容问题，设计模型安全微调方案，通过设计对比损失和正则项，引导模型输出分布逼近安全内容同时远离色情内容，该方案在色情内容抑制成功率上提升近 50%。</p>
    <a class="resume-project-card__link" href="{{ '/projects/huawei-model-safety-alignment/' | relative_url }}" target="_self">查看项目详情 <span aria-hidden="true">→</span></a>
  </article>
  <article class="resume-project-card">
    <div class="resume-project-card__meta">
      <span>字节跳动-Data-风控-抖音风控（筋斗云人才计划）</span>
      <span>风险用户理解与后训练</span>
      <time>进行中</time>
    </div>
    <h2><a href="{{ '/projects/diversion-intent-recognition/' | relative_url }}" target="_self">风险用户导流意图识别</a></h2>
    <p>针对抖音风险账号存在导流点位但承接意图不明确的问题，构建半自动证据链标注智能体，并通过蒸馏与强化学习训练 Qwen3.6-35B-A3B；当前主题识别准确率达到 88.62%，高于 Gemini 闭源模型约 3 个百分点。</p>
    <a class="resume-project-card__link" href="{{ '/projects/diversion-intent-recognition/' | relative_url }}" target="_self">查看项目详情 <span aria-hidden="true">→</span></a>
  </article>
</div>



<!-- - [Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet](https://github.com), A, B, C, **CVPR 2020** -->

<!-- # 🎖 Honors and Awards
- *2021.10* Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet. 
- *2021.09* Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet.  -->

<!-- # 📖 Educations
- *2019.06 - 2022.04 (now)*, Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet. 
- *2015.09 - 2019.06*, Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet.  -->

<!-- # 💬 Invited Talks
- *2021.06*, Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet. 
- *2021.03*, Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet.  \| [\[video\]](https://github.com/) -->

<!-- # 💻 Internships
- *2019.05 - 2020.02*, [Lorem](https://github.com/), China. -->
