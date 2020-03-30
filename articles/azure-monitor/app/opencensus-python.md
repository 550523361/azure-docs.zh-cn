---
title: 使用 Azure Monitor 监视 Python 应用程序（预览版）| Microsoft Docs
description: 提供有关将 OpenCensus Python 连接到 Azure Monitor 的说明
ms.topic: conceptual
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6ef0675e3ae3f7a5da38138177f3033051723411
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537102"
---
# <a name="set-up-azure-monitor-for-your-python-application"></a>为 Python 应用程序设置 Azure 监视器

Azure Monitor 通过与 [OpenCensus](https://opencensus.io) 集成，来支持 Python 应用程序的分布式跟踪、指标收集和日志记录。 本文将逐步介绍设置 OpenCensus for Python 并将监视数据发送到 Azure Monitor 的过程。

## <a name="prerequisites"></a>先决条件

- Azure 订阅。 如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/)。
- Python 安装。 本文使用 [Python 3.7.0](https://www.python.org/downloads/)，不过更低的版本在经过轻微的更改后也可能适用。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

登录到 Azure[门户](https://portal.azure.com/)。

## <a name="create-an-application-insights-resource-in-azure-monitor"></a>在 Azure Monitor 中创建 Application Insights 资源

首先需在 Azure Monitor 中创建一个 Application Insights 资源，该资源将生成检测密钥 (ikey)。 然后使用 ikey 配置 OpenCensus SDK，以将遥测数据发送到 Azure Monitor。

1. 选择 **"创建资源** > **开发人员工具** > **应用程序见解**"。

   ![添加 Application Insights 资源](./media/opencensus-python/0001-create-resource.png)

1. 此时会显示一个配置框。 参考下表填写输入字段。

   | 设置        | “值”           | 描述  |
   | ------------- |:-------------|:-----|
   | **名称**      | 全局唯一值 | 标识所监视的应用的名称 |
   | **资源组**     | myResourceGroup      | 用于托管 Application Insights 数据的新资源组的名称 |
   | **位置** | 美国东部 | 离你较近的位置或离托管应用的位置较近的位置 |

1. 选择 **“创建”**。

## <a name="instrument-with-opencensus-python-sdk-for-azure-monitor"></a>使用 OpenCensus Python SDK for Azure Monitor 进行检测

安装 OpenCensus Azure Monitor 导出程序：

```console
python -m pip install opencensus-ext-azure
```

有关包和集成的完整列表，请参阅[OpenCensus 包](https://docs.microsoft.com/azure/azure-monitor/app/nuget#common-packages-for-python-using-opencensus)。

> [!NOTE]
> 命令 `python -m pip install opencensus-ext-azure` 假设已经为 Python 安装设置了一个 `PATH` 环境变量。 如果尚未配置此变量，则需要提供 Python 可执行文件所在位置的完整目录路径。 结果是如下所示的命令：`C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`

SDK 使用三个 Azure Monitor 导出程序将不同类型的遥测数据发送到 Azure Monitor：跟踪、指标和日志。 有关这些遥测数据类型的详细信息，请参阅[数据平台概述](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform)。 按照以下说明通过三个导出程序发送这些遥测数据类型。

## <a name="telemetry-type-mappings"></a>遥测类型映射

下面是 OpenCensus 提供的导出程序，它映射到会在 Azure Monitor 中出现的遥测类型。

![将遥测类型从 OpenCensus 映射到 Azure Monitor 的屏幕截图](./media/opencensus-python/0012-telemetry-types.png)

### <a name="trace"></a>跟踪

> [!NOTE]
> `Trace`在 OpenCensus 中是指[分布式跟踪](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)。 向`AzureExporter` `requests` Azure`dependency`监视器发送和遥测。

1. 首先，让我们在本地生成一些跟踪数据。 在 Python IDLE 或所选编辑器中，输入以下代码。

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. 运行代码时，系统会重复提示你输入一个值。 每次输入时，值都会输出到 shell，OpenCensus Python 模块会生成相应的 `SpanData` 片段。 OpenCensus 项目将[跟踪定义为 span 树](https://opencensus.io/core-concepts/tracing/)。
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. 虽然输入值有助于演示，但最终我们希望将 `SpanData` 发出到 Azure Monitor。 将连接字符串直接传递到导出器，也可以在环境变量`APPLICATIONINSIGHTS_CONNECTION_STRING`中指定它。 根据以下代码示例修改上一步中的代码：

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. 现在当你运行 Python 脚本时，系统仍会提示你输入值，但只有此值输出到 shell 中。 创建的 `SpanData` 将发送到 Azure Monitor。 可以在 `dependencies` 下找到发出的 span 数据。 有关传出请求的更多详细信息，请参阅 OpenCensus Python[依赖项](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-dependency)。
有关传入请求的更多详细信息，请参阅 OpenCensus Python[请求](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-request)。

#### <a name="sampling"></a>采样

有关 OpenCensus 中采样的详细信息，请参阅 [OpenCensus 中的采样](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications)。

#### <a name="trace-correlation"></a>跟踪关联

有关跟踪数据中的遥测相关性的详细信息，请查看 OpenCensus Python[遥测相关性](https://docs.microsoft.com/azure/azure-monitor/app/correlation#telemetry-correlation-in-opencensus-python)。

#### <a name="modify-telemetry"></a>修改遥测

有关如何在将跟踪遥测发送到 Azure 监视器之前对其进行修改的详细信息，请参阅 OpenCensus Python[遥测处理器](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)。

### <a name="metrics"></a>指标

1. 首先，让我们生成一些本地指标数据。 我们将创建一个简单的指标，用于跟踪用户按 Enter 的次数。

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. 运行该代码时，系统会反复提示你按 Enter。 将创建一个指标用于跟踪按 Enter 的次数。 每次输入都会递增值，并且指标信息将显示在控制台中。 该信息包括当前值，以及更新指标时的当前时间戳。

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. 虽然输入值有助于演示，但最终我们希望将指标数据发出到 Azure Monitor。 将连接字符串直接传递到导出器，也可以在环境变量`APPLICATIONINSIGHTS_CONNECTION_STRING`中指定它。 根据以下代码示例修改上一步中的代码：

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')

    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. 导出程序按固定的间隔将指标数据发送到 Azure Monitor。 默认间隔为每隔 15 秒。 我们正在跟踪单个指标，因此，在每个间隔将会发送此指标数据及其包含的任何值和时间戳。 可在 `customMetrics` 下找到数据。

#### <a name="standard-metrics"></a>标准指标

默认情况下，指标导出器将向 Azure 监视器发送一组标准指标。 您可以通过在`enable_standard_metrics`指标导出器的构造函数中`False`设置标志来禁用此功能。

    ```python
    ...
    exporter = metrics_exporter.new_metrics_exporter(
      enable_standard_metrics=False,
      connection_string='InstrumentationKey=<your-instrumentation-key-here>')
    ...
    ```
以下是当前发送的标准指标列表：

- 可用内存（字节）
- CPU 处理器时间（百分比）
- 传入请求率（每秒）
- 传入请求平均执行时间（毫秒）
- 传出请求率（每秒）
- 进程 CPU 使用率（百分比）
- 处理专用字节（字节）

您应该能够在 中`performanceCounters`看到这些指标。 传入请求率将低于`customMetrics`。 有关详细信息，请参阅[性能计数器](https://docs.microsoft.com/azure/azure-monitor/app/performance-counters)。

#### <a name="modify-telemetry"></a>修改遥测

有关如何在将跟踪遥测发送到 Azure 监视器之前对其进行修改的详细信息，请参阅 OpenCensus Python[遥测处理器](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)。

### <a name="logs"></a>日志

1. 首先，让我们生成一些本地日志数据。

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  代码将持续请求输入值。 对于每个输入值将发出一个日志条目。

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. 虽然输入值有助于演示，但最终我们希望将日志数据发出到 Azure Monitor。 将连接字符串直接传递到导出器，也可以在环境变量`APPLICATIONINSIGHTS_CONNECTION_STRING`中指定它。 根据以下代码示例修改上一步中的代码：

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. 导出程序会将日志数据发送到 Azure Monitor。 可在 `traces` 下找到数据。 

> [!NOTE]
> 此上下文中的 `traces` 不同于 `Tracing`。 `traces` 是指使用 `AzureLogHandler` 时 Azure Monitor 中会出现的遥测类型。 `Tracing` 是指 OpenCensus 中的一种概念，与[分布式跟踪](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)相关。

5. 若要设置日志消息的格式，可以使用内置 Python [日志记录 API](https://docs.python.org/3/library/logging.html#formatter-objects) 中的 `formatters`。

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    format_str = '%(asctime)s - %(levelname)-8s - %(message)s'
    date_format = '%Y-%m-%d %H:%M:%S'
    formatter = logging.Formatter(format_str, date_format)
    # TODO: replace the all-zero GUID with your instrumentation key.
    handler = AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

6. 您还可以使用custom_dimensions字段将自定义属性添加到*额外*关键字参数中的日志消息。 它们会显示为 Azure Monitor 的 `customDimensions` 中的键值对。
> [!NOTE]
> 要此功能正常工作，您需要将字典传递给custom_dimensions字段。 如果传递任何其他类型的参数，记录器将忽略它们。

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )

    properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

    # Use properties in logging statements
    logger.warning('action', extra=properties)
    ```

#### <a name="sending-exceptions"></a>发送异常

OpenCensus Python 不会自动跟踪和`exception`发送遥测数据。 它们`AzureLogHandler`通过使用 Python 日志记录库使用异常通过 发送。 您可以像使用普通日志记录一样添加自定义属性。

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )

    properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

    # Use properties in exception logs
    try:
        result = 1 / 0  # generate a ZeroDivisionError
    except Exception:
        logger.exception('Captured an exception.', extra=properties)
    ```
由于必须显式记录异常，因此用户应如何记录未处理的异常。 OpenCensus 不会限制用户想要执行此操作的方式，只要他们显式记录异常遥测数据。

#### <a name="sampling"></a>采样

有关 OpenCensus 中采样的详细信息，请参阅 [OpenCensus 中的采样](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications)。

#### <a name="log-correlation"></a>日志关联

有关如何使用跟踪上下文数据扩充日志的详细信息，请参阅 OpenCensus Python [日志集成](https://docs.microsoft.com/azure/azure-monitor/app/correlation#log-correlation)。

#### <a name="modify-telemetry"></a>修改遥测

有关如何在将跟踪遥测发送到 Azure 监视器之前对其进行修改的详细信息，请参阅 OpenCensus Python[遥测处理器](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)。

## <a name="view-your-data-with-queries"></a>使用查询查看数据

可以通过“日志(分析)”选项卡查看从应用程序发送的遥测数据。****

![概述窗格的屏幕截图，其中的“日志(分析)”在红框中呈选中状态](./media/opencensus-python/0010-logs-query.png)

在“活动”下的列表中：****

- 对于使用 Azure Monitor 跟踪导出程序发送的遥测数据，传入请求显示在 `requests` 下。 传出请求或进程内请求显示在 `dependencies` 下。
- 对于使用 Azure Monitor 指标导出程序发送的遥测数据，发送的指标显示在 `customMetrics` 下。
- 对于使用 Azure Monitor 日志导出程序发送的遥测数据，日志显示在 `traces` 下。 异常显示在 `exceptions` 下。

有关如何使用查询和日志的详细信息，请参阅 [Azure Monitor 中的日志](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs)。

## <a name="learn-more-about-opencensus-for-python"></a>详细了解 OpenCensus for Python

* [GitHub 上的 OpenCensus Python](https://github.com/census-instrumentation/opencensus-python)
* [自定义](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [GitHub 上的 Azure 监视器导出器](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)
* [开放普查集成](https://github.com/census-instrumentation/opencensus-python#extensions)
* [Azure 监视器示例应用程序](https://github.com/Azure-Samples/azure-monitor-opencensus-python)

## <a name="next-steps"></a>后续步骤

* [跟踪传入请求](./../../azure-monitor/app/opencensus-python-dependency.md)
* [跟踪外出请求](./../../azure-monitor/app/opencensus-python-request.md)
* [应用程序映射](./../../azure-monitor/app/app-map.md)
* [端到端性能监视](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>警报

* [可用性测试](../../azure-monitor/app/monitor-web-app-availability.md)：创建测试来确保站点在 Web 上可见。
* [智能诊断](../../azure-monitor/app/proactive-diagnostics.md)：这些测试可自动运行，因此不需要进行任何设置。 它们会告诉你应用是否具有异常的失败请求速率。
* [指标警报](../../azure-monitor/app/alerts.md)：设置警报，以便在指标超过阈值时发出警告。 可以在编码到应用中的自定义指标中设置它们。
