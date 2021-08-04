
# Service Management

## gRPC Timeout

The concurrency per host can be measured by:
$$
Concurrency=QPS_{max}\times Timeout\_period(sec)
$$
A gRPC call might be in hidden danger when:
$$
Thread\_count \lt Concurrency \times buffer
$$
$buffer$ refers to free resource of threads, mainly depends on how many  interfaces your service has, how many rpc services your service relies on and how important your service is.(Recommanded value is 2, half of the threads are avaliable at least) 

Adjust timeout period to make sure:
$$
Thread\_count \lt QPS_{max}\times Timeout\_period(sec) \times buffer
$$


## Circuit Breaker

Recommended value:

1. Threshold: Circuit breaker works when gRPC success ratio is below 60%.
2. Waiting time: Calculate gRPC success ratio every 30s.
3. Moving window: Calculate gRPC success ratio only when the request count > 20 within a 30s window.

