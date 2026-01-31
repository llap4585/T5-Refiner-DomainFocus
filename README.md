# T5-Refiner-DomainFocus
# T5-预处理特定词语遮照增强

![Views](https://komarev.com/ghpvc/?username=llap4585&repo=T5-Refiner-DomainFocus&label=Project%20Views&color=blue&style=flat-square)

---
[⭐️English](#english) | [⭐️中文](#chinese)


[日本語](#japanese) | [Deutsch](#deutsch) | [Français](#francais) | [Español](#espanol) | [हिन्दी](#hindi) | [한국어](#korean) | [Português](#portuguese)

---

[Demo](#Demo) 

[Requirements](#Requirements)

[References](#References)

[Privacy](#Privacy)

---

<a name="chinese"></a>
## ⭐️中文
**T5-Refiner-DomainFocus** 旨在通过**预训练阶段**的策略优化，赋予模型一种内在的“语义韧性”，使其能更稳健地处理文本缺损和注入领域专业知识。

## 📖项目背景
在处理**医学档案数字化时**，**OCR（光学字符识别）** 常因纸质受损、印章遮挡等原因，导致核心术语出现“字符缺损”。
传统的 **T5** 或 **mT5** 模型(统称T5）在处理这些受损文本时存在两个主要问题：
* 随机遮蔽的局限性：导致模型只学会了根据词根“猜词”，而没有真正理解完整的医学概念。
* 分词错位问题：当术语丢失字母时，分词器会将其切碎为无意义的碎片，导致模型失去语义重心。

## ✅当前核心功能
本项目目前不依赖复杂的硬编码规则，而是通过优化数据预处理流程来增强模型能力：

* 专家词库引导的原子化遮蔽：
依托自定义词库，强制模型将专业术语（如：急性前壁心肌梗死）视为不可分割的整体进行遮蔽。通过这种方式，迫使模型从上下文的逻辑中寻找答案，而非通过残余字符投机取巧。

* 人工设定强化训练：
支持手动提高特定高难度术语的遮蔽概率（💡推荐在50%-70%，不宜超过80%），同时可同步提高整体的遮蔽率（20%-25%）。

*自动规避标点符号
防止引入干扰。

通过人为制造“极端信息缺失”的场景，强制模型在最差的输入情况下依然能保持对专业语义的准确还原。

## ❗️训练注意事项
* 防止模型提前停止：在预处理之后，T5 模型可能会出现 Loss 下降缓慢或产生局部波动的假象，导致系统错误地提前停止训练。
* 收敛判断建议：推荐增加训练时长，并根据多个阶段的 Loss 是否持续稳定下降来综合判断模型收敛情况。若训练时间不足，还原效果可能会大打折扣。

## 📊效果评估
根据初步测试对比，在 mT5-base 标准模型中：
* 标准模型表现：在专业领域的词汇还原率估算在 60% 以下，剩余 40% 的还原结果逻辑混乱，几乎无法被业务接受。
* 本项目改进后：专业词汇还原率估算达到了 85%。剩下的 15% 误差中，大部分是语义相近的词汇替代，极大地提高了文本的整体可读性和逻辑连贯性。

## ⚠️使用限制
* 上下文片段化限制：由于模型单次处理的文本长度有限，且每段文本内标记（Mask）的词汇数量受限，长文档在切分处理时可能存在上下文信息断裂的情况，导致部分跨段落的语义无法被完美捕捉。推荐回传部分上下文再训练。
* 算法局限性：由于 T5 模型本身的还原是基于统计概率算法的，因此在处理复杂的文本时，不可能保证 100% 的还原准确率。
* 领域依赖：还原效果高度依赖于预设专家词库的覆盖面与深度。

## 🌌未来开发计划
* 自动缺损感知:
利用分词器的“异常碎片”作为隐性信号。当 OCR 识别出现严重错位时，模型能通过分词序列的异常波动，自动定位到语义断裂处。
* 语义自动对齐:
无需人工指定衔接点，实现模型对 OCR 损坏文本的端到端修复。

[Demo](#Demo) 

---
<a name="Demo"></a>
## 📡 Demo

---
<a name="Requirements"></a>
## 🛠️ Requirements

```text


```
---
<a name="References"></a>
## 💪References / Citation
```markdown
This project builds upon the T5 or mT5. If you use mT5, please cite:

@inproceedings{xue-etal-2021-mt5,
    title = "m{T}5: A Massively Multilingual Pre-trained Text-to-Text Transformer",
    author = "Xue, Linting  and
      Constant, Noah  and
      Roberts, Adam  and
      Kale, Mihir  and
      Al-Rfou, Rami  and
      Siddhant, Aditya  and
      Barua, Aditya  and
      Raffel, Colin",
    booktitle = "Proceedings of the 2021 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies",
    month = jun,
    year = "2021",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2021.naacl-main.41",
    doi = "10.18653/v1/2021.naacl-main.41",
    pages = "483--498"
}

If you use this project, please cite it as:

@misc{llap4585,
    title={{T5-Refiner-DomainFocus}: Injecting domain expertise into T5 via precision vocabulary-guided masking.},
    author={llap4585},
    howpublished = {\url{https://github.com/llap4585/T5-Refiner-DomainFocus}},
    year={2026}
}

```
---

<a name="Privacy"></a>
## 🛡️ Privacy & Security

**Local Processing Only:** This tool performs all operations locally on your machine. No medical reports, patient data, or sensitive information are uploaded to any external servers or cloud services. Your data remains under your control at all times.

**Third-party Disclaimer:** All third-party libraries required for operation are provided by the user's environment. These dependencies and their components are not under the management or control of this project.

**仅限本地处理：** 本工具的所有操作均在您的本地计算机上执行。不会将任何医疗报告、患者数据或敏感信息上传到任何外部服务器或云服务。您的数据始终由您掌控。

**第三方库声明：** 本工具运行所依赖的所有第三方库均由用户环境提供，这些第三方库及其相关组件不在本项目的管理与控制范围内。


---
> **⚠️Disclaimer:** The non-English and non-Chinese versions of this documentation are provided for convenience only and were generated using machine translation. In case of any discrepancy, the Chinese version shall prevail.
