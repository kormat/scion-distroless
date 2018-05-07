package(default_visibility = ["//visibility:public"])

genrule(
    name = "toybox",
    srcs = ["@toybox//file"],
    outs = ["toybox.tar"],
    cmd = "mkdir -p sbin bin usr/bin usr/sbin; cp $(<) bin/toybox; \
           for i in $$(bin/toybox --long); do ln -sr bin/toybox $$i; done; \
           tar caf $@ sbin bin usr/bin usr/sbin",
)
