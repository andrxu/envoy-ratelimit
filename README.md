# Envoy-based Rate Limit Service

This repository demonstrates how the Envoy rate limit service works.

```
                        +-------+                                           
                        |       |    +-------------------+                  
 incoming request ->    | Proxy +->--+   API (httpbin)   |                  
                :8888   |       |    +-------------------+                  
                        +---+---+                                           
                            |                                              
                            |        +---------------------+   +-----------+
                            |        |                     |   |           |
                  ->        +-->-----+ Rate Limit Service  +->-+   Redis   |
                             :8080   |                     |   |           |
                             :8081   +---------------------+   +-----------+
```

