android studio에서 pytorch로 학습시킨 model을 바로 사용하는 것은 불가능하다. 
그래서 우리는 몇가지 step을 거쳐야한다. 

pytorch model --> onnx --> tensorflow 로 바꿀 것이다.

우선 pytorch version에서 onnx로 바꾸는 것 부터 해보자. 

## 1. Pytorch --> ONNX 

 본 내용은 [pytorch_tutorial](https://tutorials.pytorch.kr/advanced/super_resolution_with_onnxruntime.html) 을 참고하였다. 
 
 ONNX 형식으로 바꾸기 전에 잠깐 ONNX 가 무엇인지 살펴보도록하자. 
 
 ONNX 런타임은 ONNX모델을 위한 엔진으로서 성능에 초점을 맞추고 있고 여러 다양한 플랫폼과 하드웨어(윈도우, 리눅스, 맥을 비롯한 플랫폼 뿐만 아니라 CPU, GPU 등의 하드웨어)에서 효율적인 추론을 가능하게 한다. 

 ONNX 에 대해 더 자세한 설명이 필요하다면, 이 [사이트](https://cloudblogs.microsoft.com/opensource/2019/05/22/onnx-runtime-machine-learning-inferencing-0-4-release/)를 참고하도록 하자. 
 이제 본격적으로 본 튜터리얼을 위해 필요한 library들을 설치해보자. 
 
### 1) Install ONNX and ONNX Runtime
'''
pip install onnx
pip install onnxruntime
'''

### 2) import onnx

'''
import torch.onnx
'''

로 진행하면 된다. 

(물론 torch는 prerequisted library 이다.)

### 3) model 불러 오기 

사용할 모델 정의 후, 미리 학습된 가중치를 가져온다. batch_size는 임의의 수를 넣어주고 
학습된 가중치로 모델을 초기화 해준다.

이후! __모델을 추론모드로 전환__ 해야한다. 
즉 torch_model.eval() 또는 torch_model.train(False)를 호출해야하는 것이다. 
( 이는 dropout이나 batchnorm과 같은 연산들이 추론과 학습모드에서 작동 방식에 차이가 있기 때문에 필요에 따라 꼭 바꾸어 주여야한다. )

### 4) export the model to onnx version

tracing, scripting 두 가지 방법이 존재하는데 우선 tracing 부터 살펴보도록하자. 
