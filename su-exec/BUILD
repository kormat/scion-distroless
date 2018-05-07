package(default_visibility = ["//visibility:public"])

genrule(
    name = "su-exec",
    srcs = ["@suexec_source//file"],
    outs = ["suexec.tar"],
    cmd = "tar xaf $(<) --strip-components=1; make su-exec; \
        mkdir sbin; mv su-exec sbin; tar caf $@ sbin",
)
