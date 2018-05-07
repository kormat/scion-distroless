package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_docker//container:container.bzl", "container_bundle")
load("//:variables.bzl", "DOCKER_ORG", "SCION_BASE_DIGEST")

container_bundle(
    name = "app_base",
    images = {
        "%s/app_base" % DOCKER_ORG: "//base",
        "%s/app_base:debug" % DOCKER_ORG: "//base:debug",
    }
)

genrule(
    name = "scion",
    outs = ["scion.tar"],
    cmd = " \
        cntr=$$(docker create scion_app:latest); \
        mkdir -p scion; \
        cd scion; \
        docker cp -a $${cntr}:/home/scion/go/src/github.com/scionproto/scion/bin/ bin/; \
        rm bin/.keepme; \
        docker cp -a $${cntr}:/home/scion/python_local.tar.gz .; \
        docker rm $${cntr}; \
        tar caf ../$@ *;",
)
