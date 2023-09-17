# proto-pulumi-repro

This is a minimal repro for a possible bug in the `proto` tool.

This repository contains a simple Ubuntu devcontainer, that installs `proto`
when it is created.

When running the command:

```bash
proto install pulumi
```

This always fails with the following error:  

```bash
Error: plugin::failed

  × extism_call failed
```

Running this wilth `--log trace` gives the following output:

```bash
[DEBUG 2023-09-17 06:09:04] proto  Running proto v0.17.1 
[TRACE 06:09:04] starbase::app  Running startup phase with 1 systems 
[TRACE 06:09:04] starbase::app  Running analyze phase with 1 systems 
[TRACE 06:09:04] starbase::app  Running execute phase with 2 systems 
[DEBUG 06:09:04] proto_core::tool_loader:install  Traversing upwards to find a configured plugin  tool="pulumi"
[DEBUG 06:09:04] proto_core::tools_config:install:load  Loading .prototools  file="/workspaces/proto-pulumi-repro/.prototools"
[TRACE 06:09:04] starbase_utils::fs:install:load  Opening file  file="/workspaces/proto-pulumi-repro/.prototools"
[TRACE 06:09:04] starbase_utils::fs_lock:install:load  Locking file  file="/workspaces/proto-pulumi-repro/.prototools"
[TRACE 06:09:04] starbase_utils::fs_lock:install:load  Unlocking file  file="/workspaces/proto-pulumi-repro/.prototools"
[TRACE 06:09:04] warpgate::loader:install  Creating plugin loader  cache_dir="/home/vscode/.proto/plugins"
[TRACE 06:09:04] warpgate::loader:install  Loading plugin pulumi  plugin="pulumi" locator="source:prototools/pulumi.toml"
[TRACE 06:09:04] warpgate::loader:install  Using source file  plugin="pulumi" path="/workspaces/proto-pulumi-repro/prototools/pulumi.toml"
[DEBUG 06:09:04] proto_core::tool_loader:install  Loading TOML plugin  source="/workspaces/proto-pulumi-repro/prototools/pulumi.toml"
[TRACE 06:09:04] warpgate::loader:install  Loading plugin pulumi  plugin="pulumi" locator="source:https://github.com/moonrepo/schema-plugin/releases/latest/download/schema_plugin.wasm"
[TRACE 06:09:04] starbase_utils::fs:install  Reading file metadata  file="/home/vscode/.proto/plugins/pulumi-latest-15836bedba06f4b9b3be0f61ac65a7f0139868ca28b0dc33b8a1caf94d3e1543.wasm"
[TRACE 06:09:04] warpgate::loader:install  Plugin already downloaded and cached  plugin="pulumi" path="/home/vscode/.proto/plugins/pulumi-latest-15836bedba06f4b9b3be0f61ac65a7f0139868ca28b0dc33b8a1caf94d3e1543.wasm"
[TRACE 06:09:04] starbase_utils::fs:install  Reading file  file="/workspaces/proto-pulumi-repro/prototools/pulumi.toml"
[DEBUG 06:09:04] proto_core::tool:install  Creating tool pulumi and instantiating plugin 
[DEBUG 06:09:04] proto_core::tool_manifest:install:load  Loading manifest.json  file="/home/vscode/.proto/tools/pulumi/manifest.json"
[DEBUG 06:09:05] proto_core::tool:install  Created tool pulumi and its WASM runtime 
[TRACE 06:09:05] warpgate::plugin:install  Calling plugin function register_tool  plugin="pulumi" input={"id":"pulumi"}
[TRACE 06:09:05] warpgate::plugin:install  Called plugin function register_tool  plugin="pulumi" output={"env_vars":[],"inventory":{"disable_progress_bars":false},"name":"pulumi","plugin_version":"0.2.0","type_of":"CLI"}
[DEBUG 06:09:05] proto_core::tool:install  Resolving a semantic version or alias  tool="pulumi" initial_version="latest"
[DEBUG 06:09:05] proto_core::tool:install  Loading available versions  tool="pulumi"
[TRACE 06:09:05] warpgate::plugin:install  Calling plugin function load_versions  plugin="pulumi" input={"context":{"env_vars":{},"tool_dir":{"path":"/home/.proto/tools/pulumi/latest","virtual_prefix":"/home","real_prefix":"/home/vscode"},"version":"latest"},"initial":"latest"}
[TRACE 06:09:05] proto_wasm::exec_command:install  Executing command from plugin  command="git" args=["ls-remote", "--tags", "--sort", "version:refname", "https://github.com/pulumi/pulumi"] env_vars={}
[TRACE 06:09:06] proto_wasm::exec_command:install  Executed command from plugin  command="git" exit_code=0 stderr_len=0 stdout_len=59802
[TRACE 06:09:06] starbase::app  Running shutdown phase with 1 systems 
```

Note that this also fails on MacOS with the same error and trace output.
