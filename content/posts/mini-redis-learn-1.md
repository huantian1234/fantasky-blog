+++
date = '2025-12-03T20:58:48+08:00'
draft = false
title = 'Mini-Redis 学习分析（一）'
+++

# 0x01 服务端启动过程

## Server

服务端启动时，首先初始化 `Listener` 实例，准备接收客户端连接。

## Listener

`Listener` 负责管理服务端的核心组件：

| 变量 | 描述 |
| --- | --- |
| `db_holder` | 持有数据库实例 |
| `listener` | TCP 监听器，负责接收客户端连接 |
| `limit_connections` | 连接数限制器，类型为 `Arc<Semaphore>` |
| `notify_shutdown` | 用于广播关闭信号的通知器 |
| `shutdown_complete_tx` | 用于确认关闭完成的发送端 |

### run 方法

`run` 方法进入主循环，持续监听新连接。每当有客户端接入时，会创建一个 `Handler` 实例来处理该连接。

## Handler

`Handler` 负责处理单个客户端连接的全生命周期，包括读取请求、解析命令和执行操作。

---

# 0x02 命令处理流程

命令处理分为以下三个阶段：

### 1. 读取帧数据

通过 `Connection::read_frame` 方法从 socket 读取原始数据，并解析为 `Frame` 结构。

> **Mini-Redis 协议帧类型**
>
> | 类型 | 格式 | 示例 |
> | --- | --- | --- |
> | 简单字符串 | `+内容\r\n` | `+OK\r\n` |
> | 错误 | `-内容\r\n` | `-ERR unknown\r\n` |
> | 整数 | `:数字\r\n` | `:1000\r\n` |
> | 批量字符串 | `$长度\r\n内容\r\n` | `$5\r\nhello\r\n` |
> | 空值 | `$-1\r\n` | `$-1\r\n` |
> | 数组 | `*数量\r\n元素...` | `*2\r\n$3\r\nfoo\r\n$3\r\nbar\r\n` |

### 2. 解析命令

当完整的 `Frame` 解析完成后，调用 `Command::from_frame` 将其转换为具体的 `Command` 枚举类型。

### 3. 执行命令

最后调用 `Command::apply` 方法，将命令执行结果应用到数据库中，并将响应返回给客户端。