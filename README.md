# <img src="https://i.imgur.com/nPCcxts.png" height="25" style="height: 25px"> MCStatus

[![discord chat](https://img.shields.io/discord/936788458939224094.svg?logo=Discord)](https://discord.gg/C2wX7zduxC)
![supported python versions](https://img.shields.io/pypi/pyversions/mcstatus.svg)
[![current PyPI version](https://img.shields.io/pypi/v/mcstatus.svg)](https://pypi.org/project/mcstatus/)
[![Docs](https://img.shields.io/readthedocs/mcstatus?label=Docs)](https://mcstatus.readthedocs.io/)
[![Validation](https://github.com/py-mine/mcstatus/actions/workflows/validation.yml/badge.svg)](https://github.com/py-mine/mcstatus/actions/workflows/validation.yml)
[![Tox test](https://github.com/py-mine/mcstatus/actions/workflows/tox-test.yml/badge.svg)](https://github.com/py-mine/mcstatus/actions/workflows/tox-test.yml)

Mcstatus 提供了一个 API 和命令行脚本来获取 Minecraft 服务器的公开数据。具体来说，mcstatus 通过使用这些协议来检索数据：[Server List Ping](https://wiki.vg/Server_List_Ping) 和 [Query](https://wiki.vg/Query)。因为有了 mcstatus，你不需要完全理解这些协议，而可以直接在自己的程序中快速检索 Minecraft 服务器数据。

## 安装

Mcstatus 可在 [PyPI](https://pypi.org/project/mcstatus/) 上获得，可以简单地通过以下方式安装：

```bash
python3 -m pip install mcstatus
```

## 使用方法

### Python API

#### Java Edition

```python
from mcstatus import JavaServer

# 你可以将你在 Minecraft 的地址字段中输入的相同地址传递给 'lookup' 函数
# 如果你知道主机和端口，可以跳过此步骤，直接使用 JavaServer("example.org", 1234)
server = JavaServer.lookup("example.org:1234")

# 'status' 支持所有版本为 1.7 或更高版本的 Minecraft 服务器。
# 不要期望玩家列表总是完整的，因为许多服务器运行插件来隐藏此信息，限制返回的玩家数量，
# 或者甚至为了自定义消息的目的更改此列表以包含虚假玩家。
status = server.status()
print(f"服务器有 {status.players.online} 个玩家在线，响应时间为 {status.latency} 毫秒")

# 'ping' 支持所有版本为 1.7 或更高版本的 Minecraft 服务器。
# 它包含在 'status' 调用中，但如果你不需要额外的信息，它也可以单独使用。
latency = server.ping()
print(f"服务器响应时间为 {latency} 毫秒")

# 'query' 必须在服务器的 server.properties 文件中启用！
# 它可能提供比 ping 更多的信息，例如完整的玩家列表或模组信息。
query = server.query()
print(f"服务器当前在线的玩家有：{', '.join(query.players.names)}")

```

#### Bedrock Edition

```python
from mcstatus import BedrockServer

# 你可以将你在 Minecraft 的地址字段中输入的相同地址传递给 'lookup' 函数
# 如果你知道主机和端口，可以跳过此步骤，直接使用 BedrockServer("example.org", 19132)
server = BedrockServer.lookup("example.org:19132")

# 'status' 是目前 Bedrock 唯一支持的功能。
# 在这种情况下，status 包含 players.online、latency、motd、map、gamemode 和 players.max。（例如：status.gamemode）
status = server.status()
print(f"服务器有 {status.players.online} 个玩家在线，响应时间为 {status.latency} 毫秒")

```

See the [documentation](https://mcstatus.readthedocs.io) to find what you can do with our library!

### Command Line Interface

mcstatus 库包括一个简单的 CLI。安装后，可以通过以下方式使用：
```bash
python3 -m mcstatus --help
```

## License

Mcstatus 根据 Apache 2.0 许可证授权。详见 LICENSE 完整文本。
