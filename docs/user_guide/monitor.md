
## Metrics

> 每个节点 execute 的内部执行步骤包括
> - 节点 execute 前置处理
> - 节点 execute handler 前置处理
> - 节点逻辑
> - 节点 execute 后置处理
> - 节点 execute handler 后置处理

> 每个节点 schedule 的内部执行步骤包括
> - 节点 schedule 前置处理
> - 节点 schedule handler 前置处理
> - 节点逻辑
> - 节点 schedule 后置处理
> - 节点 schedule handler 后置处理


bamboo-engine 目前会向外暴露以下 prometheus metrics：

- engine_running_processes(Gauge)：正在执行的引擎进程数
- engine_running_schedules(Gauge)：正在执行的引擎调度数
- engine_process_running_time(Histogram)：进程每次执行耗时
- engine_schedule_running_time(Histogram)：调度每次执行耗时
- engine_node_execute_time(Histogram)：每种节点类型每次执行耗时
- engine_node_schedule_time(Histogram)：每种节点类型每次调度耗时
- engine_execute_pre_process_duration(Histogram)：每个节点 execute 的前置耗时
- engine_execute_post_process_duration(Histogram)：每个节点 execute 的后置耗时
- engine_schedule_pre_process_duration(Histogram)：每个节点 schedule 的前置耗时
- engine_schedule_post_process_duration(Histogram)：每个节点 schedule 的后置耗时
- engine_node_execute_pre_process_duration(Histogram)：每种节点 execute handler 的前置耗时
- engine_node_execute_post_process_duration(Histogram)：每种节点 execute handler 的后置耗时
- engine_node_schedule_pre_process_duration(Histogram)：每种节点 schedule handler 的前置耗时
- engine_node_schedule_post_process_duration(Histogram)：每种节点 schedule handler 的后置耗时

bamboo-engine 定义了运行时应该记录并向外暴露的 prometheus metrics：

- engine_runtime_context_value_read_time(Histogram)：运行时读取上下文数据耗时
- engine_runtime_context_ref_read_time(Histogram)：运行时读取上下文引用数据耗时
- engine_runtime_context_value_upsert_time(Histogram)：运行时更新上下文数据耗时
- engine_runtime_data_inputs_read_time(Histogram)：运行时读取节点输入数据耗时
- engine_runtime_data_outputs_read_time(Histogram)：运行时读取节点输出配置耗时
- engine_runtime_data_read_time(Histogram)：运行时读取节点输入数据和输出配置耗时
- engine_runtime_exec_data_inputs_read_time(Histogram)：运行时读取节点执行数据输入耗时
- engine_runtime_exec_data_outputs_read_time(Histogram)：运行时读取节点执行数据输出耗时
- engine_runtime_exec_data_read_time(Histogram)：运行时读取节点执行数据耗时
- engine_runtime_exec_data_inputs_write_time(Histogram)：运行时写入节点执行数据输入耗时
- engine_runtime_exec_data_outputs_write_time(Histogram)：运行时写入节点执行数据输出耗时
- engine_runtime_exec_data_write_time(Histogram)：运行时写入节点执行数据耗时
- engine_runtime_callback_data_read_time(Histogram)：运行时读取节点回调数据耗时
- engine_runtime_schedule_read_time(Histogram)：运行时读取调度对象耗时
- engine_runtime_schedule_write_time(Histogram)：运行时写入调度对象耗时
- engine_runtime_state_read_time(Histogram)：运行时读取节点状态对象耗时
- engine_runtime_state_write_time(Histogram)：运行时写入节点状态对象耗时
- engine_runtime_node_read_time(Histogram)：运行时读取节点数据耗时
- engine_runtime_process_read_time(Histogram)：运行时读取进程对象耗时
- engine_runtime_execute_task_claim_delay(Histogram)：execute Broker 消息延迟
- engine_runtime_schedule_task_claim_delay(Histogram)：schedule Broker 消息延迟

## 采集入口

### bamboo-pipeline

目前 bamboo-pipeline 仅支持暴露以**非 prefork**模式启动的 worker 记录的 metrics。例如，当 celery worker 以 gevent 模式启动后：

```shell
$ python manage.py celery worker -Q er_execute,er_schedule -l info -P gevent
```

worker 进程会在本地 `:8001` 端口暴露 metrics 采集入口。