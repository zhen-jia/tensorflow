# TPU Kernel Implementations

load(
    "//tensorflow/core/platform:build_config.bzl",
    "tf_proto_library_cc",
)
load(
    "//tensorflow:tensorflow.bzl",
    "tf_kernel_library",
)

# Config setting to enable go/libtpu support.
WITH_TPU_SUPPORT = "//tensorflow:with_tpu_support"

DEFAULT = "//conditions:default"

package(
    default_visibility = [
        "//tensorflow/core/tpu:__subpackages__",
        "//tensorflow/stream_executor/tpu:__subpackages__",
    ],
    licenses = ["notice"],  # Apache 2.0
)

tf_kernel_library(
    name = "kernels",
    visibility = ["//visibility:public"],
    deps = [
        ":cross_replica_ops",
        ":host_compute_ops",
        ":topk_ops",
        ":tpu_compile_op",
        ":tpu_configuration_ops",
        ":tpu_execute_op",
    ],
)

cc_library(
    name = "tpu_compile_op_common",
    srcs = ["tpu_compile_op_common.cc"],
    hdrs = ["tpu_compile_op_common.h"],
    deps = [
        ":tpu_compilation_cache_entry_unloader",
        ":tpu_compilation_cache_interface",
        ":tpu_compilation_metrics_hdrs",
        ":tpu_compile_op_options",
        ":tpu_compile_op_support",
        ":tpu_mesh_state_interface",
        ":tpu_op_consts",
        ":tpu_op_util",
        ":tpu_program_group_interface",
        ":tpu_util",
        ":tpu_util_c_api_hdrs",
        ":tpu_util_hdrs",
        "//tensorflow/compiler/jit:flags",
        "//tensorflow/compiler/jit:shape_inference",
        "//tensorflow/compiler/tf2xla:tf2xla_util",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/compiler/xla:statusor",
        "//tensorflow/compiler/xla:xla_data_proto_cc",
        "//tensorflow/compiler/xla/client:client_library",
        "//tensorflow/compiler/xla/client:compile_only_client",
        "//tensorflow/core:core_cpu_internal",
        "//tensorflow/core:framework",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core/protobuf/tpu:compilation_result_proto_cc",
        "//tensorflow/core/protobuf/tpu:compile_metadata_proto_cc",
        "//tensorflow/core/protobuf/tpu:dynamic_padding_proto_cc",
        "//tensorflow/core/tpu:tpu_api",
        "//tensorflow/core/tpu:tpu_configuration",
        "//tensorflow/core/tpu:tpu_defs",
        "//tensorflow/stream_executor/tpu:tpu_platform_interface",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
        "@com_google_absl//absl/types:variant",
    ],
    alwayslink = 1,
)

cc_library(
    name = "tpu_compile_op_options",
    srcs = ["tpu_compile_op_options.cc"],
    hdrs = ["tpu_compile_op_options.h"],
)

tf_kernel_library(
    name = "tpu_configuration_ops",
    srcs = ["tpu_configuration_ops.cc"],
    hdrs = ["tpu_configuration_ops.h"],
    deps = [
        ":tpu_compilation_cache_factory",
        ":tpu_compilation_cache_interface",
        ":tpu_compilation_cache_local_lookup",
        ":tpu_compilation_cache_lookup",
        ":tpu_mesh_state_interface",
        ":tpu_op_consts",
        "//tensorflow/c:tf_status",
        "//tensorflow/c:tf_status_helper",
        "//tensorflow/compiler/xla:util",
        "//tensorflow/core:framework",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core/platform:refcount",
        "//tensorflow/core/tpu:tpu_api",
        "//tensorflow/core/tpu:tpu_config_c_api",
        "//tensorflow/core/tpu:tpu_configuration",
        "//tensorflow/core/tpu:tpu_defs",
        "//tensorflow/stream_executor/tpu:proto_helper",
    ],
    alwayslink = 1,
)

cc_library(
    name = "tpu_compile_c_api_hdrs",
    hdrs = ["tpu_compile_c_api.h"],
    deps = [
        ":tpu_mesh_state_c_api_hdrs",
        ":tpu_program_c_api_hdrs",
        ":tpu_util_c_api_hdrs",
        "//tensorflow/core/tpu:libtftpu_header",
        "//tensorflow/stream_executor/tpu:proto_helper",
    ],
    alwayslink = True,
)

tf_proto_library_cc(
    name = "tpu_executable_info_proto",
    srcs = ["tpu_executable_info.proto"],
    cc_api_version = 2,
    protodeps = [
        "//tensorflow/compiler/xla:xla_data_proto",
        "//tensorflow/compiler/xla/service:hlo_proto",
        "//tensorflow/core:protos_all",
    ],
)

tf_proto_library_cc(
    name = "tpu_compile_proto",
    srcs = ["tpu_compile.proto"],
    cc_api_version = 2,
    protodeps = [
        ":tpu_executable_info_proto",
        "//tensorflow/compiler/tf2xla:host_compute_metadata_proto",
        "//tensorflow/compiler/xla:xla_data_proto",
        "//tensorflow/compiler/xla/service:hlo_proto",
        "//tensorflow/core:protos_all",
        "//tensorflow/core/protobuf/tpu:compile_metadata_proto",
    ],
)

cc_library(
    name = "tpu_compilation_cache_factory",
    srcs = ["tpu_compilation_cache_factory.cc"],
    hdrs = ["tpu_compilation_cache_factory.h"],
    deps = [
        ":tpu_compilation_cache_external",
        ":tpu_compilation_cache_interface",
        ":tpu_op_consts",
        "//tensorflow/core:framework",
        "//tensorflow/core/platform:status",
        "//tensorflow/core/platform:types",
    ],
)

cc_library(
    name = "tpu_compilation_cache_key",
    srcs = [],
    hdrs = [
        "tpu_compilation_cache_key.h",
    ],
    deps = [
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
    ],
)

cc_library(
    name = "tpu_compile_op_support",
    srcs = ["tpu_compile_op_support.cc"],
    hdrs = ["tpu_compile_op_support.h"],
    deps = [
        ":tpu_compilation_cache_key",
        ":tpu_compile_proto_cc",
        ":tpu_executable_info_proto_cc",
        "//tensorflow/cc:ops",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/compiler/xla:debug_options_flags",
        "//tensorflow/compiler/xla:shape_tree",
        "//tensorflow/compiler/xla:shape_util",
        "//tensorflow/compiler/xla:statusor",
        "//tensorflow/compiler/xla:xla_data_proto_cc",
        "//tensorflow/compiler/xla/service:computation_layout",
        "//tensorflow/compiler/xla/service:computation_placer",
        "//tensorflow/compiler/xla/service:dump",
        "//tensorflow/compiler/xla/service:hlo",
        "//tensorflow/compiler/xla/service:hlo_module_config",
        "//tensorflow/compiler/xla/service:hlo_module_group",
        "//tensorflow/core:framework",
        "//tensorflow/core/framework:protos_all_cc",
        "//tensorflow/core/platform:errors",
        "//tensorflow/core/protobuf/tpu:compile_metadata_proto_cc",
        "//tensorflow/stream_executor/tpu:proto_helper",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
        "@com_google_absl//absl/types:variant",
    ],
)

cc_library(
    name = "tpu_compilation_cache_entry",
    hdrs = [
        "tpu_compilation_cache_entry.h",
    ],
    deps = [
        ":tpu_executable_info_proto_cc",
        ":tpu_program_group_interface",
        "//tensorflow/compiler/xla/service:hlo_proto_cc",
        "//tensorflow/core/lib/core:refcount",
    ],
)

cc_library(
    name = "tpu_compilation_cache_lookup",
    hdrs = [
        "tpu_compilation_cache_lookup.h",
    ],
    deps = [
        ":tpu_compilation_cache_interface",
        ":tpu_compilation_cache_proto_cc",
        "//tensorflow/core/lib/core:refcount",
        "//tensorflow/core/platform:status",
        "//tensorflow/core/profiler/lib:traceme",
    ],
)

cc_library(
    name = "tpu_compilation_cache_local_lookup",
    srcs = ["tpu_compilation_cache_local_lookup.cc"],
    hdrs = ["tpu_compilation_cache_local_lookup.h"],
    deps = [
        ":tpu_compilation_cache_entry",
        ":tpu_compilation_cache_external",
        ":tpu_compilation_cache_interface",
        ":tpu_compilation_cache_lookup",
        ":tpu_compilation_cache_proto_cc",
        "//tensorflow/core/platform:status",
    ],
)

cc_library(
    name = "tpu_mesh_state_c_api_hdrs",
    hdrs = ["tpu_mesh_state_c_api.h"],
    deps = ["//tensorflow/core/tpu:libtftpu_header"],
    alwayslink = True,
)

cc_library(
    name = "tpu_mesh_state_interface",
    srcs = [],
    hdrs = ["tpu_mesh_state_interface.h"],
    deps = [
        ":tpu_compile_c_api_hdrs",
        ":tpu_mesh_state_c_api_hdrs",
        "//tensorflow/compiler/xla/service",
        "//tensorflow/core:framework",
        "//tensorflow/core/protobuf/tpu:compile_metadata_proto_cc",
        "//tensorflow/core/tpu:tpu_api",
    ],
)

cc_library(
    name = "compiled_subgraph",
    hdrs = ["compiled_subgraph.h"],
    deps = [
        ":tpu_program_group_interface",
        "//tensorflow/core:lib",
        "//tensorflow/core/platform:refcount",
    ],
)

cc_library(
    name = "tpu_program_group_interface",
    hdrs = ["tpu_program_group_interface.h"],
    deps = [
        ":tpu_compilation_cache_key",
        "//tensorflow/compiler/tf2xla:host_compute_metadata_proto_cc",
        "//tensorflow/compiler/xla/service:hlo_proto_cc",
        "//tensorflow/core/lib/core:status",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:span",
    ],
)

cc_library(
    name = "tpu_program_group",
    srcs = ["tpu_program_group.cc"],
    hdrs = ["tpu_program_group.h"],
    deps = [
        ":tpu_compile_c_api_hdrs",
        ":tpu_compile_op_common",
        ":tpu_compile_op_support",
        ":tpu_compile_proto_cc",
        ":tpu_executable_info_proto_cc",
        ":tpu_mesh_state_c_api_hdrs",
        ":tpu_mesh_state_interface",
        ":tpu_program_c_api_hdrs",
        ":tpu_program_group_interface",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/compiler/xla:xla_proto_cc",
        "//tensorflow/compiler/xla/client:compile_only_client",
        "//tensorflow/compiler/xla/service:computation_placer",
        "//tensorflow/compiler/xla/service:hlo_module_group",
        "//tensorflow/compiler/xla/service:hlo_proto_cc",
        "//tensorflow/core:lib",
        "//tensorflow/core/protobuf/tpu:compile_metadata_proto_cc",
        "//tensorflow/core/tpu:tpu_api",
        "//tensorflow/stream_executor/tpu:proto_helper",
        "//tensorflow/stream_executor/tpu:status_helper",
        "//tensorflow/stream_executor/tpu:tpu_platform_interface",
        "@com_google_absl//absl/types:optional",
    ],
)

cc_library(
    name = "tpu_compilation_cache_interface",
    srcs = ["tpu_compilation_cache_interface.cc"],
    hdrs = ["tpu_compilation_cache_interface.h"],
    deps = [
        ":compiled_subgraph",
        ":tpu_compilation_cache_entry",
        ":tpu_compilation_cache_key",
        ":tpu_compilation_cache_proto_cc",
        ":tpu_compilation_metrics_hdrs",
        ":tpu_util",
        ":tpu_util_hdrs",
        ":trace_util_hdrs",
        "//tensorflow/compiler/tf2xla:host_compute_metadata_proto_cc",
        "//tensorflow/compiler/xla:util",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/core:lib_internal",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core/profiler/lib:traceme",
        "//tensorflow/core/tpu:tpu_api",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:node_hash_map",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
    ],
    alwayslink = 1,
)

cc_library(
    name = "tpu_compilation_cache_external",
    srcs = ["tpu_compilation_cache_external.cc"],
    hdrs = [
        "tpu_compilation_cache_external.h",
    ],
    deps = [
        ":compiled_subgraph",
        ":tpu_compilation_cache_entry",
        ":tpu_compilation_cache_interface",
        ":tpu_compilation_cache_key",
        ":tpu_compilation_cache_proto_cc",
        ":tpu_compilation_metrics",  # buildcleaner: keep
        ":tpu_compilation_metrics_hdrs",
        ":tpu_compile_c_api_hdrs",
        ":tpu_compile_op_support",
        ":tpu_mesh_state_interface",
        ":tpu_op_consts",
        ":tpu_program_c_api_hdrs",
        ":tpu_program_group",
        ":tpu_util",
        ":trace_util_hdrs",
        "//tensorflow/compiler/xla/service",
        "//tensorflow/compiler/xla/service:hlo_proto_cc",
        "//tensorflow/core:framework",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "//tensorflow/core:lib_internal",
        "//tensorflow/core/profiler/lib:traceme",
        "//tensorflow/core/protobuf/tpu:compile_metadata_proto_cc",
        "@com_google_absl//absl/container:node_hash_map",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:span",
    ],
)

cc_library(
    name = "tpu_compilation_metrics_hdrs",
    hdrs = ["tpu_compilation_metrics.h"],
    deps = [
        "//tensorflow/core:lib",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "tpu_compilation_metrics",
    srcs = ["tpu_compilation_metrics.cc"],
    copts = select({
        WITH_TPU_SUPPORT: ["-DLIBTFTPU"],
        DEFAULT: [],
    }),
    deps = [
        ":tpu_compilation_metrics_hdrs",
    ],
)

cc_library(
    name = "trace_util_hdrs",
    hdrs = ["trace_util.h"],
    deps = [
        "//tensorflow/core:protos_all_cc",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "tpu_util_hdrs",
    hdrs = ["tpu_util.h"],
    deps = [
        ":tpu_compilation_cache_key",
        ":tpu_program_group_interface",
        "//tensorflow/cc:ops",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/compiler/xla:statusor",
        "//tensorflow/compiler/xla/client:compile_only_client",
        "//tensorflow/core:protos_all_cc",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/time",
    ],
)

cc_library(
    name = "tpu_util_c_api_hdrs",
    hdrs = ["tpu_util_c_api.h"],
    deps = [
        ":tpu_mesh_state_c_api_hdrs",
        "//tensorflow/core/tpu:libtftpu_header",
        "//tensorflow/stream_executor/tpu:c_api_decl",
        "//tensorflow/stream_executor/tpu:proto_helper",
    ],
    alwayslink = True,
)

cc_library(
    name = "tpu_program_c_api_hdrs",
    hdrs = ["tpu_program_c_api.h"],
    deps = [
        ":tpu_util_c_api_hdrs",
        "//tensorflow/core/tpu:libtftpu_header",
        "//tensorflow/stream_executor/tpu:proto_helper",
    ],
    alwayslink = True,
)

cc_library(
    name = "tpu_op_util",
    srcs = ["tpu_op_util.cc"],
    hdrs = ["tpu_op_util.h"],
    deps = [
        ":tpu_compilation_cache_key",
        ":tpu_compile_c_api_hdrs",
        ":tpu_mesh_state_interface",
        "//tensorflow/compiler/xla:xla_data_proto_cc",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/core/protobuf/tpu:compile_metadata_proto_cc",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "tpu_util",
    srcs = ["tpu_util.cc"],
    hdrs = ["tpu_util.h"],
    deps = [
        ":tpu_compilation_cache_key",
        ":tpu_program_group_interface",
        "//tensorflow/cc:ops",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/compiler/xla:statusor",
        "//tensorflow/compiler/xla/client:compile_only_client",
        "//tensorflow/core:lib",
        "//tensorflow/core:protos_all_cc",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/time",
    ],
    alwayslink = 1,
)

tf_proto_library_cc(
    name = "tpu_compilation_cache_proto",
    srcs = ["tpu_compilation_cache.proto"],
    cc_api_version = 2,
)

cc_library(
    name = "tpu_compile_op_hdrs",
    hdrs = ["tpu_compile_op.h"],
    deps = [
        ":tpu_compile_op_common",
        "//tensorflow/core:framework",
    ],
)

cc_library(
    name = "tpu_compilation_cache_entry_unloader",
    hdrs = ["tpu_compilation_cache_entry_unloader.h"],
    deps = [
        ":tpu_compilation_cache_interface",
        "//tensorflow/core:framework",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/synchronization",
    ],
)

cc_library(
    name = "tpu_op_consts",
    srcs = ["tpu_op_consts.cc"],
    hdrs = ["tpu_op_consts.h"],
    deps = [
        "@com_google_absl//absl/base:core_headers",
    ],
)

cc_library(
    name = "tpu_execute_c_api_hdrs",
    hdrs = ["tpu_execute_c_api.h"],
    deps = [
        ":tpu_program_c_api_hdrs",
        ":tpu_util_c_api_hdrs",
        "//tensorflow/core/tpu:libtftpu_header",
        "//tensorflow/stream_executor/tpu:tpu_executor_c_api_hdrs",
    ],
)

cc_library(
    name = "tpu_compile_op_impl",
    srcs = ["tpu_compile_op_impl.cc"],
    hdrs = ["tpu_compile_op_impl.h"],
    copts = select({
        WITH_TPU_SUPPORT: ["-DLIBTFTPU"],
        DEFAULT: [],
    }),
    deps = [
        ":tpu_compilation_cache_key",
        ":tpu_compile_c_api_hdrs",
        ":tpu_compile_op_common",
        ":tpu_compile_op_support",
        ":tpu_compile_proto_cc",
        ":tpu_mesh_state_c_api_hdrs",
        ":tpu_program_group",
        ":tpu_program_group_interface",
        ":tpu_util",
        "//tensorflow/compiler/jit:shape_inference",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/compiler/xla:status",
        "//tensorflow/core:core_cpu_internal",
        "//tensorflow/core:framework",
        "//tensorflow/stream_executor/tpu:tpu_executor",
        "//tensorflow/stream_executor/tpu:tpu_executor_c_api_hdrs",
        "@com_google_absl//absl/types:variant",
    ],
    alwayslink = 1,
)

cc_library(
    name = "tpu_compile_op_lib",
    srcs = ["tpu_compile_op.cc"],
    deps = [
        ":tpu_compile_op_hdrs",
        ":tpu_compile_op_options",
        "//tensorflow/compiler/xla:statusor",
        "//tensorflow/core/protobuf/tpu:compilation_result_proto_cc",
        "//tensorflow/stream_executor/tpu:tpu_node_context",
    ],
    alwayslink = True,
)

cc_library(
    name = "tpu_compile_op",
    deps = [
        ":tpu_compile_op_hdrs",
        ":tpu_compile_op_impl",
        ":tpu_compile_op_lib",
        ":tpu_compile_op_options",
        "//tensorflow/compiler/xla:statusor",
        "//tensorflow/core/protobuf/tpu:compilation_result_proto_cc",
        "//tensorflow/stream_executor/tpu:tpu_node_context",
    ],
    alwayslink = True,
)

cc_library(
    name = "tpu_execute_op",
    srcs = ["tpu_execute_op.cc"],
    hdrs = ["tpu_execute_op.h"],
    deps = [
        ":tpu_compilation_cache_entry",
        ":tpu_compilation_cache_external",
        ":tpu_compilation_cache_interface",
        ":tpu_compilation_cache_local_lookup",
        ":tpu_compilation_cache_lookup",
        ":tpu_executable_info_proto_cc",
        ":tpu_op_consts",
        "//tensorflow/compiler/jit:xla_device_no_jit_rewrite_registration",
        "//tensorflow/compiler/jit:xla_launch_util",
        "//tensorflow/compiler/jit:xla_tensor",
        "//tensorflow/compiler/tf2xla:common",
        "//tensorflow/compiler/tf2xla:tf2xla_util",
        "//tensorflow/compiler/xla:debug_options_flags",
        "//tensorflow/compiler/xla:shape_util",
        "//tensorflow/compiler/xla:statusor",
        "//tensorflow/compiler/xla:xla_data_proto_cc",
        "//tensorflow/compiler/xla/service:dump",
        "//tensorflow/compiler/xla/service:executable",
        "//tensorflow/compiler/xla/service:maybe_owning_device_memory",
        "//tensorflow/core:framework",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "//tensorflow/core:lib_internal",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core/profiler/lib:traceme",
        "//tensorflow/core/tpu:tpu_configuration",
        "//tensorflow/core/tpu:tpu_defs",
        "//tensorflow/core/tpu:tpu_execute",
        "//tensorflow/stream_executor",
        "//tensorflow/stream_executor/tpu:tpu_node_context",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/types:span",
    ],
    alwayslink = True,
)

cc_library(
    name = "cross_replica_ops",
    srcs = ["cross_replica_ops.cc"],
    deps = [
        "//tensorflow/compiler/tf2xla:common",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/compiler/tf2xla:xla_op_registry",
        "//tensorflow/compiler/xla:shape_util",
        "//tensorflow/compiler/xla:util",
        "//tensorflow/compiler/xla:xla_data_proto_cc",
        "//tensorflow/compiler/xla/client:xla_builder",
        "//tensorflow/core:core_cpu_internal",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "@com_google_absl//absl/strings",
    ],
    alwayslink = 1,
)

cc_library(
    name = "topk_ops",
    srcs = ["topk_ops.cc"],
    deps = [
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/compiler/tf2xla:xla_op_registry",
        "//tensorflow/compiler/xla/client:xla_builder",
        "//tensorflow/compiler/xla/client/lib:arithmetic",
        "//tensorflow/core/tpu:tpu_defs",
    ],
    alwayslink = 1,
)

cc_library(
    name = "host_compute_ops",
    srcs = ["host_compute_ops.cc"],
    deps = [
        "//tensorflow/compiler/tf2xla:common",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/core:framework",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:lib",
        "//tensorflow/core/tpu:tpu_defs",
    ],
    alwayslink = 1,
)
