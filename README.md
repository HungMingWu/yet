# Yet - rtmp / http-flv 服务器

[English readme click me](./README.EN.md)

2019年写的一个c++11 rtmp服务器。欢迎star fork watch，issue提问题或bug或需求。目标是实现一个高性能、可读可维护、模块化的流媒体服务器。后续会补充设计文档，源码文档，使用文档等。

### 大体已完成

* rtmp pub: 推rtmp流至`yet`
* rtmp sub: 从`yet`拉取rtmp流
* http-flv sub: 从`yet`拉取http-flv流
* rtmp pull 回源: 当客户端从`yet`拉流而该流不存在时，从其他服务器拉取rtmp流
* rtmp push 转推: 当客户端推rtmp流至`yet`时，将流转推至其他服务器
* gop缓存

### 下一步开发路线图

~

### 配置文件说明

```
{
  "rtmp_server_ip": "0.0.0.0",        // rtmp推流和拉流监听ip
  "rtmp_server_port": 1935,           // rtmp推流和拉流监听端口
  "http_flv_server_ip": "0.0.0.0",    // http-flv拉流监听ip
  "http_flv_server_port": 8080,       // http-flv拉流监听端口
  "rtmp_pull_host": "www.google.com", // rtmp回源。如果这个配置项存在，那么当所拉的流不存在时，yet会从这个配置域名回源（拉取rtmp流）
  "rtmp_push_host": "www.google.com"  // rtmp转推。如果这个配置项存在，那么当有推流时，yet会转推一份流去这个配置域名
}
```

### 依赖

所有依赖的第三方库都为头文件实现，它们不需要单独配置或编译，而且已经源码包含在yet中，直接可以使用。
所以简单来说，你只需要一个支持c++11的编译器，并且安装好cmake就可以完成yet的编译。

* asio
* spdlog
* nlohmann/json

### 编译

#### cmake

```
# 先进入yet工程的根目录
$./build.sh
# 启动
$./output/bin/yet ./conf/yet.json.conf
```

#### xcode

```
# 先进入yet工程的根目录
$./gen_xcode.sh
# 脚本执行完成后会在根目录下生成 xcode 目录，使用 xcode 打开 xcode/yet.xcodeproj 即可
```

### 我的环境

```
OS X EI Capitan 10.11.6
Apple LLVM version 8.0.0 (clang-800.0.42.1)

Linux xxx 2.6.32-642.15.1.el6.x86_64 #1 SMP Fri Feb 24 14:31:22 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
gcc version 7.1.0 (GCC)
```

### 性能

做了一个简单、粗略的推拉流压力测试，让心里对yet的表现大致有数。
后续会做更详细的对比（比如延时，秒开，流畅度，瞬时处理大量连接建立、断开的处理能力，多路推、多路拉的场景），以及优化，以及和其他rtmp server的对比。

压力程序和压力测试文件使用srs-bench。

以下为单线程版本yet和单进程版本nginx的cpu、内存使用情况对比。

表格一为推n路流。
表格二为推1路流，拉n路流。

| 推流数 | 单线程版本yet | 单进程版本nginx  |
| - | - | - |
|100  | 3.7%/13m | 3.7%/13m |
|1000 | 24%/133m | 25%/77m  |

| 拉流数 | 单线程版本yet | 单进程版本nginx  |
| - | - | - |
|100  | 2.7%/14m | 1.7%/8k |
|1000 | 31%/118m | 18%/22m |

### 其他
