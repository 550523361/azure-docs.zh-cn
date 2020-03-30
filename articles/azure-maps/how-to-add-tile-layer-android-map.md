---
title: 向 Android 地图添加切片图层 |微软 Azure 地图
description: 在本文中，您将学习如何使用 Microsoft Azure 地图 Android SDK 在地图上呈现切片图层。
author: philmea
ms.author: philmea
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: f98598bd1307bb1b46ff23814780c5f809b9ac90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335568"
---
# <a name="add-a-tile-layer-to-a-map-using-the-azure-maps-android-sdk"></a>使用 Azure 地图 Android SDK 将切片图层添加到地图

本文演示如何使用 Azure 地图 Android SDK 在地图上呈现切片图层。 通过图块层可以在 Azure Maps 基本地图图块顶部附加图像。 可在[缩放级别和图块网格](zoom-levels-and-tile-grid.md)文档中找到有关 Azure Maps 图块系统的详细信息。

磁贴图层从服务器加载切片。 这些图像可以像服务器上的任何其他图像一样，使用切片层可以理解的命名约定进行预渲染和存储。 或者，这些图像可以使用动态服务进行渲染，该服务可近乎实时地生成图像。 Azure 地图切片层类支持三种不同的切片服务命名约定：

* X、Y、缩放表示法 - 基于缩放级别，x 是列，y 是图块网格中图块的行位置。
* Quadkey 表示法 - 将 x、y、缩放信息合并到单个字符串值（即图块的唯一标识符）中。
* 边界框 - 边界框坐标可用于指定格式为 `{west},{south},{east},{north}` 的图像，这通常由 [Web 地图定位服务 (WMS)](https://www.opengeospatial.org/standards/wms) 使用。

> [!TIP]
> TileLayer 是直观显示地图上的大型数据集的好办法。 不仅可以从图像中生成图块层，而且还可以将矢量数据呈现为图块层。 通过将矢量数据呈现为图块层，地图控件只需加载文件大小远小于它们所代表的矢量数据的图块。 此方法可供需要呈现地图上的数百万行数据的用户使用。

传递到图块层中的图块 URL 必须是 TileJSON 资源的 http/https URL 或使用以下参数的图块 URL 模板： 

* `{x}` - 图块的 X 位置。 还需要 `{y}` 和 `{z}`。
* `{y}` - 图块的 Y 位置。 还需要 `{x}` 和 `{z}`。
* `{z}` - 图块的缩放级别。 还需要 `{x}` 和 `{y}`。
* `{quadkey}` - 基于必应地图图块系统命名约定的图块 quadkey 标识符。
* `{bbox-epsg-3857}` - EPSG 3857 空间引用系统中格式为 `{west},{south},{east},{north}` 的边界框字符串。
* `{subdomain}`- 如果指定子域值，则子域值的占位符。

## <a name="prerequisites"></a>先决条件

要完成本文中的过程，您需要安装[Azure 地图 Android SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library)来加载地图。


## <a name="add-a-tile-layer-to-the-map"></a>向地图添加切片图层

 此示例演示如何创建指向一组切片的切片图层。 这些切片使用"x、y、缩放"切片系统。 此图块层源自[爱荷华州立大学的 Iowa Environmental Mesonet](https://mesonet.agron.iastate.edu/ogc/) 的气象雷达图覆盖。 

您可以按照以下步骤向地图添加切片图层。

1. **>activity_main.xml 编辑>布局**，以便它看起来像下面的布局：

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
            app:mapcontrol_centerLat="40.75"
            app:mapcontrol_centerLng="-99.47"
            app:mapcontrol_zoom="3"
            />
    
    </FrameLayout>
    ```

2. 将以下代码段复制到类`MainActivity.java`的**onCreate（）** 方法中。

    ```Java
    mapControl.onReady(map -> {
        //Add a tile layer to the map, below the map labels.
        map.layers.add(new TileLayer(
            tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
            opacity(0.8f),
            tileSize(256)
        ), "labels");
    });
    ```
    
    上面的代码段首先使用**onReady（）** 回调方法获取 Azure 地图映射控件实例。 然后，它创建`TileLayer`一个对象，并将格式化的**xyz**磁贴`tileUrl`URL 传递到该选项中。 图层的不极性设置为`0.8`，并且由于正在使用的磁贴服务的切片为 256 像素切片，因此此信息将传递到选项中。 `tileSize` 然后，切片图层将传递到地图图层管理器中。

    添加上面的代码段后，应`MainActivity.java`如下所示：
    
    ```Java
    package com.example.myapplication;

    import android.app.Activity;
    import android.os.Bundle;
    import android.support.v7.app.AppCompatActivity;
    import com.microsoft.azure.maps.mapcontrol.layer.TileLayer;
    import java.util.Arrays;
    import java.util.List;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import static com.microsoft.azure.maps.mapcontrol.options.TileLayerOptions.tileSize;
    import static com.microsoft.azure.maps.mapcontrol.options.TileLayerOptions.tileUrl;
        
    public class MainActivity extends AppCompatActivity {
    
        static{
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
    
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {

                //Add a tile layer to the map, below the map labels.
                map.layers.add(new TileLayer(
                    tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
                    opacity(0.8f),
                    tileSize(256)
                ), "labels");
            });    
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }    
    }
    ```

如果现在运行应用程序，您应该在地图上看到一条线，如下所示：

<center>

![安卓地图线](./media/how-to-add-tile-layer-android-map/xyz-tile-layer-android.png)</center>

## <a name="next-steps"></a>后续步骤

请参阅以下文章，了解有关设置地图样式的方法

> [!div class="nextstepaction"]
> [更改 Android 地图中的地图样式](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)