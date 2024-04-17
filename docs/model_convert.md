# 模型转换

## 运行环境

- Clone the repo

```bash
git clone https://github.com/junruizh2021/keyword-spotting.git && cd keyword-spotting
```
- Install Conda: please see https://docs.conda.io/en/latest/miniconda.html

- Install miniconda
```bash
conda create -n kws python=3.9
conda activate kws
pip install -r requirements.txt
```


下面示例模型转换，示例均使用“你好问问”数据训练的模型。

## Max-pooling方案模型转换

### 1.下载模型

```bash
git clone https://www.modelscope.cn/daydream-factory/keyword-spot-dstcn-maxpooling-wenwen.git
```

模型目录结构如下 >> 

```bash
$ tree keyword-spot-dstcn-maxpooling-wenwen

keyword-spot-dstcn-maxpooling-wenwen
├── avg_30.pt
├── configuration.json
├── config.yaml
├── global_cmvn
├── README.md
```

### 2.模型转换

### 2.1 pytorch to onnx

```bash
python model_convert/export_onnx.py \
 --config keyword-spot-dstcn-maxpooling-wenwen/config.yaml \
 --checkpoint keyword-spot-dstcn-maxpooling-wenwen/avg_30.pt \
 --onnx_model keyword-spot-dstcn-maxpooling-wenwen/onnx/keyword-spot-dstcn-maxpooling-wenwen.onnx
```

### 2.2 onnx2ort. 用于端侧设备部署.

```bash
python -m onnxruntime.tools.convert_onnx_models_to_ort keyword-spot-dstcn-maxpooling-wenwen/onnx/keyword-spot-dstcn-maxpooling-wenwen.onnx
```

输出模型结构

```
tree keyword-spot-dstcn-maxpooling-wenwen

keyword-spot-dstcn-maxpooling-wenwen
├── avg_30.pt
├── configuration.json
├── config.yaml
├── global_cmvn
├── onnx
│   ├── keyword-spot-dstcn-maxpooling-wenwen.onnx  #中间模型
│   ├── keyword-spot-dstcn-maxpooling-wenwen.ort   #用于端侧部署的ort模型
│   ├── keyword-spot-dstcn-maxpooling-wenwen.required_operators.config
│   ├── keyword-spot-dstcn-maxpooling-wenwen.required_operators.with_runtime_opt.config
│   └── keyword-spot-dstcn-maxpooling-wenwen.with_runtime_opt.ort
├── README.md
└── words.txt
```

## CTC方案模型转换
*[To-do]*