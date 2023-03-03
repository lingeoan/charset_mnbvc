### 项目描述
本项目旨在对大量文本文件进行快速编码检测以辅助mnbvc语料集项目的数据清洗工作

#### 实现机制
1. 读取每个文件的前500个字符(长度可定义)
2. 尝试使用4种编码对字符进行decode ```utf_8```,```utf_16```,```gb18030```,```big5```
3. 将每一组decode的结果对中文字符串和常用中文字进行正则匹配,有匹配结果的表明符合编码要求
4. 当charset-mnbvc无法检测编码或检测到多个编码结果时,会自动调用cchardect进行备用检测机制

#### 模块安装
```
pip install charset-mnbvc
```

#### charset-mnbvc pypi url:
https://pypi.org/project/charset-mnbvc/

##### 根据文件夹获取所有文件编码
```
from charset_mnbvc import api

file_count, results = api.from_dir(
    folder_path=ifolder_path,
)

for result in results:
    print(f"文件名: {result[0]}, 编码: {result[1]}")

```

##### 获取单个文件编码
```
from charset_mnbvc import api

file_path = "test.txt"
coding_name = get_cn_charset(file_path)
print(f"文件名: {file_path}, 编码: {coding_name}")

```


#### 完整使用范例:
```
python chinese_charset_detect.py -i tests
```

测试环境说明（开发测试用机):
* MacBook Air Apple M2, 内存:16 GB, 硬盘:512G, 系统版本:13.2 (22D49)**, Python 3.9.6
* 默认使用charset_mnbvc方案, 可切换使用cchardet方案

#### 编码检测使用范例:
```
usage: chinese_charset_detect.py [-h] [-s] [-c] -i inputDirectory

对大量文本文件进行快速编码检测以辅助mnbvc语料集项目的数据清洗工作

optional arguments:
  -h, --help         show this help message and exit
  -s, --show_result  显示编码检测结果
  -c, --cchardet     使用cchardet方案
  -i inputDirectory  inputDirectory为需要检测的目录
```

##### 使用charset_mnbvc方案:
`python chinese_charset_detect.py -i /Users/alan/Downloads/mop1 -s`

测试结果:
```
编码检测进度: 100%|████████████████████████████████| 1918/1918 [00:00<00:00, 8730.84it/s]
文件名: /Users/alan/Downloads/mop/谁能教教我 真的很棘手.txt, 编码: ['gb18030']
文件名: /Users/alan/Downloads/mop/打架功夫高手请进来交流一下的说.txt, 编码: ['gb18030']
```


##### 使用cchardet方案: 
`python chinese_charset_detect.py -i /Users/alan/Downloads/mop -c -s`

测试结果:
```
编码检测进度: 100%|████████████████████████████████| 1918/1918 [00:00<00:00, 7948.88it/s]
文件名: /Users/alan/Downloads/mop/谁能教教我 真的很棘手.txt, 编码: ['gb18030']
文件名: /Users/alan/Downloads/mop/打架功夫高手请进来交流一下的说.txt, 编码: ['gb18030']
```

#### 编码转换使用范例:
NOTICE: 文件默认转换为utf-8格式, 文件转换前后会将原始文件原地复制为raw格式用于备份, 并用utf-8格式覆盖原始文件, 操作流程如下:

1: 原地复制test.txt 到 test.raw

2: 将文本使用utf-8格式覆盖到test.txt

```
usage: convert_files.py [-h] [-p PROCESS_NUM] [-c] -i inputDirectory

对大量文本文件进行快速编码检测以辅助mnbvc语料集项目的数据清洗工作

optional arguments:
  -h, --help            show this help message and exit
  -p PROCESS_NUM, --process_num PROCESS_NUM
                        指定进程数，默认为4
  -c, --cchardet        使用cchardet方案
  -i inputDirectory     inputDirectory为需要检测的目录

```

`python convert_files.py -i /Users/alan/Downloads/20230101 -p 4`

```
编码检测进度: 100%|█████████████████████████████████████████████████████| 16363/16363 [00:02<00:00, 7466.30it/s]
文件转换进度: 100%|█████████████████████████████████████████████████████| 16363/16363 [00:26<00:00, 621.95it/s]
总文件数: 16363
转换成功文件数: 16319
转换失败文件数: 44
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1053.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1078.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1093.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/472.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/670.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/502.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/927.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1268.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/528.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1057.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/598.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/822.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1178.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1190.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/410.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/376.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/983.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1026.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1036.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/950.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/628.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/761.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/976.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/948.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/741.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/621.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/595.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/914.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/727.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/647.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/450.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1073.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1065.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/877.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/725.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/669.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/480.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/720.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/907.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/536.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/508.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/911.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/1274.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!
/Users/alan/Downloads/20230101/aliyun.20230101.10.名著/910.txt False convert to utf-8 Failed, 编码格式错误:False, 可能是文件内容为空!

```