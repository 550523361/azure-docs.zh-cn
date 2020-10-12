---
title: 请求实时公共传输数据 |Microsoft Azure 映射
description: 了解如何在传输停止时请求实时公共传输数据，如抵达。 请参阅如何使用 Azure Maps 移动服务实现此目的。
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/06/2019
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 6f0cf663b42c8487495602e4cdbf1a88427f9daf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91310928"
---
# <a name="request-real-time-public-transit-data-using-the-azure-maps-mobility-service"></a>使用 Azure Maps 移动服务请求实时公共传输数据

本文介绍如何使用 Azure Maps [移动服务](https://aka.ms/AzureMapsMobilityService) 来请求实时公共传输数据。

在本文中，你将学习如何请求到达给定停止处的所有行的下一个实时到达

## <a name="prerequisites"></a>必备条件

首先需要 Azure Maps 帐户和订阅密钥才能对 Azure Maps 公用传输 Api 进行任何调用。 有关信息，请按照 [创建帐户](quick-demo-map-app.md#create-an-azure-maps-account) 创建 Azure Maps 帐户中的说明进行操作。 按照 [获取主密钥](quick-demo-map-app.md#get-the-primary-key-for-your-account) 中的步骤获取帐户的主密钥。 有关 Azure Maps 中身份验证的详细信息，请参阅[在 Azure Maps 中管理身份验证](./how-to-manage-authentication.md)。

本文使用 [Postman 应用](https://www.getpostman.com/apps)来构建 REST 调用。 可以使用你喜欢的任何 API 开发环境。

## <a name="request-real-time-arrivals-for-a-stop"></a>请求停止的实时到达

为了请求特定公用传输停止的实时到达数据，需要向 Azure Maps[移动服务](https://aka.ms/AzureMapsMobilityService)的[实时到达 API](https://aka.ms/AzureMapsMobilityRealTimeArrivals)发出请求，。 需要 **metroID** 和 **stopID** 来完成请求。 若要详细了解如何请求这些参数，请参阅有关如何 [请求公用传输路由](https://aka.ms/AMapsHowToGuidePublicTransitRouting)的指南。

让我们使用 "522" 作为地铁 ID，这是 "西雅图– Tacoma – Bellevue，WA" 领域的地铁 ID。 使用 "522---2060603" 作为 "stop ID"，此总线将停止在 "Ne 24 日 St & 162nd，Bellevue WA"。 若要请求接下来的五个实时到达数据，则在此停止时，请完成以下步骤：

1. 打开 Postman 应用，让我们创建一个集合来存储请求。 在 Postman 应用顶部附近，选择“新建”。 在“新建”窗口中，选择“集合”。  命名集合，然后选择“创建”按钮。

2. 若要创建请求，请再次选择“新建”。 在“新建”窗口中，选择“请求”。 在“请求名称”中，输入请求名称。 选择你在上一步中创建的集合，作为要保存请求的位置。 然后选择“保存”。

    ![在 Postman 中创建请求](./media/how-to-request-transit-data/postman-new.png)

3. 在 "生成器" 选项卡上选择 " **获取** HTTP" 方法，然后输入以下 URL 创建 GET 请求。 `{subscription-key}`将替换为 Azure Maps 的主键。

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

4. 成功请求后，会收到以下响应。  请注意，参数 "scheduleType" 定义了预计的到达时间是基于实时数据还是静态数据。

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```

## <a name="next-steps"></a>后续步骤

了解如何使用移动服务请求传输数据：

> [!div class="nextstepaction"]
> [如何请求传输数据](how-to-request-transit-data.md)

浏览 Azure Maps 移动服务 API 文档：

> [!div class="nextstepaction"]
> [移动服务 API 文档](https://aka.ms/AzureMapsMobilityService)
