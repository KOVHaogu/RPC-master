# Yin.RPC

[![Java](https://img.shields.io/badge/Java-8+-orange.svg)](https://www.oracle.com/java/)
[![Netty](https://img.shields.io/badge/Netty-4.1.6-blue.svg)](https://netty.io/)
[![Spring](https://img.shields.io/badge/Spring-4.3.9-green.svg)](https://spring.io/)
[![ZooKeeper](https://img.shields.io/badge/ZooKeeper-3.4.6-red.svg)](https://zookeeper.apache.org/)

> 一个基于 Netty、ZooKeeper、Spring 的轻量级 RPC 框架

Yin.RPC 是一个轻量级、高性能的分布式远程过程调用（RPC）框架，专为 Java 应用程序设计。它采用现代化的网络通信技术和服务发现机制，提供简单易用的注解驱动开发体验。

## ✨ 核心特性

- 🚀 **高性能网络通信** - 基于 Netty 4.x 实现，支持长连接和异步调用
- 💗 **智能健康检测** - 内置心跳检测机制，确保连接稳定性
- 🔄 **JSON 序列化** - 使用 FastJSON 进行高效的数据序列化
- 📝 **注解驱动** - 接近零配置，基于注解实现服务调用
- 🗂️ **服务注册中心** - 基于 ZooKeeper 实现服务注册与发现
- 🔌 **动态连接管理** - 支持客户端连接的动态管理和监听
- 🔍 **服务发现** - 自动发现和监听服务变化
- 🏗️ **Spring 集成** - 无缝集成 Spring 框架

## 🏗️ 项目架构

```
Yin.RPC/
├── Yin.rpc/          # RPC 服务端模块
│   ├── annotation/   # 注解定义
│   ├── server/       # 服务器实现
│   ├── handler/      # 网络处理器
│   ├── medium/       # 中间件组件
│   └── model/        # 数据模型
└── Yin.consumer/     # RPC 客户端模块
    ├── core/         # 核心客户端功能
    ├── proxy/        # 动态代理
    ├── handler/      # 客户端处理器
    └── zk/           # ZooKeeper 集成
```

## 🚀 快速开始

### 服务端开发

#### 1. 创建服务接口

```java
public interface TestRemote {
    Response testUser(User user);  
}
```

#### 2. 实现服务类

```java
@Remote
public class TestRemoteImpl implements TestRemote {
    @Resource
    private TestService service;
    
    public Response testUser(User user) {
        service.test(user);
        Response response = ResponseUtil.createSuccessResponse(user);
        return response;
    }
}
```

#### 3. 创建业务服务

```java
@Service
public class TestService {
    public void test(User user) {
        System.out.println("调用了TestService.test");
    }
}
```

### 客户端开发

#### 1. 定义远程接口

```java
public interface TestRemote {
    Response testUser(User user);
}
```

#### 2. 使用远程服务

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=RemoteInvokeTest.class)
@ComponentScan("\\")
public class RemoteInvokeTest {
    
    @RemoteInvoke
    public static TestRemote userremote;
    
    @Test
    public void testSaveUser() {
        User user = new User();
        user.setId(1000);
        user.setName("张三");
        userremote.testUser(user);
    }
}
```

## 📊 性能测试

| 调用次数 | 平均响应时间 | 说明 |
|---------|-------------|------|
| 10,000  | < 1ms       | 小规模调用 |
| 100,000 | < 2ms       | 中等规模调用 |
| 1,000,000 | < 5ms     | 大规模调用 |

## 🔧 技术栈

- **网络通信**: Netty 4.1.6.Final
- **序列化**: FastJSON 1.2.20
- **服务注册**: Apache ZooKeeper 3.4.6
- **服务治理**: Apache Curator 2.4.2
- **框架集成**: Spring 4.3.9.RELEASE
- **构建工具**: Maven
- **测试框架**: JUnit 4.12

## 📋 系统要求

- Java 8+
- Maven 3.0+
- ZooKeeper 3.4+

## 🔧 安装配置

### 1. 克隆项目

```bash
git clone https://github.com/KOVHaogu/Yin.RPC.git
cd Yin.RPC
```

### 2. 编译项目

```bash
# 编译服务端模块
cd Yin.rpc
mvn clean compile

# 编译客户端模块
cd ../Yin.consumer
mvn clean compile
```

### 3. 启动 ZooKeeper

```bash
# 下载并启动 ZooKeeper
zkServer.sh start
```

### 4. 运行服务端

```bash
cd Yin.rpc
mvn exec:java -Dexec.mainClass="server.SpringServer"
```

### 5. 运行客户端测试

```bash
cd Yin.consumer
mvn test
```

## 📖 使用指南

### 注解说明

- `@Remote`: 标记远程服务实现类
- `@RemoteInvoke`: 标记需要注入的远程服务接口
- `@Service`: Spring 服务注解

### 配置说明

框架采用约定优于配置的设计理念，大部分配置都有默认值：

- **ZooKeeper 连接**: `localhost:2181`
- **服务端口**: `8080`
- **序列化方式**: JSON
- **连接池大小**: 自动管理

## 🤝 贡献指南

欢迎贡献代码！请遵循以下步骤：

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📝 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情

## 📞 联系方式

如有问题或建议，请发送邮件至：1035090753@qq.com

## 🙏 致谢

感谢以下开源项目：

- [Netty](https://netty.io/) - 高性能网络应用框架
- [Apache ZooKeeper](https://zookeeper.apache.org/) - 分布式协调服务
- [Spring Framework](https://spring.io/) - 企业级应用框架
- [FastJSON](https://github.com/alibaba/fastjson) - 高性能 JSON 库

---

⭐ 如果这个项目对你有帮助，请给个 Star！