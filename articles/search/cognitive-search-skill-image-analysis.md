---
title: 图像分析认知技能
titleSuffix: Azure Cognitive Search
description: 在 Azure 认知搜索中的 AI 扩充管道中，使用 ImageAnalysis 认知技能通过图像分析来提取语义文本。
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: d535866881fa6ed73b51eb6039baa9d515b770b2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85080832"
---
# <a name="image-analysis-cognitive-skill"></a>图像分析认知技能

图像分析技能根据图像内容提取一组丰富的可视特征  。 例如，可从图像生成标题栏、生成标记或识别名人和地标。 此技能使用认知服务中的[计算机视觉](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)提供的机器学习模型。 

> [!NOTE]
> 如果事务量较少（不到 20 个事务），则这些事务可以在 Azure 认知搜索中免费执行，但较大的工作负荷需要[附加可计费认知服务资源](cognitive-search-attach-cognitive-services.md)。 调用认知服务中的 API 以及在 Azure 认知搜索中的文档破解阶段提取图像时，会产生费用。 提取文档中的文本不会产生费用。
>
> 内置技能执行按现有[认知服务即用即付价格](https://azure.microsoft.com/pricing/details/cognitive-services/)计费。 图像提取定价如 [Azure 认知搜索定价页](https://azure.microsoft.com/pricing/details/search/)所述。


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>技能参数

参数区分大小写。

| 参数名称     | 说明 |
|--------------------|-------------|
| `defaultLanguageCode` |  表示要返回的语言的字符串。 该服务以指定的语言返回识别结果。 如果未指定此属性，则默认值为“en”。 <br/><br/>支持的语言为： <br/>en - 英语（默认）** <br/> *es* -西班牙语 <br/> *ja* -日语 <br/> *pt* -葡萄牙语 <br/> zh - 简体中文**|
| `visualFeatures` |    表示要返回的可视特征类型的一组字符串。 有效的可视特征类型包括：  <ul><li>*成人*-检测图像在本质上是色情（表现为裸体或性爱法案），还是底层（描述极端暴力或血糖）。 还会检测到色情的暗示内容（也称为猥亵内容）。</li><li>*品牌*-检测图像中的各种品牌，包括近似位置。 *品牌*视觉功能仅提供英语版本。</li><li> *categories* - 根据认知服务[计算机视觉文档](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy)中定义的分类对图像内容进行分类。 </li><li>*说明*-以支持的语言提供完整的句子的图像内容。</li><li>人*脸*-检测是否存在面部。 如果存在，则生成坐标、性别和年龄。</li><li>   *对象*-检测图像中的各种对象，包括近似位置。 *对象视觉对象*功能仅以英语提供。</li><li> tags - 使用与图像内容相关字词的详细列表来标记图像**。</li></ul> 可视特征的名称区分大小写。 请注意， *color*和*imageType*视觉功能已弃用，但仍可通过[自定义技能](https://docs.microsoft.com/azure/search/cognitive-search-custom-skill-interface)访问此功能。|
| `details` | 表示要返回的特定于域的详细信息的一组字符串。 有效的可视特征类型包括： <ul><li>*名人*-识别映像中检测到的名人。</li><li>*特征点*-识别映像中检测到的特征点。 </li></ul> |

## <a name="skill-inputs"></a>技能输入

| 输入名称      | 说明                                          |
|---------------|------------------------------------------------------|
| `image`         | 复杂类型。 当前仅适用于“/document/normalized_images”字段，当 ```imageAction``` 设置为非 ```none``` 值时由 Azure Blob 索引器生成。 请参阅[此示例](#sample-output)获取详细信息。|



##  <a name="sample-skill-definition"></a>示例技能定义

```json
        {
            "description": "Extract image analysis.",
            "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
            "context": "/document/normalized_images/*",
            "defaultLanguageCode": "en",
            "visualFeatures": [
                "tags",
                "categories",
                "description",
                "faces",
                "brands"
            ],
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                }
            ],
            "outputs": [
                {
                    "name": "categories"
                },
                {
                    "name": "tags"
                },
                {
                    "name": "description"
                },
                {
                    "name": "faces"
                },
                {
                    "name": "brands"
                }
            ]
        }
```
### <a name="sample-index-for-only-the-categories-description-faces-and-tags-fields"></a>示例索引（仅适用于类别、说明、人脸和标记字段）
```json
{
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": true,
            "searchable": true,
            "filterable": false,
            "facetable": false,
            "sortable": true
        },
        {
            "name": "blob_uri",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "facetable": false,
            "sortable": true
        },
        {
            "name": "content",
            "type": "Edm.String",
            "sortable": false,
            "searchable": true,
            "filterable": false,
            "facetable": false
        },
        {
            "name": "categories",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "name",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "score",
                    "type": "Edm.Double",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "detail",
                    "type": "Edm.ComplexType",
                    "fields": [
                        {
                            "name": "celebrities",
                            "type": "Collection(Edm.ComplexType)",
                            "fields": [
                                {
                                    "name": "name",
                                    "type": "Edm.String",
                                    "searchable": true,
                                    "filterable": false,
                                    "facetable": false
                                },
                                {
                                    "name": "faceBoundingBox",
                                    "type": "Collection(Edm.ComplexType)",
                                    "fields": [
                                        {
                                            "name": "x",
                                            "type": "Edm.Int32",
                                            "searchable": false,
                                            "filterable": false,
                                            "facetable": false
                                        },
                                        {
                                            "name": "y",
                                            "type": "Edm.Int32",
                                            "searchable": false,
                                            "filterable": false,
                                            "facetable": false
                                        }
                                    ]
                                },
                                {
                                    "name": "confidence",
                                    "type": "Edm.Double",
                                    "searchable": false,
                                    "filterable": false,
                                    "facetable": false
                                }
                            ]
                        },
                        {
                            "name": "landmarks",
                            "type": "Collection(Edm.ComplexType)",
                            "fields": [
                                {
                                    "name": "name",
                                    "type": "Edm.String",
                                    "searchable": true,
                                    "filterable": false,
                                    "facetable": false
                                },
                                {
                                    "name": "confidence",
                                    "type": "Edm.Double",
                                    "searchable": false,
                                    "filterable": false,
                                    "facetable": false
                                }
                            ]
                        }
                    ]
                }
            ]
        },
        {
            "name": "description",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "tags",
                    "type": "Collection(Edm.String)",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "captions",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "text",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": false,
                            "facetable": false
                        },
                        {
                            "name": "confidence",
                            "type": "Edm.Double",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        }
                    ]
                }
            ]
        },
        {
            "name": "faces",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "age",
                    "type": "Edm.Int32",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "gender",
                    "type": "Edm.String",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "faceBoundingBox",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "x",
                            "type": "Edm.Int32",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        },
                        {
                            "name": "y",
                            "type": "Edm.Int32",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        }
                    ]
                }
            ]
        },
        {
            "name": "tags",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "name",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "confidence",
                    "type": "Edm.Double",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                }
            ]
        }
    ]
}

```
### <a name="sample-output-field-mapping-for-the-above-index"></a>示例输出字段映射（适用于上述索引）
```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/normalized_images/*/categories/*",
            "targetFieldName": "categories"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/tags/*",
            "targetFieldName": "tags"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/description",
            "targetFieldName": "description"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/faces/*",
            "targetFieldName": "faces"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/brands/*/name",
            "targetFieldName": "brands"
        }
```
### <a name="variation-on-output-field-mappings-nested-properties"></a>输出字段映射的变体（嵌套属性）

可以将输出字段映射定义为较低级别的属性，例如仅地标或名人。 在这种情况下，请确保索引架构有一个字段专门包含地标。

```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/normalized_images/*/categories/detail/celebrities/*",
            "targetFieldName": "celebrities"
        }
```
##  <a name="sample-input"></a>示例输入

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "image": {
                    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
                    "width": 500,
                    "height": 300,
                    "originalWidth": 5000,
                    "originalHeight": 3000,
                    "rotationFromOriginal": 90,
                    "contentOffset": 500,
                    "pageNumber": 2
                }
            }
        }
    ]
}
```

##  <a name="sample-output"></a>示例输出

```json
{
  "values": [
    {
      "recordId": "1",
      "data": {
        "categories": [
          {
            "name": "abstract_",
            "score": 0.00390625
          },
          {
            "name": "people_",
            "score": 0.83984375,
            "detail": {
              "celebrities": [
                {
                  "name": "Satya Nadella",
                  "faceBoundingBox": [
                        {
                            "x": 273,
                            "y": 309
                        },
                        {
                            "x": 395,
                            "y": 309
                        },
                        {
                            "x": 395,
                            "y": 431
                        },
                        {
                            "x": 273,
                            "y": 431
                        }
                    ],
                  "confidence": 0.999028444
                }
              ],
              "landmarks": [
                {
                  "name": "Forbidden City",
                  "confidence": 0.9978346
                }
              ]
            }
          }
        ],
        "adult": {
          "isAdultContent": false,
          "isRacyContent": false,
          "isGoryContent": false,
          "adultScore": 0.0934349000453949,
          "racyScore": 0.068613491952419281,
          "goreScore": 0.08928389008070282
        },
        "tags": [
          {
            "name": "person",
            "confidence": 0.98979085683822632
          },
          {
            "name": "man",
            "confidence": 0.94493889808654785
          },
          {
            "name": "outdoor",
            "confidence": 0.938492476940155
          },
          {
            "name": "window",
            "confidence": 0.89513939619064331
          }
        ],
        "description": {
          "tags": [
            "person",
            "man",
            "outdoor",
            "window",
            "glasses"
          ],
          "captions": [
            {
              "text": "Satya Nadella sitting on a bench",
              "confidence": 0.48293603002174407
            }
          ]
        },
        "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
        "metadata": {
          "width": 1500,
          "height": 1000,
          "format": "Jpeg"
        },
        "faces": [
          {
            "age": 44,
            "gender": "Male",
            "faceBoundingBox": [
                {
                    "x": 1601,
                    "y": 395
                },
                {
                    "x": 1653,
                    "y": 395
                },
                {
                    "x": 1653,
                    "y": 447
                },
                {
                    "x": 1601,
                    "y": 447
                }
            ]
          }
        ],
        "objects": [
          {
            "rectangle": {
              "x": 25,
              "y": 43,
              "w": 172,
              "h": 140
            },
            "object": "person",
            "confidence": 0.931
          }
        ],
        "brands":[  
           {  
              "name":"Microsoft",
              "confidence": 0.903,
              "rectangle":{  
                 "x":20,
                 "y":97,
                 "w":62,
                 "h":52
              }
           }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>错误案例
在以下错误案例中，未提取任何元素。

| 错误代码 | 描述 |
|------------|-------------|
| `NotSupportedLanguage` | 不支持提供的语言。 |
| `InvalidImageUrl` | 图片 URL 格式不正确或无法访问。|
| `InvalidImageFormat` | 输入数据不是有效的图像。 |
| `InvalidImageSize` | 输入的图像太大。 |
| `NotSupportedVisualFeature`  | 指定的特征类型无效。 |
| `NotSupportedImage` | 不受支持的图片，例如儿童色情内容。 |
| `InvalidDetails` | 不受支持的特定于域的模型。 |

如果收到类似于 `"One or more skills are invalid. Details: Error in skill #<num>: Outputs are not supported by skill: Landmarks"` 的错误，请检查路径。 名人和地标都是 `detail` 下的属性。

```json
"categories":[  
      {  
         "name":"building_",
         "score":0.97265625,
         "detail":{  
            "landmarks":[  
               {  
                  "name":"Forbidden City",
                  "confidence":0.92013400793075562
               }
            ]
```

## <a name="see-also"></a>另请参阅

+ [内置技能](cognitive-search-predefined-skills.md)
+ [如何定义技能集](cognitive-search-defining-skillset.md)
+ [创建索引器 (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
