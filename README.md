# T5-Refiner-DomainFocus
# T5-预处理特定词语遮照增强

（Code will be uploaded later.）

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
<a name="english"></a>
## ⭐️ English

**T5-Refiner-DomainFocus** aims to empower models with intrinsic "semantic resilience" through **pre-training stage** strategy optimization, **enabling more robust handling of text corruption and the injection of domain-specific expertise.**

### 📖 Project Background
During **the digitization of medical records**, **OCR (Optical Character Recognition)** often suffers from "character defects" in core terminology due to damaged paper, stamp occlusion, or other physical factors.
Traditional **T5** or **mT5** models (collectively referred to as T5) face two major challenges when processing such corrupted text:
* Limitations of Random Masking: The model learns to "guess" words based on sub-word roots rather than truly understanding complete medical concepts.
* Tokenization Misalignment: When letters are missing from a term, the tokenizer breaks it into meaningless fragments, causing the model to lose its semantic focus.

### ✅ Core Features
This project enhances model capability by optimizing the data preprocessing pipeline rather than relying on complex hard-coded rules:

* Expert Lexicon-Guided Atomic Masking:
By leveraging custom lexicons, the model is forced to treat professional terms (e.g., Acute Anterior Myocardial Infarction) as indivisible units during masking. This forces the model to derive answers from contextual logic rather than taking shortcuts via residual characters.

* Manual Enhanced Training:
Supports manual adjustment of masking probabilities for specific high-difficulty terms (💡Recommended: 50%-70%, not to exceed 80%), while simultaneously increasing the global masking rate (20%-25%).

* Automatic Punctuation Avoidance:
Prevents the introduction of noise interference during the masking process.

By creating scenarios of "extreme information loss," the model is compelled to maintain accurate reconstruction of professional semantics even under the worst input conditions.

### ❗️ Training Notes
* Preventing Early Stopping: After preprocessing, T5 models may exhibit slow loss reduction or local fluctuations, which can trick systems into stopping training prematurely.
* Convergence Judgment: It is recommended to extend training duration and evaluate convergence based on whether the loss decreases steadily across multiple stages. Insufficient training will significantly degrade restoration performance.

### 📊 Evaluation
Based on preliminary testing with the mT5-base standard model:
* Standard Model Performance: The restoration rate for specialized terminology is estimated to be below 60%. The remaining 40% of results are often logically incoherent and unacceptable for professional use.
* With DomainFocus Improvement: The estimated restoration rate reaches 85%. Of the remaining 15% error margin, most are semantic synonyms, which greatly improves the overall readability and logical consistency of the text.

### ⚠️ Limitations
* Context Fragmentation: Due to the limited sequence length and the restricted number of masks per segment, long documents may suffer from semantic disconnection during chunking. It is recommended to feed partial overlapping context back during re-training.
* Algorithmic Limits: Since T5 restoration is based on statistical probability, it is impossible to guarantee 100% accuracy when dealing with highly complex text.
* Domain Dependency: The restoration effectiveness is highly dependent on the coverage and depth of the predefined expert lexicon.

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
* 随机遮蔽的局限性：导致模型只学会了根据词根“猜词”，而没有真正理解完整的医学概念。
* 分词错位问题：当术语丢失字母时，分词器会将其切碎为无意义的碎片，导致模型失去语义重心。

### ✅当前核心功能
本项目目前不依赖复杂的硬编码规则，而是通过优化数据预处理流程来增强模型能力：

* 专家词库引导的原子化遮蔽：
依托自定义词库，强制模型将专业术语（如：急性前壁心肌梗死）视为不可分割的整体进行遮蔽。通过这种方式，迫使模型从上下文的逻辑中寻找答案，而非通过残余字符投机取巧。

* 人工设定强化训练：
支持手动提高特定高难度术语的遮蔽概率（💡推荐在50%-70%，不宜超过80%），同时可同步提高整体的遮蔽率（20%-25%）。

* 自动规避标点符号：
防止引入干扰。

通过人为制造“极端信息缺失”的场景，强制模型在最差的输入情况下依然能保持对专业语义的准确还原。

### ❗️训练注意事项
* 防止模型提前停止：在预处理之后，T5 模型可能会出现 Loss 下降缓慢或产生局部波动的假象，导致系统错误地提前停止训练。
* 收敛判断建议：推荐增加训练时长，并根据多个阶段的 Loss 是否持续稳定下降来综合判断模型收敛情况。若训练时间不足，还原效果可能会大打折扣。

### 📊效果评估
根据初步测试对比，在 mT5-base 标准模型中：
* 标准模型表现：在专业领域的词汇还原率估算在 60% 以下，剩余 40% 的还原结果逻辑混乱，几乎无法被业务接受。
* 本项目改进后：专业词汇还原率估算达到了 85%。剩下的 15% 误差中，大部分是语义相近的词汇替代，极大地提高了文本的整体可读性和逻辑连贯性。

### ⚠️使用限制
* 上下文片段化限制：由于模型单次处理的文本长度有限，且每段文本内标记（Mask）的词汇数量受限，长文档在切分处理时可能存在上下文信息断裂的情况，导致部分跨段落的语义无法被完美捕捉。推荐回传部分上下文再训练。
* 算法局限性：由于 T5 模型本身的还原是基于统计概率算法的，因此在处理复杂的文本时，不可能保证 100% 的还原准确率。
* 领域依赖：还原效果高度依赖于预设专家词库的覆盖面与深度。

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
* ランダムマスキングの限界：モデルが語根に基づいた「単語の推測」を学習するにとどまり、完全な医療概念を真に理解できない。
* トークナイズの不一致問題：用語の文字が欠落すると、トークナイザーがそれを無意味な断片に細分化してしまい、モデルが意味の重心を失う。

### ✅現在のコア機能
本プロジェクトは現在、複雑なハードコーディングのルールに依存せず、データの前処理フローを最適化することでモデルの能力を強化しています：

* 専門用語集ガイドによるアトミック・マスキング：
カスタム用語集に基づき、専門用語（例：急性前壁心筋梗塞）を分割不可能な一体として強制的にマスキングします。この方法により、残存文字からの憶測ではなく、文脈の論理から答えを見つけ出すようモデルに強制します。

* アーティフィシャル・セッティングによる強化トレーニング：
特定の高難度用語のマスキング確率を手動で高めることをサポート（💡推奨50%-70%、80%を超えないこと）。同時に、全体のマスキング率（20%-25%）を同期して高めることが可能です。

* 句読点の自動回避：
ノイズの混入を防止します。

意図的に「極端な情報の欠落」シーンを作り出すことで、最悪の入力状況下でも専門的な意味を正確に復元できる能力をモデルに強制します。

### ❗️訓練上の注意点
* モデルの早期停止の防止：前処理後、T5モデルは損失（Loss）の下落が緩やかになったり、局所的な変動が生じたりする「見かけ上の停滞」が発生し、システムが誤って訓練を早期終了させる可能性があります。
* 収束判断の推奨：訓練時間を延長し、複数のフェーズで損失が継続的に安定して下落しているかに基づいて、モデルの収束を総合的に判断することを推奨します。訓練時間が不足すると、復元効果が大幅に低下する可能性があります。

### 📊効果評価
mT5-base標準モデルを用いた初期テストの比較：
* 標準モデルのパフォーマンス：専門分野の語彙復元率は推定60%以下。残りの40%は論理が混乱しており、業務利用はほぼ不可能です。
* 本プロジェクトによる改善後：専門語彙の復元率は推定85%に達しました。残りの15%の誤差の大部分は意味の近い語彙への置換であり、テキスト全体の可読性と論理的な一貫性が大幅に向上しました。

### ⚠️使用制限
* コンテキストの断片化の制限：モデルが一度に処理できるテキスト長には制限があり、また各テキストセグメント内でマスク（Mask）される語彙数も限られているため、長いドキュメントを分割処理する際に文脈情報が断絶し、セグメントを跨ぐ意味を完璧に捉えられない場合があります。一部のコンテキストを再度含めてトレーニングすることを推奨します。
* アルゴリズムの限界：T5モデル自体の復元は統計的確率アルゴリズムに基づいているため、複雑なテキストを処理する際に100%の復元精度を保証することは不可能です。
* ドメイン依存性：復元効果は、あらかじめ設定された専門用語集の網羅性と深さに強く依存します。

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
* Grenzen der zufälligen Maskierung: Dies führt dazu, dass das Modell nur lernt, Wörter basierend auf Wortstämmen zu „raten“, anstatt medizinische Konzepte wirklich vollständig zu verstehen.
* Tokenisierungs-Fehlausrichtung: Wenn Buchstaben in Fachbegriffen fehlen, zerlegt der Tokenizer diese in bedeutungslose Fragmente, wodurch das Modell seinen semantischen Fokus verliert.

### ✅ Aktuelle Kernfunktionen
Dieses Projekt verlässt sich derzeit nicht auf komplexe Hardcoding-Regeln, sondern stärkt die Modellfähigkeiten durch die Optimierung des Daten-Preprocessing-Workflows:

* Atomare Maskierung gesteuert durch Experten-Vokabular:
Auf Basis eines benutzerdefinierten Vokabulars wird das Modell gezwungen, Fachbegriffe (z. B. akuter Vorderwandmyokardinfarkt) als untrennbare Einheit zu maskieren. Auf diese Weise wird das Modell gezwungen, Antworten aus der Logik des Kontextes zu finden, anstatt durch verbleibende Zeichen oberflächliche Rückschlüsse zu ziehen.

* Verstärktes Training durch manuelle Einstellungen:
Unterstützt die manuelle Erhöhung der Maskierungswahrscheinlichkeit für spezifische, hochgradig schwierige Begriffe (💡 empfohlen bei 50%-70%, nicht über 80%), während gleichzeitig die allgemeine Maskierungsrate (20%-25%) synchron erhöht werden kann.

* Automatische Vermeidung von Satzzeichen:
Verhindert die Einführung von Störfaktoren.

Durch die künstliche Erzeugung von Szenarien mit „extremem Informationsverlust“ wird das Modell gezwungen, selbst bei schlechtesten Eingabebedingungen eine präzise Wiederherstellung der Fachsemantik beizubehalten.

### ❗️ Hinweise zum Training
* Vorzeitigen Stopp des Modells verhindern: Nach dem Preprocessing kann es bei T5-Modellen zu einer Täuschung durch langsam sinkenden Loss oder lokale Schwankungen kommen, was dazu führt, dass das System das Training fälschlicherweise vorzeitig stoppt.
* Empfehlung zur Konvergenzbeurteilung: Es wird empfohlen, die Trainingsdauer zu erhöhen und die Konvergenz des Modells basierend auf dem kontinuierlichen und stabilen Sinken des Loss über mehrere Phasen hinweg umfassend zu beurteilen. Bei unzureichender Trainingszeit kann der Wiederherstellungseffekt stark beeinträchtigt werden.

### 📊 Effektivitätsbewertung
Basierend auf vorläufigen Vergleichstests im mT5-base Standardmodell:
* Leistung des Standardmodells: Die Wiederherstellungsrate von Fachvokabular wird auf unter 60% geschätzt, wobei die restlichen 40% logisch verwirrend und für den geschäftlichen Einsatz kaum akzeptabel sind.
* Nach der Verbesserung durch dieses Projekt: Die Wiederherstellungsrate von Fachvokabular erreichte geschätzte 85%. Von den verbleibenden 15% Fehlerquote entfällt der Großteil auf semantisch ähnliche Wortsubstitutionen, was die allgemeine Lesbarkeit und logische Kohärenz des Textes erheblich verbessert.

### ⚠️ Nutzungseinschränkungen
* Einschränkung durch Kontext-Fragmentierung: Da die Textlänge pro Verarbeitungsschritt begrenzt ist und die Anzahl der maskierten Wörter pro Textsegment limitiert ist, kann es bei der Aufteilung langer Dokumente zu Brüchen in den Kontextinformationen kommen. Dies führt dazu, dass einige segmentübergreifende Semantiken nicht perfekt erfasst werden können. Es wird empfohlen, Teile des Kontextes für das Re-Training zurückzugeben.
* Algorithmische Grenzen: Da die Wiederherstellung des T5-Modells auf statistischen Wahrscheinlichkeitsalgorithmen basiert, kann eine 100%ige Genauigkeit bei komplexen Texten nicht garantiert werden.
* Domänenabhängigkeit: Der Wiederherstellungseffekt hängt stark von der Abdeckung und Tiefe des vordefinierten Experten-Vokabulars ab.

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
* Limites du masquage aléatoire : Le modèle apprend uniquement à « deviner » les mots à partir des racines, sans véritablement comprendre les concepts médicaux complets.
* Problème de désalignement de la tokenisation : Lorsqu'un terme perd des lettres, le tokenizer le fragmente en morceaux dénués de sens, faisant perdre au modèle son centre de gravité sémantique.

### ✅ Fonctions clés actuelles
Ce projet ne repose pas sur des règles codées en dur complexes, mais renforce les capacités du modèle en optimisant le flux de prétraitement des données :

* Masquage atomique guidé par un lexique d'experts :
S'appuyant sur un lexique personnalisé, il force le modèle à considérer les termes techniques (ex : infarctus aigu du myocarde de la paroi antérieure) comme un tout indivisible lors du masquage. De cette manière, le modèle est contraint de chercher des réponses dans la logique du contexte plutôt que de spéculer sur des caractères résiduels.

* Entraînement renforcé par paramétrage manuel :
Permet d'augmenter manuellement la probabilité de masquage de certains termes particulièrement difficiles (💡 recommandé entre 50% et 70%, ne pas dépasser 80%), tout en augmentant simultanément le taux de masquage global (20%-25%).

* Évitement automatique de la ponctuation :
Empêche l'introduction d'interférences.

En créant artificiellement des scénarios de « perte d'information extrême », le modèle est contraint de maintenir une restitution précise de la sémantique professionnelle, même dans les pires conditions d'entrée.

### ❗️ Précautions d'entraînement
* Prévenir l'arrêt prématuré du modèle : Après le prétraitement, le modèle T5 peut donner l'illusion d'une baisse lente de la perte (Loss) ou de fluctuations locales, ce qui peut amener le système à arrêter l'entraînement prématurément par erreur.
* Conseils pour juger de la convergence : Il est recommandé d'augmenter la durée d'entraînement et de juger de la convergence de manière globale en vérifiant si la perte continue de descendre de façon stable sur plusieurs étapes. Si le temps d'entraînement est insuffisant, l'effet de restauration pourrait être considérablement réduit.

### 📊 Évaluation des résultats
Selon les tests comparatifs préliminaires sur le modèle standard mT5-base :
* Performance du modèle standard : Le taux de restauration du vocabulaire spécialisé est estimé à moins de 60 %, les 40 % restants étant logiquement confus et pratiquement inacceptables pour une utilisation métier.
* Après amélioration par ce projet : Le taux de restauration du vocabulaire spécialisé atteint environ 85 %. Parmi les 15 % d'erreurs restantes, la plupart sont des substitutions par des termes sémantiquement proches, ce qui améliore considérablement la lisibilité globale et la cohérence logique du texte.

### ⚠️ Limites d'utilisation
* Limitation de la fragmentation du contexte : En raison de la longueur limitée du texte traité en une seule fois et du nombre restreint de mots masqués (Mask) par segment, le traitement de documents longs peut entraîner une rupture des informations contextuelles, empêchant la capture parfaite de la sémantique entre les paragraphes. Il est recommandé de réinjecter une partie du contexte pour le réentraînement.
* Limites algorithmiques : La restauration du modèle T5 étant basée sur des algorithmes de probabilité statistique, il est impossible de garantir une précision de restauration de 100 % lors du traitement de textes complexes.
* Dépendance au domaine : L'efficacité de la restauration dépend fortement de la couverture et de la profondeur du lexique d'experts prédéfini.

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
* Limitaciones del enmascaramiento aleatorio: El modelo solo aprende a "adivinar palabras" basándose en raíces léxicas, sin comprender realmente los conceptos médicos completos.
* Problemas de desalineación de la tokenización: Cuando un término pierde letras, el tokenizador lo fragmenta en pedazos sin sentido, lo que hace que el modelo pierda el centro de gravedad semántico.

### ✅ Funciones Principales Actuales
Este proyecto no depende actualmente de reglas complejas codificadas a mano, sino que mejora las capacidades del modelo optimizando el flujo de preprocesamiento de datos:

* Enmascaramiento Atómico guiado por léxico experto:
Basándose en un léxico personalizado, se obliga al modelo a considerar los términos técnicos (ej. infarto agudo de miocardio de la pared anterior) como un todo indivisible al enmascararlos. De esta forma, se fuerza al modelo a buscar respuestas en la lógica del contexto, en lugar de especular a través de caracteres residuales.

* Entrenamiento Reforzado mediante configuración manual:
Permite aumentar manualmente la probabilidad de enmascaramiento de términos específicos de alta dificultad (💡 recomendado entre 50%-70%, no debe superar el 80%), pudiendo aumentar simultáneamente la tasa de enmascaramiento general (20%-25%).

* Evasión Automática de signos de puntuación:
Previene la introducción de interferencias.

Al crear artificialmente escenarios de "pérdida extrema de información", se obliga al modelo a mantener una restauración precisa de la semántica profesional incluso en las peores condiciones de entrada.

### ❗️ Notas sobre el Entrenamiento
* Prevenir la parada temprana del modelo: Tras el preprocesamiento, el modelo T5 puede mostrar una caída lenta de la pérdida (Loss) o fluctuaciones locales, lo que podría llevar al sistema a detener el entrenamiento prematuramente por error.
* Sugerencia para juzgar la convergencia: Se recomienda aumentar la duración del entrenamiento y juzgar la convergencia del modelo basándose en si la pérdida disminuye de forma estable y continua a través de múltiples etapas. Si el tiempo de entrenamiento es insuficiente, el efecto de restauración podría verse seriamente afectado.

### 📊 Evaluación de Resultados
Según las comparaciones de pruebas preliminares en el modelo estándar mT5-base:
* Rendimiento del modelo estándar: La tasa de restauración de vocabulario profesional se estima por debajo del 60%, y el 40% restante de los resultados carece de coherencia lógica, siendo casi inaceptable para el uso empresarial.
* Tras las mejoras de este proyecto: La tasa de restauración de vocabulario profesional estimada alcanzó el 85%. En el 15% de error restante, la mayoría son sustituciones por sinónimos cercanos, lo que mejora enormemente la legibilidad general y la coherencia lógica del texto.

### ⚠️ Limitaciones de Uso
* Limitación por fragmentación de contexto: Debido a que la longitud de texto que el modelo procesa por vez es limitada, y el número de palabras enmascaradas (Mask) dentro de cada fragmento también lo está, los documentos largos pueden sufrir rupturas de información contextual al ser segmentados, lo que impide capturar perfectamente la semántica entre párrafos. Se recomienda reintroducir parte del contexto para el reentrenamiento.
* Limitaciones del algoritmo: Dado que la restauración del modelo T5 se basa en algoritmos de probabilidad estadística, es imposible garantizar una precisión de restauración del 100% al tratar textos complejos.
* Dependencia del dominio: El efecto de restauración depende altamente de la cobertura y profundidad del léxico experto preestablecido.

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
* यादृच्छिक मास्किंग (Random Masking) की सीमाएँ: मॉडल केवल शब्द की जड़ के आधार पर "शब्द का अनुमान लगाना" सीखता है, जबकि वह पूर्ण चिकित्सा अवधारणाओं को वास्तव में नहीं समझ पाता।
* टोकनाइज़ेशन मिसअलाइनमेंट की समस्या: जब शब्दावली के अक्षर गायब होते हैं, तो टोकनाइज़र उसे अर्थहीन टुकड़ों में काट देता है, जिससे मॉडल अपना सिमेंटिक केंद्र खो देता है।

### ✅वर्तमान मुख्य विशेषताएं
यह प्रोजेक्ट वर्तमान में जटिल हार्ड-कोडिंग नियमों पर निर्भर रहने के बजाय, डेटा प्री-प्रोसेसिंग प्रवाह को अनुकूलित करके मॉडल की क्षमता को बढ़ाता है:

* विशेषज्ञ शब्दावली द्वारा निर्देशित परमाणु मास्किंग (Atomic Masking):
कस्टम शब्दावली के आधार पर, मॉडल को पेशेवर शब्दों (जैसे: तीव्र पूर्वकाल दीवार रोधगलन / Acute anterior wall myocardial infarction) को एक अविभाज्य इकाई के रूप में मास्क करने के लिए मजबूर किया जाता है। इस तरह, मॉडल को शेष अक्षरों से अनुमान लगाने के बजाय संदर्भ के तर्क से उत्तर खोजने के लिए मजबूर किया जाता है।

* मैन्युअल सेटिंग द्वारा सुदृढ़ीकरण प्रशिक्षण:
विशिष्ट कठिन शब्दों की मास्किंग संभावना को मैन्युअल रूप से बढ़ाने का समर्थन (💡अनुशंसित 50%-70%, 80% से अधिक नहीं), साथ ही कुल मास्किंग दर (20%-25%) को भी बढ़ाया जा सकता है।

* विराम चिह्नों से स्वचालित बचाव:
हस्तक्षेप को रोकने के लिए।

जानबूझकर "अत्यधिक सूचना कमी" के परिदृश्य बनाकर, मॉडल को सबसे खराब इनपुट स्थितियों में भी पेशेवर अर्थों को सटीक रूप से पुनर्स्थापित करने के लिए मजबूर किया जाता直।

### ❗️प्रशिक्षण के लिए सावधानियां
* मॉडल को समय से पहले रुकने से रोकना: प्री-प्रोसेसिंग के बाद, T5 मॉडल में लॉस (Loss) के धीरे-धीरे गिरने या स्थानीय उतार-चढ़ाव का भ्रम हो सकता है, जिससे सिस्टम गलती से प्रशिक्षण को जल्दी रोक सकता है।
* अभिसरण (Convergence) निर्णय का सुझाव: प्रशिक्षण का समय बढ़ाने की सिफारिश की जाती है, और मॉडल के अभिसरण का आकलन इस आधार पर करें कि क्या कई चरणों में लॉस लगातार और स्थिरता से गिर रहा है। यदि प्रशिक्षण का समय अपर्याप्त है, तो बहाली का प्रभाव बहुत कम हो सकता है।

### 📊प्रभाव का मूल्यांकन
mT5-base मानक मॉडल के साथ प्रारंभिक परीक्षण तुलना के अनुसार:
* मानक मॉडल का प्रदर्शन: पेशेवर डोमेन शब्दावली बहाली दर 60% से कम होने का अनुमान है, शेष 40% परिणाम तार्किक रूप से भ्रमित हैं और व्यावसायिक उपयोग के लिए लगभग अस्वीकार्य हैं।
* इस प्रोजेक्ट के सुधार के बाद: पेशेवर शब्दावली बहाली दर का अनुमान 85% तक पहुंच गया है। शेष 15% त्रुटियों में से अधिकांश समान अर्थ वाले शब्दों का प्रतिस्थापन हैं, जिससे टेक्स्ट की समग्र पठनीयता और तार्किक निरंतरता में काफी सुधार हुआ है।

### ⚠️उपयोग की सीमाएं
* संदर्भ विखंडन (Context Fragmentation) की सीमा: मॉडल द्वारा एक बार में संसाधित टेक्स्ट की लंबाई सीमित होने के कारण, लंबे दस्तावेजों को काटते समय संदर्भ की जानकारी टूट सकती है, जिससे कुछ क्रॉस-पैराग्राफ अर्थ पूरी तरह से कैप्चर नहीं हो पाते। संदर्भ के कुछ हिस्सों को वापस भेजकर पुन: प्रशिक्षित करने की सिफारिश की जाती है।
* एल्गोरिथम की सीमाएं: चूंकि T5 मॉडल की बहाली सांख्यिकीय संभाव्यता एल्गोरिदम पर आधारित है, इसलिए जटिल टेक्स्ट को संभालते समय 100% सटीकता की गारंटी देना असंभव है।
* डोमेन निर्भरता: बहाली का प्रभाव पूर्व-निर्धारित विशेषज्ञ शब्दावली के कवरेज और गहराई पर अत्यधिक निर्भर करता है।

### 🌌भविष्य की विकास योजनाएं
* स्वचालित कमी की पहचान (Automatic Defect Sensing):
टोकनाइज़र के "असामान्य टुकड़ों" को छिपे हुए संकेतों के रूप में उपयोग करना। जब OCR पहचान में गंभीर विसंगति होती है, तो मॉडल टोकन अनुक्रम के असामान्य उतार-चढ़ाव के माध्यम से स्वचालित रूप से सिमेंटिक ब्रेक का पता लगा सकेगा।
* स्वचालित सिमेंटिक संरेखण:
मैन्युअल रूप से कनेक्शन बिंदु निर्दिष्ट किए बिना, OCR क्षतिग्रस्त टेक्स्ट की एंड-टू-एंड मरम्मत प्राप्त करना।

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
