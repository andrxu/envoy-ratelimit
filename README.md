# Envoy Rate Limit Service

This repository demonstrates how the Envoy rate limit service works without including too many details. You may want to refer to https://github.com/envoyproxy/ratelimit for detailed documentation.

```
                        +-------+                                           
                        |       |    +-------------------+                  
 incoming request ->    | Proxy +->--+   API (httpbin)   |                  
                :8888   |       |    +-------------------+                  
                        +---+---+                                           
                            |                                              
                            |        +---------------------+   +-----------+
                            |        |                     |   |           |
     grpc client  ->        +-->-----+ Rate Limit Service  +->-+   Redis   |
                :8081                |                     |   |           |
                                     +----------+----------+   +-----------+
                                                ^
                                             (rules) 
```

## How to run this 

Set up everything first: 
```
docker-compose up
```

### Tests

Run the following command multiple times:
```
curl -v http://localhost:8888/test
```
Expected behavior:
```
HTTP code 200
HTTP code 429 
```

Due to the [rules](./ratelimit/rules.yaml#L13-L27):

```
  - key: source_cluster
    value: proxy
    descriptors:
      - key: destination_cluster
        value: httpbin_cluster
        rate_limit:
          unit: minute
          requests_per_unit: 1

```

Run the following command multiple times:

```
 curl -v -H "foo:f1"  http://localhost:8888/header
```

Expected behavior:
```
HTTP code 200
HTTP code 200
HTTP code 429 
```

Due to the [rules](./ratelimit/rules.yaml#L41-L44):
```
  - key: foo
    rate_limit:
      unit: minute
      requests_per_unit: 2
```

Run go client:
```
  cd client
  make test
```
