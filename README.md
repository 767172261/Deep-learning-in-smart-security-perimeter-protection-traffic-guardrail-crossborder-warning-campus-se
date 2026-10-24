
---

### 🚧 翻越护栏/围栏/围墙行为检测数据集 

| **项目** | **详细信息** |
|---------|-------------|
| **数据集名称** | 翻越护栏数据集（或：越界行为检测数据集） |
| **任务类型** | 目标检测（Object Detection） / 行为识别（间接） |
| **图像总数** | 648 张 |
| **标注文件数量** | 648 个（与图像一一对应） |
| **标注格式** | - XML（Pascal VOC 格式）<br>- TXT（YOLO 格式，已划分） |
| **数据划分（基于 YOLO 格式）** | - **训练集（train）**：518 张 + 518 个 `.txt`<br>- **验证集（val）**：130 张 + 130 个 `.txt` |
| **图像内容** | 包含人员正在或已完成 **翻越护栏、围栏、围墙** 的行为场景 |
| **场景多样性** | 涵盖白天、黄昏、夜间（部分带补光）、不同角度、不同遮挡程度 |
| **目标类别（nc=1）** | `overstep`（翻越）<br>✅ 所有标注均围绕“正在翻越”的人体进行边界框标注 |
| **标注质量** | 人工精标，边界框紧密贴合人体躯干与动作姿态，排除静态站立人员 |
| **适用模型** | YOLOv5 / YOLOv8 / YOLOv11 / Faster R-CNN / SSD 等主流检测模型 |
| **是否可直接训练** | ✅ 是（提供已划分的 YOLO 格式 `.txt` 文件，可直接用于训练） |
| **数据来源** | 公共监控视频抽帧 + 开源数据增强 + 少量合成数据 |
| **应用场景** | 智慧安防、周界防护、交通护栏越界预警、校园安全监控等 |

---

### 📁 推荐数据组织结构（YOLO 训练友好）

```bash
overstep_dataset/
├── images/
│   ├── train/      # 518 张训练图像
│   └── val/        # 130 张验证图像
├── labels/
│   ├── train/      # 518 个 .txt 标签文件（YOLO格式）
│   └── val/        # 130 个 .txt 标签文件
├── annotations/    # 原始 Pascal VOC XML 文件
│   └── *.xml       # 648 个 XML 文件（可选保留）
├── dataset.yaml    # YOLO 配置文件
└── README.md
```

---

### 🧾 `dataset.yaml` 配置文件示例

```yaml
# dataset.yaml
train: ./overstep_dataset/images/train
val: ./overstep_dataset/images/val

# 类别数量
nc: 1

# 类别名称
names:
  - overstep  # 翻越行为（人体）
```

---

### 📊 数据集统计摘要

| 统计项 | 数量 | 说明 |
|--------|------|------|
| 总图像数 | 648 | 覆盖多种翻越姿态 |
| 训练集图像 | 518 | 占比 80% |
| 验证集图像 | 130 | 占比 20% |
| 标注总数 | 648 | 每图至少1个 `overstep` 标注 |
| 平均每图目标数 | ~1.2 | 少数图像含多人翻越 |
| 图像尺寸范围 | 1920×1080, 1280×720, 720×576 等 | 多来自监控摄像头 |

---

### ✅ 数据集优势

| 优势 | 说明 |
|------|------|
| ✅ **行为聚焦** | 专一标注“翻越”动作，排除干扰（如行走、站立） |
| ✅ **双格式支持** | 同时提供 XML 和 TXT 标注，兼容 VOC 与 YOLO 生态 |
| ✅ **预划分数据集** | 无需手动拆分，开箱即用，避免划分偏差 |
| ✅ **真实场景** | 来自实际监控场景，具备光照、遮挡、模糊等挑战性 |
| ✅ **高实用性** | 可直接部署于周界安防系统，实现自动越界报警 |

---

### 🎯 典型应用场景

| 应用场景 | 功能实现 |
|--------|----------|
| **智慧园区安防** | 自动识别非法翻墙入侵行为，联动报警系统 |
| **交通护栏监控** | 检测行人翻越马路护栏，用于交通管理与执法 |
| **铁路/地铁周界** | 防止人员非法进入轨道区域 |
| **学校/小区围墙** | 提升校园与社区安全，预防非法闯入 |
| **AI视频分析平台** | 接入RTSP流，实现24小时无人值守越界检测 |

---

### ⚠️ 使用建议

- **类别扩展**：可结合 `person` 类别，区分“正常通行”与“翻越行为”。
- **数据增强**：建议使用 Mosaic、MixUp、随机光照增强提升泛化能力。
- **模型轻量化**：若需边缘部署（如摄像头端），推荐使用 YOLOv8n 或 YOLOv5s。
- **后处理逻辑**：可结合跟踪（ByteTrack）判断“翻越”轨迹，减少误报。

---




![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fa039a431a114812a6a7ffa349b628c3.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/68ed0e89240a4aed8ee765441aba6c73.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/313dcfd1c7684ca2836fb5da925a638b.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7be232f440444f7eaa8752821be9363e.png)
使用 **YOLOv8** 模型来训练这个“翻越护栏”检测数据集。你已经提供了带 `XML`（VOC格式）和 `TXT`（YOLO格式）标注的数据集，并且已经划分好训练集和验证集，非常适合直接用于目标检测任务。

以下是完整的训练流程，包括：环境配置、数据结构说明、调用预训练模型、训练代码、推理代码、评估代码。

代码示例，同学，仅供参考学习使用。

---

## ✅ 一、环境准备（CUDA + Anaconda + 虚拟环境 + 依赖安装）

### 1. 检查 CUDA 驱动
```bash
nvidia-smi
```
确保输出中显示你的 GPU 和驱动版本。

### 2. 安装 Anaconda
前往官网下载并安装：https://www.anaconda.com/products/distribution

### 3. 创建 Python 虚拟环境
```bash
conda create -n fence_climbing python=3.9
conda activate fence_climbing
```

### 4. 安装必要依赖
```bash
# PyTorch (根据你的 CUDA 版本选择，这里以 11.8 为例)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# YOLOv8 (Ultralytics 最新版)
pip install ultralytics

# 其他工具
pip install opencv-python matplotlib pandas
```

> ✅ 验证安装：
```python
import torch
print(torch.cuda.is_available())  # 应输出 True
```

---

## ✅ 二、数据集结构与 `data.yaml` 配置

### 假设你的数据结构如下：
```
fence_dataset/
├── images/
│   ├── train/        # 518 张
│   └── val/          # 130 张
├── labels/
│   ├── train/        # 518 个 .txt 文件
│   └── val/          # 130 个 .txt 文件
└── data.yaml         # 数据集配置文件
```

> ✅ 说明：你已有 `.txt` 格式的 YOLO 标注文件，无需转换。

---

### 创建 `data.yaml` 文件（放在根目录）
```yaml
# data.yaml
train: ./fence_dataset/images/train
val: ./fence_dataset/images/val

# 类别数量
nc: 1

# 类别名称
names: ['climbing']
```

> ⚠️ 注意：
> - 所有 `.txt` 标注中的类别索引必须是 `0`（因为 nc=1）。
> - 每行格式为：`0 x_center y_center width height`（归一化到 [0,1]）。

---

## ✅ 三、训练代码（使用 YOLOv8 官方预训练模型）

```python
from ultralytics import YOLO
import torch

# 设置设备
device = 'cuda' if torch.cuda.is_available() else 'cpu'
print(f"Using device: {device}")

# 加载预训练模型（推荐使用 yolov8m 或 yolov8l 提升小目标检测能力）
model = YOLO('yolov8s.pt')  # 可选: yolov8n, yolov8s, yolov8m, yolov8l

# 开始训练
results = model.train(
    data='fence_dataset/data.yaml',      # 数据配置文件路径
    epochs=150,                          # 训练轮数
    imgsz=640,                           # 输入图像大小
    batch=16,                            # 批次大小（根据显存调整）
    name='yolov8s_fence_climbing',       # 实验名称
    project='runs/fence',                # 保存路径
    device=device,                       # 使用 GPU
    workers=4,                           # 数据加载线程数
    optimizer='AdamW',                   # 更稳定的优化器
    lr0=0.001,                           # 初始学习率
    patience=20,                         # 早停：验证 loss 连续 20 轮不下降则停止
    save_period=10,                      # 每 10 轮保存一次
    exist_ok=True,
    augment=True,                        # 启用数据增强（提升泛化）
    close_mosaic=10                      # 最后10轮关闭 Mosaic 增强，提升收敛
)

print("✅ 训练完成！")
```

> 📁 模型将保存在 `runs/fence/yolov8s_fence_climbing/` 目录下，包含：
> - `weights/best.pt`：最佳模型
> - `weights/last.pt`：最后一轮模型
> - `results.png`：训练曲线
> - `confusion_matrix.png`：混淆矩阵

---

## ✅ 四、推理代码（检测新图像或视频）

### 1. 单张图像检测
```python
from ultralytics import YOLO

# 加载训练好的模型
model = YOLO('runs/fence/yolov8s_fence_climbing/weights/best.pt')

# 检测图像
results = model.predict(
    source='test_image.jpg',
    conf=0.3,           # 置信度阈值
    iou=0.45,           # NMS IOU 阈值
    show=False,         # 是否弹窗显示
    save=True,          # 保存结果图
    project='inference_output',
    name='result'
)

# 显示结果
for r in results:
    im_array = r.plot()  # 绘制边界框和标签
    im = r.orig_img  # 原始图像
    im = im_array if im_array is not None else im
    import cv2
    cv2.imshow('Detection', im)
    cv2.waitKey(0)
cv2.destroyAllWindows()
```

### 2. 视频检测（摄像头或文件）
```python
results = model.predict(
    source=0,           # 0=摄像头，也可传入视频路径
    show=True,          # 实时显示
    save=False,
    conf=0.3
)
```

### 3. 批量检测文件夹
```python
results = model.predict(
    source='fence_dataset/images/val/',
    save=True,
    project='inference_output',
    name='val_batch'
)
```

---

## ✅ 五、模型评估代码（在验证集上评估）

```python
from ultralytics import YOLO

# 加载最佳模型
model = YOLO('runs/fence/yolov8s_fence_climbing/weights/best.pt')

# 在验证集上评估
metrics = model.val(
    data='fence_dataset/data.yaml',
    split='val',        # 使用验证集
    batch=16,
    imgsz=640,
    device='cuda' if torch.cuda.is_available() else 'cpu',
    project='val_results',
    name='fence_val'
)

# 输出关键指标
print(f"mAP@0.5: {metrics.box.map:.4f}")
print(f"mAP@0.5:0.95: {metrics.box.map50_95:.4f}")
print(f"Precision: {metrics.box.precision:.4f}")
print(f"Recall: {metrics.box.recall:.4f}")
print(f"FPS: {metrics.speed['inference']:.2f}")  # 推理速度
```

---

## ✅ 六、可视化训练结果（可选）

```python
from PIL import Image

# 查看训练曲线
Image.open("runs/fence/yolov8s_fence_climbing/results.png").show()
```

---

## ✅ 七、模型导出（用于部署）

```python
# 导出为 ONNX（通用部署）
model.export(format='onnx', dynamic=True, opset=13)

# 导出为 TensorRT（高性能推理）
# model.export(format='engine', device=0)

# 导出为 TorchScript
model.export(format='torchscript')
```

---

## ✅ 总结

| 步骤 | 内容 |
|------|------|
| 🧰 环境 | CUDA + Anaconda + PyTorch + Ultralytics |
| 📁 数据 | `images/train/val`, `labels/train/val`, `data.yaml` |
| 🏃‍♂️ 训练 | `model.train()` + 预训练权重 |
| 🔍 推理 | `model.predict()` 支持图像/视频/摄像头 |
| 📊 评估 | `model.val()` 获取 mAP、Precision、Recall |
| 🚀 部署 | 导出为 ONNX/TensorRT |

---

以上文字及代码仅供参考学习使用。
