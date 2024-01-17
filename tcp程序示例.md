# 高级C语言网络编程

**概述**

C语言网络编程的范围涵盖了使用C语言进行网络通信的各个方面。它包括使用套接字（sockets）API进行底层网络编程，以及构建网络应用程序的高级概念和技术。

1. 套接字编程：使用`socket()`函数创建套接字、使用`bind()`函数绑定套接字到本地地址、使用`listen()`函数监听连接请求、使用`accept()`函数接受连接请求等。

2. 传输层协议：如TCP（传输控制协议）和UDP（用户数据报协议）。TCP提供可靠的、面向连接的通信，而UDP提供无连接的、不可靠的通信。

3. 网络地址转换：进行主机字节序和网络字节序之间的转换。使用函数如`htonl()`、`htons()`、`ntohl()`和`ntohs()`等，用于在主机字节序和网络字节序之间进行转换。

4. 多线程和并发：网络编程中，多线程和并发技术常用于同时处理多个客户端请求。C语言提供了线程库（如pthread）以及相关的同步机制（如互斥锁和条件变量），用于实现多线程和并发编程。

5. 客户端-服务器模型：C语言网络编程通常涉及构建客户端和服务器应用程序。客户端向服务器发起连接请求，服务器接受请求并提供服务。这涉及到使用套接字编程进行双向通信、处理并发连接、实现基本的协议等。

6. 网络协议和协议栈：C语言网络编程需要了解各种网络协议和协议栈的工作原理。这包括了解IP协议、TCP协议、UDP协议以及其他常用的网络协议。



## 1、CS的构建

### 1、server端

``` C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <netinet/in.h>
#include <arpa/inet.h>

#define PORT 8888
#define BUFFER_SIZE 1024

int main()
{
    int listenfd, connfd;//监听套接字，连接套接字
    struct sockaddr_in server_addr, client_addr;//存储IP地址，server和client
    socklen_t client_len;//存储客户端地址结构体的长度
    char buffer[BUFFER_SIZE];//存储发送的数据

    // 创建套接字
    listenfd = socket(AF_INET, SOCK_STREAM, 0);//ipv4，tcp，默认
    if (listenfd == -1) {
        perror("socket");
        exit(EXIT_FAILURE);
    }

    // 设置服务器地址
    memset(&server_addr, 0, sizeof(server_addr));//将addr的内存清空
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(PORT);
    server_addr.sin_addr.s_addr = INADDR_ANY;

    // 绑定套接字
    if (bind(listenfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) == -1) {
        perror("bind");
        exit(EXIT_FAILURE);
    }//将服务器套接字绑定到服务器地址上

    // 监听连接请求
    if (listen(listenfd, 10) == -1) {
        perror("listen");
        exit(EXIT_FAILURE);
    }

    printf("Server listening on port %d...\n", PORT);

    // 接受连接请求
    client_len = sizeof(client_addr);
    connfd = accept(listenfd, (struct sockaddr *)&client_addr, &client_len);
    if (connfd == -1) {
        perror("accept");
        exit(EXIT_FAILURE);
    }

    printf("Connected with client: %s:%d\n", inet_ntoa(client_addr.sin_addr), ntohs(client_addr.sin_port));

    while (1) {
        // 从客户端接收数据
        ssize_t n = read(connfd, buffer, BUFFER_SIZE);
        if (n > 0) {
            printf("Received from client: %s", buffer);

            // 向客户端发送响应
            write(connfd, buffer, n);
        } else if (n == 0) {
            printf("Connection closed by the client.\n");
            break;
        } else {
            perror("read");
            exit(EXIT_FAILURE);
        }
    }

    close(connfd);
    close(listenfd);

    return 0;
}
```

#### bind函数：

`bind()` 函数是用于将一个套接字与特定的地址进行绑定的操作。它将一个本地地址（包括 IP 地址和端口号）与套接字相关联，使得该套接字能够监听和接受来自该地址的连接请求，或者发送数据到该地址。

函数原型如下所示：

```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

- `sockfd`：要绑定的套接字描述符。
- `addr`：指向要绑定的地址结构体的指针，通常是 `struct sockaddr` 类型的指针。
- `addrlen`：指定地址结构体的长度。

`bind()` 函数通常在服务器端使用。服务器在启动时，需要绑定一个特定的 IP 地址和端口号，以便客户端能够连接到该服务器。通过调用 `bind()` 函数，服务器将套接字与指定的地址绑定在一起，使得服务器能够监听该地址上的连接请求。

如果 `bind()` 函数执行成功，它会返回 0；如果发生错误，返回值为 -1。

需要注意的是，一旦套接字绑定成功，就不能再次绑定到同一个地址。如果需要重新绑定，必须先关闭之前的套接字。

#### listen函数：

`listen()` 函数用于将一个套接字设置为监听状态，以便接受客户端的连接请求。它将套接字标记为被动套接字，即用于接受传入连接的套接字。

函数原型如下所示：

```c
int listen(int sockfd, int backlog);
```

- `sockfd`：要设置为监听状态的套接字描述符。
- `backlog`：指定等待连接队列的最大长度，即同时等待处理的连接请求的数量。

`listen()` 函数通常在服务器端使用。在服务器启动后，通过调用 `listen()` 函数将套接字设置为监听状态，以准备接受客户端的连接请求。

当套接字处于监听状态时，它将开始监听指定的地址和端口号，等待客户端的连接请求。连接请求将被放入等待连接队列中，等待服务器接受或拒绝处理。通过指定 `backlog` 参数，可以设置等待连接队列的最大长度，即同时等待处理的连接请求的数量。超过该数量的连接请求将被拒绝。

如果 `listen()` 函数执行成功，它会返回 0；如果发生错误，返回值为 -1。在发生错误时，可以使用 `perror()` 函数输出错误信息，帮助诊断问题。

需要注意的是，只有在调用 `listen()` 函数之后，才能使用 `accept()` 函数来接受客户端的连接请求。

#### accept函数：

`accept()` 函数用于接受客户端的连接请求，并创建一个新的套接字用于与客户端进行通信。它在服务器端被调用，用于建立与客户端之间的连接。

函数原型如下所示：

```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

- `sockfd`：监听套接字描述符，即之前通过 `listen()` 设置为监听状态的套接字描述符。
- `addr`：指向用于存储客户端地址信息的结构体指针，通常是 `struct sockaddr` 类型的指针。
- `addrlen`：指向表示客户端地址长度的变量的指针。在调用 `accept()` 函数时，需要将其初始化为客户端地址结构体的长度，并在接受连接后更新为实际的客户端地址长度。

`accept()` 函数的调用会阻塞程序，直到有客户端连接请求到达。当有连接请求到达时，`accept()` 函数会接受该连接请求，并返回一个新的套接字描述符，即用于与客户端进行通信的套接字。这个新的套接字可以使用和普通的套接字操作一样的方式进行读写操作，用于与客户端进行数据交换。

同时，`accept()` 函数会将客户端的地址信息存储在 `addr` 指向的结构体中，通过 `addrlen` 参数传递实际的客户端地址长度。

如果 `accept()` 函数执行成功，它会返回一个新的套接字描述符用于与客户端通信；如果发生错误，返回值为 -1。在发生错误时，可以使用 `perror()` 函数输出错误信息，帮助诊断问题。

注意：`accept()` 函数在成功接受连接请求后，会产生一个新的套接字，而原始的监听套接字仍然保持在监听状态，可以继续接受其他客户端的连接请求。

### 2.client端

~~~ c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <netinet/in.h>
#include <arpa/inet.h>

#define SERVER_IP "127.0.0.1"
#define PORT 8888
#define BUFFER_SIZE 1024

int main()
{
    int sockfd;
    struct sockaddr_in server_addr;//定义服务器端
    char buffer[BUFFER_SIZE];

    // 创建套接字
    sockfd = socket(AF_INET, SOCK_STREAM, 0);//创建ipv4下的tcp连接
    if (sockfd == -1) {
        perror("socket");
        exit(EXIT_FAILURE);
    }

    // 设置服务器地址
    memset(&server_addr, 0, sizeof(server_addr));//将内存清空
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(PORT);
    if (inet_pton(AF_INET, SERVER_IP, &server_addr.sin_addr) <= 0) {
        perror("inet_pton");
        exit(EXIT_FAILURE);
    }

    // 连接服务器
    if (connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) == -1) {
        perror("connect");
        exit(EXIT_FAILURE);
    }

    while (1) {
        printf("Enter message: ");
        fgets(buffer, BUFFER_SIZE, stdin);

        // 向服务器发送数据
        write(sockfd, buffer, strlen(buffer));

        // 从服务器接收响应
        ssize_t n = read(sockfd, buffer, BUFFER_SIZE);
        if (n > 0) {
            printf("Received from server: %s", buffer);
        } else if (n == 0) {
            printf("Connection closed by the server.\n");
            break;
        } else {
            perror("read");
            exit(EXIT_FAILURE);
        }
    }

    close(sockfd);

    return 0;
}

~~~

#### socket函数：

`socket()` 函数是一个系统调用，用于创建一个新的套接字（socket）。

在C语言中，`socket()` 函数的原型如下：

```c
#include <sys/types.h>
#include <sys/socket.h>

int socket(int domain, int type, int protocol);
```

该函数接受三个参数：

1. `domain`：指定套接字的协议域（protocol family），决定了套接字的通信类型。常见的协议域包括 `AF_INET`（用于IPv4网络）、`AF_INET6`（用于IPv6网络）、`AF_UNIX`（用于本地进程间通信）等。

2. `type`：指定套接字的类型，决定了套接字的特性和行为。常见的套接字类型包括 `SOCK_STREAM`（面向连接的可靠字节流套接字，如TCP）、`SOCK_DGRAM`（无连接的不可靠数据报套接字，如UDP）等。

3. `protocol`：指定套接字所使用的协议。通常情况下，可以将其设置为0，表示根据 `domain` 和 `type` 参数自动选择合适的协议。

`socket()` 函数返回一个整数类型的文件描述符，用于唯一标识创建的套接字。如果函数调用失败，将返回值设置为-1，并且可以通过检查 `errno` 变量来获取具体的错误信息。

使用 `socket()` 函数创建套接字后，可以通过其他函数（例如 `bind()`、`listen()`、`connect()`）来设置套接字的相关属性和进行进一步的操作，以实现网络通信的需求。

#### connect函数：

`connect()` 函数用于建立与远程服务器的连接，将本地套接字与远程服务器的套接字进行关联，从而实现网络通信。

在C语言中，`connect()` 函数的原型如下：

```c
#include <sys/types.h>
#include <sys/socket.h>

int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

该函数接受三个参数：

1. `sockfd`：要连接的套接字的文件描述符，即通过 `socket()` 函数创建的套接字。

2. `addr`：指向目标服务器地址的指针，通常是一个 `struct sockaddr` 类型的指针，可以是 `struct sockaddr_in` 或 `struct sockaddr_in6`。需要根据套接字的协议域来进行强制类型转换。

3. `addrlen`：目标服务器地址结构体的大小，以字节为单位。可以使用 `sizeof()` 运算符来获取地址结构体的大小。

`connect()` 函数的执行过程如下：

1. `connect()` 函数将本地套接字与目标服务器的套接字进行关联。

2. 它会向目标服务器发送连接请求，并在内核中建立连接。

3. 如果连接成功建立，`connect()` 函数将返回0。如果连接失败，将返回-1，并且可以通过检查 `errno` 变量来获取具体的错误信息。

在连接建立后，可以使用该套接字进行数据的发送和接收。对于面向连接的套接字类型，通常在调用 `connect()` 函数后，可以通过 `read()` 和 `write()` 函数在客户端和服务器之间进行双向通信。

需要注意的是，`connect()` 函数是一个阻塞函数，即在连接建立完成之前，它会一直阻塞程序的执行。

#### read和write函数：

`read()` 和 `write()` 是在C语言中用于进行文件读取和写入操作的函数。在网络编程中，它们也常用于套接字文件描述符上的数据传输。

1. `read()` 函数：
   ````c
   #include <unistd.h>
   
   ssize_t read(int fd, void *buf, size_t count);
   
   //read() 函数从文件描述符 fd 中读取数据，并将其存储到缓冲区 buf 中。count参数指定要读取的最大字节数。
   
   //返回值是实际读取的字节数，如果出现错误，返回值可能为-1。常见的错误包括连接断开、读取超时等。如果返回值为0，表示已到达文件末尾或连接关闭。
   
2. `write()` 函数：
   
   ````c
   #include <unistd.h>
   
   ssize_t write(int fd, const void *buf, size_t count);
   
   //write()函数将缓冲区 buf 中的数据写入到文件描述符 fd 对应的文件或套接字中。count 参数指定要写入的字节数。
   
   //返回值是实际写入的字节数，如果出现错误，返回值可能为-1。常见的错误包括连接断开、写入超时等。

在网络编程中，`read()` 和 `write()` 函数通常用于在套接字上进行数据的接收和发送。客户端可以使用 `write()` 函数将数据发送给服务器，服务器则使用 `read()` 函数接收客户端发送的数据。这样可以实现双向通信。

需要注意的是，`read()` 和 `write()` 函数都是阻塞函数，即在读写操作完成之前会一直阻塞程序的执行。

### 3.并发服务器的构建

#### fork函数：

`fork()` 函数是一个系统调用，用于在UNIX和类UNIX操作系统中创建一个新的进程。它会复制当前进程（称为父进程），创建一个新的进程（称为子进程），使得父进程和子进程在不同的执行路径上同时执行。

在C语言中，`fork()` 函数的原型如下：

```c
#include <unistd.h>

pid_t fork(void);
```

该函数没有参数，返回值是一个进程ID（PID）。在不同的情况下，`fork()` 函数的返回值会有不同的含义：

- 如果返回值为负数，表示创建子进程失败，具体的负数值表示错误的类型。

- 如果返回值为0，表示当前执行的是子进程。子进程可以通过调用 `getpid()` 函数获取自己的进程ID。

- 如果返回值大于0，表示当前执行的是父进程。返回值是子进程的进程ID，父进程可以通过调用 `getpid()` 函数获取自己的进程ID。

`fork()` 函数的执行过程如下：

1. 当调用 `fork()` 函数时，操作系统会创建一个新的进程，复制父进程的代码、数据、堆栈等信息，并为子进程分配一个唯一的进程ID。

2. 子进程从 `fork()` 函数返回的地方开始执行，而父进程继续执行后续的代码。

**在TCP通信中，`fork()` 函数可以用于创建一个子进程，使得父进程和子进程可以同时执行不同的逻辑，实现并发处理客户端连接的需求。**

通常情况下，TCP服务器在接受客户端连接后，会创建一个新的子进程来处理与该客户端的通信。这样可以实现以下的工作流程：

1. 服务器通过调用 `fork()` 函数创建一个子进程。这个子进程是服务器的副本，它继承了父进程的文件描述符、内存等资源。

2. 在子进程中，服务器关闭不需要的文件描述符（如监听套接字），然后开始与客户端进行通信。它可以使用 `read()` 和 `write()` 函数来接收和发送数据。

3. 在父进程中，服务器可以继续监听其他客户端的连接请求。父进程可以在一个循环中等待并接受连接，创建多个子进程来处理不同的客户端连接。

通过使用 `fork()` 函数，服务器可以同时处理多个客户端连接，而不需要串行地处理每个连接请求。这样可以提高服务器的并发性能和响应能力。

#### exec函数：

`exec()` 函数是一个系统调用，用于在UNIX和类UNIX操作系统中执行一个新的程序。它会将当前进程替换为新的程序，加载新的可执行文件，并开始执行新的程序的代码。

在C语言中，`exec()` 函数有多个变种，包括 `execl()`、`execv()`、`execle()`、`execve()` 等，它们的使用方式略有不同，但都用于执行新的程序。

以 `execl()` 函数为例，其原型如下：

```c
#include <unistd.h>

int execl(const char *path, const char *arg, ...);
```

- `path` 参数是一个字符串，指定要执行的可执行文件的路径。
- `arg` 参数是一个字符串，指定可执行文件的名称。
- `...` 表示可变参数，用于传递给可执行文件的命令行参数。

在TCP中，`exec()` 函数通常用于服务器进程在接受客户端连接后，创建一个新的子进程来处理与客户端的通信。

具体的作用如下：

1. 服务器通过调用 `fork()` 函数创建一个子进程。子进程是服务器的副本，它继承了父进程的文件描述符、内存等资源。

2. 在子进程中，服务器关闭不需要的文件描述符（如监听套接字），然后调用 `exec()` 函数来加载并执行新的程序。新的程序可以是另一个可执行文件，它负责与客户端进行通信。

3. 在父进程中，服务器可以继续监听其他客户端的连接请求，创建多个子进程来处理不同的客户端连接。

通过使用 `exec()` 函数，服务器可以在子进程中加载不同的程序来处理与客户端的通信。这样可以实现灵活的服务器架构，使得服务器可以根据不同的需求选择不同的处理逻辑。

使用 `exec()` 函数的优势是可以动态地加载不同的程序，而不需要重新编译和链接整个服务器代码。这样可以实现服务器的模块化和可扩展性。

总结起来，`exec()` 函数在TCP连接中的作用是创建一个新的子进程，并用新的程序替换子进程的执行环境，以实现不同的处理逻辑。这样可以让服务器根据不同的客户端需求动态地加载和执行不同的程序，提供灵活和可扩展的服务。

## TIME-WAIT状态

TIME_WAIT状态是TCP协议中的一种状态，它出现在TCP连接的关闭过程中。当一台主机（通常是客户端）主动关闭TCP连接时，它会进入TIME_WAIT状态，并在一段时间内保持这个状态。

TIME_WAIT状态的存在是为了确保网络中的所有分组都能够正常传递和处理完毕，以防止出现连接复用时的混淆。在TIME_WAIT状态下，主机将继续接收来自对端的可能残留的分组，同时等待一段时间，以确保对端收到了自己发送的最后一个确认分组（ACK）。

TIME_WAIT状态的持续时间通常由TCP协议栈的实现决定，一般为2倍的最大报文段生存时间（2MSL，Maximum Segment Lifetime），MSL是一个估计的报文在网络中存活的最长时间。这段时间内，连接的端口号会被保留，以避免与后续连接冲突。

TIME_WAIT状态的作用是：

1. 确保连接的双方都能够正常关闭连接，避免数据的丢失或重复。

2. 防止旧的连接分组在网络中延迟到达并被错误地应用到后续连接。

3. 允许先前连接的所有分组在网络中被丢弃，以确保后续连接不会受到干扰。

在某些情况下，TIME_WAIT状态的持续时间可能会导致一些问题，例如端口资源耗尽或无法快速重启服务。在这种情况下，可以通过调整操作系统的TCP参数来缩短TIME_WAIT状态的持续时间，或使用SO_REUSEADDR套接字选项来允许更快地重用端口。但需要注意，对TIME_WAIT状态的调整应慎重进行，以避免可能的连接混乱和数据冲突。

# 2、基本套接字编程

## 1、iPv4套接字地址结构

在IPv4套接字编程中，使用的地址结构是`struct sockaddr_in`，该结构定义在`<netinet/in.h>`头文件中。`struct sockaddr_in`包含以下成员：

```c
struct sockaddr_in {
    sa_family_t sin_family;     // 地址族，一般为 AF_INET
    in_port_t sin_port;         // 16位端口号，使用网络字节序
    struct in_addr sin_addr;    // 32位IPv4地址，使用网络字节序
    char sin_zero[8];           // 用于填充，一般置为0
};
```

其中，`sa_family`表示地址族，通常为`AF_INET`表示IPv4地址族。

`sin_port`是16位的端口号，用于标识应用程序中的特定服务。通常使用`htons`函数将主机字节序的端口号转换为网络字节序。

`sin_addr`是一个`struct in_addr`类型的结构体，用于保存32位的IPv4地址。`struct in_addr`定义如下：

```c
struct in_addr {
    in_addr_t s_addr;    // 32位IPv4地址，使用网络字节序
};
```

`sin_zero`是一个8字节的填充字段，一般用于对齐。

在套接字编程中，通常使用`struct sockaddr_in`来表示IPv4的地址信息，将其作为参数传递给函数，例如`bind`、`connect`、`accept`等函数。在使用之前，需要将IP地址和端口号进行适当的转换，使其符合网络字节序。

示例：

```c
#include <netinet/in.h>
#include <arpa/inet.h>

int main() {
    struct sockaddr_in server_addr;
    
    // 设置IPv4地址族
    server_addr.sin_family = AF_INET;
    
    // 设置端口号，将本地字节序转换为网络字节序
    server_addr.sin_port = htons(8080);
    
    // 设置IPv4地址，将点分十进制的IP地址转换为网络字节序
    inet_pton(AF_INET, "192.168.0.1", &(server_addr.sin_addr));
    
    // 其他字段置零
    memset(server_addr.sin_zero, 0, sizeof(server_addr.sin_zero));
    
    return 0;
}
```

## 2、字节排序函数

### 1、大小端的区别

大小端（Endianness）是指在存储多字节数据时，字节的顺序排列方式。它决定了多字节数据的最低有效字节（LSB，Least Significant Byte）和最高有效字节（MSB，Most Significant Byte）的存储顺序。

主要有两种类型的大小端：

1. 大端序（Big-Endian）：在大端序中，最高有效字节（MSB）存储在最低的内存地址，最低有效字节（LSB）存储在最高的内存地址。类比人类读写数字时，先读取最高位，再逐渐向低位读取。

2. 小端序（Little-Endian）：在小端序中，最低有效字节（LSB）存储在最低的内存地址，最高有效字节（MSB）存储在最高的内存地址。类比人类读写数字时，先读取最低位，再逐渐向高位读取。

这种字节序的差异主要体现在多字节数据类型（如整数、浮点数）的存储和传输中，特别是在不同的计算机体系结构之间。在网络通信中，一般使用大端序作为网络字节序（Network Byte Order）。

例如，对于16位整数值0x1234来说：

- 在大端序中，高字节0x12存储在低地址，低字节0x34存储在高地址。

- 在小端序中，低字节0x34存储在低地址，高字节0x12存储在高地址。

需要注意的是，大小端只涉及多字节数据的存储方式，对于单个字节的数据（如字符），大小端的问题并不适用。

在实际编程中，当涉及到跨平台数据传输或存储时，需要考虑大小端的问题，以确保数据的正确解析和处理。

#### 大小端的判断函数

``` c
#include <stdio.h>
int main()
{
  union{
    short a=0;
    char b[sizeof(short)];
  }un;
  a=0x0102;
  if(b[0]==1&&b[1]==2)
    printf("bigendian");
  else if(b[0]==2&&b[1]==1)
    printf("smallendian");
  else
    printf("unkonwn");
  return 0;
}
```



### 2、字节序

主机字节序（Host Byte Order）和网络字节序（Network Byte Order）是用于描述数据在不同主机之间传输和存储时的字节顺序。

- 主机字节序：主机字节序是指当前计算机体系结构所使用的字节序。对于大多数个人计算机和服务器，主机字节序通常与处理器的字节序相同。例如，x86架构的计算机通常使用小端序（Little-Endian）作为主机字节序，而PowerPC架构的计算机通常使用大端序（Big-Endian）作为主机字节序。

- 网络字节序：网络字节序是一种标准化的字节序，用于在不同主机之间进行数据传输和协议交互。它使用大端序（Big-Endian）作为统一的字节序。因此，在网络通信中，数据需要以网络字节序进行传输和解析，以确保不同主机之间的互操作性。

在网络编程中，为了保证数据在不同主机之间的正确传输和解析，常常需要进行主机字节序与网络字节序之间的转换。这可以通过使用字节序转换函数（如`htons`、`htonl`、`ntohs`、`ntohl`）来实现，将数据从主机字节序转换为网络字节序进行发送，然后在接收端再将数据从网络字节序转换回主机字节序进行解析和处理。

总结起来，主机字节序是当前计算机所使用的字节序，而网络字节序是一种统一的字节序，用于在网络中进行数据交换和协议通信。字节序转换函数用于在主机字节序和网络字节序之间进行转换，以确保数据的正确性和跨平台的互操作性。

## iPv4和iPv6的转换

`inet_pton`和`inet_ntop`是用于IPv4和IPv6地址转换的函数，用于在网络地址表示形式和字符串表示形式之间进行转换。

1. `inet_pton`（Presentation to Network）函数用于将字符串表示的网络地址转换为用于存储和传输的二进制形式。

```c
#include <arpa/inet.h>

int inet_pton(int af, const char *src, void *dst);
```

- `af`：地址族（Address Family），指定地址的类型，可以是`AF_INET`（IPv4）或`AF_INET6`（IPv6）。
- `src`：包含网络地址的字符串表示形式。
- `dst`：指向存储转换结果的内存缓冲区。

函数返回值为：
- 1：转换成功。
- 0：输入地址无效。
- -1：转换失败，发生错误。

示例用法：

```c
#include <stdio.h>
#include <arpa/inet.h>

int main() {
    const char* ip_address = "192.0.2.1";
    struct in_addr dst;

    if (inet_pton(AF_INET, ip_address, &dst) == 1) {
        printf("IPv4 address: 0x%X\n", dst.s_addr);
    } else {
        printf("Invalid address\n");
    }

    return 0;
}
```

2. `inet_ntop`（Network to Presentation）函数用于将存储和传输的二进制形式网络地址转换为字符串表示形式。

```c
#include <arpa/inet.h>

const char *inet_ntop(int af, const void *src, char *dst, socklen_t size);
```

- `af`：地址族，指定地址的类型，可以是`AF_INET`（IPv4）或`AF_INET6`（IPv6）。
- `src`：指向存储网络地址的二进制形式的内存缓冲区。
- `dst`：指向存储转换结果的字符数组。
- `size`：`dst`缓冲区的大小。

函数返回值为：
- 非空指针：转换成功，返回指向转换结果的指针（即`dst`）。
- 空指针：转换失败，发生错误。

示例用法：

```c
#include <stdio.h>
#include <arpa/inet.h>

int main() {
    struct in_addr addr;
    addr.s_addr = htonl(0xC0000201); // 192.0.2.1

    char ip_address[INET_ADDRSTRLEN];

    if (inet_ntop(AF_INET, &addr, ip_address, INET_ADDRSTRLEN) != NULL) {
        printf("IPv4 address: %s\n", ip_address);
    } else {
        printf("Conversion failed\n");
    }

    return 0;
}
```

这两个函数在网络编程中经常用于将字符串形式的IP地址转换为二进制形式（`inet_pton`），或将二进制形式的IP地址转换为字符串形式（`inet_ntop`）。它们提供了方便的方法来处理网络地址的转换和表示。

# 3、I/O模型

五种io模型：

- 阻塞式：进程调用recvfrom，然后等待数据

- 非阻塞式：轮训（poll），进程反复调用recvfrom等待返回成功指示

- 信号驱动：进程继续进行，建立signo信号处理程序

- 多路复用：，等待数据准备好进程受阻于select（），等待多个套接字的任意一个变为可读

- 异步：

  非阻塞操作：异步 I/O 模型使用非阻塞的 I/O 操作，允许应用程序在 I/O 操作进行中继续执行其他任务，而不需要等待操作完成。

  事件驱动：应用程序通过事件循环不断检查已完成的 I/O 操作或其他事件。

  回调机制：应用程序在发起 I/O 操作时注册回调函数，该函数将在 I/O 操作完成后被调用，用于处理结果或通知应用程序。

# 4、网络检查

## 1、traceroute

Traceroute（跟踪路由）是一种网络诊断工具，用于确定数据包从源主机到目标主机所经过的路径（路由）以及每个跳点的延迟。它通过发送一系列的探测数据包（通常使用 ICMP 协议）并监听返回的数据包来实现。

Traceroute 的工作原理如下：

1. Traceroute 发送探测数据包：Traceroute 在发送数据包时，将初始 TTL（Time to Live）字段设置为 1，并将目标主机作为数据包的目的地发送出去。

2. 探测数据包经过路由器：当数据包经过一个路由器时，该路由器会将 TTL 减 1。如果 TTL 减为 0，路由器将丢弃该数据包，并向源主机发送一个 "Time Exceeded" ICMP 错误消息。

3. 接收 "Time Exceeded" 错误消息：当源主机接收到 "Time Exceeded" 错误消息时，它可以确定该数据包经过了一个路由器，并记录该路由器的 IP 地址。

4. 增加 TTL 值并重复过程：源主机增加 TTL 的值，再次发送探测数据包。这样，数据包会经过下一个路由器，并在每个跳点重复上述过程。

5. 接收目的主机的回复：当探测数据包到达目标主机时，目标主机会向源主机发送一个 ICMP Echo Reply 消息，表示到达目的地。

通过这种方式，Traceroute 将逐步追踪数据包经过的路由器，并测量每个跳点的延迟。它将这些信息显示给用户，以便分析网络的路径和性能。

Traceroute 在网络故障排除、网络优化和网络拓扑映射等方面非常有用。它可以帮助识别网络中的瓶颈、延迟问题或丢包现象，并提供有关网络路径和网络设备的详细信息。

## 2、ping

Ping 是一种网络工具，用于测试主机之间的连通性和测量往返延迟（Round-Trip Time，RTT）。它基于 ICMP 协议（Internet Control Message Protocol）来发送探测数据包并接收响应。

Ping 的工作原理如下：

1. 发送 ICMP Echo Request 数据包：源主机发送一个 ICMP Echo Request 数据包到目标主机。该数据包包含一个唯一的标识符和一个序列号。

2. 目标主机接收 ICMP Echo Request：目标主机接收到 ICMP Echo Request 数据包后，会生成一个 ICMP Echo Reply 数据包作为响应。

3. 目标主机发送 ICMP Echo Reply：目标主机将 ICMP Echo Reply 数据包发送回源主机。该数据包包含相同的标识符和序列号，以便源主机可以将其与发送的请求进行匹配。

4. 源主机接收 ICMP Echo Reply：源主机接收到 ICMP Echo Reply 数据包后，计算往返延迟（RTT），即数据包从源主机发送到目标主机并返回的时间。

5. 显示结果：Ping 工具将显示往返延迟、丢包率以及其他相关信息。通常，Ping 会显示每个数据包的往返延迟时间和丢包情况，并提供统计信息，如平均延迟、最小延迟和最大延迟。

通过发送 ICMP Echo Request 数据包并测量往返时间，Ping 工具可以判断主机之间的连通性和网络性能。如果目标主机能够接收并响应 ICMP Echo Request 数据包，则可以确定两台主机之间的基本连通性。而通过测量往返延迟，可以评估网络的延迟情况，以便诊断网络性能问题。

需要注意的是，有些网络设备或防火墙可能会屏蔽 ICMP Echo Request 数据包，因此在某些情况下，Ping 可能无法正常工作。此外，Ping 工具也可以用于检测主机的存活状态和网络中断的位置，并在网络故障排除和监测中发挥重要作用。

# 5、进程控制

## 1、defunct状态

"Defunct"状态通常指的是在操作系统中的一个进程状态，有时也称为"僵尸进程"（Zombie process）。

当一个进程结束运行时，其进程控制块（PCB）会保留在系统中，以便父进程可以查询其退出状态。在正常情况下，父进程会调用`wait()`或`waitpid()`系统调用来获取子进程的退出状态，并清理子进程的资源。

然而，如果父进程没有正确处理子进程的退出状态，或者父进程已经终止而无法处理子进程的退出状态，那么子进程的PCB就会处于"defunct"或"僵尸"状态。

在"defunct"状态下，子进程已经终止，但其进程描述符和一些元数据仍然存在于系统中。这种状态下的进程不再执行任何代码，并且不占用系统资源。但它们的存在可以通过系统进程表或父进程的进程列表来检测。

"defunct"状态通常是一个临时状态，操作系统会负责清理这些僵尸进程，释放其占用的资源。一旦父进程正确处理了子进程的退出状态，操作系统会将僵尸进程从进程表中移除。

如果系统中存在大量的僵尸进程，可能会导致进程表溢出或系统性能下降。因此，父进程应该适时地处理子进程的退出状态，以避免产生大量的僵尸进程。

# 2、wait函数

`wait()`和`waitpid()`是在Unix/Linux操作系统中用于等待子进程结束并获取其退出状态的系统调用函数。

### 1、wait()函数

`wait()`: 这个系统调用会使调用它的进程（通常是父进程）暂停执行，直到某个子进程结束。当子进程结束后，父进程可以通过`wait()`调用来获取子进程的退出状态。`wait()`函数的原型如下：

```c
#include <sys/types.h>
#include <sys/wait.h>

pid_t wait(int *status);
```

其中`status`参数是一个指向整型变量的指针，用于存储子进程的退出状态。

### 2、waitpid（）函数

`waitpid()`: 这个系统调用与`wait()`类似，但它提供了更灵活的选项来等待指定的子进程结束。通过`waitpid()`，父进程可以指定等待的子进程ID，以及一些额外的选项。`waitpid()`函数的原型如下：

```c
#include <sys/types.h>
#include <sys/wait.h>

pid_t waitpid(pid_t pid, int *status, int options);
```

其中`pid`参数指定要等待的子进程ID。`status`参数用于存储子进程的退出状态，`options`参数提供了一些附加选项，如`WNOHANG`（非阻塞）等。

这两个函数在父进程中使用，用于等待子进程的终止，并获取其退出状态。通过这些函数，父进程可以在适当的时候处理子进程的退出状态，进行进一步的处理或清理工作。

### 区别

- waitpid（）可以查看特定id的子进程的状态

- 没有已经终止的子进程时，waitpid可以不阻塞，但wait会阻塞
- wait函数只能处理一个子进程终止，但是waitpid可以处理多个

# 6、套接字选项

## 1、setsockopt函数

`setsockopt()`是一个用于设置套接字选项的系统调用函数。通过`setsockopt()`函数，可以设置套接字的各种选项，包括网络层、传输层和套接字层的选项。

函数原型如下：

```c
#include <sys/types.h>
#include <sys/socket.h>

int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
```

参数说明：
- `sockfd`：套接字描述符
- `level`：选项的级别，可以是`SOL_SOCKET`（套接字级别）、`IPPROTO_TCP`（TCP协议级别）等
- `optname`：选项的名称，如`SO_REUSEADDR`（地址重用）、`TCP_NODELAY`（禁用Nagle算法）等
- `optval`：指向存储选项值的缓冲区的指针
- `optlen`：选项值的长度

