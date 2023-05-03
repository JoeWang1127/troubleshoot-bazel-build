workspace(
    name = "com_google_googleapis",
)   
    
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

##############################################################################
# Common
##############################################################################

load("//:repository_rules.bzl", "switched_rules_by_language")

switched_rules_by_language(
    name = "com_google_googleapis_imports",
    cc = False,
    csharp = False,
    gapic = True,
    go = False,
    grpc = True,
    java = True,
    nodejs = False,
    php = False,
    python = False,
    ruby = False,
)

_bazel_skylib_version = "1.4.0"

_bazel_skylib_sha256 = "f24ab666394232f834f74d19e2ff142b0af17466ea0c69a3f4c276ee75f6efce"

http_archive(
    name = "bazel_skylib",
    sha256 = _bazel_skylib_sha256,
    urls = ["https://github.com/bazelbuild/bazel-skylib/releases/download/{0}/bazel-skylib-{0}.tar.gz".format(_bazel_skylib_version)],
)

# Protobuf depends on very old version of rules_jvm_external.
# Importing older version of rules_jvm_external first (this is one of the things that protobuf_deps() call
# below will do) breaks the Java client library generation process, so importing the proper version explicitly before calling protobuf_deps().
RULES_JVM_EXTERNAL_TAG = "4.5"

RULES_JVM_EXTERNAL_SHA = "b17d7388feb9bfa7f2fa09031b32707df529f26c91ab9e5d909eb1676badd9a6"
    
http_archive(
    name = "rules_jvm_external",
    sha256 = RULES_JVM_EXTERNAL_SHA,
    strip_prefix = "rules_jvm_external-%s" % RULES_JVM_EXTERNAL_TAG,
    url = "https://github.com/bazelbuild/rules_jvm_external/archive/%s.zip" % RULES_JVM_EXTERNAL_TAG,
)

load("@rules_jvm_external//:repositories.bzl", "rules_jvm_external_deps")
rules_jvm_external_deps()
load("@rules_jvm_external//:setup.bzl", "rules_jvm_external_setup")
rules_jvm_external_setup()

# Python rules should go early in the dependencies list, otherwise a wrong
# version of the library will be selected as a transitive dependency of gRPC.
http_archive(
    name = "rules_python",
    strip_prefix = "rules_python-0.9.0",
    url = "https://github.com/bazelbuild/rules_python/archive/0.9.0.tar.gz",
)

http_archive(
    name = "rules_pkg",
    sha256 = "8a298e832762eda1830597d64fe7db58178aa84cd5926d76d5b744d6558941c2",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_pkg/releases/download/0.7.0/rules_pkg-0.7.0.tar.gz",
        "https://github.com/bazelbuild/rules_pkg/releases/download/0.7.0/rules_pkg-0.7.0.tar.gz",
    ],
)

load("@rules_pkg//:deps.bzl", "rules_pkg_dependencies")

rules_pkg_dependencies()

http_archive(
    name = "com_google_protobuf",
    sha256 = "930c2c3b5ecc6c9c12615cf5ad93f1cd6e12d0aba862b572e076259970ac3a53",
    strip_prefix = "protobuf-3.21.12",
    urls = ["https://github.com/protocolbuffers/protobuf/archive/v3.21.12.tar.gz"],
)

load("@com_google_protobuf//:protobuf_deps.bzl", "protobuf_deps")

protobuf_deps()

http_archive(
    name = "rules_proto",
    sha256 = "602e7161d9195e50246177e7c55b2f39950a9cf7366f74ed5f22fd45750cd208",
    strip_prefix = "rules_proto-97d8af4dc474595af3900dd85cb3a29ad28cc313",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_proto/archive/97d8af4dc474595af3900dd85cb3a29ad28cc313.tar.gz",
        "https://github.com/bazelbuild/rules_proto/archive/97d8af4dc474595af3900dd85cb3a29ad28cc313.tar.gz",
    ],
)

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")

rules_proto_dependencies()

rules_proto_toolchains()

_rules_gapic_version = "0.24.0"

_rules_gapic_sha256 = "be9b551b22b05755efddfd8ba97102d3191364c85eb8adc7cc6c466859bfa52e"

http_archive(
    name = "rules_gapic",
    sha256 = _rules_gapic_sha256,
    strip_prefix = "rules_gapic-%s" % _rules_gapic_version,
    urls = ["https://github.com/googleapis/rules_gapic/archive/v%s.tar.gz" % _rules_gapic_version],
)

load("//:lts_config.bzl", "lts_rules")

lts_rules(name = "java_lts_rules")
    
load("@java_lts_rules//:lts_config.bzl", "GAPIC_GENERATOR_JAVA_VERSION", "LTS")

print("LTS = {}".format(LTS))

_gapic_generator_java_version = GAPIC_GENERATOR_JAVA_VERSION
# _gapic_generator_java_version = "2.18.0"

print("_gapic_generator_java_version = {}".format(_gapic_generator_java_version))

load("@rules_jvm_external//:defs.bzl", "maven_install")
maven_install(
    artifacts = [
        "com.google.api:gapic-generator-java:" + _gapic_generator_java_version,
    ],
    #Update this False for local development
    fail_on_missing_checksum = True,
    repositories = [
        "m2Local",
        "https://repo.maven.apache.org/maven2/",
    ],
) 

http_archive(
    name = "gapic_generator_java",
    strip_prefix = "gapic-generator-java-%s" % _gapic_generator_java_version,
    urls = ["https://github.com/googleapis/gapic-generator-java/archive/v%s.zip" % _gapic_generator_java_version],
)

# gax-java is part of gapic-generator-java repository
http_archive(
    name = "com_google_api_gax_java",
    strip_prefix = "gapic-generator-java-%s/gax-java" % _gapic_generator_java_version,
    urls = ["https://github.com/googleapis/gapic-generator-java/archive/v%s.zip" % _gapic_generator_java_version],
)

load("@com_google_api_gax_java//:repository_rules.bzl", "com_google_api_gax_java_properties")

com_google_api_gax_java_properties(
    name = "com_google_api_gax_java_properties",
    file = "@com_google_api_gax_java//:dependencies.properties",
)

load("@com_google_api_gax_java//:repositories.bzl", "com_google_api_gax_java_repositories")

com_google_api_gax_java_repositories()

load("@io_grpc_grpc_java//:repositories.bzl", "grpc_java_repositories")

grpc_java_repositories()
