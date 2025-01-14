技术栈：c++，shell，TCP/UDP，docker，cmake， 网络编程， 系统编程
项目简介: 是一个基于 IO多路复用 的 安全可靠的 UDP 文本传输框架。 

1. docker 模块: 使用 docker 构建整个 safe-udp 项目环境，通过 dockerfile 安装 glog、cmake 等依赖项，并使用 Shell 编写容器操作脚本，以实现项目构建流程的自动化和部署的便利性。
2. UDP_Transport 模块:  
  - 封装应用层 UDP Data Segment: 序列号、确认号、ACK、FIN等字段；并封装 序列化/反序列化 接口供上层调用。
  - 根据 RFC规范 加权计算 平滑 RTT 和超时重传 RTO ，并与 IO多路复用(select) 结合设置 超时逻辑，监听客户端的响应。
  - 封装 Sliding Window 和 Buffer，跟踪和管理 发送/接收数据 包的缓冲区，并设置相应索引指针，处理接收到 ACK逻辑。
  - 动态调节接收/拥塞窗口大小，自适应开启 慢启动、拥塞避免、拥塞发生、超时重传、快恢复 功能，并计算 cwnd、ssthresh 的值。
  - 封装统计模块，统计慢启动、拥塞避免、超时重传 的数据包量和比率，反馈网络性能和行为，利于后续网络服务优化。
  - 动态模拟 网络丢包和时延 状况， 校验 safe-udp 的可靠性和容错性(重传机制) 。
3. 项目管理和构建: 使用 cmake 作为项目的构建系统，构建 udp_tansport 动态库，供第三方模块调用；并通过 git ，clang-format等工具，管理仓库代码。
4. 功能测试和验证模块：构建 server、client 测试程序，验证safe-udp 可靠传输功能；并编写 diff.sh 脚本判断收发的文本内容是否完备
