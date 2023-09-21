map: https://github.com/tunnima/my-docs

# Installation:
https://github.com/slince/spike-go/releases
```text
tar -zxf spiked.....
mv linux_amd64* spike
```
# Configuration:
## Server A.A.A.A:
```text
./spiked init
```
**vi spiked.yaml**
```text
host: A.A.A.A
port: 8001
users:
  - username: nira
    password: irabhaetyeBoomari
log:
#  console: true
  level: warn
  file: "./spiked.log"
```
**And at last:**
```text
./spiked --config=./spiked.yaml
```
## Client B.B.B.B:
```text
./spike init
```
**vi spike.yaml**
```text
host: A.A.A.A # server host
port: 8001
user:
     username: nira
     password: irabhaetyeBoomari

log:
     console: true  # enable console output
     level: warn  # trace debug info warn error
     file: ./spike.log # generate log file

tunnels:
  - protocol: tcp
    local_port: 4445
    server_port: 3125

#  - protocol: udp
#    local_host: 8.8.8.8
#    local_port: 53
#    server_port: 6202

#  - protocol: http
#    local_port: 80
#    server_port: 6203
#    headers:
#      x-spike: yes
```
**And at last:**
```text
./spike --config=./spike.yaml
```
# MAIN

<p align="center">
    <img src="https://raw.githubusercontent.com/slince/spike/master/resources/logo.png" width="200"/>
</p>

Spike is a fast reverse proxy written in golang that helps to expose your local services to the internet.

[简体中文](./README-zh_CN.md)

## Installation

Download the latest programs from [Release](https://github.com/slince/spike-go/releases) according to your operating system and architecture.

## Schematic diagram

<p align="center">
    <img src="https://raw.githubusercontent.com/slince/spike-go/master/etc/diagram.png"/>
</p>

## Configure the server

A public machine that can be accessed on the internet is needed. Assuming already. There are two ways to start the server
 
### Based on defaults

Use the following command to start the server

```bash
$ spiked -p 6200
```

The above command can create a basic service. If you want to customize more information, you should start the server based on
the configuration file.

### Based on the configuration file.

- Creates a configuration file

Execute the following command to create it.

```bash
$ spiked init
```

Yaml,Xml,Ini and Json(default) files are supported. Use the following command for help.


```bash
$ spiked init -h
```

- Open the configuration file and modify the parameters.

- Executes the following command to start the service.
 
```bash
 $ spiked --config=/home/conf/spiked.yaml
```

## Configure the client.

You should first create a configuration file for the client.

- Execute the following command to create it

```bash
$ spike init
```
Use the following command for help about this command

```bash
$ spike init -h
```

- Open the configuration file and modify the parameters.

- Start the client service.
 
```bash
$ spike --config=/home/conf/spike.yaml
```


## Tunnel

Tunnels only need to be defined on the client side, The server does not need to do anything.

> Now supports tcp udp and http

Open the configuration file for the client and modify the parameters for "tunnel".

```yaml
tunnels:
  - protocol: tcp
    local_port: 3306
    server_port: 6201

  - protocol: udp
    local_host: 8.8.8.8
    local_port: 53
    server_port: 6202

  - protocol: http
    local_port: 80
    server_port: 6203
    headers:
      x-spike: yes
```

Restarts the client service. 

1. Visit `http://{SERVER_IP}:6203`, the service will be forwarded to the local `127.0.0.1:80`.
2. The services based on the tcp can use the tunnel, such as: mysql, redis, ssh and so on; The following is an example of proxy mysql service
    
    Execute the following command to visit the local mysql service.

    ```bash
    $ mysql -h {SERVER_IP} -P 6201
    ```

## Client authentication

The authentication is not enabled on the server based on defaults.You should start the server based on configuration file,
if you want to enable this.

- Enable authentication

Open the configuration file "spiked.yaml" for the server and modify parameters for `users` and restart the service.

```yaml
users:
  - username: admin
    password: admin
```
- Modify the client identity information

```yaml
user:
  username: admin
  password: admin
```

Open the configuration file for the client and modify parameters for "auth". Keep the same parameters as the server.

## Configure log

The default to open the console and file two forms of the log; the first will print the logs to the console; the second 
will write all the logs to the specified file;  Default log level is "info"; You can adjust this in the configuration file.

## List Commands

```bash
$ spike list
 _____   _____   _   _   _    _____
/  ___/ |  _  \ | | | | / /  | ____|
| |___  | |_| | | | | |/ /   | |__
\___  \ |  ___/ | | | |\ \   |  __|
 ___| | | |     | | | | \ \  | |___
/_____/ |_|     |_| |_|  \_\ |_____|

Usage:
  spike [flags]
  spike [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  help        Help about any command
  init        Create a configuration file in the current directory
  version     Print spike version
  view-proxy  Show proxy of the server

Flags:
      --config string     Config file (default is Current dir/spike.yaml) (default "**/spike.yaml")
  -h, --help              help for spike
  -H, --host string       Server host (default "127.0.0.1")
  -p, --password string   Password for the given user (default "admin")
  -P, --port int          Server port (default 6200)
  -u, --username string   User for login (default "admin")

Use "spike [command] --help" for more information about a command.
```

## Changelog

See [CHANGELOG.md](./CHANGELOG.md)

## License
 
The MIT license. See [MIT](https://opensource.org/licenses/MIT)
