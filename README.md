# mock-server
mock-server is a handy mock server tool for REST API service dependency in unit testing.  
It sets up a real server with custom registerd routers.  
User is able to customize different response status code and body as they want.  
This is very useful for unit testing.  
User can return different response status codes and bodies as needed to improve testing coverage.  
For further usage, please refer to source code: [server.go](https://github.com/Dataman-Cloud/mock-server/blob/master/server/server.go)  

## Features
1. It supports multiple data format(string, interface, file, io.Reader) when register custom request body and response body.
2. It supports return different status codes.
3. It supports one request url with different response codes and bodies based on request query and request url. 

## Installation
Once you have [installed Go](https://golang.org/doc/install#releases), run these commands to install the mock-server package:
```
go get github.com/Dataman-Cloud/mock-server/server
```

## Usage Example
```
mockServer := NewServer()
defer mockServer.Close()

//server address: mockServer.Addr
//server listen port: mockServer.Port

mockServer.AddRouter("/_ping", "get").RGroup().
	Reply(200)

envs := map[string]interface{}{
                "Version":       "1.10.1",
                "ApiVersion":    "1.22",
        }

//add a router with requested url and custom response
mockServer.AddRouter("/version", "get").RGroup().
        Reply(200).
        WJSON(envs)
mockServer.AddRouter("/")

//register different responses based on request query/body
router1 := mockServer.AddRouter("/tasks", "get")
router1.RGroup().
        RQuery(`filters={"service":{"9mnpnzenvg8p8tdbtq4wvbkcz":true}}`).
        Reply(200).
        WFile("./tasks.json")
router1.RGroup().
	RQuery("filters={}")
        Reply(400)

//this line of code is needed after add router,otherwise mock server does not work.
mockServer.Register()
```
