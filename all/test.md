```mermaid
flowchart TD
    A[源代码] --> B[错误构建方式<br>在特定CPU上编译]
    B --> C[产生高度优化的镜像<br>含特定扩展指令]
    C --> D{在另一品牌CPU运行?}
    D --❌ 是 --> E[失败: Illegal instruction]

    A --> F[正确构建方式<br>使用交叉编译]
    F --> G[指定通用目标架构<br>生成兼容性镜像]
    G --> H{在任意ARMv8<br>CPU运行?}
    H --✅ 是 --> I[成功运行]
    subgraph Z [构建关键配置]
        J[使用多架构构建工具<br>(docker buildx)]
        K[选择通用目标平台<br>linux/arm64]
        L[避免使用-march=native<br>-mcpu=特定型号等参数]
        M[使用通用基础镜像]
        J --- K --- L --- M
    end

    F --> Z
```