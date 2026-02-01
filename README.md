# T5-Refiner-DomainFocus
# T5-预处理特定词语遮照增强

![Views](https://komarev.com/ghpvc/?username=llap4585&repo=T5-Refiner-DomainFocus&label=Project%20Views&color=blue&style=flat-square)

---
<a name="Introduction"></a>
## Introduction
[⭐️English](#english) | [⭐️中文](#chinese)


[日本語](#japanese) | [Deutsch](#deutsch) | [Français](#francais) | [Español](#espanol) | [हिन्दी](#hindi) | [한국어](#korean) | [Português](#portuguese)

---

[Demo](#Demo) 

[Requirements](#Requirements)

[References](#References)

[Privacy](#Privacy)

---
<a name="english"></a>
## ⭐️ English

**T5-Refiner-DomainFocus** aims to empower models with intrinsic "semantic resilience" through **pre-training stage** strategy optimization, **enabling more robust handling of text corruption and the injection of domain-specific expertise.**

### 📖 Project Background
During **the digitization of medical records**, **OCR (Optical Character Recognition)** often suffers from "character defects" in core terminology due to damaged paper, stamp occlusion, or other physical factors.
Traditional **T5** or **mT5** models (collectively referred to as T5) face two major challenges when processing such corrupted text:
* **Limitations of Random Masking**: The model learns to "guess" words based on sub-word roots rather than truly understanding complete medical concepts.
* **Tokenization Misalignment**: When letters are missing from a term, the tokenizer breaks it into meaningless fragments, causing the model to lose its semantic focus.

### ✅ Core Features
This project does not rely on complex hard-coded rules. Instead, it enhances model capabilities through optimized data preprocessing, masking strategies, and execution workflows:

**Note:** This project uses the T5 tokenizer itself.  

* **Generating training chunks from long texts based on paragraphs and punctuation**  
  Automatically constructs samples from novels, documents, or domain-specific corpora, splitting text based on paragraph boundaries and both Chinese and English punctuation. The last sentence of the previous text block is retained as a context prefix to ensure semantic continuity, mitigate cross-block semantic breaks, and constrain sample length to fit the model's context window.

* **Atomic masking guided by an expert vocabulary**  
  Using a custom domain-specific vocabulary, professional terms (e.g., *Acute Anterior Wall Myocardial Infarction*, *Percutaneous Coronary Intervention*) are masked as indivisible units. This prevents character-level shortcuts and forces the model to rely on contextual semantics and domain logic for reconstruction.

* **Manually controllable enhanced masking strategy**  
  Supports increasing the masking probability for high-difficulty terms (**💡 recommended 50%–70%, not exceeding 80%**) while simultaneously adjusting the overall mask ratio (around 20%–25%), enabling targeted reinforcement of the model's reasoning ability on weaker knowledge points.

* **Automatic avoidance of punctuation and non-semantic tokens**  
  Masking automatically skips Chinese and English punctuation, symbols, and tokenizer unknown characters (`<unk>`), ensuring that every masked span corresponds to a meaningful semantic unit.

* **Automatic merging of consecutive or overlapping spans**  
  Adjacent or overlapping mask spans are automatically merged to maintain the correct order of `<extra_id_n>` input/output pairs, ensuring continuous training signals and full compatibility with T5's original span corruption training methodology.

* **Multithreaded parallel processing and automated sample aggregation**  
  Supports adjustable thread counts for parallel sample generation, significantly improving efficiency on large-scale corpora. All generated samples are automatically aggregated in their original order and output as a standardized JSONL dataset ready for direct training.

**By combining context-aware chunking, keyword-prioritized masking, random span corruption, and artificially created "extreme information loss" scenarios, the model is trained to maintain accurate understanding and reconstruction of professional semantics even under highly challenging input conditions.**

> Applicable scenarios: medical, legal, technical documentation, or any domain requiring precise semantic understanding.

### ❗️ Training Notes
* **Preventing Early Stopping**: After preprocessing, T5 models may exhibit slow loss reduction or local fluctuations, which can trick systems into stopping training prematurely.
* **Convergence Judgment**: It is recommended to extend training duration and evaluate convergence based on whether the loss decreases steadily across multiple stages. Insufficient training will significantly degrade restoration performance.

### 📊 Evaluation
Based on preliminary testing with the mT5-base standard model:
* **Standard Model Performance**: The restoration rate for specialized terminology is estimated to be below 60%. The remaining 40% of results are often logically incoherent and unacceptable for professional use.
* **With DomainFocus Improvement**: The estimated restoration rate reaches 85%. Of the remaining 15% error margin, most are semantic synonyms, which greatly improves the overall readability and logical consistency of the text.

### ⚠️ Limitations
* **Context Fragmentation**: Due to the limited sequence length and the restricted number of masks per segment, long documents may suffer from semantic disconnection during chunking. It is recommended to feed partial overlapping context back during re-training.
* **Algorithmic Limits**: Since T5 restoration is based on statistical probability, it is impossible to guarantee 100% accuracy when dealing with highly complex text.
* **Domain Dependency**: The restoration effectiveness is highly dependent on the coverage and depth of the predefined expert lexicon.

### 🌌 Future Roadmap
* Automatic Defect Sensing:
Utilizing "tokenization fragments" as implicit signals. When OCR recognition is severely misaligned, the model will automatically locate semantic ruptures via anomalies in the token sequence.
* Semantic Auto-Alignment:
Eliminating the need for manual anchor points to achieve end-to-end restoration of OCR-damaged text.

[Demo](#Demo)

---
<a name="chinese"></a>
## ⭐️中文
**T5-Refiner-DomainFocus** 旨在通过**预训练阶段**的策略优化，赋予模型一种内在的“语义韧性”，**使其能更稳健地处理文本缺损和注入领域专业知识。**

### 📖项目背景
在处理**医学档案数字化时**，**OCR（光学字符识别）** 常因纸质受损、印章遮挡等原因，导致核心术语出现“字符缺损”。
传统的 **T5** 或 **mT5** 模型(统称T5）在处理这些受损文本时存在两个主要问题：
* **随机遮蔽的局限性**：导致模型只学会了根据词根“猜词”，而没有真正理解完整的医学概念。
* **分词错位问题**：当术语丢失字母时，分词器会将其切碎为无意义的碎片，导致模型失去语义重心。

### ✅当前核心功能
本项目目前不依赖复杂的硬编码规则，而是通过优化数据预处理、掩码策略与执行流程来增强模型能力：

**注意:** 本项目使用的是 T5 自带的分词器。

* **长文本按段落与标点切分生成训练样本块**  
  自动从小说、文档或专业语料构造样本，基于段落边界和中英文标点切分，同时保留前文末尾句作为上下文前缀，保证样本语义连续，缓解跨段语义断裂，并对单样本长度进行约束以适配模型上下文窗口。

* **专家词库引导的原子化遮蔽**  
  利用自定义领域词库，将专业术语（如：急性前壁心肌梗死、经皮冠状动脉介入治疗）作为不可分割的整体进行遮蔽，阻断字符级投机路径，迫使模型依赖上下文语义与领域逻辑进行还原。

* **人工可控强化遮蔽策略**  
  支持对高难度术语手动提升遮蔽概率（**💡推荐 50%–70%，不宜超过 80%**），并可同步调整总体遮蔽率（约 20%–25%），实现定向强化模型在薄弱知识点上的推理能力。

* **自动规避标点符号与非语义 token**  
  遮蔽过程中自动跳过中英文标点、符号及 tokenizer 未知字符（`<unk>`），保证每个 mask span 都对应有效语义单元。

* **连续或重叠 span 自动合并**  
  对相邻或重叠的掩码 span 进行自动合并，确保生成的 `<extra_id_n>` 输入/输出对顺序统一、训练信号连续，完全兼容 T5 原论文的 span corruption 训练规范。

* **多线程并行处理与自动化样本回传**  
  支持可调线程数并行生成样本，大幅提升大规模语料处理效率；所有生成样本自动回传并按原文顺序汇总，最终输出为可直接用于训练的标准化 JSONL 数据集。

**通过上下文感知的数据构造、关键词优先遮蔽、随机 span 掩码及人为制造的“极端信息缺失”场景，模型在最不利输入条件下依然能够保持对专业语义的准确理解与还原能力。**  

> 适用场景：医学、法律、技术文档等需要精确语义理解的专业语料。

### ❗️训练注意事项
* **防止模型提前停止**：在预处理之后，T5 模型可能会出现 Loss 下降缓慢或产生局部波动的假象，导致系统错误地提前停止训练。
* **收敛判断建议**：推荐增加训练时长，并根据多个阶段的 Loss 是否持续稳定下降来综合判断模型收敛情况。若训练时间不足，还原效果可能会大打折扣。

### 📊效果评估
根据初步测试对比，在 mT5-base 标准模型中：
* **标准模型表现**：在专业领域的词汇还原率估算在 60% 以下，剩余 40% 的还原结果逻辑混乱，几乎无法被业务接受。
* **本项目改进后**：专业词汇还原率估算达到了 85%。剩下的 15% 误差中，大部分是语义相近的词汇替代，极大地提高了文本的整体可读性和逻辑连贯性。

### ⚠️使用限制
* **上下文片段化限制**：由于模型单次处理的文本长度有限，且每段文本内标记（Mask）的词汇数量受限，长文档在切分处理时可能存在上下文信息断裂的情况，导致部分跨段落的语义无法被完美捕捉。推荐回传部分上下文再训练。
* **算法局限性**：由于 T5 模型本身的还原是基于统计概率算法的，因此在处理复杂的文本时，不可能保证 100% 的还原准确率。
* **领域依赖**：还原效果高度依赖于预设专家词库的覆盖面与深度。

### 🌌未来开发计划
* 自动缺损感知:
利用分词器的“异常碎片”作为隐性信号。当 OCR 识别出现严重错位时，模型能通过分词序列的异常波动，自动定位到语义断裂处。
* 语义自动对齐:
无需人工指定衔接点，实现模型对 OCR 损坏文本的端到端修复。

[Demo](#Demo) 

---
<a name="japanese"></a>
## 日本語（機械翻訳）
**T5-Refiner-DomainFocus** は、**事前学習段階**の戦略的最適化を通じて、モデルに固有の「意味的弾力性（Semantic Resilience）」を付与することを目的としています。これにより、**テキストの欠損をより堅牢に処理し、ドメイン専門知識を注入することが可能になります。**

### 📖プロジェクト背景
**医療アーカイブのデジタル化**において、**OCR（光学文字認識）** は、紙の損傷や印影の重なりなどの原因により、核心的な用語に「文字欠損」が生じることがよくあります。
従来の **T5** または **mT5** モデル（総称してT5）は、これらの損傷したテキストを処理する際に2つの主要な問題を抱えています：
* **ランダムマスキングの限界**：モデルが語根に基づいた「単語の推測」を学習するにとどまり、完全な医療概念を真に理解できない。
* **トークナイズの不一致問題**：用語の文字が欠落すると、トークナイザーがそれを無意味な断片に細分化してしまい、モデルが意味の重心を失う。

### ✅現在のコア機能
本プロジェクトは、複雑なハードコーディングルールに依存せず、データ前処理、マスク戦略、および処理フローの最適化によりモデルの能力を強化します。

**注意:** このプロジェクトでは、T5 自身のトークナイザーを使用しています。

* **長文を段落および句読点で分割して学習サンプルを生成**  
  小説、文書、専門コーパスなどから自動的にサンプルを構築し、段落境界と日英句読点に基づいて分割します。さらに前のテキストブロックの末尾文をコンテキストとして保持することで、サンプル間の意味の連続性を確保し、段落を跨ぐ意味の断絶を緩和します。また、モデルのコンテキストウィンドウに合わせて単一サンプルの長さを制約します。

* **専門用語辞書に基づく原子化マスキング（Atomic Masking）**  
  カスタムの専門用語辞書を利用し、専門用語（例：急性前壁心筋梗塞、経皮的冠動脈インターベンション）を分割不可能な単位としてマスクします。これにより、文字単位での推測を防ぎ、モデルがコンテキストの意味や領域の論理に基づいて復元するよう促します。

* **人工制御による強化マスキング戦略**  
  高難度の専門用語についてマスク確率を手動で上げることが可能（**💡推奨 50%–70%、80%を超えないこと**）で、同時に全体のマスク率（約20%–25%）も調整できます。これにより、モデルが弱い知識領域での推論能力を強化します。

* **句読点および非意味トークンの自動回避**  
  マスキング時に日英の句読点、記号、および tokenizer 未知文字（`<unk>`）を自動でスキップし、すべてのマスク範囲が有効な意味単位に対応するよう保証します。

* **連続または重複するspanの自動統合**  
  隣接または重複するマスク範囲を自動で統合し、生成される `<extra_id_n>` 入力/出力ペアの順序を統一、学習信号の連続性を確保します。これにより、T5論文に準拠した span corruption 学習規格に完全対応します。

* **マルチスレッド並列処理とサンプルの自動回収**  
  スレッド数を調整可能な並列処理により、大規模コーパスのサンプル生成効率を大幅に向上させます。生成されたすべてのサンプルは自動で回収され、原文順に整理された後、学習に直接使用可能な標準JSONLデータセットとして出力されます。

**コンテキスト認識型のデータ構築、キーワード優先マスキング、ランダムspanマスク、および人工的に作られた「極端な情報欠損」シナリオにより、モデルは最も不利な入力条件下でも専門用語の正確な理解と復元能力を保持できます。**

> 適用例：医学、法律、技術文書など、正確な意味理解が求められる専門コーパス。

### ❗️訓練上の注意点
* **モデルの早期停止の防止**：前処理後、T5モデルは損失（Loss）の下落が緩やかになったり、局所的な変動が生じたりする「見かけ上の停滞」が発生し、システムが誤って訓練を早期終了させる可能性があります。
* **収束判断の推奨**：訓練時間を延長し、複数のフェーズで損失が継続的に安定して下落しているかに基づいて、モデルの収束を総合的に判断することを推奨します。訓練時間が不足すると、復元効果が大幅に低下する可能性があります。

### 📊効果評価
mT5-base標準モデルを用いた初期テストの比較：
* **標準モデルのパフォーマンス**：専門分野の語彙復元率は推定60%以下。残りの40%は論理が混乱しており、業務利用はほぼ不可能です。
* **本プロジェクトによる改善後**：専門語彙の復元率は推定85%に達しました。残りの15%の誤差の大部分は意味の近い語彙への置換であり、テキスト全体の可読性と論理的な一貫性が大幅に向上しました。

### ⚠️使用制限
* **コンテキストの断片化の制限**：モデルが一度に処理できるテキスト長には制限があり、また各テキストセグメント内でマスク（Mask）される語彙数も限られているため、長いドキュメントを分割処理する際に文脈情報が断絶し、セグメントを跨ぐ意味を完璧に捉えられない場合があります。一部のコンテキストを再度含めてトレーニングすることを推奨します。
* **アルゴリズムの限界**：T5モデル自体の復元は統計的確率アルゴリズムに基づいているため、複雑なテキストを処理する際に100%の復元精度を保証することは不可能です。
* **ドメイン依存性**：復元効果は、あらかじめ設定された専門用語集の網羅性と深さに強く依存します。

### 🌌今後の開発計画
* 自動欠損検知：
トークナイザーの「異常な断片」を隠れた信号として利用します。OCR認識に深刻なズレが生じた際、モデルがトークンシーケンスの異常な変動を通じて、意味の断絶箇所を自動的に特定できるようにします。
* 意味の自動アライメント：
手動で接続点を指定することなく、OCRで損傷したテキストをモデルがエンドツーエンドで修復できるようにします。

[Demo](#Demo)

---
<a name="deutsch"></a>
## Deutsch (Maschinelle Übersetzung)
**T5-Refiner-DomainFocus** zielt darauf ab, dem Modell durch strategische Optimierung in der **Pre-Training-Phase** eine intrinsische „semantische Resilienz“ zu verleihen, damit es **Textdefekte robuster verarbeiten und Fachwissen aus spezifischen Domänen injizieren kann.**

### 📖 Projekthintergrund
Bei der **Digitalisierung medizinischer Archive** führt **OCR (optische Zeichenerkennung)** aufgrund von beschädigtem Papier, Stempelüberdeckungen usw. häufig zu „Zeichendefekten“ bei zentralen Fachbegriffen.
Herkömmliche **T5-** oder **mT5-Modelle** (zusammenfassend T5) haben zwei Hauptprobleme bei der Verarbeitung dieser beschädigten Texte:
* **Grenzen der zufälligen Maskierung**: Dies führt dazu, dass das Modell nur lernt, Wörter basierend auf Wortstämmen zu „raten“, anstatt medizinische Konzepte wirklich vollständig zu verstehen.
* **Tokenisierungs-Fehlausrichtung**: Wenn Buchstaben in Fachbegriffen fehlen, zerlegt der Tokenizer diese in bedeutungslose Fragmente, wodurch das Modell seinen semantischen Fokus verliert.

### ✅ Aktuelle Kernfunktionen
Dieses Projekt ist derzeit nicht auf komplexe Hardcodierungsregeln angewiesen, sondern verbessert die Modellfähigkeiten durch optimierte Datenvorverarbeitung, Maskierungsstrategien und Ablaufsteuerung:

**Hinweis:** Dieses Projekt verwendet den T5-eigenen Tokenizer.

* **Erstellung von Trainingsdatenblöcken aus langen Texten anhand von Absätzen und Satzzeichen**  
  Automatische Generierung von Samples aus Romanen, Dokumenten oder Fachtexten, basierend auf Absatzgrenzen und chinesischen/englischen Satzzeichen. Gleichzeitig werden die letzten Sätze des vorherigen Blocks als Kontextpräfix beibehalten, um die semantische Kontinuität der Samples sicherzustellen, semantische Brüche zwischen Absätzen zu reduzieren und die Länge einzelner Samples an das Kontextfenster des Modells anzupassen.

* **Atomare Maskierung gesteuert durch Experten-Wortlisten**  
  Verwendung benutzerdefinierter Fachwortlisten, um Fachbegriffe (z. B. *akuter Vorderwandinfarkt*, *perkutane Koronarintervention*) als unteilbare Einheiten zu maskieren. Dies verhindert spekulative Charakter-basierte Vorhersagen und zwingt das Modell, die Maske anhand des Kontextes und der Fachlogik wiederherzustellen.

* **Manuell steuerbare verstärkte Maskierungsstrategie**  
  Ermöglicht die manuelle Erhöhung der Maskierungswahrscheinlichkeit für schwierige Begriffe (**💡 empfohlen 50%–70%, nicht über 80%**) und gleichzeitig die Anpassung der Gesamtmaskierungsrate (ca. 20%–25%), um gezielt die Fähigkeit des Modells zu stärken, in schwierigen Wissensbereichen semantische Schlussfolgerungen zu ziehen.

* **Automatische Vermeidung von Satzzeichen und nicht-semantischen Token**  
  Während der Maskierung werden automatisch Satzzeichen, Symbole und unbekannte Token des Tokenizers (`<unk>`) übersprungen, sodass jeder maskierte Bereich einem sinnvollen semantischen Einheit entspricht.

* **Automatische Zusammenführung von aufeinanderfolgenden oder überlappenden Spans**  
  Zusammenführung benachbarter oder überlappender Maskenspans, um sicherzustellen, dass die generierten `<extra_id_n>` Eingabe-/Ausgabe-Paare in der richtigen Reihenfolge und mit kontinuierlichem Trainingssignal vorliegen, vollständig kompatibel mit der Span-Corruption-Methode des T5-Originalpapiers.

* **Mehrthreading und automatisches Zurückschreiben der Samples**  
  Unterstützung einer konfigurierbaren Anzahl von Threads zur parallelen Generierung von Samples, wodurch die Verarbeitung großer Korpora erheblich beschleunigt wird. Alle erzeugten Samples werden automatisch zurückgeführt und in der ursprünglichen Reihenfolge zusammengeführt, schließlich als standardisiertes JSONL-Dataset ausgegeben, das direkt für das Training verwendet werden kann.

**Durch kontextbewusste Datenkonstruktion, prioritäre Maskierung von Schlüsselbegriffen, zufällige Span-Maskierung und absichtlich erzeugte „extreme Informationsverluste“-Szenarien bleibt das Modell selbst unter widrigsten Eingabebedingungen in der Lage, die Fachsemantik korrekt zu verstehen und wiederherzustellen.**

> Anwendungsbereiche: Medizinische, juristische, technische Dokumente und andere Fachtexte, die ein präzises semantisches Verständnis erfordern.

### ❗️ Hinweise zum Training
* **Vorzeitigen Stopp des Modells verhindern**: Nach dem Preprocessing kann es bei T5-Modellen zu einer Täuschung durch langsam sinkenden Loss oder lokale Schwankungen kommen, was dazu führt, dass das System das Training fälschlicherweise vorzeitig stoppt.
* **Empfehlung zur Konvergenzbeurteilung**: Es wird empfohlen, die Trainingsdauer zu erhöhen und die Konvergenz des Modells basierend auf dem kontinuierlichen und stabilen Sinken des Loss über mehrere Phasen hinweg umfassend zu beurteilen. Bei unzureichender Trainingszeit kann der Wiederherstellungseffekt stark beeinträchtigt werden.

### 📊 Effektivitätsbewertung
Basierend auf vorläufigen Vergleichstests im mT5-base Standardmodell:
* **Leistung des Standardmodells**: Die Wiederherstellungsrate von Fachvokabular wird auf unter 60% geschätzt, wobei die restlichen 40% logisch verwirrend und für den geschäftlichen Einsatz kaum akzeptabel sind.
* **Nach der Verbesserung durch dieses Projekt**: Die Wiederherstellungsrate von Fachvokabular erreichte geschätzte 85%. Von den verbleibenden 15% Fehlerquote entfällt der Großteil auf semantisch ähnliche Wortsubstitutionen, was die allgemeine Lesbarkeit und logische Kohärenz des Textes erheblich verbessert.

### ⚠️ Nutzungseinschränkungen
* **Einschränkung durch Kontext-Fragmentierung**: Da die Textlänge pro Verarbeitungsschritt begrenzt ist und die Anzahl der maskierten Wörter pro Textsegment limitiert ist, kann es bei der Aufteilung langer Dokumente zu Brüchen in den Kontextinformationen kommen. Dies führt dazu, dass einige segmentübergreifende Semantiken nicht perfekt erfasst werden können. Es wird empfohlen, Teile des Kontextes für das Re-Training zurückzugeben.
* **Algorithmische Grenzen**: Da die Wiederherstellung des T5-Modells auf statistischen Wahrscheinlichkeitsalgorithmen basiert, kann eine 100%ige Genauigkeit bei komplexen Texten nicht garantiert werden.
* **Domänenabhängigkeit**: Der Wiederherstellungseffekt hängt stark von der Abdeckung und Tiefe des vordefinierten Experten-Vokabulars ab.

### 🌌 Zukünftige Entwicklungspläne
* Automatische Defekterkennung:
Nutzung „anormaler Fragmente“ des Tokenizers als implizite Signale. Wenn OCR-Erkennungen schwerwiegende Fehlausrichtungen aufweisen, kann das Modell über abnormale Schwankungen in der Token-Sequenz semantische Brüche automatisch lokalisieren.
* Automatische semantische Ausrichtung:
End-to-End-Reparatur von OCR-beschädigten Texten durch das Modell, ohne dass manuell Verknüpfungspunkte angegeben werden müssen.

[Demo](#Demo)

---
<a name="francais"></a>
## Français (Traduction automatique)
**T5-Refiner-DomainFocus** vise à doter le modèle d'une « résilience sémantique » intrinsèque grâce à l'optimisation des stratégies lors de la **phase de pré-entraînement**, **lui permettant de gérer plus solidement les lacunes textuelles et d'injecter une expertise métier.**

### 📖 Contexte du projet
Lors de la **numérisation d'archives médicales**, l'**OCR (Reconnaissance Optique de Caractères)** entraîne souvent des « lacunes de caractères » dans les termes clés en raison de dommages sur le papier ou de l'obstruction par des tampons.
Les modèles **T5** ou **mT5** conventionnels (collectivement appelés T5) présentent deux problèmes majeurs lors du traitement de ces textes endommagés :
* **Limites du masquage aléatoire** : Le modèle apprend uniquement à « deviner » les mots à partir des racines, sans véritablement comprendre les concepts médicaux complets.
* **Problème de désalignement de la tokenisation** : Lorsqu'un terme perd des lettres, le tokenizer le fragmente en morceaux dénués de sens, faisant perdre au modèle son centre de gravité sémantique.

### ✅ Fonctions clés actuelles
Ce projet ne dépend pas de règles codées complexes, mais renforce les capacités du modèle grâce à l'optimisation du prétraitement des données, des stratégies de masquage et du flux d'exécution :

**Remarque:** Ce projet utilise le tokenizer intégré de T5.

* **Découpage des textes longs en échantillons d'entraînement selon les paragraphes et la ponctuation**  
  Génération automatique d'échantillons à partir de romans, documents ou corpus spécialisés, en se basant sur les limites des paragraphes et la ponctuation en chinois et en anglais. Les phrases finales du bloc précédent sont conservées comme préfixe contextuel pour assurer la continuité sémantique et réduire les ruptures entre les segments, tout en limitant la longueur de chaque échantillon pour s'adapter à la fenêtre de contexte du modèle.

* **Masquage atomique guidé par un lexique d'experts**  
  Utilisation d'un lexique de domaine personnalisé pour traiter les termes spécialisés (ex : infarctus aigu du myocarde de la paroi antérieure, intervention coronaire percutanée) comme des unités indivisibles lors du masquage. Cela bloque les chemins de prédiction basés sur les caractères et force le modèle à s'appuyer sur le contexte et la logique du domaine pour restaurer le contenu.

* **Stratégie de masquage renforcée et contrôlable manuellement**  
  Possibilité d'augmenter manuellement la probabilité de masquage pour des termes difficiles (**💡 recommandé 50%–70%, ne pas dépasser 80%**), tout en ajustant simultanément le taux de masquage global (environ 20%–25%), afin de renforcer de manière ciblée la capacité du modèle à raisonner sur des points de connaissance faibles.

* **Évitement automatique de la ponctuation et des tokens non sémantiques**  
  Le masquage ignore automatiquement la ponctuation chinoise et anglaise, les symboles ainsi que les caractères inconnus du tokenizer (`<unk>`), garantissant que chaque span masqué correspond à une unité sémantique valide.

* **Fusion automatique des spans consécutifs ou chevauchants**  
  Les spans de masquage adjacents ou chevauchants sont fusionnés automatiquement, assurant un ordre cohérent des entrées/sorties `<extra_id_n>` et un signal d'entraînement continu, entièrement compatible avec la norme de span corruption du papier original T5.

* **Traitement parallèle multithread et retour automatisé des échantillons**  
  Génération d'échantillons en parallèle avec un nombre de threads configurable, améliorant considérablement l'efficacité sur de grands corpus ; tous les échantillons générés sont automatiquement réintégrés et triés selon l'ordre original du texte, pour produire un dataset JSONL standard directement exploitable pour l'entraînement.

**Grâce à la construction de données consciente du contexte, au masquage prioritaire des mots-clés, au masquage aléatoire de spans et à la création de scénarios de "perte d'information extrême", le modèle reste capable de comprendre et de restaurer avec précision le sens des termes spécialisés même dans des conditions d'entrée les plus défavorables.**

> Cas d'utilisation : corpus spécialisés nécessitant une compréhension sémantique précise, tels que la médecine, le droit ou les documents techniques.

### ❗️ Précautions d'entraînement
* **Prévenir l'arrêt prématuré du modèle** : Après le prétraitement, le modèle T5 peut donner l'illusion d'une baisse lente de la perte (Loss) ou de fluctuations locales, ce qui peut amener le système à arrêter l'entraînement prématurément par erreur.
* **Conseils pour juger de la convergence** : Il est recommandé d'augmenter la durée d'entraînement et de juger de la convergence de manière globale en vérifiant si la perte continue de descendre de façon stable sur plusieurs étapes. Si le temps d'entraînement est insuffisant, l'effet de restauration pourrait être considérablement réduit.

### 📊 Évaluation des résultats
Selon les tests comparatifs préliminaires sur le modèle standard mT5-base :
* **Performance du modèle standard** : Le taux de restauration du vocabulaire spécialisé est estimé à moins de 60 %, les 40 % restants étant logiquement confus et pratiquement inacceptables pour une utilisation métier.
* **Après amélioration par ce projet** : Le taux de restauration du vocabulaire spécialisé atteint environ 85 %. Parmi les 15 % d'erreurs restantes, la plupart sont des substitutions par des termes sémantiquement proches, ce qui améliore considérablement la lisibilité globale et la cohérence logique du texte.

### ⚠️ Limites d'utilisation
* **Limitation de la fragmentation du contexte** : En raison de la longueur limitée du texte traité en une seule fois et du nombre restreint de mots masqués (Mask) par segment, le traitement de documents longs peut entraîner une rupture des informations contextuelles, empêchant la capture parfaite de la sémantique entre les paragraphes. Il est recommandé de réinjecter une partie du contexte pour le réentraînement.
* **Limites algorithmiques** : La restauration du modèle T5 étant basée sur des algorithmes de probabilité statistique, il est impossible de garantir une précision de restauration de 100 % lors du traitement de textes complexes.
* **Dépendance au domaine** : L'efficacité de la restauration dépend fortement de la couverture et de la profondeur du lexique d'experts prédéfini.

### 🌌 Plan de développement futur
* Perception automatique des lacunes :
Utiliser les « fragments anormaux » du tokenizer comme signaux implicites. En cas de décalage grave de l'OCR, le modèle pourra localiser automatiquement les ruptures sémantiques via les fluctuations anormales de la séquence de tokens.
* Alignement sémantique automatique :
Réaliser une réparation de bout en bout des textes endommagés par l'OCR sans avoir besoin de spécifier manuellement les points de jonction.

[Demo](#Demo)

---
<a name="espanol"></a>
## Español (Traducción Automática)
**T5-Refiner-DomainFocus** está diseñado para dotar al modelo de una "resiliencia semántica" intrínseca mediante la optimización estratégica de la **fase de preentrenamiento**, **permitiéndole manejar de manera más robusta la pérdida de texto e inyectar conocimientos especializados del dominio.**

### 📖 Antecedentes del Proyecto
Al procesar la **digitalización de archivos médicos**, el **OCR (Reconocimiento Óptico de Caracteres)** suele presentar "defectos de caracteres" en términos clave debido a daños en el papel, obstrucción por sellos, entre otros motivos.
Los modelos tradicionales **T5** o **mT5** (colectivamente T5) presentan dos problemas principales al manejar estos textos dañados:
* **Limitaciones del enmascaramiento aleatorio**: El modelo solo aprende a "adivinar palabras" basándose en raíces léxicas, sin comprender realmente los conceptos médicos completos.
* **Problemas de desalineación de la tokenización**: Cuando un término pierde letras, el tokenizador lo fragmenta en pedazos sin sentido, lo que hace que el modelo pierda el centro de gravedad semántico.

### ✅ Funciones Principales Actuales
Este proyecto actualmente no depende de reglas codificadas complejas, sino que mejora la capacidad del modelo a través de la optimización del preprocesamiento de datos, las estrategias de enmascaramiento y el flujo de ejecución:

**Nota:** Este proyecto utiliza el tokenizador propio de T5.

* **Generación de bloques de entrenamiento a partir de textos largos mediante segmentación por párrafos y puntuación**  
  Construye automáticamente muestras a partir de novelas, documentos o corpus especializados, segmentando según los límites de párrafos y la puntuación en chino e inglés. Además, conserva la última oración del bloque anterior como prefijo contextual, asegurando la continuidad semántica de las muestras, mitigando rupturas entre párrafos y ajustando la longitud de cada muestra para adaptarse a la ventana de contexto del modelo.

* **Enmascaramiento atómico guiado por un vocabulario de expertos**  
  Aprovecha un vocabulario de dominio personalizado para tratar términos especializados (por ejemplo: infarto agudo de pared anterior, intervención coronaria percutánea) como unidades indivisibles al enmascarar, bloqueando rutas de predicción a nivel de carácter y obligando al modelo a restaurar la información basándose en la semántica del contexto y la lógica del dominio.

* **Estrategia de enmascaramiento reforzada y controlable manualmente**  
  Permite aumentar manualmente la probabilidad de enmascaramiento de términos complejos (**💡 recomendado 50%–70%, no superar 80%**) y ajustar simultáneamente la tasa de enmascaramiento global (aprox. 20%–25%), para reforzar de manera dirigida la capacidad de razonamiento del modelo sobre puntos de conocimiento débiles.

* **Evitar automáticamente puntuación y tokens no semánticos**  
  Durante el enmascaramiento, se omiten automáticamente signos de puntuación en chino e inglés, símbolos y caracteres desconocidos para el tokenizer (`<unk>`), asegurando que cada span enmascarado corresponda a una unidad semántica válida.

* **Fusión automática de spans consecutivos o superpuestos**  
  Los spans de enmascaramiento adyacentes o superpuestos se fusionan automáticamente, garantizando que los pares de entrada/salida `<extra_id_n>` se generen en orden, con señales de entrenamiento continuas, cumpliendo totalmente con la especificación de span corruption de T5.

* **Procesamiento paralelo multihilo y retorno automático de muestras**  
  Soporta generación paralela de muestras con un número configurable de hilos, aumentando significativamente la eficiencia en corpus de gran escala; todas las muestras generadas se devuelven automáticamente y se reorganizan según el orden original, produciendo finalmente un conjunto de datos JSONL estandarizado listo para entrenamiento.

**Mediante la construcción de datos consciente del contexto, enmascaramiento prioritario de palabras clave, spans aleatorios y escenarios de “pérdida extrema de información” artificialmente creados, el modelo puede mantener la comprensión y restauración precisa de la semántica profesional incluso en las condiciones de entrada más desfavorables.**

> Escenarios de aplicación: corpus especializados en medicina, derecho, documentos técnicos u otros que requieran comprensión semántica precisa.

### ❗️ Notas sobre el Entrenamiento
* **Prevenir la parada temprana del modelo**: Tras el preprocesamiento, el modelo T5 puede mostrar una caída lenta de la pérdida (Loss) o fluctuaciones locales, lo que podría llevar al sistema a detener el entrenamiento prematuramente por error.
* **Sugerencia para juzgar la convergencia**: Se recomienda aumentar la duración del entrenamiento y juzgar la convergencia del modelo basándose en si la pérdida disminuye de forma estable y continua a través de múltiples etapas. Si el tiempo de entrenamiento es insuficiente, el efecto de restauración podría verse seriamente afectado.

### 📊 Evaluación de Resultados
Según las comparaciones de pruebas preliminares en el modelo estándar mT5-base:
* **Rendimiento del modelo estándar**: La tasa de restauración de vocabulario profesional se estima por debajo del 60%, y el 40% restante de los resultados carece de coherencia lógica, siendo casi inaceptable para el uso empresarial.
* **Tras las mejoras de este proyecto**: La tasa de restauración de vocabulario profesional estimada alcanzó el 85%. En el 15% de error restante, la mayoría son sustituciones por sinónimos cercanos, lo que mejora enormemente la legibilidad general y la coherencia lógica del texto.

### ⚠️ Limitaciones de Uso
* **Limitación por fragmentación de contexto**: Debido a que la longitud de texto que el modelo procesa por vez es limitada, y el número de palabras enmascaradas (Mask) dentro de cada fragmento también lo está, los documentos largos pueden sufrir rupturas de información contextual al ser segmentados, lo que impide capturar perfectamente la semántica entre párrafos. Se recomienda reintroducir parte del contexto para el reentrenamiento.
* **Limitaciones del algoritmo**: Dado que la restauración del modelo T5 se basa en algoritmos de probabilidad estadística, es imposible garantizar una precisión de restauración del 100% al tratar textos complejos.
* **Dependencia del dominio**: El efecto de restauración depende altamente de la cobertura y profundidad del léxico experto preestablecido.

### 🌌 Plan de Desarrollo Futuro
* Percepción Automática de Defectos:
Utilizar los "fragmentos anómalos" del tokenizador como señales implícitas. Cuando el OCR presenta una desalineación grave, el modelo podrá localizar automáticamente la ruptura semántica a través de las fluctuaciones anormales en la secuencia de tokens.
* Alineación Semántica Automática:
Lograr la reparación de extremo a extremo del texto dañado por OCR sin necesidad de especificar manualmente los puntos de unión.

[Demo](#Demo)

---
<a name="hindi"></a>
## हिन्दी（मशीन अनुवाद）
**T5-Refiner-DomainFocus** का उद्देश्य **पूर्व-प्रशिक्षण चरण (pre-training stage)** के रणनीतिक अनुकूलन के माध्यम से मॉडल को एक आंतरिक "सिमेंटिक लचीलापन (Semantic Resilience)" प्रदान करना है, **ताकि यह टेक्स्ट की कमी को अधिक मजबूती से संभाल सके और डोमेन विशेषज्ञता को समाहित कर सके।**

### 📖प्रोजेक्ट की पृष्ठभूमि
**चिकित्सा अभिलेखागार (medical archives) के डिजिटलीकरण** के दौरान, **OCR (ऑप्टिकल कैरेक्टर रिकग्निशन)** अक्सर कागज की क्षति या स्टैम्प के अवरोध जैसे कारणों से मुख्य शब्दावली में "कैरेक्टर की कमी" पैदा कर देता है।
पारंपरिक **T5** या **mT5** मॉडल (सामूहिक रूप से T5) इन क्षतिग्रस्त टेक्स्ट को संभालते समय दो मुख्य समस्याओं का सामना करते हैं:
* **यादृच्छिक मास्किंग (Random Masking) की सीमाएँ**: मॉडल केवल शब्द की जड़ के आधार पर "शब्द का अनुमान लगाना" सीखता है, जबकि वह पूर्ण चिकित्सा अवधारणाओं को वास्तव में नहीं समझ पाता।
* **टोकनाइज़ेशन मिसअलाइनमेंट की समस्या**: जब शब्दावली के अक्षर गायब होते हैं, तो टोकनाइज़र उसे अर्थहीन टुकड़ों में काट देता है, जिससे मॉडल अपना सिमेंटिक केंद्र खो देता है।

### ✅वर्तमान मुख्य विशेषताएं
इस परियोजना में वर्तमान में जटिल हार्डकोडेड नियमों पर निर्भरता नहीं है, बल्कि डेटा पूर्व-संसाधन, मास्किंग रणनीतियों और निष्पादन प्रवाह को अनुकूलित करके मॉडल की क्षमता बढ़ाई जाती है:

**ध्यान दें:** इस प्रोजेक्ट में T5 का अपना टोकनाइज़र उपयोग किया गया है।

* **लंबे टेक्स्ट को पैरा और विराम चिह्न के अनुसार प्रशिक्षण नमूना ब्लॉक में विभाजित करना**  
  स्वचालित रूप से उपन्यास, दस्तावेज़ या पेशेवर कॉर्पस से नमूने तैयार किए जाते हैं, पैरा की सीमाओं और अंग्रेज़ी-चीनी विराम चिह्नों के आधार पर विभाजन किया जाता है, साथ ही पिछले टेक्स्ट की अंतिम पंक्ति को संदर्भ पूर्ववर्ती (context prefix) के रूप में रखा जाता है ताकि नमूने की अर्थपूर्ण निरंतरता बनी रहे, अंशों के बीच अर्थ संबंध टूटने की संभावना कम हो और प्रत्येक नमूने की लंबाई मॉडल के संदर्भ विंडो के अनुरूप नियंत्रित रहे।

* **विशेषज्ञ शब्दकोश द्वारा निर्देशित परमाणु मास्किंग (Atomic Masking)**  
  कस्टम डोमेन शब्दकोश का उपयोग करके पेशेवर शब्दों (जैसे: तीव्र अग्रभाग हृदयाघात, पर्कुटेनियस कोरोनरी इंटरवेंशन) को अपरिभाज्य इकाई के रूप में मास्क किया जाता है, जिससे अक्षर-स्तरीय अनुमान लगाने के रास्ते अवरुद्ध होते हैं और मॉडल को केवल संदर्भ अर्थ और डोमेन लॉजिक पर भरोसा करके पुनर्निर्माण करना पड़ता है।

* **मानव-नियंत्रित सशक्त मास्किंग रणनीति**  
  उच्च कठिनाई वाले शब्दों के लिए मास्किंग संभावना मैन्युअली बढ़ाने का समर्थन (**💡 सुझाया गया: 50%–70%, 80% से अधिक नहीं**) और समग्र मास्किंग दर (लगभग 20%–25%) को समकालिक रूप से समायोजित करना संभव है, जिससे कमजोर ज्ञान बिंदुओं पर मॉडल की तर्क क्षमता को लक्षित रूप से सशक्त किया जा सके।

* **स्वचालित रूप से विराम चिह्न और गैर-अर्थपूर्ण token से बचाव**  
  मास्किंग प्रक्रिया के दौरान अंग्रेज़ी-चीनी विराम चिह्न, प्रतीक और tokenizer के अज्ञात अक्षरों (`<unk>`) को स्वचालित रूप से छोड़ दिया जाता है, जिससे हर mask span वास्तविक अर्थयुक्त इकाई के अनुरूप रहे।

* **सतत या ओवरलैपिंग span का स्वचालित विलय**  
  आसन्न या ओवरलैपिंग मास्क span को स्वचालित रूप से मिलाया जाता है, यह सुनिश्चित करता है कि उत्पन्न `<extra_id_n>` इनपुट/आउटपुट जोड़े अनुक्रमित और प्रशिक्षण सिग्नल लगातार हों, और यह पूरी तरह से T5 मूल शोध पत्र के span corruption प्रशिक्षण विनिर्देश के अनुकूल हो।

* **मल्टी-थ्रेडेड पैरेलल प्रोसेसिंग और स्वचालित नमूना रिटर्न**  
  समायोज्य थ्रेड संख्या के साथ पैरेलल रूप से नमूने उत्पन्न करने का समर्थन, बड़े पैमाने पर कॉर्पस प्रोसेसिंग की दक्षता को काफी बढ़ाता है; सभी उत्पन्न नमूने स्वचालित रूप से वापस लौटाए जाते हैं और मूल अनुक्रम में एकत्र किए जाते हैं, अंततः उन्हें प्रशिक्षण के लिए सीधे उपयोग योग्य मानकीकृत JSONL डेटासेट के रूप में आउटपुट किया जाता है।

**संदर्भ-संवेदनशील डेटा निर्माण, कीवर्ड प्राथमिकता मास्किंग, यादृच्छिक span मास्क और जानबूझकर निर्मित “अत्यधिक सूचना ह्रास” परिदृश्यों के माध्यम से, मॉडल सबसे प्रतिकूल इनपुट स्थितियों में भी पेशेवर अर्थ की सटीक समझ और पुनर्निर्माण बनाए रख सकता है।**  

> उपयुक्त परिदृश्य: चिकित्सा, कानूनी, तकनीकी दस्तावेज़ आदि ऐसे पेशेवर कॉर्पस जिनमें सटीक अर्थ समझ की आवश्यकता होती है।

### ❗️प्रशिक्षण के लिए सावधानियां
* **मॉडल को समय से पहले रुकने से रोकना**: प्री-प्रोसेसिंग के बाद, T5 मॉडल में लॉस (Loss) के धीरे-धीरे गिरने या स्थानीय उतार-चढ़ाव का भ्रम हो सकता है, जिससे सिस्टम गलती से प्रशिक्षण को जल्दी रोक सकता है।
* **अभिसरण (Convergence) निर्णय का सुझाव**: प्रशिक्षण का समय बढ़ाने की सिफारिश की जाती है, और मॉडल के अभिसरण का आकलन इस आधार पर करें कि क्या कई चरणों में लॉस लगातार और स्थिरता से गिर रहा है। यदि प्रशिक्षण का समय अपर्याप्त है, तो बहाली का प्रभाव बहुत कम हो सकता है।

### 📊प्रभाव का मूल्यांकन
mT5-base मानक मॉडल के साथ प्रारंभिक परीक्षण तुलना के अनुसार:
* **मानक मॉडल का प्रदर्शन**: पेशेवर डोमेन शब्दावली बहाली दर 60% से कम होने का अनुमान है, शेष 40% परिणाम तार्किक रूप से भ्रमित हैं और व्यावसायिक उपयोग के लिए लगभग अस्वीकार्य हैं।
* **इस प्रोजेक्ट के सुधार के बाद**: पेशेवर शब्दावली बहाली दर का अनुमान 85% तक पहुंच गया है। शेष 15% त्रुटियों में से अधिकांश समान अर्थ वाले शब्दों का प्रतिस्थापन हैं, जिससे टेक्स्ट की समग्र पठनीयता और तार्किक निरंतरता में काफी सुधार हुआ है।

### ⚠️उपयोग की सीमाएं
* **संदर्भ विखंडन (Context Fragmentation) की सीमा**: मॉडल द्वारा एक बार में संसाधित टेक्स्ट की लंबाई सीमित होने के कारण, लंबे दस्तावेजों को काटते समय संदर्भ की जानकारी टूट सकती है, जिससे कुछ क्रॉस-पैराग्राफ अर्थ पूरी तरह से कैप्चर नहीं हो पाते। संदर्भ के कुछ हिस्सों को वापस भेजकर पुन: प्रशिक्षित करने की सिफारिश की जाती है।
* **एल्गोरिथम की सीमाएं**: चूंकि T5 मॉडल की बहाली सांख्यिकीय संभाव्यता एल्गोरिदम पर आधारित है, इसलिए जटिल टेक्स्ट को संभालते समय 100% सटीकता की गारंटी देना असंभव है।
* **डोमेन निर्भरता**: बहाली का प्रभाव पूर्व-निर्धारित विशेषज्ञ शब्दावली के कवरेज और गहराई पर अत्यधिक निर्भर करता है।

### 🌌भविष्य की विकास योजनाएं
* स्वचालित कमी की पहचान (Automatic Defect Sensing):
टोकनाइज़र के "असामान्य टुकड़ों" को छिपे हुए संकेतों के रूप में उपयोग करना। जब OCR पहचान में गंभीर विसंगति होती है, तो मॉडल टोकन अनुक्रम के असामान्य उतार-चढ़ाव के माध्यम से स्वचालित रूप से सिमेंटिक ब्रेक का पता लगा सकेगा।
* स्वचालित सिमेंटिक संरेखण:
मैन्युअल रूप से कनेक्शन बिंदु निर्दिष्ट किए बिना, OCR क्षतिग्रस्त टेक्स्ट की एंड-टू-एंड मरम्मत प्राप्त करना।

[Demo](#Demo)

---
<a name="korean"></a>
## 한국어（기계 번역）
**T5-Refiner-DomainFocus**는 **사전 학습 단계**의 전략적 최적화를 통해 모델에 내재적인 '의미적 회복탄력성(Semantic Resilience)'을 부여하여, **텍스트 결손을 더욱 견고하게 처리하고 도메인 전문 지식을 주입할 수 있도록 설계되었습니다.**

### 📖 프로젝트 배경
**의료 기록 디지털화** 과정에서 **OCR(광학 문자 인식)**은 종이 손상, 직인 가려짐 등의 사유로 핵심 용어에 '문자 결손'이 발생하는 경우가 빈번합니다.
기존의 **T5** 또는 **mT5** 모델(통칭 T5)은 이러한 손상된 텍스트를 처리할 때 두 가지 주요 문제를 겪습니다:
* **랜덤 마스킹의 한계**: 모델이 어근에 기반한 '단어 추측'만 학습하게 되어, 완전한 의료 개념을 진정으로 이해하지 못함.
* **토큰화 어긋남 문제**: 용어의 글자가 유실될 때 토큰라이저가 이를 의미 없는 파편으로 쪼개버려 모델이 의미적 중심을 잃게 됨.

### ✅ 현재 핵심 기능
본 프로젝트는 현재 복잡한 하드코딩 규칙에 의존하지 않고, 데이터 전처리, 마스킹 전략 및 실행 프로세스를 최적화하여 모델 성능을 향상시키는 방식으로 설계되었습니다.

**주의:** 본 프로젝트는 T5 자체 토크나이저를 사용합니다。

* **긴 텍스트를 단락 및 문장 부호 기준으로 나누어 학습 샘플 생성**  
  소설, 문서 또는 전문 코퍼스에서 자동으로 샘플을 생성하며, 단락 경계와 한영 문장 부호를 기준으로 분할합니다. 이전 텍스트의 마지막 문장을 컨텍스트 접두사로 유지하여 샘플의 의미 연속성을 보장하고, 단락 간 의미 단절을 완화하며, 단일 샘플 길이를 모델의 컨텍스트 창에 맞게 제한합니다.

* **전문가 단어 사전을 활용한 원자적 마스킹**  
  사용자 정의 도메인 단어 사전을 활용하여, 전문 용어(예: 급성 전벽 심근경색, 경피적 관상동맥 중재술)를 분리할 수 없는 단위로 마스킹합니다. 이를 통해 문자 단위 추측 경로를 차단하고, 모델이 컨텍스트 의미와 도메인 논리에 기반하여 복원하도록 강제합니다.

* **수동 제어 가능한 강화 마스킹 전략**  
  고난도 용어에 대해 마스킹 확률을 수동으로 조정 가능하며 (**💡권장 50%–70%, 80% 이상은 비추천**), 전체 마스킹 비율(약 20%–25%)도 동시 조정할 수 있어, 모델이 약한 지식 영역에서 추론 능력을 강화하도록 설계되었습니다.

* **문장 부호 및 의미 없는 토큰 자동 회피**  
  마스킹 과정에서 한영 문장 부호, 특수 기호, tokenizer에서 인식되지 않는 문자(`<unk>`)를 자동으로 회피하여, 모든 마스크 span이 유효한 의미 단위와 대응되도록 보장합니다.

* **연속 또는 중첩된 span 자동 병합**  
  인접하거나 중첩된 마스크 span을 자동으로 병합하여 `<extra_id_n>` 입력/출력 순서를 일관되게 유지하고, 학습 신호를 연속적으로 제공하며 T5 원 논문의 span corruption 학습 규격을 완전히 준수합니다.

* **멀티스레드 병렬 처리 및 샘플 자동 회수**  
  스레드 수 조정이 가능한 병렬 처리로 대규모 코퍼스 처리 속도를 크게 향상시키며, 생성된 모든 샘플을 자동으로 회수하고 원본 순서대로 정렬하여, 최종적으로 학습에 바로 사용할 수 있는 표준 JSONL 데이터셋으로 출력합니다.

**컨텍스트 인지 기반 데이터 구성, 키워드 우선 마스킹, 랜덤 span 마스킹 및 의도적으로 생성된 '극단적 정보 결손' 시나리오를 통해, 모델은 가장 불리한 입력 조건에서도 전문 용어의 정확한 의미 이해와 복원이 가능하도록 학습됩니다.**  

> 적용 분야: 의학, 법률, 기술 문서 등 정확한 의미 이해가 요구되는 전문 코퍼스.

### ❗️ 학습 시 주의사항
* **모델 조기 종료 방지**: 전처리 이후 T5 모델은 Loss 하락이 느려지거나 국소적인 변동이 발생하는 가짜 정체 현상이 나타날 수 있으며, 이로 인해 시스템이 학습을 잘못 조기 종료할 수 있습니다.
* **수렴 판단 권장**: 학습 시간을 늘리고, 여러 단계에서 Loss가 지속적으로 안정되게 하락하는지를 종합적으로 판단하여 수렴 여부를 결정할 것을 권장합니다. 학습 시간이 부족할 경우 복원 효과가 크게 떨어질 수 있습니다.

### 📊 효과 평가
mT5-base 표준 모델과의 예비 테스트 비교 결과:
* **표준 모델 성능**: 전문 도메인 어휘 복원율이 60% 이하로 추정되며, 나머지 40%의 복원 결과는 논리가 혼란스러워 실제 업무에 적용하기 어려운 수준입니다.
* **본 프로젝트 개선 후**: 전문 어휘 복원율이 약 85%에 도달했습니다. 나머지 15%의 오차 대부분도 의미가 유사한 어휘로 대체된 것이어서, 텍스트 전체의 가독성과 논리적 일관성이 크게 향상되었습니다.

### ⚠️ 사용 제한
* **문맥 단편화 제한**: 모델이 한 번에 처리할 수 있는 텍스트 길이에 한계가 있고, 각 텍스트 구간 내 마스킹되는 어휘 수가 제한적이어서, 긴 문서를 분할 처리할 때 문맥 정보가 끊겨 구간을 넘나드는 의미를 완벽하게 포착하지 못할 수 있습니다. 일부 문맥을 포함하여 재학습하는 것을 권장합니다.
* **알고리즘의 한계**: T5 모델 자체의 복원은 통계적 확률 알고리즘에 기반하므로, 복잡한 텍스트를 처리할 때 100%의 복원 정확도를 보장하는 것은 불가능합니다.
* **도메인 의존성**: 복원 효과는 사전에 설정된 전문가 사전의 커버리지와 깊이에 크게 의존합니다.

### 🌌 향후 개발 계획
* 자동 결손 감지:
토큰라이저의 '이상 파편'을 암시적 신호로 활용합니다. OCR 인식에 심각한 어긋남이 발생했을 때, 모델이 토큰 시퀀스의 이상 변동을 통해 의미 단절 부위를 자동으로 위치 파악할 수 있도록 합니다.
* 의미 자동 정렬:
수동으로 연결 지점을 지정할 필요 없이, OCR로 손상된 텍스트에 대해 모델이 엔드 투 엔드(End-to-End) 복구를 수행하도록 구현합니다.

[Demo](#Demo)

---
<a name="portuguese"></a>
## Português (Tradução Automática)
**T5-Refiner-DomainFocus** visa, através da otimização estratégica na **fase de pré-treinamento**, conferir ao modelo uma "resiliência semântica" intrínseca, **permitindo que ele lide de forma mais robusta com a perda de texto e a injeção de conhecimento especializado de domínio.**

### 📖 Contexto do Projeto
No processamento de **digitalização de arquivos médicos**, o **OCR (Reconhecimento Óptico de Caracteres)** frequentemente apresenta "defeitos de caracteres" em termos essenciais devido a danos no papel, obstrução por carimbos, entre outros motivos.
Os modelos tradicionais **T5** ou **mT5** (coletivamente chamados de T5) apresentam dois problemas principais ao lidar com esses textos danificados:
* **Limitações do Mascaramento Aleatório**: Faz com que o modelo aprenda apenas a "adivinhar palavras" com base em radicais, sem entender verdadeiramente os conceitos médicos completos.
* **Problema de Desalinhamento da Tokenização**: Quando um termo perde letras, o tokenizador o fragmenta em pedaços sem sentido, fazendo com que o modelo perca o foco semântico.

### ✅ Principais Funcionalidades Atuais
Este projeto atualmente não depende de regras de codificação complexas, mas aprimora a capacidade do modelo por meio da otimização do pré-processamento de dados, das estratégias de máscara e do fluxo de execução:

**Nota:** Este projeto utiliza o próprio tokenizer do T5.

* **Divisão de textos longos em blocos de treinamento por parágrafo e pontuação**  
  Constrói automaticamente amostras a partir de romances, documentos ou corpora especializados, dividindo com base em limites de parágrafos e pontuação em chinês e inglês, ao mesmo tempo em que mantém a frase final do bloco anterior como prefixo contextual, garantindo a continuidade semântica das amostras, mitigando quebras de sentido entre blocos e controlando o tamanho das amostras para se adequar à janela de contexto do modelo.

* **Máscara atomizada guiada por dicionário de termos especializados**  
  Utiliza um dicionário personalizado de termos de domínio para tratar termos técnicos (por exemplo: infarto agudo de parede anterior, intervenção coronária percutânea) como unidades indivisíveis durante a aplicação da máscara, bloqueando caminhos de previsão por caractere e forçando o modelo a reconstruir o conteúdo com base na lógica e semântica do contexto.

* **Estratégia de máscara reforçada e controlável manualmente**  
  Permite aumentar manualmente a probabilidade de mascaramento de termos de alta complexidade (**💡 recomendado entre 50%–70%, não ultrapassar 80%**) e ajustar simultaneamente a taxa total de máscara (aproximadamente 20%–25%), fortalecendo a capacidade de raciocínio do modelo em pontos fracos do conhecimento.

* **Evitando automaticamente pontuação e tokens semânticos irrelevantes**  
  Durante o mascaramento, ignora automaticamente pontuações em chinês e inglês, símbolos e caracteres desconhecidos do tokenizer (`<unk>`), garantindo que cada span mascarado corresponda a uma unidade semântica válida.

* **Mesclagem automática de spans consecutivos ou sobrepostos**  
  Mescla spans de máscara adjacentes ou sobrepostos automaticamente, garantindo que os pares de entrada/saída `<extra_id_n>` sejam ordenados corretamente, com sinal de treinamento contínuo, totalmente compatíveis com a norma de span corruption do artigo original do T5.

* **Processamento paralelo multithread e retorno automático das amostras**  
  Suporta a geração de amostras em paralelo com número de threads configurável, aumentando significativamente a eficiência do processamento de grandes corpora; todas as amostras geradas são retornadas automaticamente e reorganizadas na ordem original, resultando em um dataset JSONL padronizado pronto para treinamento.

**Por meio da construção de dados sensíveis ao contexto, mascaramento priorizado por palavras-chave, spans aleatórios e cenários de "perda extrema de informação" artificialmente criados, o modelo mantém a compreensão e reconstrução precisa da semântica especializada mesmo nas condições de entrada mais adversas.**

> Cenários de aplicação: corpora médicos, jurídicos, documentos técnicos ou qualquer contexto que exija compreensão precisa da semântica especializada.

### ❗️ Observações de Treinamento
* **Prevenção de Parada Precoce**: Após o pré-processamento, o modelo T5 pode apresentar uma queda lenta na Loss ou flutuações locais ilusórias, levando o sistema a interromper o treinamento prematuramente por erro.
* **Sugestão de Julgamento de Convergência**: Recomenda-se aumentar o tempo de treinamento e julgar a convergência do modelo de forma abrangente, baseando-se na estabilidade da queda da Loss em múltiplas etapas. Se o tempo de treinamento for insuficiente, o efeito de restauração pode ser drasticamente reduzido.

### 📊 Avaliação de Resultados
De acordo com testes comparativos preliminares no modelo padrão mT5-base:
* **Desempenho do Modelo Padrão**: A taxa de restauração de vocabulário especializado é estimada em menos de 60%, com os 40% restantes apresentando resultados logicamente confusos, sendo quase inaceitáveis para o negócio.
* **Após Melhorias Deste Projeto**: A taxa de restauração de vocabulário especializado atingiu estimadamente 85%. Nos 15% de erro restantes, a maioria são substituições por termos semanticamente próximos, melhorando significativamente a legibilidade geral e a coerência lógica do texto.

### ⚠️ Limitações de Uso
* **Restrição de Fragmentação de Contexto**: Devido ao limite de extensão de texto processado por vez e ao número limitado de termos mascarados em cada segmento, documentos longos podem sofrer quebras de informação contextual durante o corte, impedindo a captura perfeita de semânticas que cruzam parágrafos. Recomenda-se reenviar parte do contexto para re-treinamento.
* **Limitações do Algoritmo**: Como a restauração do próprio modelo T5 é baseada em algoritmos de probabilidade estatística, é impossível garantir 100% de precisão na restauração de textos complexos.
* **Dependência de Domínio**: O efeito de restauração depende altamente da abrangência e profundidade do glossário especializado predefinido.

### 🌌 Plano de Desenvolvimento Futuro
* Percepção Automática de Defeitos:
Utilizar "fragmentos anômalos" do tokenizador como sinais implícitos. Quando o reconhecimento de OCR apresentar desalinhamentos graves, o modelo poderá localizar automaticamente a quebra semântica através das flutuações anormais na sequência de tokens.
* Alinhamento Semântico Automático:
Realizar a reparação ponta a ponta de textos danificados por OCR sem a necessidade de especificar manualmente os pontos de conexão.

[Demo](#Demo)

---
<a name="Demo"></a>
## 📡 Demo

-[Demo-EN](./Demo-EN.txt)

（Most of the application environment for this project is in Chinese, which may cause minor issues with processing English, such as splitting by punctuation. The T5 model itself does support English.）

- ⭐️[Demo-CN](./Demo-CN.txt)

[Introduction](#Introduction)
  
---
<a name="Requirements"></a>
## 🛠️ Requirements

```text
transformers>=4.30.0  # HuggingFace Transformers
regex                
tqdm                
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
> **⚠️Disclaimer:** The non-English and non-Chinese versions of this documentation are provided for convenience only and were generated using machine translation. README may have been revised multiple times, and non-Chinese content may be missing. In case of any discrepancy, the Chinese version shall prevail. 
