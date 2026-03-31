---
name: binwalk-analyze
description: Analyze firmware and binary files using binwalk — scan signatures, extract embedded files, calculate entropy, identify CPU architecture. Use when the user asks to analyze firmware, extract embedded files from a binary, check file entropy, or identify packed/compressed data.
---

# binwalk-analyze — 固件分析与提取

使用 binwalk 对固件镜像和二进制文件进行签名扫描、文件提取、熵分析。

## Pre-check

1. **binwalk 可用**: `/usr/bin/binwalk` (v2.3.3)
2. **输入文件**: `file $ARGUMENTS` 确认文件存在且类型合理

binwalk 缺失时: `apt install binwalk`。

## Usage

```bash
/usr/bin/binwalk $ARGUMENTS
```

## Usage Examples

```bash
# 签名扫描
/usr/bin/binwalk firmware.bin

# 提取嵌入文件
/usr/bin/binwalk -e firmware.bin

# 递归提取嵌套结构
/usr/bin/binwalk -eM firmware.bin

# 提取到指定目录
/usr/bin/binwalk -e -C /tmp/fw-out firmware.bin

# 熵分析 + 保存 PNG
/usr/bin/binwalk -EJ firmware.bin

# CPU 架构识别
/usr/bin/binwalk -Y firmware.bin
```

## Output Structure

`binwalk -e firmware.bin` 提取后：

```
_firmware.bin.extracted/
├── 0.tar              # offset 0x0 处提取
├── 12345.squashfs     # 识别的文件系统
├── squashfs-root/     # 解压后的文件系统
│   ├── bin/
│   ├── etc/
│   └── ...
└── ...
```

文件名 = 嵌入数据在原文件中的偏移量。

## Workflow

1. `binwalk $ARGUMENTS` — 签名扫描，了解内部结构
2. `binwalk -E $ARGUMENTS` — 熵分析（高熵 >0.95 = 加密或压缩）
3. `binwalk -e $ARGUMENTS` — 提取；嵌套结构用 `-eM`
4. 检查提取出的文件系统（配置、密钥、脚本、ELF）
5. ELF 二进制交给 ida-analyze 深入分析

## Error Handling

| 症状 | 原因 | 修复 |
|------|------|------|
| 扫描无结果 | 加密或自定义格式 | `-E` 看熵；全程高平坦熵 = 加密 |
| 提取目录为空 | 缺解压工具 | `apt install squashfs-tools` 等 |
| 提取不完整 | 文件截断/损坏 | 检查文件大小，`-l` 限制扫描范围 |

## Integration with ida-analyze

```bash
/usr/bin/binwalk -eM firmware.bin
find _firmware.bin.extracted/ -type f -exec file {} \; | grep ELF
/home/user/q7h2q9/tools/IDA9.1/ida-analyze <path_to_elf>
```
