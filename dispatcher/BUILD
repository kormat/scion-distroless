package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_docker//container:container.bzl", "container_image")

env = {
    "ZLOG_CFG": "/share/conf/dispatcher.zlog.conf",
}

container_image(
    name = "dispatcher",
    base = "//base",
    cmd = ["/app/dispatcher"],
    env = env,
    tars = ["dispatcher_app.tar"],
)

container_image(
    name = "debug",
    base = ":dispatcher",
    tars = ["//toybox"],
)

genrule(
    name = "dispatcher_tar",
    srcs = ["//:scion"],
    outs = ["dispatcher_app.tar"],
    tools = ["//tools:tar_extract_file"],
    cmd = "mkdir -p app; ./$(location //tools:tar_extract_file) $(<) bin/dispatcher app; tar cf $@ app"
)
