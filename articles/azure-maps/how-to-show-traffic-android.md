---
title: 在 Android 地图上显示流量数据 |微软 Azure 地图
description: 在本文中，您将了解如何使用 Microsoft Azure 地图 Android SDK 在地图上显示流量数据。
author: philmea
ms.author: philmea
ms.date: 02/27/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: e5611eeb08ac370e12cf452d57a87e449fbd80da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335378"
---
# <a name="show-traffic-data-on-the-map-using-azure-maps-android-sdk"></a>使用 Azure 地图 Android SDK 在地图上显示流量数据

流数据和事件数据是可在地图上显示的两种类型的流量数据。 本指南介绍如何显示这两种类型的流量数据。 事件数据包括建筑、道路封闭和事故等基于点和线的数据。 流量数据显示有关道路上交通流量的指标。

## <a name="prerequisites"></a>先决条件

在地图上显示流量之前，需要[创建 Azure 帐户](quick-demo-map-app.md#create-an-account-with-azure-maps)[并获取订阅密钥](quick-demo-map-app.md#get-the-primary-key-for-your-account)。 然后，您需要安装 Azure[映射 Android SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library)并加载地图。

## <a name="incidents-traffic-data"></a>事件流量数据 

您需要导入以下库才能调用`setTraffic`和`incidents`：

```java
import static com.microsoft.com.azure.maps.mapcontrol.options.TrafficOptions.incidents;
```

 以下代码段演示如何在地图上显示流量数据。 我们将布尔值传递给`incidents`方法，并将其传递给`setTraffic`方法。 

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mapControl.getMapAsync(map - > {
        map.setTraffic(incidents(true));
    }
}
```

## <a name="flow-traffic-data"></a>流量数据

您首先需要导入以下库来调用`setTraffic`和`flow`：

```java
import com.microsoft.azure.maps.mapcontrol.options.TrafficFlow;
import static com.microsoft.azure.maps.mapcontrol.options.TrafficOptions.flow;
```

使用以下代码段设置流量流数据。 与上一节中的代码类似，我们将`flow`方法的返回值传递给 方法。 `setTraffic` 有四个值可以传递给`flow`，每个值将触发`flow`以返回相应的值。 然后，返回`flow`值将作为 参数传递给`setTraffic`。 有关以下四个值，请参阅下表：

| | |
| :-- | :-- |
| 流量流.无 | 地图上不显示流量数据 |
| 流量流. | 显示与道路自由流动速度相关的交通数据 |
| 流量流.RELATIVE_DELAY | 显示低于平均预期延迟的区域 |
| 流量。 | 显示道路上所有车辆的绝对速度 |

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mapControl.getMapAsync(map -> {
        map.setTraffic(flow(TrafficFlow.RELATIVE)));
    }
}
```

## <a name="show-incident-traffic-data-by-clicking-a-feature"></a>通过单击功能显示事件流量数据

要获取特定功能的事件，可以使用下面的代码。 单击某个功能时，代码逻辑会检查事件并生成有关事件的消息。 屏幕底部将显示一条消息，其中显示详细信息。

1. 首先，您需要> **activity_main.xml 编辑>布局**，以便它看起来像下面的布局。 您可以用所需的值`mapcontrol_centerLat`替换`mapcontrol_centerLng`和`mapcontrol_zoom`。 回想一下，缩放级别的值介于 0 和 22 之间。 在缩放级别 0 时，整个世界都适合单个磁贴。

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
           app:mapcontrol_centerLat="47.6050"
           app:mapcontrol_centerLng="-122.3344"
           app:mapcontrol_zoom="12"
           />

   </FrameLayout>
   ```

2. 将以下代码添加到**主活动.java**文件中。 默认情况下，包包含在内，因此请确保将包保持在顶部。

   ```java
   package <yourpackagename>;
   import androidx.appcompat.app.AppCompatActivity;

   import android.os.Bundle;
   import android.widget.Toast;

   import com.microsoft.azure.maps.mapcontrol.AzureMaps;
   import com.microsoft.azure.maps.mapcontrol.MapControl;
   import com.mapbox.geojson.Feature;
   import com.microsoft.azure.maps.mapcontrol.events.OnFeatureClick;

   import com.microsoft.azure.maps.mapcontrol.options.TrafficFlow;
   import static com.microsoft.azure.maps.mapcontrol.options.TrafficOptions.flow;
   import static com.microsoft.azure.maps.mapcontrol.options.TrafficOptions.incidents;

   public class MainActivity extends AppCompatActivity {

       static {
           AzureMaps.setSubscriptionKey("Your Azure Maps Subscription Key");
       }

       MapControl mapControl;

       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);

           mapControl = findViewById(R.id.mapcontrol);

           mapControl.onCreate(savedInstanceState);

           //Wait until the map resources are ready.
           mapControl.getMapAsync(map -> {

               map.setTraffic(flow(TrafficFlow.RELATIVE));
               map.setTraffic(incidents(true));

               map.events.add((OnFeatureClick) (features) -> {

                   if (features != null && features.size() > 0) {
                       Feature incident = features.get(0);
                       if (incident.properties() != null) {


                           StringBuilder sb = new StringBuilder();
                           String incidentType = incident.getStringProperty("incidentType");
                           if (incidentType != null) {
                               sb.append(incidentType);
                           }
                           if (sb.length() > 0) sb.append("\n");
                           if ("Road Closed".equals(incidentType)) {
                               sb.append(incident.getStringProperty("from"));
                           } else {
                               String description = incident.getStringProperty("description");
                               if (description != null) {
                                   for (String word : description.split(" ")) {
                                       if (word.length() > 0) {
                                           sb.append(word.substring(0, 1).toUpperCase());
                                           if (word.length() > 1) {
                                               sb.append(word.substring(1));
                                           }
                                           sb.append(" ");
                                       }
                                   }
                               }
                           }
                           String message = sb.toString();

                           if (message.length() > 0) {
                               Toast.makeText(this,message,Toast.LENGTH_LONG).show();
                           }
                       }
                   }
               });
           });
       }

       @Override
       public void onResume() {
           super.onResume();
           mapControl.onResume();
       }

       @Override
       protected void onStart(){
           super.onStart();
           mapControl.onStart();
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

3. 将上述代码合并到应用程序中后，即可单击某个功能并查看流量事件的详细信息。 根据**activity_main.xml**文件中使用的纬度、经度和缩放级别值，您将看到类似于下图的结果：

   <center>

   ![地图上的事件流量](./media/how-to-show-traffic-android/android-traffic.png)

   </center>

## <a name="next-steps"></a>后续步骤

查看以下指南，了解如何向地图添加更多数据：

> [!div class="nextstepaction"]
> [添加符号图层](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [添加图块层](how-to-add-tile-layer-android-map.md)

> [!div class="nextstepaction"]
> [将形状添加到 Android 地图](how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [显示功能信息](display-feature-information-android.md)
