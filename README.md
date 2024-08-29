# 中文动词指代消解语料库

该仓库分享动词驱动事件的共指关系中文语料库，其来源于[中文信息学报](http://jcip.cipsc.org.cn/) 论文 **《动词驱动事件的共指关系中文语料库构建及大模型评测》** 。

## 工作介绍

该工作研究了中文动词所驱动事件的共指关系，将其分为以下几类：

```
共指
├── 强共指：动词驱动相同事件
└── 弱相关：动词驱动不同事件，这些事件之间具有相关性
    ├── 词义包含：事件包含，体现在动词的词义包含
    ├── 角色包含：事件包含，体现在动词角色的包含
    └── 角色变更：同一事件的不同发生情况，体现在动词角色的变更
```

本文在论文中对上述体系进行了详细介绍，构建了数据规范，并采用人工标注方式构建了相应语料库。该数据集文本选自北京大学计算语言学研究所发布的《人民日报》标注语料库。

## 数据集介绍

数据集涵盖1,000篇文档，见文件 [samples.jsonl](samples.jsonl)。

### 数据集格式介绍

数据集以jsonl文件存储，每一行为一篇文档的数据，以下对json格式进行说明：

```json
{
    "Doc_ID": "19980101-01-003",
    "Sentences": {
        "0": "19980101-xx-xxx-xxx",
        "1": "19980101-xx-xxx-xxx",
        "2": "19980101-xx-xxx-xxx",
        ......
    },
    "Verbs": {
        "verb_0": {
            "Sentence_ID": 0,
            "Index": [
                2,
                4
            ]
        },
        "verb_1": {
            "Sentence_ID": 1,
            "Index": [
                40,
                44
            ]
        },
        ......
    },
    "Coreference_Clusters": {
        "Cluster_0": [
            "verb_0",
            "verb_12"
        ],
        "Cluster_1": [
            "verb_1",
            "verb_13"
        ],
        "Cluster_2": [
            "verb_2"
        ],
        ......
    },
    "Role-Contain": {
        "Cluster_5": [
            "Cluster_19",
            "Cluster_21",
            "Cluster_38"
        ],
        "Cluster_20": [
            "Cluster_51"
        ],
        "Cluster_21": [
            "Cluster_52"
        ],
        ......
    },
    "Meaning-Contain": {
        "Cluster_6": [
            "Cluster_20",
        ],
        "Cluster_22": [
            "Cluster_31"
        ]
        ......
    },
    "Role-Change": [
        ["Cluster_10", "Cluster_12"], 
        ["Cluster_14", "Cluster_42"], 
        ......
    ]
}
```

以上给出其中一篇数据样例，字段含义为：

| 字段                 | 解释                                                         |
| -------------------- | :----------------------------------------------------------- |
| Doc_ID               | 该篇目在《人民日报》标注语料库中的篇目编号<br />             |
| Sentences            | key：段落顺序号<br />value：该段落在《人民日报》标注语料库中的编号<br />使用者需预先获取[人民日报标注语料库](https://klcl.pku.edu.cn/gxzy/231686.htm)原始数据，按照编号获取段落文本、删除词性标签，并用其替换"字符串"19980101-xx-xxx-xxx" |
| Verbs                | 所提取的动词字典<br />verb_x：该动词在文档内的编号<br />Sentence_ID：该动词所属段落<br />Index：该动词在所属段落中的索引 |
| Coreference_Clusters | 共指链<br />Cluster_x：共指链编号<br />共指链以列表形式存储，共指链内动词以verb_x记录 |
| Role-Contain         | 如上例所示，Cluster_5共指链内动词角色包含Cluster_19共指链内动词 |
| Meaning-Contain      | 如上例所示，Cluster_6共指链内动词词义包含Cluster_20共指链内动词 |
| Role-Change          | 如上例所示，共指链Cluster_10内动词与共指链Cluster_12内动词为角色变更关系 |

