### 计算机网络与HTTP课程复习笔记

---

#### 1. 互联网传输协议服务

**TCP服务**:
- **提供可靠性**：发送和接收进程之间的可靠传输
- **流量控制**：发送方不会压垮接收方
- **拥塞控制**：网络过载时限制发送方
- **不提供**：时序、最小吞吐量保证和安全性
- **面向连接**：需要在客户端和服务器进程之间建立连接

**UDP服务**:
- **不提供可靠性**：发送和接收进程之间的不可靠数据传输
- **不提供**：流量控制、拥塞控制、时序、吞吐量保证、安全性或连接建立
- **适用场景**：实时应用（如视频流、在线游戏）

**正确陈述**:
- TCP提供可靠性，而UDP不提供（答案C）

---

#### 2. Web与HTTP概述

**Web页面组成**:
- 由多个对象组成，每个对象可以存储在面向不同的Web服务器上
- 对象类型包括HTML文件、JPEG图像、Java小程序、音频文件等
- 基础HTML文件包含多个引用对象，每个对象都可以通过URL进行访问

**URL示例**:
- www.someschool.edu/someDept/pic.gif
  - 主机名: www.someschool.edu
  - 路径名: /someDept/pic.gif

---

#### 3. HTTP概述（续）

**HTTP使用TCP**:
- **建立连接**：客户端启动到服务器的TCP连接（端口80）
- **连接接受**：服务器接受来自客户端的TCP连接
- **消息交换**：HTTP消息在浏览器（HTTP客户端）和Web服务器（HTTP服务器）之间交换
- **连接关闭**：TCP连接关闭（HTTP/1.0关闭连接，HTTP/1.1支持持久连接）

**HTTP是无状态的**:
- 服务器不维护客户端请求的历史信息
- 维护状态的协议复杂且资源消耗大，需要处理状态一致性问题

---

#### 4. 维护用户/服务器状态：Cookie

**Cookie的工作原理**:
1. **初始请求和响应**：
   - 客户端发送常规HTTP请求
   - 服务器在响应消息中包含Set-Cookie头部（如Set-Cookie: 1678）
   - 客户端存储接收到的Cookie

2. **后续请求和响应**：
   - 客户端在后续请求中自动包含之前存储的Cookie（如Cookie: 1678）
   - 服务器根据Cookie中的ID执行特定的操作

3. **一段时间后（如一周后）**：
   - 客户端发送请求时仍会包含之前存储的Cookie
   - 服务器识别用户并提供相应的服务

**Cookie的作用**:
- 状态保持：传递状态信息，保持会话连续性
- 用户识别：通过Cookie中的用户ID，服务器识别回访用户
- 后台数据库交互：服务器将Cookie信息与后台数据库关联，访问用户历史记录或偏好设置

---

#### 5. 套接字（Sockets）

**套接字的作用**:
- 进程通过套接字发送和接收消息
- 套接字类似于门，进程通过它传输数据
- 发送进程依赖传输基础设施将消息传递到接收进程的套接字
- 网络通信涉及两个套接字：一个在发送端，一个在接收端

---

#### 6. 进程间通信（Processes Communicating）

**进程定义**:
- 进程是在主机内运行的程序实例

**同一主机内的进程通信**:
- 使用进程间通信（IPC）进行通信（由操作系统定义）

**不同主机上的进程通信**:
- 通过交换消息进行通信

**客户端和服务器进程**:
- 客户端进程：发起通信的进程
- 服务器进程：等待被联系的进程

**P2P架构中的进程**:
- 具有P2P架构的应用程序包含客户端进程和服务器进程

---

#### 7. 对等网络架构（Peer-to-Peer Architecture）

**主要特点**:
- 没有始终在线的服务器
- 任意终端系统直接通信
- 对等体从其他对等体请求服务，同时为其他对等体提供服务
  - 自我扩展性：新的对等体带来新的服务能力和需求

**管理复杂性**:
- 对等体间歇性连接并且改变IP地址
- 需要复杂的管理机制

**示例**:
- P2P文件共享（如BitTorrent）
- 区块链（如比特币）

---

#### 8. 客户端-服务器范例（Client-Server Paradigm）

**服务器**:
- 始终在线的主机
- 固定IP地址
- 通常部署在数据中心，便于扩展

**客户端**:
- 与服务器联系并通信
- 可能间歇性连接
- 可能具有动态IP地址
- 客户端之间不直接通信

**示例**:
- HTTP、IMAP、FTP

---

### 9. 安全TCP（Securing TCP）

**基础的TCP和UDP套接字**:
- 没有加密
- 明文密码通过套接字发送，经过互联网时是明文

**传输层安全协议（TLS）**:
- 提供加密的TCP连接
- 确保数据完整性
- 端点认证

**TLS在应用层实现**:
- 应用使用TLS库，TLS库再使用TCP进行传输

**TLS套接字API**:
- 明文发送到套接字，通过互联网时被加密

---

### 10. 非持久性HTTP: 响应时间

**RTT（Round Trip Time）**:
- RTT是指从客户端发送一个小数据包到服务器并接收到服务器的响应所需的时间。

**非持久性HTTP的响应时间**:
1. **建立TCP连接（1个RTT）**：
   - 建立TCP连接需要完成三次握手，这个过程需要一个RTT的时间。
   - 三次握手过程：客户端发送SYN包，服务器响应SYN-ACK包，客户端返回ACK包。

2. **发送HTTP请求和接收HTTP响应的前几个字节（1个RTT）**：
   - 建立TCP连接后，客户端发送HTTP请求到服务器，并等待服务器的响应。
   - 服务器接收到请求后，处理请求并发送HTTP响应的前几个字节到客户端。

3. **文件传输时间**：
   - 客户端接收到HTTP响应的前几个字节后，开始接收完整的文件数据。
   - 传输文件的时间取决于文件的大小和网络带宽。

**总响应时间计算**:
- 总响应时间 = 建立TCP连接的时间（1个RTT）+ 发送HTTP请求和接收响应前几个字节的时间（1个RTT）+ 文件传输时间
- 总响应时间 = 2个RTT + 文件传输时间

**图示**:
1. **启动TCP连接**：
   - 客户端发送SYN包（0.5个RTT）
   - 服务器返回SYN-ACK包（0.5个RTT）
   - 客户端返回ACK包（完成1个RTT）

2. **发送HTTP请求**：
   - 客户端发送HTTP请求包（0.5个RTT）
   - 服务器返回HTTP响应的前几个字节（0.5个RTT，完成第2个RTT）

3. **传输文件**：
   - 服务器传输文件数据到客户端
   - 客户端接收文件数据



### Computer Networks and HTTP Course Review Notes

---

#### 1. Internet Transport Protocols Services

**TCP Service**:
- **Provides reliability**: Reliable transport between sending and receiving processes
- **Flow control**: Sender won't overwhelm the receiver
- **Congestion control**: Throttle sender when the network is overloaded
- **Does not provide**: Timing, minimum throughput guarantee, and security
- **Connection-oriented**: Requires connection setup between client and server processes

**UDP Service**:
- **Unreliable transport**: Unreliable data transfer between sending and receiving processes
- **Does not provide**: Flow control, congestion control, timing, throughput guarantee, security, or connection setup
- **Use cases**: Real-time applications (e.g., video streaming, online gaming)

**Correct Statement**:
- TCP provides reliability, while UDP does not (Answer C)

---

#### 2. Web and HTTP Overview

**Web Page Composition**:
- Consists of multiple objects, each of which can be stored on different web servers
- Object types include HTML files, JPEG images, Java applets, audio files, etc.
- Base HTML file includes several referenced objects, each accessible by a URL

**URL Example**:
- www.someschool.edu/someDept/pic.gif
  - Host name: www.someschool.edu
  - Path name: /someDept/pic.gif

---

#### 3. HTTP Overview (Continued)

**HTTP Uses TCP**:
- **Connection Setup**: Client initiates a TCP connection to the server (port 80)
- **Connection Acceptance**: Server accepts the TCP connection from the client
- **Message Exchange**: HTTP messages are exchanged between the browser (HTTP client) and the web server (HTTP server)
- **Connection Closure**: TCP connection is closed (HTTP/1.0 closes the connection, HTTP/1.1 supports persistent connections)

**HTTP is Stateless**:
- Server maintains no information about past client requests
- Protocols that maintain state are complex and resource-intensive, requiring consistency management

---

#### 4. Maintaining User/Server State: Cookies

**Cookie Workflow**:
1. **Initial Request and Response**:
   - Client sends a usual HTTP request
   - Server includes a Set-Cookie header in the response (e.g., Set-Cookie: 1678)
   - Client stores the received cookie

2. **Subsequent Requests and Responses**:
   - Client automatically includes the stored cookie in subsequent requests (e.g., Cookie: 1678)
   - Server performs specific actions based on the cookie ID

3. **After Some Time (e.g., One Week Later)**:
   - Client continues to include the cookie in requests
   - Server recognizes the user and provides appropriate services

**Cookie Benefits**:
- State maintenance: Transmits state information to maintain session continuity
- User identification: Server identifies returning users via cookie ID
- Backend database interaction: Server links cookie information with backend database to access user history or preferences

---

#### 5. Sockets

**Role of Sockets**:
- Processes send and receive messages through sockets
- Sockets are analogous to doors; processes use them to transfer data
- The sending process relies on transport infrastructure to deliver messages to the receiving process's socket
- Network communication involves two sockets: one on the sending side and one on the receiving side

---

#### 6. Processes Communicating

**Process Definition**:
- A process is a program running within a host

**Intra-host Process Communication**:
- Processes within the same host communicate using inter-process communication (IPC) defined by the OS

**Inter-host Process Communication**:
- Processes on different hosts communicate by exchanging messages

**Client and Server Processes**:
- Client process: Initiates communication
- Server process: Waits to be contacted

**P2P Architecture Processes**:
- Applications with P2P architecture have both client and server processes

---

#### 7. Peer-to-Peer (P2P) Architecture

**Key Characteristics**:
- No always-on server
- Arbitrary end systems directly communicate
- Peers request service from other peers and provide service to other peers
  - Self-scalability: New peers bring new service capacity and demands

**Management Complexity**:
- Peers are intermittently connected and change IP addresses
- Complex management mechanisms are required

**Examples**:
- P2P file sharing (e.g., BitTorrent)
- Blockchain (e.g., Bitcoin)

---

#### 8. Client-Server Paradigm

**Server**:
- Always-on host
- Fixed IP address
- Typically hosted in data centers for scalability

**Clients**:
- Contact and communicate with the server
- May be intermittently connected
- May have dynamic IP addresses
- Clients do not directly communicate with each other

**Examples**:
- HTTP, IMAP, FTP

---

### 9. Securing TCP

**Vanilla TCP and UDP Sockets**:
- No encryption
- Cleartext passwords are sent through sockets in cleartext

**Transport Layer Security (TLS)**:
- Provides encrypted TCP connections
- Ensures data integrity
- Endpoint authentication

**TLS Implemented in the Application Layer**:
- Applications use TLS libraries, which use TCP for transmission

**TLS Socket API**:
- Cleartext sent into the socket is encrypted during transmission over the internet

---

### 10. Non-persistent HTTP: Response Time

**RTT (Round Trip Time)**:
- Time for a small packet to travel from the client to the server and back

**Non-persistent HTTP Response Time**:
1. **Establishing TCP Connection (1 RTT)**:
   - Establishing a TCP connection requires a three-way handshake, which takes approximately 1 RTT.
   - Steps: Client sends SYN, server responds with SYN-ACK, client responds with ACK.

2. **Sending HTTP Request and Receiving HTTP Response Header (1 RTT)**:
   - After the TCP connection is established, the client sends an HTTP request to the server and waits for the server's response header.
   - This process takes approximately 1 RTT.

3. **File Transmission Time**:
   - After receiving the HTTP response header, the client begins receiving the full file.
   - The time taken depends on the file size and network bandwidth.

**Total Response Time Calculation**:
- Total response time = Time to establish a TCP connection (1 RTT) + Time to send an HTTP request and receive the response header (1 RTT) + File transmission time
- Total response time = 2 RTT + File transmission time

**Diagram Explanation**:
1. **Initiating TCP Connection**:
   - Client sends SYN (0.5 RTT)
   - Server responds with SYN-ACK (0.5 RTT)
   - Client responds with ACK (1 RTT total)

2. **Sending HTTP Request**:
   - Client sends HTTP request (0.5 RTT)
   - Server responds with HTTP response header (0.5 RTT, 1 RTT total)

3. **File Transmission**:
   - Server transmits the file data to the client
   - Client receives the file data

---

I hope these notes will be helpful for your review. If you have any further questions or need additional explanations, feel free to ask!