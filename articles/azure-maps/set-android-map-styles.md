---
title: 使用 Azure Maps Android SDK 设置地图样式 |Microsoft Azure 映射
description: 了解设置地图样式的两种方法。 请参阅如何使用布局文件或活动类中的 Azure Maps Android SDK 来调整样式。
author: anastasia-ms
ms.author: v-stharr
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 15dbe7d30652d0ace78bca4dc053757d57361c1a
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92895301"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>使用 Azure Maps Android SDK 设置地图样式

本文介绍两种使用 Azure Maps Android SDK 设置地图样式的方法。 Azure Maps 有六个不同的地图样式可供选择。 有关支持的地图样式的详细信息，请参阅 [Azure Maps 中支持的地图样式](./supported-map-styles.md)。


## <a name="prerequisites"></a>先决条件

若要完成本文中的过程，需要安装 [Azure Maps Android SDK](./how-to-use-android-map-control-library.md) 来加载地图。


## <a name="set-map-style-in-the-layout"></a>在布局中设置地图样式

您可以在活动类的布局文件中设置地图样式。 编辑 **res > 布局 > activity_main.xml** ，因此如下所示：

```XML
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

`mapcontrol_style`上述属性将地图样式设置为 **grayscale_dark** 。 

<center>

![样式-grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>在活动类中设置地图样式

可以在 activity 类中设置地图样式。 将以下代码片段复制到类的 **onCreate ( # B1** 方法 `MainActivity.java` 。 此代码会将地图样式设置为 **satellite_road_labels** 。

```Java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![样式-卫星道路标签](./media/set-android-map-styles/satellite-road-labels.png)</center>