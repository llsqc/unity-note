### 回顾服务端需要做的事情
1. 创建套接字Socket
2. 用Bind方法将套接字与本地地址绑定
3. 用Listen方法监听
4. 用Accept方法等待客户端连接
5. 建立连接，Accept返回新套接字
6. 用Send和Receive相关方法收发数据
7. 用Shutdown方法释放连接
8. 关闭套接字

### 实现服务端基本逻辑
1.创建套接字Socket（TCP）
```c#
Socket socketTcp = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
```

2. 用Bind方法将套接字与本地地址绑定
```c#
try
{
    IPEndPoint ipPoint = new IPEndPoint(IPAddress.Parse("127.0.0.1"), 8080);
    socketTcp.Bind(ipPoint);
}
catch (Exception e)
{
    Console.WriteLine("绑定报错" + e.Message);
    return;
}
```

3. 用Listen方法监听
```c#
socketTcp.Listen(1024);
Console.WriteLine("服务端绑定监听结束，等待客户端连入");
```

4. 用Accept方法等待客户端连接
5. 建立连接，Accept返回新套接字
```c#
Socket socketClient = socketTcp.Accept();
Console.WriteLine("有客户端连入");
```

6. 用Send和Receive相关方法收发数据
```c#
//发送
socketClient.Send(Encoding.UTF8.GetBytes("欢迎连入服务端"));
//接受
byte[] result = new byte[1024];
//返回值为接受到的字节数
int receiveNum = socketClient.Receive(result);
Console.WriteLine("接受到了{0}发来的消息：{1}",
    socketClient.RemoteEndPoint.ToString(),
    Encoding.UTF8.GetString(result, 0, receiveNum));
```

7. 用Shutdown方法释放连接
```c#
socketClient.Shutdown(SocketShutdown.Both);
```

8. 关闭套接字
```c#
socketClient.Close();
```

### 服务端服务多个客户端
```c#
using System;
using System.Collections.Generic;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Threading;

namespace TeachTcpServerExercises
{
    class Program
    {
        static Socket socket;
        //用于存储 客户端连入的 Socket 之后可以获取他们来进行通信
        static List<Socket> clientSockets = new List<Socket>();
 
        static bool isClose = false;
        static void Main(string[] args)
        {
            //1.建立Socket 绑定 监听 
            socket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
            IPEndPoint ipPoint = new IPEndPoint(IPAddress.Parse("127.0.0.1"), 8080);
            socket.Bind(ipPoint);
            socket.Listen(1024);

            //2.等待客户端连接（这节课需要特别处理的地方）
            Thread acceptThread = new Thread(AcceptClientConnect);
            acceptThread.Start();

            //3.收发消息（这节课需要特别处理的地方）
            Thread receiveThread = new Thread(ReceiveMsg);
            receiveThread.Start();

            //4.关闭相关
            while (true)
            {
                string input = Console.ReadLine();
                //定义一个规则 关闭服务器 断开所有连接
                if(input == "Quit")
                {
                    isClose = true;
                    for (int i = 0; i < clientSockets.Count; i++)
                    {
                        clientSockets[i].Shutdown(SocketShutdown.Both);
                        clientSockets[i].Close();
                    }
                    clientSockets.Clear();
                    break;
                }
                //定义一个规则 广播消息 就是让所有客户端收到服务端发送的消息
                else if(input.Substring(0, 2) == "B:")
                {
                    for (int i = 0; i < clientSockets.Count; i++)
                    {
                        clientSockets[i].Send(Encoding.UTF8.GetBytes(input.Substring(2)));
                    }
                }
            }
        }

        static void AcceptClientConnect()
        {
            while (!isClose)
            {
                Socket clientSocket = socket.Accept();
                clientSockets.Add(clientSocket);
                clientSocket.Send(Encoding.UTF8.GetBytes("欢迎你连入服务端"));
            }
        }

        static void ReceiveMsg()
        {
            Socket clientSocket;
            byte[] result = new byte[1024 * 1024];
            int receiveNum;
            int i;
            while (!isClose)
            {
                for (i = 0; i < clientSockets.Count; i++)
                {
                    clientSocket = clientSockets[i];
                    //判断 该socket是否有可以接收的消息 返回值就是字节数
                    if(clientSocket.Available > 0)
                    {
                        //客户端即使没有发消息过来 这句代码也会执行
                        receiveNum = clientSocket.Receive(result);
                        //如果直接在这收到消息 就处理 可能造成问题
                        //不能够即使的处理别人的消息
                        //为了不影响别人消息的处理 我们把消息处理 交给新的线程，为了节约线程相关的开销 我们使用线程池
                        ThreadPool.QueueUserWorkItem(HandleMsg, (clientSocket, Encoding.UTF8.GetString(result, 0, receiveNum)));
                    }
                }
            }
        }

        static void HandleMsg(object obj)
        {
            (Socket s, string str) info = ((Socket s, string str))obj;
            Console.WriteLine("收到客户端{0}发来的信息：{1}", info.s.RemoteEndPoint, info.str);
        }
    }
}

```

### 面向对象封装
```c#
using System;
using System.Collections.Generic;
using System.Net.Sockets;
using System.Text;
using System.Threading;
/// </summary>
/// 客户端Socket
/// </summary>
namespace TeachTcpServerExercises2
{
    class ClientSocket
    {
        private static int CLIENT_BEGIN_ID = 1;
        public int clientID;
        public Socket socket;

        public ClientSocket(Socket socket)
        {
            this.clientID = CLIENT_BEGIN_ID;
            this.socket = socket;
            ++CLIENT_BEGIN_ID;
        }

        /// <summary>
        /// 是否是连接状态
        /// </summary>
        public bool Connected => this.socket.Connected;

        //我们应该封装一些方法
        //关闭
        public void Close()
        {
            if (socket != null)
            {
                socket.Shutdown(SocketShutdown.Both);
                socket.Close();
                socket = null;
            }
        }
        //发送
        public void Send(string info)
        {
            if (socket != null)
            {
                try
                {
                    socket.Send(Encoding.UTF8.GetBytes(info));
                }
                catch (Exception e)
                {
                    Console.WriteLine("发消息出错" + e.Message);
                    Close();
                }
            }

        }
        //接收
        public void Receive()
        {
            if (socket == null)
                return;
            try
            {
                if (socket.Available > 0)
                {
                    byte[] result = new byte[1024 * 5];
                    int receiveNum = socket.Receive(result);
                    ThreadPool.QueueUserWorkItem(MsgHandle, Encoding.UTF8.GetString(result, 0, receiveNum));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("收消息出错" + e.Message);
                Close();
            }
        }

        private void MsgHandle(object obj)
        {
            string str = obj as string;
            Console.WriteLine("收到客户端{0}发来的消息：{1}", this.socket.RemoteEndPoint, str);
        }

    }
}
```

```c#
using System;
using System.Collections.Generic;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Threading;
/// <summary>
/// 服务器端Socket
/// </summary>
namespace TeachTcpServerExercises2
{
    class ServerSocket
    {
        //服务端Socket
        public Socket socket;
        //客户端连接的所有Socket
        public Dictionary<int, ClientSocket> clientDic = new Dictionary<int, ClientSocket>();

        private bool isClose;

        //开启服务器端
        public void Start(string ip, int port, int num)
        {
            isClose = false;
            socket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
            IPEndPoint ipPoint = new IPEndPoint(IPAddress.Parse(ip), port);
            socket.Bind(ipPoint);
            socket.Listen(num);
            ThreadPool.QueueUserWorkItem(Accept);
            ThreadPool.QueueUserWorkItem(Receive);
        }

        //关闭服务器端
        public void Close()
        {
            isClose = true;
            foreach (ClientSocket client in clientDic.Values)
            {
                client.Close();
            }
            clientDic.Clear();

            socket.Shutdown(SocketShutdown.Both);
            socket.Close();
            socket = null;
        }

        //接受客户端连入
        private void Accept(object obj)
        {
            while (!isClose)
            {
                try
                {
                    //连入一个客户端
                    Socket clientSocket = socket.Accept();
                    ClientSocket client = new ClientSocket(clientSocket);
                    client.Send("欢迎连入服务器");
                    clientDic.Add(client.clientID, client);
                }
                catch (Exception e)
                {
                    Console.WriteLine("客户端连入报错" + e.Message);
                }
            }
        }
        //接收客户端消息
        private void Receive(object obj)
        {
            while (!isClose)
            {
                if (clientDic.Count > 0)
                {
                    foreach (ClientSocket client in clientDic.Values)
                    {
                        client.Receive();
                    }
                }
            }
        }

        public void Broadcast(string info)
        {
            foreach (ClientSocket client in clientDic.Values)
            {
                client.Send(info);
            }
        }
    }
}
```