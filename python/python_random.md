---
layout: post 
---

# 问题

我们知道在由于random包中生成的是伪随机数，因此通过设置seed可以固定住随机结果。

但有一次使用时发现设置完seed之后结果依然改变：

代码如下：

```python
 19     random.seed(args.seed)
 20
 21     # read all filenames in list files
 22     with open(args.file, 'r') as f:
 23     ¦   filenames = f.readlines()
 24     filenames = [item.replace('\n', '') for item in filenames]
 25     filenames.sort()
 26     random.shuffle(filenames)
 27     print('total filenames: {}'.format(len(filenames)))
 28     print('==== filenames[:4]', filenames[:4])
 29
 30     # select patient, if None, all patients are selected
 31     patient_ID = [filename[:4] for filename in filenames]
 32     set_patient_ID = set(patient_ID)
 33     print('total {} patients'.format(len(set_patient_ID)))
 34     selected_patient_ID = list(set_patient_ID)
 35     #selected_patient_ID.sort()
 36     selected_patient_ID = selected_patient_ID[:args.patient_num]
 37     #selected_patient_ID = list(set_patient_ID)[:args.patient_num]
 38     print('select {} pathents'.format(len(selected_patient_ID)))
 39     print('selected pathent IDs:\n', selected_patient_ID)
 40     print('-'*100)
 41     print('===== selected_patient_ID[:4]', selected_patient_ID[:4])
```

# 解决方法
我们发现28行每次输出固定，但是41行每次输出不固定。

查找原因，发现是第32行每次将list改为set之后，会被自动排序，但还是没理解为什么会导致出现了随机。

解决方法是增加第35行代码即可，如下：
```python
 19     random.seed(args.seed)
 20
 21     # read all filenames in list files
 22     with open(args.file, 'r') as f:
 23     ¦   filenames = f.readlines()
 24     filenames = [item.replace('\n', '') for item in filenames]
 25     filenames.sort()
 26     random.shuffle(filenames)
 27     print('total filenames: {}'.format(len(filenames)))
 28     print('==== filenames[:4]', filenames[:4])
 29
 30     # select patient, if None, all patients are selected
 31     patient_ID = [filename[:4] for filename in filenames]
 32     set_patient_ID = set(patient_ID)
 33     print('total {} patients'.format(len(set_patient_ID)))
 34     selected_patient_ID = list(set_patient_ID)
 35     selected_patient_ID.sort()
 36     selected_patient_ID = selected_patient_ID[:args.patient_num]
 37     #selected_patient_ID = list(set_patient_ID)[:args.patient_num]
 38     print('select {} pathents'.format(len(selected_patient_ID)))
 39     print('selected pathent IDs:\n', selected_patient_ID)
 40     print('-'*100)
 41     print('===== selected_patient_ID[:4]', selected_patient_ID[:4])
```
