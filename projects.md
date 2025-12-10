---
layout: page
title: Research
---

<style>
    .project-card {
        display: flex;
        align-items: center;
        background-color: #f9f9f9;
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        margin-bottom: 20px;
    }
    .project-card img {
        width: 200px; /* 固定图片宽度 */
        height: 200px; /* 固定图片高度 */
        object-fit: contain; 
        border-radius: 5px;
        margin-right: 20px;
    }
    .project-card h2 {
        margin: 0 0 10px;
        font-size: 24px;
    }
    .project-card p {
        margin: 0;
        font-size: 14px;
        color: #555;
    }
    .project-card a {
        display: inline-block;
        margin-top: 15px;
        font-size: 14px;
        color: #007bff;
        text-decoration: none;
    }
    @media (max-width: 768px) {
        .project-card {
            flex-direction: column;
            text-align: center;
        }
        .project-card img {
            margin-right: 0;
            margin-bottom: 15px;
        }
    }
</style>

<!-- 项目 list -->

<div class="project-card">
    <img src="/assets/img/DynamicDefense-3-1.png">
    <div>
        <h2>Dynamic Defense to Adversarial RFMLS</h2>
        <p>Adversarial attack is a critical threat in Radio Frequency Machine Learning Systems (RFMLS). However, current state-of-the-art static defense sacrifice the performance on benign input, which cannot meet the QoS requirement of the communication system. In contrast, we propose a novel dynamic defense that can effectively improve the system's robustness without lossing performance on benign input.</p>
        <a href="/assets/pdf/HyperAdv_Dynamic_Defense_Against_Adversarial_Radio_Frequency_Machine_Learning_Systems.pdf" target="_blank">Learn More →</a>
    </div>
</div>

<div class="project-card">
    <img src="/assets/img/distributed_dnn-3-1.png">
    <div>
        <h2>Resilience of Entropy Model in Split Computing</h2>
        <p>Neural data compression has been applied to split computing to reduce the communication overhead. However, the robustness of this learning-driven compression remains unexplored. In this work, we systematically investigated how noise and interference in the input can increase the size of compressed data, hence introduces latency to split computing.</p>
        <a href="https://arxiv.org/pdf/2403.00942" target="_blank">Learn More →</a>
    </div>
</div>

<div class="project-card">
    <img src="/assets/img/Distributed-AdvML-1.png">
    <div>
        <h2>AdvML in Split Computing</h2>
        <p> Split computing can accelerate DNN applications by distributing the large model across mobile and edge devices. However, this can also expose the intermediate output to attackers. We provide a comprehensive assessment of the robustness in the intermediate output space with theory and experiment.</p>
        <a href="https://arxiv.org/pdf/2309.17401" target="_blank">Learn More →</a>
    </div>
</div>

<div class="project-card">
    <img src="/assets/img/system_framework_rings_v3-1.png">
    <div>
        <h2>Cross-Layer Spectrum Sensing</h2>
        <p>Spectrum sensing has long been investigated in both the analog and digital domains. While incorporating technologies across these domains can boost its development, it requires a comprehensive knowledge spanning from the RF frontend to digital signal processing. We present the first effort to combine an on-chip analog spectrum sensor with learning-driven spectrum segmentation to achieve rapid and accurate sensing in the Sub-6 GHz bandwidth.</p>
        <a href="https://par.nsf.gov/servlets/purl/10502127" target="_blank">Learn More →</a>
    </div>
</div>

<div class="project-card">
    <img src="/assets/img/setup_v2-1.png">
    <div>
        <h2>Wideband Semantic Spectrum Segmentation</h2>
        <p>Existing learning-driven spectrum sensing has two major challenges: 1) the bulky neural network requires a large computation capacity which is difficult to scale up to larger bandwidth; 2) the algorithm heavily relies on the training data which can barely generalize to complex spectrum conditions. We present a novel framework, a.k.a "semantic spectrum segmentation" to tackle these challenges. </p>
        <a href="https://arxiv.org/pdf/2402.03465" target="_blank">Learn More →</a>
    </div>
</div>
