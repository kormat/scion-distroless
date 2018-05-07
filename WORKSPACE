workspace(name = "scion_distroless")

load("//:variables.bzl", "SU_EXEC_COMMIT", "SU_EXEC_SHA256")

git_repository(
    name = "io_bazel_rules_go",
    remote = "https://github.com/bazelbuild/rules_go.git",
    tag = "0.11.1",
)

load("@io_bazel_rules_go//go:def.bzl", "go_rules_dependencies", "go_register_toolchains")

go_rules_dependencies()

go_register_toolchains()

git_repository(
    name = "distroless",
    commit = "813d1ddef217f3871e4cb0a73da100aeddc638ee",
    remote = "https://github.com/GoogleContainerTools/distroless.git",
)

load("@distroless//package_manager:package_manager.bzl",
    "package_manager_repositories",
    "dpkg_src",
    "dpkg_list",
)

package_manager_repositories()

dpkg_src(
    name = "debian_stretch",
    arch = "amd64",
    distro = "stretch",
    sha256 = "4cb2fac3e32292613b92d3162e99eb8a1ed7ce47d1b142852b0de3092b25910c",
    snapshot = "20180406T095535Z",
    url = "http://snapshot.debian.org/archive",
)

dpkg_src(
    name = "debian_stretch_backports",
    arch = "amd64",
    distro = "stretch-backports",
    sha256 = "2863af9484d2d6b478ef225a8c740dac9a14015a594241a0872024c873123cdd",
    snapshot = "20180406T095535Z",
    url = "http://snapshot.debian.org/archive",
)

dpkg_src(
    name = "debian_stretch_security",
    package_prefix = "http://snapshot.debian.org/archive/debian-security/20180405T165926Z/",
    packages_gz_url = "http://snapshot.debian.org/archive/debian-security/20180405T165926Z/dists/stretch/updates/main/binary-amd64/Packages.gz",
    sha256 = "a503fb4459eb9e862d080c7cf8135d7d395852e51cc7bfddf6c3d6cc4e11ee5f",
)

dpkg_list(
    name = "package_bundle",
    packages = [
        # Version required to skip a security fix to the pre-release library
        # TODO: Remove when there is a security fix or dpkg_list finds the recent version
        "libc6=2.24-11+deb9u3",
        "ca-certificates",
        "openssl",
        "libssl1.0.2",
        "libssl1.1",
        "libbz2-1.0",
        "libdb5.3",
        "libffi6",
        "libncursesw5",
        "liblzma5",
        "libexpat1",
        "libreadline7",
        "libtinfo5",
        "libsqlite3-0",
        "mime-support",
        "netbase",
        "readline-common",
        "tzdata",

        #c++
        "libgcc1",
        "libgomp1",
        "libstdc++6",

        #java
        "zlib1g",
        "openjdk-8-jre-headless",

        #python
        "libpython2.7-minimal",
        "python2.7-minimal",
        "libpython2.7-stdlib",

        #python3
        "libpython3.5-minimal",
        "python3.5-minimal",
        "libpython3.5-stdlib",

        #local
        "libcapnp-dev",
        "strace",
    ],
    # Takes the first package found: security updates should go first
    # If there was a security fix to a package before the stable release, this will find
    # the older security release. This happened for stretch libc6.
    sources = [
        "@debian_stretch_security//file:Packages.json",
        "@debian_stretch_backports//file:Packages.json",
        "@debian_stretch//file:Packages.json",
    ],
)

# For the debug image
http_file(
    name = "toybox",
    urls = ["http://www.landley.net/toybox/downloads/binaries/0.7.6/toybox-x86_64"],
    sha256 = "b6fbc37f93c504fcf4520b4121ebfde651f917ff4c62b72130a92041f1d23914",
    executable = True,
)

# Docker rules.
git_repository(
    name = "io_bazel_rules_docker",
    remote = "https://github.com/bazelbuild/rules_docker.git",
    commit = "e4df7a3b11ebfa419a8f8b6392b70b6fe9d49702",
)

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
    container_repositories = "repositories",
)

container_repositories()

git_repository(
    name = "runtimes_common",
    remote = "https://github.com/GoogleCloudPlatform/runtimes-common.git",
    tag = "v0.1.0",
)

http_file(
    name = "suexec_source",
    url = "https://github.com/kormat/su-exec/archive/customize.tar.gz",
    sha256 = SU_EXEC_SHA256,
)
