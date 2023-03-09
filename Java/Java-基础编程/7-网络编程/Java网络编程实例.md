```java
## TCP Server
public TcpServer{
    //1 创建TCP协议的服务器套接字ServerSocket 并指定端口号
    //2 调用accept() 接受客户端请求
    //3、获取输入流读取客户端发送的数据
    //4、
    //5、关闭
    public staticvoidmain(String[] args) thros Exception{
        ServerSocket listener = new ServerSocket(8899);
        Socket socket = listener.accept();//阻塞方法 如果没有客户端请求则阻塞
        InputStream is =  socket.getInputStream();
        BufferReader br = new BufferReader (new InputStreamReader(is,"utf-8"));
		System.out.println(        br.readeLine("客户端发送"+data));
        
        br.close();
        socket.close();
        listener.close();
    }
}    


//客户端
public class TcpClient{
    //1 创建客户端套接字，并且指定服务器的地址和端口
    //2 获取输出流 发送数据给服务器
    //3 获取输入流 读取服务器回复的数据
    //4 关闭释放资源
    public static void main(String[] args){
        Socket sockert = new Socket("192.168.1.103",8899);
        BufferWriter bw = socket.getOutputStream();
        BufferdWrite br= new BufferWrite(new Out InputOutStreamWriter(os,"utf-8"))
        String data=br.readLine();

        br.close();
        socket.close();
        
    }
}
```

