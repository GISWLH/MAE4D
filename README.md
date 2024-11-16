# MAE4D
A implemention of "Masked Autoencoders As Spatiotemporal Learners"

MAE4D也许有希望帮助重现次网格降水过程

![](https://imagecollection.oss-cn-beijing.aliyuncs.com/office/20241116223111.png)

## Model

模型的结构可以概括如下：

1. **输入处理**:
   - 输入是一个形状为 `[N, C, T, H, W]` 的视频帧（N：批次，C：通道，T：时间帧，H 和 W：高度和宽度）。
   - 使用 `PatchEmbed` 模块将输入的视频帧分割成空间和时间维度的 patch，并嵌入为向量。
2. **编码器 (Encoder)**:
   - 多层 Transformer 块构成的编码器。
   - 编码阶段执行随机掩码（Masking），只处理未被掩盖的部分。
   - 编码器输出一个潜在表示 (Latent) 和掩码信息。
3. **解码器 (Decoder)**:
   - 解码器通过嵌入未掩盖的潜在表示与掩码嵌入 (`mask_token`) 还原完整的视频帧。
   - 解码器由线性映射、位置嵌入和多层 Transformer 块组成。
   - 最终通过预测器输出被掩盖 patch 的像素值。
4. **损失计算 (Loss)**:
   - 使用均方误差 (MSE) 作为损失，计算掩盖区域的恢复误差。

**总结的关键模块顺序**: 输入 → Patch 分割 → 编码器（随机掩码 + Transformer）→ 解码器（Transformer + 预测器）→ 损失计算
