# 1.电脑配置
    mac mini4 内存64G，硬盘2T
# 2.本地部署大模型平台
    - OMLX
    - 版本号：0.3.10

# 3.本地模型列表:
    - Qwen3.6-27B-UD-MLX-4bit
    - Qwen3.6-35B-A3B-4bit
    - gemma-4-26b-a4b-it-4bit

# 4.omlx模型设置选项：
    4.1 模型类别下拉框：
        - 自动检测
        - llm
        - vlm
        - Embedding
        - Audio STT
        - Audio TTS
        - Audio STS
    4.2 REASONING PARSER下拉框：
        - None
        - llama (Meta-Llama-3, Llama-3.1, Llama-3.2)
        - kimi (Kimi-K2, Kimi-K2.5)
        - deepseek_r1 (DeepSeek-R1, DeepSeek-R1-0528)
        - deepseek_v3_1 (DeepSeek-V3.1, DeepSeek-V3.2-Exp)
        - qwen_3_coder (Qwen3.5, Qwen3.6, Qwen3-Coder, Qwen3-Coder-Next)
        - qwen_3_5 (Qwen3.5, Qwen3.6, Qwen3-Coder, Qwen3-Coder-Next)
        - qwen_3 (Qwen3, Qwen3-Next)
        - harmony (gpt-oss)
        - deepseek_v3_2 (DeepSeek-V3.2)
        - minimax (MiniMax-M2.5, MiniMax-M2.7)
        - glm_4_7 (GLM-5, GLM-4.7)
        - deepseek_v4 (DeepSeek-V4)
    4.3 Ctx Window
    4.4 最大Token数
    4.5 温度
    4.6 Top P
    4.7 Top K
    4.8 Min P
    4.9 重复惩罚
    4.10 Presence Penalty
    4.11 TTL（秒）
    4.12 启用思考（yes/no）
    4.13 思考Token限制（yes/no）
        - yes时输入限制推理模型的思考Token数量
    4.14 限制工具结果Token（yes/no）
        - yes时输入截断过长的工具结果（如文件读取）到 Token 限制
    4.15 强制采样（yes/no）
    4.16 Trust Remote Code（yes/no）
        - 说明：Lets the model repo run arbitrary Python at load. Only enable for trusted publishers.
    4.17 聊天模板参数（可以点击添加模板）
        4.17.1 模板类别：
            - enable_thinking（可勾选是否强制）
                · true
                · false
            - reasoning_effort（可勾选是否强制）
                · low
                · medium
                · high
            - 自定义（可勾选是否强制）（如果添加自定义模板，需要提供键值对设置）
                · 键
                · 值
    4.18 TurboQuant KV Cache（yes/no）
        4.18.1 yes时 Bits per channel下拉框选择
            - 2-bit
            - 2.5-bit
            - 3-bit
            - 3.5-bit
            - 4-bit
            - 6-bit
            - 8-bit
    4.19 SpecPrefill（yes/no）
        4.19.1 yes时 Draft Model下拉框选择本地已有的模型
        4.19.2 yes时 Keep Rate下拉框选择
            - 10% — Aggressive (~5-7x, some quality loss)
            - 20% — Balanced (~3x, recommended)
            - 25% — Conservative+ (~2.5x)
            - 30% — Conservative (~2.2x)
            - 40% — Mild (~1.8x)
            - 50% — Minimal (~1.5x)
        4.19.3 yes时 Threshold (tokens)输入值（Min tokens to trigger (shorter prompts use full prefill)）
    4.20 DFlash（yes/no）
        （Block diffusion speculative decoding for 3-4x faster generation.Supports Qwen (3, 3.5, 3.6) and Gemma4 model families.Requires a DFlash draft model checkpoint.Single-stream only: requests run one at a time.）
        4.20.1 yes时 Draft Model下拉框选择本地已有的模型
        （DFlash draft checkpoint (e.g. z-lab/Qwen3-4B-DFlash-b16, z-lab/gemma-4-26B-A4B-it-DFlash). Note: -DFlash suffix only; -assistant variants are for MTP.）
        4.20.2 Quantization（yes/no）
            Enable quantization for the draft model (weight, activation bits & group size).
            4.20.2.1 yes时 Weight Bits下拉框选择
                - 2-bit
                - 4-bit
                - 8-bit
            4.20.2.2 yes时 Activation Bits下拉框选择
                - 16-bit
                - 32-bit
            4.20.2.3 yes时 Activation Bits下拉框选择
                - 32
                - 64
                - 128
        4.20.3 Max Context (fallback threshold)
            Prompts at or above this token count switch to BatchedEngine. Leave empty for unlimited.
        4.20.4 Long-context tuning
            4.20.4.1 输入Draft window size
                Draft model sliding-attention window. Helps stabilise acceptance on long contexts. Leave empty for dflash default (1024).
            4.20.4.2 输入Draft sink size
                Attention-sink tokens always kept regardless of window. Leave empty for dflash default (64).
        4.20.5 Verify mode下拉框选择
                - adaptive(default)
                - dflash
                - ddtree
                - off
        4.20.6 In-memory cache（yes/no）
            DFlash L1 prefix snapshot cache in RAM. Speeds up multi-turn chats with shared prefixes.
            4.20.6.1 yes时输入 In-memory cache max entries
                Maximum number of prefix snapshots kept in L1 cache. Each entry stores KV + draft GDN state for one conversation prefix.
            4.20.6.2 yes时输入 In-memory cache size (GiB)
                Byte budget for L1 snapshots; LRU evicts when exceeded.
        4.20.7 SSD cache size（yes/no）
            L2 spill of evicted L1 entries to disk. Uses the oMLX paged SSD cache directory (dflash_l2/).
            4.26.1 yes时输入 SSD cache size (GiB)
                Disk budget for L2 spill; oldest entries are evicted when exceeded.
    4.21 Native MTP（yes/no）
        使用模型内置的 MTP head 将单一请求解码速度提升约 1.5 倍。(仅限 Qwen 3.5/3.6 和 Deepseek-v4)单一流：请求逐个处理。
        mlx-lm PR #990 by AirRunner, PR #15 by 0xClandestine Config declares MTP layers but the converted weights are missing mtp.* tensors. Re-convert from HF with a converter that preserves MTP weights.
    4.22 VLM MTP (Gemma 4, experimental)（yes/no）
        Speculative decoding for Gemma 4 VLM via an external gemma4_assistant drafter (mlx-vlm 191d7c8+). Drafter is loaded and attached at engine start; decode-path wiring lands in a follow-up.Disable DFlash / SpecPrefill / MTP / TurboQuant KV first — VLM MTP owns the speculative decode path.

# 5.本地激活模型
    5.1 最多激活2个模型
    5.2 gemma-4-26b-a4b-it-4bit固定
    5.3 qwen系列模型优先Qwen3.6-27B-UD-MLX-4bit，也有可能会激活Qwen3.6-35B-A3B-4bit

# 6.模型设置要求
    6.1 请你根据电脑配置、模型平台以及模型型号给我列出最优的模型设置
    6.2 需要包含# 4.omlx模型设置选项 中的所有选项配置
    6.3 要避免模型的过度思考、工具调用失败、“僵尸假死”、内存溢出、防止出错时胡言乱语、确保模型在遇到工具调用失败后，绝对不会陷入无限自我复读的死循环
    6.4 需要让模型成为绝对不假死、不犯低级错误的自主开发主力
    6.5 同时需要保证token的输出速度
    6.6 开启【限制工具结果 Token】后，好像会影响工具调用，让模型陷入死循环，如果需要开启【限制工具结果 Token】，请你给出合理的配置，避免工具调用失败
    6.7 如果需要开启【DFlash】或【SpecPrefill】，请你给出对应的真实存在，能下载的，正确的，对应的`Draft Model`，不能直接给示例的模型
    6.8 我所下载的这些模型，都无法开启【Native MTP】，提示`Config declares MTP layers but the converted weights are missing mtp.* tensors. Re-convert from HF with a converter that preserves MTP weights.`，qwen系列的模型，我需要下载什么版本才能使用到`Native MTP`
    6.9 每一个模型配置说明模型属于稠密型，还是其它类型的模型，并且详细说明为什么需要开启`DFlash`，开启后的优点和缺点，或者使用其它加速配置时，也需要详细说明

# 7.前端调用工具
    7.1 roo code或cline
    7.2 如果需要，提供前端调用工具的提示词
    7.3 主要用于Java开发，前端开发，小程序开发
    7.4 需要有很强的架构能力、代码重构能力、遵循编码规范