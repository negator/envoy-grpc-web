# envoy-grpc-web

This runs on a mac, with Docker For Mac and Bazel installed.


On a command line run:
`bazel run //:helloworld`

In a separate terminal run:
`curl -v 'http://localhost:6552/' -H 'X-User-Agent: grpc-web-javascript/0.1' -H 'Origin: http://localhost:8080' -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept-Language: en-US,en;q=0.9' -H 'Content-Type: application/grpc-web-text' -H 'Accept: application/grpc-web-text' --data-binary 'AAAAAAA='`

You can view envoy upstream and downstream logs with:
`docker logs envoy_upstream` and `docker logs envoy_downstream`
