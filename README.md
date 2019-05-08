# Tools

由于某些原因大部分内容 暂且移入私有库。

batch_size=5 on weibo dataset, bert-base-chinese

| speed/example per second    | fp16    |  fp32  |
| -------- | :----:  | :----: |
| ft-all   | **62-63/4000**   |   None    |
| ft-last  | **19-20/4000**      |   **18-19/4000**    |


| gpu-memory/MB    | fp16    |  fp32  |
| -------- | :----:  | :----: |
| ft-all   | **6343**   |   None    |
| ft-last  | 1700+      |   **2643**    |
