导入模块：

import socket: 导入用于网络通信的 socket 模块。
import time: 导入用于处理时间的 time 模块。
import sys: 导入用于处理命令行参数和系统相关功能的 sys 模块。
import random: 导入用于生成随机数的 random 模块。
定义 main 函数：

检查命令行参数数量：如果参数数量不等于3（包括脚本名），打印使用说明并退出程序。
获取命令行参数：从命令行获取目标主机地址和端口号。
创建UDP套接字：使用 socket.socket(socket.AF_INET, socket.SOCK_DGRAM) 创建一个UDP套接字，并设置超时时间为600毫秒。
生成随机序列号：使用 random.randint(30000, 40000) 生成一个随机的初始序列号。
初始化RTT值的列表：用于存储每个Ping请求的往返时间（RTT）。
发送和接收Ping消息：

循环发送15个Ping请求：使用 for i in range(15) 启动一个循环。
获取当前时间戳：使用 time.time() 获取当前时间。
生成Ping消息：使用 f"PING {sequence_number + i} {current_time}\r\n" 生成消息。
发送Ping消息：使用 client_socket.sendto(message.encode(), (host, port)) 发送消息。
打印发送的消息和目标地址：使用 print(f"Sent: {message.strip()} to {host}:{port}") 打印发送的信息。
记录发送时间：使用 send_time = time.time() 记录发送时间。
接收响应：使用 client_socket.recvfrom(1024) 接收服务器响应。
记录接收时间：使用 receive_time = time.time() 记录接收时间。
计算RTT：使用 rtt = (receive_time - send_time) * 1000 计算往返时间并转换为毫秒。
打印RTT：使用 print(f"ping to {host}, seq = {sequence_number + i}, rtt = {rtt:.2f} ms") 打印RTT。
存储RTT值：将RTT值添加到 rtt_values 列表。
处理超时异常：如果在设定时间内没有收到响应，捕获 socket.timeout 异常，并打印超时信息。
计算和打印RTT统计值：

计算最小、最大和平均RTT值：如果 rtt_values 列表不为空，计算并打印最小、最大和平均RTT值。
打印无响应信息：如果没有接收到任何响应，打印相应信息。
关闭套接字：使用 client_socket.close() 关闭UDP套接字。

import socket
import time
import sys
import random

def main():
    if len(sys.argv) != 3:
        print("Usage: python3 PingClient.py <host> <port>")
        sys.exit(1)

        host = sys.argv[1]
        port = int(sys.argv[2])
    
    # 创建套接字socket
    client_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    client_skcket = socket.settimeout(0.6)

    sequence_number = random.randint(30000,40000)

    rtt_values = []
    for i in range(15):
        current_time = time.time()
        message = f"PING{sequence_number + i}{current_time}\r\n"

        try:
            client_socket.sendto(message.encode(),(host,port))
            print(f"Sent:{message.strip()} to{host}:{port}")
            sent_time = time.time()
            response,server = client_socket.recvfrom(1024)
            receive_time = time.time()
            rtt = (receive_time - sent_time) * 1000
            print(f"ping to {host},seq = {sequence_number + i},rt = {rtt:.2f} ms")
            rtt_values.append(rtt)
        except socket.timeout:
            print(f"ping to {host},seq = {sequence_number + i},time out")
        
    if rtt_values:
        min_rtt = min(rtt_values)
        max_rtt = max(rtt_values)
        avg_rtt = sum(rtt_values)/len(rtt_values)

        print(f"minimum = {min_rtt:.2f} ms, maximum = {max_rtt:.2f} ms, average = {avg_rtt:.2f} ms")
    else:
        print("No packets were received")
    
    client_socket.close()


    if __name__ =="__main__":
        main() 