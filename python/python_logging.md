---
layout: post
---

- [Demo](#demo)
- [Reference](#reference)

## Demo

```python 
import logging
import sys

logging.basicConfig(filename='./test.log', filemode='a', level=logging.INFO,
                    format='%(asctime)s %(message)s', datefmt='%H:%M:%S')
logger = logging.getLogger()
logger.addHandler(logging.StreamHandler(sys.stdout))

logger.info('this is a test log')
```

## Reference

1. [Logging facility for Python](https://docs.python.org/3.6/library/logging.html)
2. [python logging模块](https://www.cnblogs.com/liujiacai/p/7804848.html)
3. [python中logging日志模块详解](https://www.cnblogs.com/xianyulouie/p/11041777.html)
