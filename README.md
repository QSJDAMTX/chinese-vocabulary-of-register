# 现代汉语词汇语体属性资源

本项目开源了以下研究所构建的词汇语体数据资源：

莫凯洁,胡韧奋. 现代汉语词汇语体属性探测模型研究 [J]. 语言文字应用, 2023, (04): 118-131. 

该研究采用《现代汉语词典》第七版（下称《现汉》）中标注 `<书><口>` 的词语来训练词汇语体属性探测模型，使用模型对《国际中文教育中文水平等级标准》词表（下称《标准》）和《义务教育常用词表（草案）》主表（下称《常用词表》）的共 25500 个词语进行了语体正式度测量。

## 一、模型预测情况

开源的词表主要使用了两种模型及其加权组合的预测分数：
1. 模型一：基于词向量特征的 SVM 模型（Support Vector Machine），对《现汉》的拟合效果最佳。
2. 模型二：使用词本身特征 + 语料库语境特征训练的 RF 模型（Random Forest），对不同词汇已有一定区分效果，加权分数为 60% 时得到的正式度分值对不同词汇已有一定区分效果。

### 不同模型的预测特性

1. SVM（词向量特征）：对《现汉》词汇语体的预测效果最佳，设置词向量特征，对词汇的文言特征较敏感。
2. RF（语料库特征 + 词本身特征） ：对《现汉》词汇语体的预测效果尚可，根据语体特性设置特征，对词汇的文言特征不敏感。

具体情况如下表所示：

| 词汇        | SVM（词向量特征） | RF（语料库特征 + 词本身特征） |
|------------|------------------|-----------------------------|
| 纪传体_n   | 0.0107           | 0.5163                      |
| 会计师_n    | 0.1333           | 0.4097                      |
| 祖国_n      | 0.1345           | 0.4690                      |
| 蠲除_v      | 0.9939           | 0.7168                      |
| 胪陈_v      | 0.9781           | 0.7725                      |
| 翛然_a      | 0.9999           | 0.7747                      |

本项目开源SVM（词向量特征）模型预测分数、RF（语料库特征 + 词本身特征）模型预测分数、0.6RF+0.4SVM的加权预测分数，使用者可以根据需求选择需要的预测结果，也可调整加权分数进行重新组合。

## 二、词表说明

项目中有 "level"、"edu" 文件夹，代表《标准》和《常用词表》。每个文件夹中各有 svm、rf、0.6rf_0.4svm 三个文件，代表 SVM（词向量特征）、RF（语料库特征 + 词本身特征）、加权组合模型预测的词汇分数。每个文件中有 word, label, formality 三列，word 为词汇，label 在《标准》中为难度等级/在《常用词表》中为学段，formality 为模型预测的正式程度。Word 词汇中的词性是由 pyltp 标注，该模型使用的是 863 词性标注集 [http://ltp.ai/docs/appendix.html](http://ltp.ai/docs/appendix.html)。

## 三、注意事项

1. 每一个词表都经过“特征抽取-模型预测”阶段，因特征语料为随机抽取，所以同一词在不同词表中的正式程度可能会有一定波动。
2. 《现汉》中缺乏对 i 类词（熟语、习语、成语等）的语体标注，模型学习缺乏监督信号，所以在词表中 i 类词的正式度预测值仅供参考。
