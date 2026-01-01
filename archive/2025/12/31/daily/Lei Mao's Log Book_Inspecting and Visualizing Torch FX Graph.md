---
title: Inspecting and Visualizing Torch FX Graph
url: https://leimao.github.io/blog/Inspecting-Visualizing-Torch-FX-Graph/
source: Lei Mao's Log Book
date: 2025-12-31
fetch_date: 2026-01-01T03:39:00.216553
---

# Inspecting and Visualizing Torch FX Graph

[![Lei Mao's Log Book](/images/favicon/android-chrome-512x512.png)](/)

[Lei Mao's Log Book](/)[Curriculum](/curriculum)[Blog](/blog)[Articles](/article)[Projects](/project)[Publications](/publication)[Readings](/reading)[Life](/life)[Essay](/essay)[Photography](/photography)[Archives](/archives)[Categories](/categories)[Tags](/tags)[FAQs](/faq)

# Inspecting and Visualizing Torch FX Graph

12-31-202512-31-2025 [blog](/blog/) 13 minutes read (About 1882 words)  visits

## Introduction

PyTorch modules can consist of nested modules. Unlike ONNX models, understanding a PyTorch module can be challenging because we will have to look into the source code of the module to understand its structure and operations. Torch FX graph is the intermediate representation (IR) of PyTorch modules. It can be inspected and visualized to help us understand the structure and operations of PyTorch modules.

In this blog post, I would like to quickly demonstrate how to inspect and visualize Torch FX graphs using Torch FX `FxGraphDrawer` on a simple multi-layer perceptron (MLP) module. I will also show how to use `TorchFunctionMode` and `TorchDispatchMode` to log the function calls and dispatches during the execution of the module.

## Torch FX Graph Inspection and Visualization

The following program creates a simple MLP module with two linear layers and a ReLU activation function. It then performs Torch FX symbolic tracing, Torch Export to ATen IR, and Torch Export to [Core ATen IR](https://docs.pytorch.org/docs/2.9/torch.compiler_ir.html#core-aten-ir) on the MLP module. The resulting graphs are printed and visualized using `FxGraphDrawer`. Finally, it uses `TorchFunctionMode` and `TorchDispatchMode` to log the function calls and dispatches during the execution of the module.

python torch\_fx\_graph\_mlp.py

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 ``` | ``` import torch import torch.fx from torch.fx.passes.graph_drawer import FxGraphDrawer  from torch.overrides import TorchFunctionMode, resolve_name from torch.utils._python_dispatch import TorchDispatchMode   # https://docs.pytorch.org/docs/2.9/notes/extending.html#extending-all-torch-api-with-modes class FunctionLog(TorchFunctionMode):      def __torch_function__(self, func, types, args, kwargs=None):         # print(f"Function Log: {resolve_name(func)}(*{args}, **{kwargs})")         print(f"Function Log: {resolve_name(func)}")         return func(*args, **(kwargs or {}))   class DispatchLog(TorchDispatchMode):      def __torch_dispatch__(self, func, types, args, kwargs=None):         # print(f"Dispatch Log: {func}(*{args}, **{kwargs})")         print(f"Dispatch Log: {func}")         return func(*args, **(kwargs or {}))   class MLP(torch.nn.Module):      def __init__(self, input_dim=16, hidden_dim=32, output_dim=16):         super().__init__()         self.fc1 = torch.nn.Linear(input_dim, hidden_dim)         self.fc2 = torch.nn.Linear(hidden_dim, output_dim)         self.relu = torch.nn.ReLU()      def forward(self, x):         x = self.relu(self.fc1(x))         x = self.fc2(x)         return x   if __name__ == "__main__":      input_dim = 16     hidden_dim = 32     output_dim = 16      module = MLP(input_dim=input_dim,                  hidden_dim=hidden_dim,                  output_dim=output_dim)     module.eval()      # Torch FX Symbolic Tracing     torch_symbolic_traced = torch.fx.symbolic_trace(module)     torch_symbolic_traced_graph_drawer = FxGraphDrawer(         torch_symbolic_traced, "mlp_symbolic_traced_graph_drawer")     print("Torch FX Symbolic Traced Graph:")     print(torch_symbolic_traced.graph)      # Get the graph object and write to a file     with open("mlp_symbolic_traced_graph.svg", "wb") as f:         f.write(             torch_symbolic_traced_graph_drawer.get_dot_graph().create_svg())      # Torch Export to ATen IR     args = (torch.randn(1, input_dim), )     exported_program = torch.export.export(module, args)     exported_aten_graph_module = exported_program.graph_module     print("MLP Exported ATen Graph:")     print(exported_aten_graph_module)      exported_aten_graph_drawer = FxGraphDrawer(         exported_aten_graph_module, "mlp_exported_aten_graph_drawer")     with open("mlp_exported_aten_graph.svg", "wb") as f:         f.write(exported_aten_graph_drawer.get_dot_graph().create_svg())      # Torch Export to Core ATen IR     core_aten_exported_program = exported_program.run_decompositions()     core_aten_exported_aten_graph_module = core_aten_exported_program.graph_module     print("MLP Core ATen Exported Graph:")     print(core_aten_exported_aten_graph_module)      core_aten_exported_aten_graph_drawer = FxGraphDrawer(         core_aten_exported_aten_graph_module,         "mlp_core_aten_exported_aten_graph_drawer")     with open("mlp_core_aten_exported_aten_graph.svg", "wb") as f:         f.write(             core_aten_exported_aten_graph_drawer.get_dot_graph().create_svg())      print("TorchFunctionMode logging:")     with torch.inference_mode(), FunctionLog():         # result = module(*args)         # result = torch_symbolic_traced(*args)         # result = exported_program.module()(*args)         result = core_aten_exported_program.module()(*args)      print("TorchDispatchMode logging:")     with torch.inference_mode(), DispatchLog():         # result = module(*args)         # result = torch_symbolic_traced(*args)         # result = exported_program.module()(*args)         result = core_aten_exported_program.module()(*args) ``` |

We could use NVIDIA PyTorch Docker container to run the PyTorch program.

|  |  |
| --- | --- |
| ``` 1 ``` | ``` $ docker run -it --rm --gpus all -v $(pwd):/mnt -w /mnt nvcr.io/nvidia/pytorch:25.11-py3 ``` |

|  |  |
| --- | --- |
| ``` 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 ``` | ``` # graphviz and pydot are required for FxGraphDrawer $ apt update && apt install -y graphviz $ pip install pydot $ python torch_fx_graph_mlp.py Torch FX Symbolic Traced Graph: graph():     %x : [num_users=1] = placeholder[target=x]     %fc1 : [num_users=1] = call_module[target=fc1](args = (%x,), kwargs = {})     %relu : [num_users=1] = call_module[target=relu](args = (%fc1,), kwargs = {})     %fc2 : [num_users=1] = call_module[target=fc2](args = (%relu,), kwargs = {})     return fc2 MLP Exported ATen Graph: GraphModule()    def forward(self, p_fc1_weight, p_fc1_bias, p_fc2_weight, p_fc2_bias, x):     linear = torch.ops.aten.linear.default(x, p_fc1_weight, p_fc1_bias);  x = p_fc1_weight = p_fc1_bias = None     relu = torch.ops.aten.relu.default(linear);  linear = None     linear_1 = torch.ops.aten.linear.default(relu, p_fc2_weight, p_fc2_bias);  relu = p_fc2_weight = p_fc2_bias = None     return (linear_1,)  # To see more debug info, please use `graph_module.print_readable()` MLP Core ATen Exported Graph: GraphModule()    def forward(self, p_fc1_weight, p_fc1_bias, p_fc2_weight, p_fc2_bias, x):     permute = torch.ops.aten.permute.default(p_fc1_weight, [1, 0]);  p_fc1_weight = None     addmm = torch.ops.aten.addmm.default(p_fc1_bias, x, permute);  p_fc1_bias = x = permute = None     relu = torch.ops.aten.relu.default(addmm);  addmm = None     permute_1 = torch.ops.aten.permute.default(p_fc2_weight, [1, 0]);  p_fc2_weight = None     addmm_1 = torch.ops.aten.addmm.default(p_fc2_bias, relu, permute_1);  p_fc2_bias = relu = permute_1 = None     return (addmm_1,)  # To see more debug info, please use `graph_module.print_readable()` TorchFunctionMode logging: Function Log: torch.Tensor.grad_fn.__get__ Function Log: torch.Tensor.grad_fn.__get__ Function Log: torch.Tensor.grad_f...