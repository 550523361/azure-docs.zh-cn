---
title: 添加实体 - LUIS
description: 创建实体，以便从语言理解 (LUIS) 应用的用户话语中提取关键数据。 提取的实体数据由客户端应用程序用来 fullfil 客户请求。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: diberry
ms.openlocfilehash: 8be5dac2d1d5e8a5a3ceafff6b51b2d89e03993f
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83593269"
---
# <a name="add-entities-to-extract-data"></a>添加实体以提取数据

创建实体，以便从语言理解 (LUIS) 应用的用户话语中提取关键数据。 你的客户端应用程序使用提取的实体数据来 fullfil 客户请求。

实体表示要提取的话语中的字词或短语。 实体描述与意向相关的信息，它们有时对于应用执行任务至关重要。 在将示例查询文本添加到意向或从（之前或之后）将示例查询文本添加到目的时，可以创建实体。

## <a name="plan-entities-then-create-and-label"></a>规划实体，然后创建并标记

可以从示例最谈话创建计算机学习的实体，也可以从 "**实体**" 页创建。

通常，最佳做法是在门户中创建计算机学习实体之前，花费时间规划实体。 然后，从示例查询文本创建计算机学习的实体，其中详细介绍了子实体和你当时知道的功能。 [可以分解实体教程](tutorial-machine-learned-entity.md)演示了如何使用此方法。

在规划实体的过程中，您可能会知道您需要文本匹配实体（例如预生成实体、正则表达式实体或列表实体）。 你可以从 "**实体**" 页创建它们，然后将它们标记为示例最谈话。

进行标记时，可以标记各个实体，然后生成到父计算机学习的实体。 或者，你可以从一个由计算机学习的父实体开始，然后分解为子实体。

> [!TIP]
>标记可能指示实体的所有单词，即使在客户端应用程序中提取的单词不会使用。

## <a name="creating-an-entity-before-or-with-labeling"></a>在标记之前创建实体

使用下表来了解将每个实体创建或添加到应用中的实体。

|实体类型|在 LUIS 门户中的何处创建实体|
|--|--|
|机器学习实体|实体或意向详细信息|
|列表实体|实体或意向详细信息|
|正则表达式实体|实体|
|Pattern.any 实体|实体|
|预生成实体|实体|
|预生成的域实体|实体|

您可以从 "**实体**" 页创建所有实体，也可以在 "**目的详细信息**" 页上的示例查询文本中，创建一些实体作为标签实体的一部分。 您只能在 "**目的详细信息**" 页的示例查询文本中_标记_实体。

## <a name="create-a-machine-learned-entity"></a>创建机器学习的实体

[!INCLUDE [Create and label entities in machine-learned tutorial](includes/decomposable-tutorial-links.md)]

## <a name="create-a-text-matching-entity"></a>创建文本匹配实体

使用文本匹配实体提供多种方法来提取数据：

|文本匹配实体|目标|
|--|--|
|[列表实体](reference-entity-list.md)|规范名称和同义词作为替代形式的列表|
|正则表达式实体|使用正则表达式实体匹配文本|
|[预生成实体](luis-reference-prebuilt-entities.md)|匹配常见数据类型，如数字、电子邮件、日期|
|预生成的域实体|使用选定的使用者域匹配|
|[Pattern.any](reference-entity-pattern-any.md)| 匹配可能与周围文本容易混淆的实体|

预生成的实体无需提供任何自定义训练数据即可工作。 其他实体需要你提供客户定型数据（如列出实体的项）或表达式（如正则表达式或模式）。

<a name="add-list-entities"></a>

### <a name="how-to-create-a-new-custom-entity"></a>如何创建新的自定义实体

1. 登录到[LUIS 门户](https://www.luis.ai)，选择**订阅**和**创作资源**，查看分配给该创作资源的应用。
1. 通过在 **"我的应用**" 页上选择应用程序的名称来打开应用。
1. 选择 "**实体**" 页。
1. 选择 " **+ 创建**"，然后选择实体类型。
1. 继续配置实体，并在完成后选择 "**创建**"。

### <a name="add-list-entities-for-exact-matches"></a>添加列表实体以进行完全匹配

列表实体表示一组固定、封闭的相关单词。 当您作为作者，可以更改列表时，LUIS 不会扩大或缩小列表。 你还可以使用[列表的 json 格式](reference-entity-list.md#example-json-to-import-into-list-entity)导入到现有的列表实体。

下面的列表演示规范名称和同义词。

|颜色列表项名称|颜色-同义词|
|--|--|
|Red|crimson、血、apple、消防引擎|
|蓝色|天空，azure，钴|
|绿色|王，酸橙色|

使用过程来创建一个列表实体。 创建列表实体后，不需要在意向中标记示例最谈话。 使用精确文本匹配列表项和同义词。
1. 登录到[LUIS 门户](https://www.luis.ai)，选择**订阅**和**创作资源**，查看分配给该创作资源的应用。
1. 通过在 **"我的应用**" 页上选择应用程序的名称来打开应用。
1. 从 "**生成**" 部分的左侧面板中选择 "**实体**"，然后选择 " **+ 创建**"。

1. 在 "**创建实体类型**" 对话框中，输入实体的名称，如 `Colors` 和 "选择**列表**"。
1. 在 "**创建列表实体**" 对话框中的 "**添加新**子列表 ..." 中，输入列表项名称（如 `Green` ），然后添加同义词。

    > [!div class="mx-imgBorder"]
    > ![在实体详细信息页中创建颜色列表作为列表实体。](media/how-to-add-entities/create-list-entity-of-colors.png)

1. 添加完列表项和同义词后，选择 "**创建**"。

    对应用进行一组更改后，请记住**训练**应用。 请不要在一次更改后对应用进行训练。

    > [!NOTE]
    > 此过程演示如何从 "**意向详细信息**" 页中的示例查询文本创建并标记列表实体。 你还可以从 "**实体**" 页创建相同的实体。

## <a name="add-a-role-for-an-entity"></a>为实体添加角色

角色是基于上下文的实体的命名子类型。

### <a name="add-a-role-to-distinguish-different-contexts"></a>添加角色以区分不同的上下文

在下面的查询文本中，有两个位置，每个位置都由围绕它的单词（例如和）在语义上指定 `to` `from` ：

`Pick up the package from Seattle and deliver to New York City.`

在此过程中，将 `origin` 和 `destination` 角色添加到预生成的 geographyV2 实体。
1. 登录到[LUIS 门户](https://www.luis.ai)，选择**订阅**和**创作资源**，查看分配给该创作资源的应用。
1. 通过在 **"我的应用**" 页上选择应用程序的名称来打开应用。
1. 在“生成”部分的左侧面板中选择“实体”********。

1. 选择 " **+ 添加预生成实体**"。 选择**geographyV2** ，然后选择 "**完成**"。 这会将预生成的实体添加到应用。

    如果发现模式在包含 Pattern.any 时错误提取实体，请使用[显式列表](reference-pattern-syntax.md#explicit-lists)来更正此问题。

1. 从实体的 "**实体**" 页列表中选择新添加的 "预生成的 geographyV2" 实体。
1. 若要添加新的角色，请选择 " **+** 下一步"，**不添加任何角色**。
1. 在 "**类型角色 ...** " 文本框中，输入角色的名称， `Origin` 然后输入。 添加第二个角色名称， `Destination` 然后输入。

    > [!div class="mx-imgBorder"]
    > ![将 Origin 角色添加到 Location 实体的屏幕截图](media/how-to-add-entities//add-role-to-prebuilt-geographyv2-entity.png)

    该角色将添加到预生成的实体中，但不会添加到使用该实体的任何最谈话。

### <a name="label-text-with-a-role-in-an-example-utterance"></a>使用查询文本中的角色标签文本
1. 登录到[LUIS 门户](https://www.luis.ai)，选择**订阅**和**创作资源**，查看分配给该创作资源的应用。
1. 通过在 **"我的应用**" 页上选择应用程序的名称来打开应用。
1. 请参阅意向详细信息页，其中包含使用角色的示例最谈话。
1. 若要用角色标记，请选择示例查询文本中的实体标签（文本下的实线），然后从下拉列表中选择 "从**实体调色板查看**"。

    > [!div class="mx-imgBorder"]
    > ![在实体调色板中选择视图的屏幕截图](media/how-to-add-entities/select-text-label-with-entity-palette-for-role.png)

    将在右侧打开实体调色板。

1. 选择实体，然后前往调色板底部，选择 "角色"。

    > [!div class="mx-imgBorder"]
    > ![在实体调色板中选择视图的屏幕截图](media/how-to-add-entities/select-role-from-entity-palette-entity-inspector.png)

<a name="add-pattern-any-entities"></a>

## <a name="add-a-patternany-entity"></a>添加 pattern.any 实体

[模式。任何](luis-concept-entity-types.md)实体仅适用于[模式](luis-how-to-model-intent-pattern.md)，而不是意向的示例最谈话。 此类实体有助于 LUIS 查找不同长度和字词选择的实体结尾。 由于此实体会在模式中使用，因此，LUIS 可识别实体末尾在话语模板中的位置。

### <a name="steps-to-create-a-patternany-entity"></a>创建模式的步骤。任何实体
1. 登录到[LUIS 门户](https://www.luis.ai)，选择**订阅**和**创作资源**，查看分配给该创作资源的应用。
1. 通过在 **"我的应用**" 页上选择应用程序的名称来打开应用。
1. 从 "**生成**" 部分的左侧面板中选择 "**实体**"，然后选择 " **+ 创建**"。

1. 在 "**选择实体类型**" 对话框中，在 "**名称**" 框中输入实体名称，然后选择 "**模式"。任何**作为**类型**，然后选择 "**创建**"。

    使用此实体[创建模式查询文本](luis-how-to-model-intent-pattern.md)后，将使用组合的计算机学习和文本匹配算法提取实体。

### <a name="create-a-pattern-template-utterance-to-use-patternany-entity"></a>创建模式模板查询文本，以使用模式。任何实体

若要使用 pattern.any 实体，请在“模式”页的“提升应用性能”部分中，通过正确的大括号语法（例如 `Where is **{HumanResourcesFormTitle}** on the server?`）添加模式********。

如果发现模式在包含 Pattern.any 时错误提取实体，请使用[显式列表](reference-pattern-syntax.md#explicit-lists)来更正此问题。

## <a name="do-not-change-entity-type"></a>不要更改实体类型

LUIS 不允许更改实体类型，因为它不知道构造该实体要添加或删除的内容。 若要更改类型，最好创建一个名称稍微不同的、类型正确的新实体。 实体创建后，在每句话语中，删除旧标记实体名称并添加新的实体名称。 重新标记所有话语后，即可删除旧实体。

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用预构建的模型](howto-add-prebuilt-models.md)

了解详细信息：
* 如何[定型](luis-how-to-train.md)
* 如何[测试](luis-interactive-test.md)
* 如何[发布](luis-how-to-publish-app.md)
* 模式
    * [概念](luis-concept-patterns.md)
    * [语法](reference-pattern-syntax.md)
* [预建实体 GitHub 存储库](https://github.com/Microsoft/Recognizers-Text)
* [数据提取概念](luis-concept-data-extraction.md)



