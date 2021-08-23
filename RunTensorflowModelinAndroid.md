아래 내용은 [reference](https://www.tensorflow.org/lite/guide/build_android?hl=ko) 사이트를 참고하여서 정리한 내용입니다.

우선 환경 설정을 해야하는데, 

Tensorflow를 Android에서 사용할 수 있는 환경을 만들어줘야한다. 

Docker를 사용하는 방법과 Docker를 사용하지 않는 방법 두 가지 모두가 사이트에 소개 되어있고, 오늘 Docker를 사용하지 않는 방법을 정리해보려한다. 

우선 Tensorflow를 빌드할 수 있는 시스템인 Bazel을 설치해야한다고 한다. 근데 Bazel을 사용해서 빌드하려면 결국 Android NDK 및 SDK가 설치되어있어야한다. 


정리하면, 

1. Bazel 빌드시스템 [설치](https://docs.bazel.build/versions/4.2.0/install-windows.html)
2. Android NDK
3. Android SDK
NDK는 C나 C++ 언어(native-code languages)로 작성된 프로그램을 JAVA에서 다시 만들 필요없이 재사용할 수 있게 해주는 toolkit이다. 

### Requirements 

## Installing Bazel

Bazel을 설치하려면 'Visual C++ Redistributable for Visual Studio 2015'가 필요하다. ([Visual C++ 설치](https://www.microsoft.com/en-us/download/details.aspx?id=48145)) 

[Install the Bazel from github](https://github.com/bazelbuild/bazel/releases)

나는 4.2.0 버전으로 다운 받았다. 
source가 엄청 많은데, 그 중에서도 bazel-4.2.0-windows-x86_64.zip을 다운 받았다. 
zip 안에는 Bazel.exe 하나만 있었다. 
실행해보니 아래와 같은 창이 떴다. 
![image](https://user-images.githubusercontent.com/42258047/130380738-2b2f9cfa-5d7d-46bc-9872-4e8d4b14681a.png)

이걸 항상 이렇게 실행할 수는 없으니 평소에 좀 쉽게 access 할 수 있도록 설정을 해주자. 

command line에 

```
set PATH=%PATH%;<path to the Bazel binary>
```
를 해주면 된다고 설명 되어있는데 

현재 bazel.exe 파일이 있는 폴더를 PATH로 설정해주면 된다. 
 
나는 window로 진행 중이라, window에서 cmd를 켜고 아래와 같이 커맨드를 쳤다. 

```
set PATH=./Bazel/
bazel version
```

결과는 아래와 같이 나왔다. 
![image](https://user-images.githubusercontent.com/42258047/130381482-71156846-8e9a-4c8a-bb6b-f476c75503bf.png)

label에 4.2.0으로 뜨는 걸봐서는 잘 된 거 같다!

일단 다음으로 넘어가보자. 

### Installing NDK

따로 파일을 다운 받을 수 있긴 한데 android SDK를 이용해서 설치해보자. 

1) 프로젝트를 연 상태에서 Tools>SDK Manager를 클릭한다. 
2) NDK(Side by side) 항목을 체크해준다. 
3) Ok 누르고 창 닫기

![image](https://user-images.githubusercontent.com/42258047/130382161-b9c95840-f517-40a8-8c16-860d332dfa8a.png)

## WORKSPACE 및 .bazelrc 구성하기


