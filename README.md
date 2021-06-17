

<center><h1>RT-AK ֮ K210���</h1></center>

- [1. ���](#1-���)
- [2. Ŀ¼�ṹ](#2-Ŀ¼�ṹ)
- [3. �����в�����ϸ˵��](#3-�����в�����ϸ˵��)
- [4. �����װ](#4-�����װ)
- [5. ����������](#5-����������)
- [6. ��Ŀ���̱���](#6-��Ŀ���̱���)
- [7. ����ڲ���������](#7-����ڲ���������)

## 1. ���

**����Ŀ�� `RT-AK` ����Ŀ��һ���Ӳ��**

����Ŀ֧���Կ��� `K210` оƬΪĿ��Ӳ��ƽ̨�� AI ����������ڲ�����ʹ�ü�骿���ԭ���ṩ�� `NNCase` ����

- ����Ŀ֧�ֵ�ģ����������������

  - `TFLite`
  - `Caffe`
  - `ONNX`
  
- ֧�ֵ�����

  - [TFLite ops](./k_tools/tflite_ops.md)
  - [Caffe ops](./k_tools/caffe_ops.md)
  - [ONNX ops](./k_tools/onnx_ops.md)

- ����Ŀ�����Ĺ���

  - ������빤���� (��Ӧ��ϵͳѡ���Ӧ�İ汾�������� Windows Ϊ��)

     [xpack-riscv-none-embed-gcc-8.3.0-1.2-win32-x64.zip](https://github.com/xpack-dev-tools/riscv-none-embed-gcc-xpack/releases/tag/v8.3.0-1.2) | `Version: v8.3.0-1.2`

  - ԭ������
    - NNCase ģ��ת�����ߣ��Ѿ���ǰ���غã�λ�� `./k_tools`�� [�ٷ����ϴ�����](https://github.com/kendryte/nncase/blob/master/docs/USAGE_ZH.md)

    - K-Flash ��¼���ߣ�[������ѡ�� K-Flash.zip ](https://github.com/kendryte/kendryte-flash-windows/releases)

      > PS: [linux��python�ű���¼](https://github.com/kendryte/kflash.py)

- ����Ŀ���������ĵ�
  - [RT-AK֮K210�����������](./docs/RT-AK֮K210�����������.md)

## 2. Ŀ¼�ṹ

```shell
./
������ backend_plugin_k210                 # ��ģ����Ϣע�ᵽ RT-AK Lib ���
��?? ������ backend_k210_kpu.c
��?? ������ backend_k210_kpu.h
��?? ������ readme.md
������ datasets                            # ����ģ�����������ݼ�����
��?? ������ mnist_datasets
��?? ������ readme.md
������ docs                                # RT-AK ֮ K210 �������ĵ�
��?? ������ images
��?? ������ Q&A.md
��?? ������ RT-AK֮K210�����������.md
��?? ������ version.md
������ generate_rt_ai_model_h.py
������ k210.c                              # rt_ai_<model_name>_model.c ʾ���ļ�
������ k_tools                             # k210 ԭ�������Լ�����ĵ�
��?? ������ kendryte_datasheet_20180919020633.pdf
��?? ������ kendryte_standalone_programming_guide_20190704110318_zh-Hans.pdf
��?? ������ ncc
��?? ������ ncc.exe                         # k210 ģ��ת�� kmodel ģ�͹���
��?? ������ readme.md
������ plugin_k210_parser.py               # RT-AK ֮ k210 ������в���
������ plugin_k210.py                      # RT-AK ֮ k210 �������
������ README.md
```

## 3. �����в�����ϸ˵��

$$
RT-AK �����еĲ��� = ��RT-AK �������� + K210 ���������
$$

- RT-AK ����������[����](https://github.com/RT-Thread/RT-AK/tree/main/RT-AK/rt_ai_tools#0x03-%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E)

- �ò����� RT-AK ֮ K210 ����Ĳ���˵������� `plugin_k210_parser.py` 

������Ҫע����ǼӴֲ��ֵ�������������ע�⿴���������

��ϸ��ʹ��˵�����Ķ������½�

| Parameter                    | Description                                                  |
| ---------------------------- | ------------------------------------------------------------ |
| --embed_gcc                  | ������빤����·����**�Ǳ���**������У������� `rt_config.py` �ļ��������ָ��������Ҫ�ڱ����ʱ��ָ���ù�����·�� |
| --ext_tools                  | `NNCase` ·������ģ��ת��Ϊ `kmodel`��Ĭ���� `./platforms/k210/k_tools` |
| **--inference_type**         | �Ƿ�ģ������Ϊ���Σ������ `float`����������������ʹ�� `KPU` ���٣�Ĭ���� `uint8` |
| --dataset                    | ģ����������������Ҫ�õ������ݼ���ֻ��Ҫ������ `--inference-type` Ϊ `uint8` ʱ�ṩ������� |
| --dataset_format             | ����ָ������У׼���ĸ�ʽ��Ĭ���� `image`���������Ƶ֮������ݼ�������Ҫ����Ϊ `raw` |
| --weights_quantize_threshold | �����Ƿ����� `conv2d` �� `matmul weights` ����ֵ����� `weights` �ķ�Χ���������ֵ��`nncase` ������������ |
| --output_quantize_threshold  | �����Ƿ����� `conv2d` �� `matmul weights` ����ֵ����������Ԫ�ظ���С�������ֵ��`nncase` �������������� |
| --no_quantized_binary        | ���� `quantized binary` ���ӣ�`nncase` ������ʹ�� `float binary` ���ӡ� |
| --dump_weights_range         | ��һ������ѡ�������ʱ `ncc` ���ӡ�� `conv2d weights` �ķ�Χ�� |
| --convert_report             | ģ��ת���� `kmodel` ����־�����Ĭ���� `./platforms/k210/convert_report.txt` |
| --model_types                | `RT-AK Tools` ��֧�ֵ�ģ�����ͣ�Ŀǰģ��֧�ַ�Χ��`tflite��onnx��caffe` |
| --enable_rt_lib              | �� `project/rtconfgi.h` �д򿪺궨�� `RT_AI_USE_K210`��Ĭ���� `RT_AI_USE_K210` |
| **--clear**                  | �Ƿ���Ҫɾ�� `convert_report.txt` ��Ĭ�� `False`             |

## 4. �����װ

�ò������������װ��

ֻ��Ҫ��¡����Ŀ���̣�[RT-AK](https://github.com/RT-Thread/RT-AK)

���뵽 `RT-AK/rt_ai_tools` ·���£�

**����Ҫ**��ִ�� `python aitools.py --xxx` ��ͬʱָ�� `platform` ����Ϊ K210 ���ɣ�������Զ����ء�

## 5. ����������

��Ҫ���뵽 `RT-AK/rt_ai_tools` ·���£�ִ�������е�ĳһ������

���� `your_project_path` ��ӵ�� `RT-Thread` ϵͳ�� `BSP` ·������������ṩ��һ�� `BSP` ��[���ص�ַ](http://117.143.63.254:9012/www/RT-AK/sdk-bsp-k210.zip)

�����ṩ�� `BSP` �� `K210` �� `SDK` �� `V0.5.6` �汾��

> https://github.com/RT-Thread/rt-thread/bsp/k210 �µ� SDK ��ߵ� v0.5.7���������е� v0.5.6 ����������ṩ�� BSP�����������ṩ�� BSP Ϊ׼��Ҳ��ӭ��λͬѧ�� Github ���� issue �� pr

```bash
# ����������ʹ�� KPU ���٣� --inference_type
$ python aitools.py --project=<your_project_path> --model=<your_model_path> --platform=k210  --inference_type=float

# ��������ָ��������빤����·��
$ python aitools.py --project=<your_project_path> --model=<your_model_path> --platform=k210 --embed_gcc=<your_RISCV-GNU-Compiler_path> --inference_type=float

# ����Ϊ uint8��ʹ�� KPU ���٣��������ݼ�ΪͼƬ
$ python aitools.py --project=<your_project_path> --model=<your_model_path> --platform=k210 --embed_gcc=<your_RISCV-GNU-Compiler_path> --dataset=<your_val_dataset>

# ����Ϊ uint8��ʹ�� KPU ���٣��������ݼ�Ϊ��Ƶ֮���ͼƬ��--dataset_format
$ python aitools.py --project=<your_project_path> --model=<your_model_path> --platform=k210 --embed_gcc=<your_RISCV-GNU-Compiler_path> --dataset=<your_val_dataset> --dataset_format=raw

# ʾ��(����ģ�ͣ�ͼƬ���ݼ�)
$ python aitools.py --project="D:\Project\k210_val" --model="./Models/facelandmark.tflite" --model_name=facelandmark --platform=k210 --embed_gcc="D:\Project\k210_third_tools\xpack-riscv-none-embed-gcc-8.3.0-1.2\bin" --dataset="./platforms/plugin_k210/datasets/images"
```

������

```shell
# ָ��ת����ģ�����ƣ�--model_name Ĭ��Ϊ network
$ python aitools.py --project=<your_project_path> --model=<your_model_path> --model_name=<model_name> --platform=k210 --embed_gcc=<your_RISCV-GNU-Compiler_path> --dataset=<your_val_dataset> --clear

# ������ģ��ת����־��--clear
$ python aitools.py --project=<your_project_path> --model=<your_model_path> --platform=k210 --embed_gcc=<your_RISCV-GNU-Compiler_path> --dataset=<your_val_dataset> --clear
```

## 6. ��Ŀ���̱���

**��Ҫ׼���ý�����빤������ xpack-riscv-none-embed-gcc-8.3.0-1.2**

���ñ��뻷����

```shell
set RTT_EXEC_PATH=your_toolchains
# �����޸�rtconfig.py �ļ����ڵ�22������ os.environ['RTT_EXEC_PATH'] = r'your_toolchains'
scons -j 6	
```

���������ȷ���󣬻���� `rtthread.elf`��`rtthread.bin`�ļ���

���� `rtthread.bin` ��Ҫ��д���豸�н������С�

## 7. ����ڲ���������

- [x] �ж�ģ���Ƿ�֧��
- [x] ģ��ת���� `kmodel` ģ�ͣ������� `project/applications` 
- [x] `kmodel` ģ��ת��Ϊʮ�����ƣ������� `project/applications` 
- [x] ���� `rt_ai_<model_name>_model.h` �ļ��������� `project/applications` 
- [x] ���� `rt_ai_<model_name>_model.c` �ļ��������� `project/applications` 
- [x] �� `project` ��д�� `RTT_EXEC_PATH` ��������
- [x] �ж��Ƿ�ɾ�� `convert_report.txt`
- [ ] ~~�Զ�����ģ�������Ӧ�ô���~~

<details>
<summary>���ܺ���</summary> 
<pre><code>
### 7.1 Function1 - �ж�ģ���Ƿ�֧��
<br>
- ������`is_support_model_type(model_types, model)`
- ���ܣ��ж�ģ�������Ƿ�֧��
- input: (model_types, model)
- output: model name
<br>
### 7.2 Function2 - ת�� kmodel
<br>
- ������`convert_kmodel(model, project, dataset, kmodel_name, convert_report)`
- ���ܣ�������ģ��ת�� `kmodel` ģ�ͣ�����·����`project/applications/<kmodel_name>.kmodel` ������������־����Ϊ��`./platforms/k210/convert_report.txt`
- input: (model, project, dataset, kmodel_name, convert_report)
- output: ģ��ת���������־
<br>
### 7.3 Function3 -  ת��16����
<br>
- ������`hex_read_model(self, project, model)`
- ���ܣ��� `kmodel` ģ��ת��Ϊʮ�����ƣ�`project/applications/<kmodel_name>_kmodel.c` 
- input: (project, model)
<br>
### 7.4 Function4 - ���� rt_ai_model.h
<br>
- ������`rt_ai_model_gen(convert_report, project, model_name)`
- ���ܣ����� `./platforms/k210/convert_report.txt` ���� `rt_ai_<model_name>_model.h` �ļ�
- input: (convert_report, project, model_name)
- output: `rt_ai_<model_name>_model.h` �ļ�����
<br>
### 7.5 Function5 - ���� rt_ai_model.c
<br>
- ������`load_rt_ai_example(rt_ai_example, project, old_name, new_name, platform)`
- ���ܣ����� `Documents/k210.c` ���� `rt_ai_<model_name>_model.c` �ļ�
- input: (Documents_path, project, default_name, kmodel_name, platform)
<br>
### 7.6 Function6 - RTT_EXEC_PATH ��������
<br>
- ������`set_gcc_path(project, embed_gcc)`
- ���ܣ��� `project/rtthread.py` �ļ��ĵ�ʮ����д�� `RTT_EXEC_PATH` �����������Ͳ����� `env` ���ֶ�ָ��·���ˡ�
- input: (project, embed_gcc)
</code></pre>
</details>