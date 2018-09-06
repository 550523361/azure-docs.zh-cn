---
title: 使用 Azure Maps 添加图钉 | Microsoft Docs
description: 如何向 Javascript 地图添加图钉
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 0dafb09e1704e8e446b034975f0c25a740050599
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382578"
---
# <a name="add-pins-to-the-map"></a>向地图添加图钉

本文展示了如何向地图添加图钉。  

## <a name="understand-the-code"></a>了解代码

<iframe height='500' scrolling='no' title='向地图添加图钉' src='//codepen.io/azuremaps/embed/ZrVpEa/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅 <a href='https://codepen.io'>CodePen</a> 上由 Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) 提供的 Pen <a href='https://codepen.io/azuremaps/pen/ZrVpEa/'>Add a pin to a map</a>（向地图添加图钉）。
</iframe>

在上面的代码中，第一个代码块构造地图对象。 有关说明，可以参阅[创建地图](./map-create.md)。

第二个代码块将创建一个图钉并将其添加到地图。 图钉是 [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) 的一个 [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest)，使用 [PinProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinproperties?view=azure-iot-typescript-latest) 作为其 Feature 属性。 可使用 `new atlas.data.Feature(new atlas.data.Point())` 创建图钉并定义其属性。 图钉层是一个图钉数组。 可使用 map 类的 [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins) 函数向地图添加图钉层并定义图钉层的属性。 请参阅 [PinLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinlayeroptions?view=azure-iot-typescript-latest) 中介绍的图钉层属性。 

## <a name="next-steps"></a>后续步骤

详细了解本文中使用的类和方法： 
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
有关可向地图添加的更多代码示例，请参阅以下文章： 
* [添加弹出项](./map-add-popup.md)
* [添加形状](./map-add-shape.md)

