# Tools

工欲善其事，必先利其器。

由于某些原因暂且移入私有库。

batch_size=5 on weibo dataset, bert-base-chinese

| speed/example per second    | fp16    |  fp32  |
| -------- | :----:  | :----: |
| ft-all   | 56/4000   |   None    |
| ft-last  | 20/4000      |   19/4000    |


| gpu-memory/MB    | fp16    |  fp32  |
| -------- | :----:  | :----: |
| ft-all   | 6500+   |   None    |
| ft-last  | 1700+      |   2600+    |
