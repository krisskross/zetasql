#
# Copyright 2019 ZetaSQL Authors
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

package(default_visibility = ["//zetasql:__subpackages__"])

load("//zetasql/resolved_ast:build_rules.bzl", "gen_resolved_ast_files")

py_binary(
    name = "gen_resolved_ast",
    srcs = [
        "gen_resolved_ast.py",
    ],
    deps = [
        ":resolved_ast_enums_py_pb2",
        "@io_abseil_py//absl:app",
        "@io_abseil_py//absl/flags",
        "@io_abseil_py//absl/logging",
        "@jinja//:jinja2",
        "@markupsafe//:markupsafe",
    ],
)

gen_resolved_ast_files(
    name = "run_gen_resolved_ast_proto",
    srcs = [
        "resolved_ast.proto.template",
        "resolved_node_kind.proto.template",
    ],
    outs = [
        "resolved_ast.proto",
        "resolved_node_kind.proto",
    ],
)

gen_resolved_ast_files(
    name = "run_gen_resolved_ast_cc",
    srcs = [
        "resolved_ast.cc.template",
        "resolved_ast.h.template",
        "resolved_ast_deep_copy_visitor.cc.template",
        "resolved_ast_deep_copy_visitor.h.template",
        "resolved_ast_visitor.h.template",
        "resolved_node_kind.h.template",
    ],
    outs = [
        "resolved_ast.cc",
        "resolved_ast.h",
        "resolved_ast_deep_copy_visitor.cc",
        "resolved_ast_deep_copy_visitor.h",
        "resolved_ast_visitor.h",
        "resolved_node_kind.h",
    ],
)

proto_library(
    name = "resolved_node_kind_proto",
    srcs = ["resolved_node_kind.proto"],
)

cc_proto_library(
    name = "resolved_node_kind_cc_proto",
    deps = [":resolved_node_kind_proto"],
)

java_proto_library(
    name = "resolved_node_kind_java_proto",
    deps = [":resolved_node_kind_proto"],
)

# TODO How do I make the library with generated headers live in public/?
cc_library(
    name = "resolved_ast",
    srcs = [
        "resolved_ast.cc",
        "resolved_ast_deep_copy_visitor.cc",
        "resolved_ast_helper.cc",
        "resolved_column.cc",
        "resolved_node.cc",
    ],
    hdrs = [
        "resolved_ast.h",
        "resolved_ast_deep_copy_visitor.h",
        "resolved_ast_helper.h",
        "resolved_ast_visitor.h",
        "resolved_column.h",
        "resolved_node.h",
        "resolved_node_kind.h",
    ],
    copts = ["-Wno-sign-compare"],
    deps = [
        ":resolved_ast_cc_proto",
        ":resolved_ast_enums_cc_proto",
        ":resolved_node_kind_cc_proto",
        ":serialization_cc_proto",
        "//zetasql/base",
        "//zetasql/base:map_util",
        "//zetasql/base:ret_check",
        "//zetasql/base:status",
        "//zetasql/base:statusor",
        "//zetasql/common:errors",
        "//zetasql/public:catalog",
        "//zetasql/public:constant",
        "//zetasql/public:function",
        "//zetasql/public:id_string",
        "//zetasql/public:parse_location",
        "//zetasql/public:parse_location_range_cc_proto",
        "//zetasql/public:simple_catalog",
        "//zetasql/public:strings",
        "//zetasql/public:type",
        "//zetasql/public:type_annotation_cc_proto",
        "//zetasql/public:value",
        "//zetasql/public/proto:type_annotation_cc_proto",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
        "@com_google_protobuf//:protobuf",
    ],
)

cc_library(
    name = "make_node_vector",
    srcs = [
        "make_node_vector_internal.h",
    ],
    hdrs = [
        "make_node_vector.h",
    ],
    copts = ["-Wno-sign-compare"],
    deps = [
        ":resolved_ast",
        "@com_google_absl//absl/meta:type_traits",
    ],
)

cc_library(
    name = "validator",
    srcs = ["validator.cc"],
    hdrs = ["validator.h"],
    copts = ["-Wno-sign-compare"],
    deps = [
        ":resolved_ast",
        ":resolved_ast_enums_cc_proto",
        ":resolved_node_kind_cc_proto",
        "//zetasql/base",
        "//zetasql/base:map_util",
        "//zetasql/base:ret_check",
        "//zetasql/base:source_location",
        "//zetasql/base:status",
        "//zetasql/base:stl_util",
        "//zetasql/common:errors",
        "//zetasql/public:catalog",
        "//zetasql/public:constant",
        "//zetasql/public:function",
        "//zetasql/public:function_cc_proto",
        "//zetasql/public:language_options",
        "//zetasql/public:strings",
        "//zetasql/public:templated_sql_function",
        "//zetasql/public:type",
        "//zetasql/public:type_cc_proto",
        "//zetasql/public:value",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/strings",
        "@com_google_protobuf//:protobuf",
    ],
)

proto_library(
    name = "resolved_ast_enums_proto",
    srcs = ["resolved_ast_enums.proto"],
)

cc_proto_library(
    name = "resolved_ast_enums_cc_proto",
    deps = [":resolved_ast_enums_proto"],
)

java_proto_library(
    name = "resolved_ast_enums_java_proto",
    deps = [":resolved_ast_enums_proto"],
)

load("@com_google_protobuf//:protobuf.bzl", "py_proto_library")

py_proto_library(
    name = "resolved_ast_enums_py_pb2",
    srcs = ["resolved_ast_enums.proto"],
)

proto_library(
    name = "resolved_ast_proto",
    srcs = ["resolved_ast.proto"],
    deps = [
        ":resolved_ast_enums_proto",
        ":resolved_node_kind_proto",
        ":serialization_proto",
        "//zetasql/proto:function_proto",
        "//zetasql/public:type_proto",
        "//zetasql/public/proto:type_annotation_proto",
        "@com_google_protobuf//:descriptor_proto",
    ],
)

cc_proto_library(
    name = "resolved_ast_cc_proto",
    deps = [":resolved_ast_proto"],
)

java_proto_library(
    name = "resolved_ast_java_proto",
    deps = [":resolved_ast_proto"],
)

proto_library(
    name = "serialization_proto",
    srcs = ["serialization.proto"],
    deps = [
        "//zetasql/public:parse_location_range_proto",
        "//zetasql/public:type_proto",
        "//zetasql/public:value_proto",
    ],
)

cc_proto_library(
    name = "serialization_cc_proto",
    deps = [":serialization_proto"],
)

java_proto_library(
    name = "serialization_java_proto",
    deps = [":serialization_proto"],
)
