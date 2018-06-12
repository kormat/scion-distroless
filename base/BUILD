package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_docker//container:container.bzl", "container_image")
load("@package_bundle//file:packages.bzl", "packages")

container_image(
    name = "base",
    debs = [
        packages["libc6"],
        packages["strace"], # Too valuable not to have in the base image.
    ],
    env = {"PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"},
    tars = [
        "@distroless//base:tmp.tar",
        "@distroless//base:nsswitch.tar",
        "@debian_stretch//file:os_release.tar",
        "//su-exec",
    ],
    workdir = "/share",
    entrypoint = ["/sbin/su-exec"],
    creation_time = "{BUILD_TIMESTAMP}",
    stamp = True,
)

# A debug image with toybox available.
container_image(
    name = "debug",
    base = ":base",
    tars = ["//toybox"],
)
