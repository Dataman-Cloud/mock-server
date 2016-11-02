# mock-server
mock-server is a handy mock server tool for REST API service dependency in unit testing

## senario

## usage

## example
```
mockServer := NewServer()
defer mockServer.Close()

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
```
