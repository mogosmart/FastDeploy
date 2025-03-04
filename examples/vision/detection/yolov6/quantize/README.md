# YOLOv6量化模型部署
FastDeploy已支持部署量化模型,并提供一键模型自动化压缩的工具.
用户可以使用一键模型自动化压缩工具,自行对模型量化后部署, 也可以直接下载FastDeploy提供的量化模型进行部署.

## FastDeploy一键模型自动化压缩工具
FastDeploy 提供了一键模型自动化压缩工具, 能够简单地通过输入一个配置文件, 对模型进行量化.
详细教程请见: [一键模型自动化压缩工具](../../../../../tools/auto_compression/)
## 下载量化完成的YOLOv6s模型
用户也可以直接下载下表中的量化模型进行部署.(点击模型名字即可下载)

Benchmark表格说明:
- Rtuntime时延为模型在各种Runtime上的推理时延,包含CPU->GPU数据拷贝,GPU推理,GPU->CPU数据拷贝时间. 不包含模型各自的前后处理时间.
- 端到端时延为模型在实际推理场景中的时延, 包含模型的前后处理.
- 所测时延均为推理1000次后求得的平均值, 单位是毫秒.
- INT8 + FP16 为在推理INT8量化模型的同时, 给Runtime 开启FP16推理选项
- INT8 + FP16 + PM, 为在推理INT8量化模型和开启FP16的同时, 开启使用Pinned Memory的选项,可加速GPU->CPU数据拷贝的速度
- 最大加速比, 为FP32时延除以INT8推理的最快时延,得到最大加速比.
- 策略为量化蒸馏训练时, 采用少量无标签数据集训练得到量化模型, 并在全量验证集上验证精度, INT8精度并不代表最高的INT8精度.
- CPU为Intel(R) Xeon(R) Gold 6271C, 所有测试中固定CPU线程数为1.  GPU为Tesla T4, TensorRT版本8.4.15.

#### Runtime Benchmark
| 模型                 |推理后端            |部署硬件    | FP32 Runtime时延   | INT8 Runtime时延 | INT8 + FP16 Runtime时延  | INT8+FP16+PM Runtime时延  | 最大加速比    | FP32 mAP | INT8 mAP | 量化方式   |
| ------------------- | -----------------|-----------|  --------     |--------      |--------      | --------- |-------- |----- |----- |----- |
| [YOLOv6s](https://bj.bcebos.com/paddlehub/fastdeploy/yolov6s_ptq_model.tar)            | TensorRT  |    GPU    |       9.47    |   3.23    |  4.09      |2.81    |  3.37            | 42.5 | 40.7|量化蒸馏训练 |
| [YOLOv6s](https://bj.bcebos.com/paddlehub/fastdeploy/yolov6s_ptq_model.tar)            | Paddle-TensorRT |    GPU    |       9.31    | None|  4.17  | 2.95       |  3.16            | 42.5 | 40.7|量化蒸馏训练 |
| [YOLOv6s](https://bj.bcebos.com/paddlehub/fastdeploy/yolov6s_ptq_model.tar)          | ONNX Runtime     |    CPU    |   334.65     |  126.38      | None | None|     2.65   |42.5| 36.8|量化蒸馏训练 |
| [YOLOv6s](https://bj.bcebos.com/paddlehub/fastdeploy/yolov6s_ptq_model.tar)             | Paddle Inference  |    CPU    |    352.87   |    123.12    |None | None|     2.87         |42.5| 40.8|量化蒸馏训练 |


#### 端到端 Benchmark
| 模型                 |推理后端            |部署硬件    | FP32 Runtime时延   | INT8 Runtime时延 | INT8 + FP16 Runtime时延  | INT8+FP16+PM Runtime时延  | 最大加速比    | FP32 mAP | INT8 mAP | 量化方式   |
| ------------------- | -----------------|-----------|  --------     |--------      |--------      | --------- |-------- |----- |----- |----- |
| [YOLOv6s](https://bj.bcebos.com/paddlehub/fastdeploy/yolov6s_ptq_model.tar)            | TensorRT  |    GPU    |       15.66    |   11.30   |  10.25      |9.59   |  1.63           | 42.5 | 40.7|量化蒸馏训练 |
| [YOLOv6s](https://bj.bcebos.com/paddlehub/fastdeploy/yolov6s_ptq_model.tar)            | Paddle-TensorRT |    GPU    |       15.03   | None|  11.36 | 9.32       |  1.61            | 42.5 | 40.7|量化蒸馏训练 |
| [YOLOv6s](https://bj.bcebos.com/paddlehub/fastdeploy/yolov6s_ptq_model.tar)          | ONNX Runtime     |    CPU    |   348.21    |  126.38      | None | None| 2.82       |42.5| 36.8|量化蒸馏训练 |
| [YOLOv6s](https://bj.bcebos.com/paddlehub/fastdeploy/yolov6s_ptq_model.tar)             | Paddle Inference  |    CPU    |    352.87   |    121.64    |None | None|    3.04       |42.5| 40.8|量化蒸馏训练 |




## 详细部署文档

- [Python部署](python)
- [C++部署](cpp)
