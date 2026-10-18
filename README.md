```mermaid
flowchart TB
    subgraph Encoder【编码器，堆叠N层】
        In1["源序列 + 位置编码"] --> MHA1["多头自注意力 Multi-Head Attention"]
        MHA1 --> AddNorm1["残差连接 + LayerNorm"]
        AddNorm1 --> FFN1["前馈网络 Position-wise FFN"]
        FFN1 --> AddNorm2["残差连接 + LayerNorm"]
    end

    subgraph Decoder【解码器，堆叠N层】
        In2["目标序列 + 位置编码"] --> MaskMHA["掩码多头自注意力 Masked MHA"]
        MaskMHA --> AddNorm3["残差 + LayerNorm"]
        AddNorm3 --> CrossMHA["交叉注意力 Cross-Attention<br/>Q来自Decoder，K,V来自Encoder输出"]
        CrossMHA --> AddNorm4["残差 + LayerNorm"]
        AddNorm4 --> FFN2["前馈网络 Position-wise FFN"]
        FFN2 --> AddNorm5["残差 + LayerNorm"]
    end

    Encoder --> CrossMHA
    Decoder --> Linear["Linear + Softmax"]
    Linear --> Out["预测下一个Token"]
```
