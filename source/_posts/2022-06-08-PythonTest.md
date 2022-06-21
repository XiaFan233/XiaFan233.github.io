---
title: Python Test
date: 2022-06-08 18:00:00
categories: 
- [Python, test]
tags: 
- test
---

# Pytorch

``` Python
import torch
import torch.backends.cudnn as cudnn


def test_cuda():
    ok = torch.cuda.is_available()
    print("PyTorch:{}".format(ok))
    gpu_name = torch.cuda.get_device_name(0)
    print("GPU:{}".format(gpu_name))
    cuda_v = torch.version.cuda
    print("cuda:{}".format(cuda_v))
    cudnn_v = cudnn.version()
    print("cudnn:{}".format(cudnn_v))


if __name__ == "__main__":
    test_cuda()

```

# OpenCV

``` Python
import numpy as np
import cv2


def test_opencv():
    numpy_v = np.__version__
    print("numpy:{}".format(numpy_v))
    opencv_v = cv2.__version__
    print("opencv:{}".format(opencv_v))


if __name__ == "__main__":
    test_opencv()

```