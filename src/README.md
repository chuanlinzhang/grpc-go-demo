1. 先安装Protobuf 编译器 protoc，下载地址：https://github.com/google/protobuf/releases
   我的是windows，下载win的压缩包，将压缩包bin目录下的exe放到环境PATH目录中即可
2. 然后获取插件支持库
  // gRPC运行时接口编解码支持库
  go get -u github.com/golang/protobuf/proto
  // 从 Proto文件(gRPC接口描述文件) 生成 go文件 的编译器插件
  go get -u github.com/golang/protobuf/protoc-gen-go
3. 获取go的gRPC包（翻墙即可顺利下载）
  go get google.golang.org/grpc
4. 接口文件 /src/
    syntax = "proto3";
    // 定义包名
    package test;
    
    // 可以定义多个服务，每个服务内可以定义多个接口
    service Waiter {
      // 定义接口 (结构体可以复用)
      // 方法 (请求消息结构体) returns (返回消息结构体) {}
      rpc DoMD5 (Req) returns (Res) {}
    }
  
    // 定义 Req 消息结构
    message Req {
      // 类型 字段 = 标识号
      string jsonStr = 1;
    }
  
    // 定义 Res 消息结构
    message Res {
      string backJson = 1;
    }
  // PS：jsonStr和backJson只是随手写的名字，并没有用json
5. 然后将proto文件编译为go文件
  // protoc --go_out=plugins=grpc:{输出目录}  {proto文件}
  protoc --go_out=plugins=grpc:./test/ ./test.proto
  or
  rotoc --go_out=plugins=grpc:./test/ *.proto（当前目前含有proto文件）


参考链接：https://www.jianshu.com/p/20ed82218163