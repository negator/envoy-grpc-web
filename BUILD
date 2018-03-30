load("@io_bazel_rules_go//go:def.bzl", "go_binary")

exports_files(["envoy_upstream.yaml", "envoy_downstream.yaml"])

go_binary(
    name = "helloworld",
    srcs = [
        "main.go"
    ],
    deps = [
        "@org_golang_google_grpc//:go_default_library",
        "@org_golang_google_grpc//examples/helloworld/helloworld:go_default_library",
        "@org_golang_google_grpc//reflection:go_default_library",
        "@org_golang_x_net//context:go_default_library"
    ],
    data = [
        "envoy_downstream",
        "envoy_upstream"
    ]
)


#### Downstream ####
genrule(
    name = "envoy_downstream",
    outs = ["envoy-upstream"],
    srcs = ["//conf:envoy_downstream.yaml"],
    cmd = "docker rm -f envoy_downstream || true; docker run -d -p 6552:6552 -v `(pwd)`/`dirname $(location //conf:envoy_downstream.yaml)`:/usr/local/envoy/conf --name envoy_downstream envoyproxy/envoy:4dd49d8809f7aaa580538b3c228dd99a2fae92a4 envoy -l trace -c /usr/local/envoy/conf/envoy_downstream.yaml > $@; sleep 5",
    local = 1,    
    visibility = ["//visibility:public"],
)

#### Upstream ####
genrule(
    name = "envoy_upstream",
    outs = ["envoy-downstream"],
    srcs = ["//conf:envoy_upstream.yaml"],
    cmd = "docker rm -f envoy_upstream || true; docker run -d -p 6553:6553 -v `(pwd)`/`dirname $(location //conf:envoy_upstream.yaml)`:/usr/local/envoy/conf --name envoy_upstream envoyproxy/envoy:4dd49d8809f7aaa580538b3c228dd99a2fae92a4 envoy -l trace -c /usr/local/envoy/conf/envoy_upstream.yaml > $@; sleep 5",
    local = 1,    
    visibility = ["//visibility:public"],
)
