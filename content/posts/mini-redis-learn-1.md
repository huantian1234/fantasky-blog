+++
date = '2025-12-03T20:58:48+08:00'
draft = false
title = 'mini-redis学习分析01'
+++

#  0x01服务端启动过程
## Server
初始化启动Listener
## Listener
| 变量 | 描述 |
| --- | --- |
| `db_holder` | — |
| `listener` | 管理 TCP 连接 |
| `limit_connections` | 限制连接数，类型：`Arc<Semaphore>` |
| `notify_shutdown` | 关闭通知 |
| `shutdown_complete_tx` | 关闭完成通知 |

### run方法
循环监听连接,启动Handler

## Handler
开始处理连接

# 0x02命令处理流程

connecton->read_frame 将socket数据读取到Frame

> mini-Redis 协议帧类型
> - `简单字符: +内容\r\n`
> - `错误: -内容\r\n`
> - `整数: :数字\r\n`
> - `批量字符串: $长度\r\n内容\r\n`
> - `空值: $-1\r\n`
> - `数组: *数量\r\n元素1元素2...`


解析到完整的Frame之后用 Command::from_frame转换成Command数据结构

最后调用apply应用结果到db当中