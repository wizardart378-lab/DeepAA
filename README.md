DeepAA
====

This is convolutional neural networks generating ASCII art.
This repository is under construction.

This work is accepted by [NIPS 2017 Workshop, Machine Learning for Creativity and Design](https://nips2017creativity.github.io/)
The paper: [ASCII Art Synthesis with Convolutional Networks](https://nips2017creativity.github.io/doc/ASCII_Art_Synthesis.pdf)

[Web application (using previous version model)](https://tar-bin.github.io/DeepAAonWeb/) (by [tar-bin](https://github.com/tar-bin))

![image sample](https://github.com/OsciiArt/DeepAA/blob/master/sample%20images/images%20generated%20with%20CNN/21%20generated.png)


## Change log
+ 2017/12/2 added light model
## Requirements

+ TensorFlow (1.3.0)
+ Keras (2.0.8)
+ NumPy (1.13.3)
+ Pillow (4.2.1)
+ Pandas (0.18.0)
+ Scikit-learn (0.19.0)
+ h5py (2.7.1)
+ model's weight (download it from [here](https://drive.google.com/open?id=0B90WglS_AQWebjBleG5uRXpmbUE) and place it in dir `model`.)
+ training data (additional, download it from  [here](https://drive.google.com/open?id=1L5n5ICrsXtsWkT-aq2et1FTzp-RH3CeS), extract it and place the extracted directory in dir `data`.)
)

## How to use
please change the line 15 of `output.py `

```
image_path = 'sample images/original images/21 original.png' # put the path of the image that you convert.
```
into the path of image file that you use.
You should use a grayscale line image.

then run `output.py `.
converted images will be output at `output/ `.

You can select light model by change the line 13, 14  of `output.py ` into
```
model_path = "model/model_light.json"
weight_path = "model/weight_light.hdf5"
```
미니콘다에서 실행하고 위의 라이브러리를 미니콘다 프롬포트에서 실행했습니다.
## DeepAA 프로젝트 실행 오류 및 해결 목록
이 프로젝트는 Python 3.6 및 TensorFlow 1.x/Keras 2.x 등 오래된 라이브러리 버전을 기준으로 작성되어, 최신 환경에서 실행 시 여러 호환성 문제가 발생했습니다.

1. 환경 활성화 오류 (conda 명령어 인식 불가)
문제: PowerShell에서 conda activate deepaa 실행 시, conda라는 명령어를 찾을 수 없다는 오류가 발생했습니다.

원인: Conda 환경 변수(PATH)가 PowerShell에 제대로 설정되어 있지 않았습니다.

해결: **PowerShell 대신 Anaconda Prompt (또는 Miniconda Prompt)**를 사용했습니다. 이 프롬프트는 Conda 명령어에 최적화되어 있습니다.

2. Python 경로 오류 (ModuleNotFoundError: No module named 'keras')
문제: Keras가 설치되어 있음에도 keras 모듈을 찾을 수 없다고 나왔습니다.

원인: 프롬프트가 (deepaa) 환경이 아닌 (base) 기본 환경에 머물러 있었습니다. base 환경에는 keras가 설치되어 있지 않았습니다.

해결: Anaconda Prompt에서 conda activate deepaa 명령어를 실행하여, Keras가 설치된 (deepaa) 환경으로 정확히 진입했습니다.

3. 파일 경로 문법 오류 (SyntaxError: (unicode error) 'unicodeescape')
문제: output.py의 15번째 줄 이미지 경로에서 문법 오류가 발생했습니다.

원인: 윈도우 경로(C:\Users...)의 백슬래시(\) 기호를 파이썬이 특수 명령(\U...)으로 잘못 해석했습니다.

해결: output.py의 image_path 경로를 백슬래시(\) 대신 **슬래시(/)**를 사용하도록 수정했습니다.

image_path = "C:/Users/flash/Downloads/mm.png"

4. 필수 라이브러리 누락 (ImportError: \load_weights` requires h5py.`)
문제: Keras가 모델 가중치 파일(.hdf5)을 불러오지 못했습니다.

원인: .hdf5 파일을 읽는 데 필수적인 h5py 라이브러리가 deepaa 환경에 설치되지 않았습니다.

해결: (deepaa) 환경에서 pip install h5py==2.7.1 (README 권장 버전) 명령어로 h5py를 설치했습니다.

5. 모델 파일 경로 오류 (OSError: Unable to open file ... weight.hdf5)
문제: h5py 설치 후에도 weight.hdf5 파일을 찾을 수 없다고 나왔습니다.

원인: 다운로드한 실제 파일명은 weight_light.hdf5인데, output.py 코드는 weight.hdf5를 불러오도록 하드코딩되어 있었습니다.

해결: output.py의 13, 14번째 줄 model_path와 weight_path 변수값을 _light가 붙은 파일명 (model_light.json, weight_light.hdf5)으로 직접 수정했습니다.

6. Pandas 라이브러리 버전 오류 (AttributeError: 'Series' object has no attribute 'as_matrix')
문제: char_list.csv 파일 처리 중 오류가 발생했습니다.

원인: 스크립트가 오래된 Pandas 함수인 .as_matrix()를 사용했으나, 설치된 최신 Pandas 버전에서는 이 함수가 .to_numpy()로 변경되었습니다.

해결: output.py의 46번째 줄 코드를 .as_matrix()에서 **.to_numpy()**로 수정했습니다.

7. 텍스트 파일 저장 오류 (UnicodeEncodeError: 'cp949' codec can't encode...)
문제: 변환된 AA 텍스트를 .txt 파일로 저장하는 마지막 단계에서 오류가 발생했습니다.

원인: 모델이 생성한 일본어 특수 문자(\u333b 등)를 윈도우의 기본 한글 인코딩 방식(cp949)이 처리하지 못했습니다.

해결: output.py의 126번째 줄 open() 함수에 encoding='utf-8' 옵션을 추가하여, 모든 문자를 지원하는 UTF-8 방식으로 파일을 저장하도록 변경했습니다.

f=open(..., 'w', encoding='utf-8')





## License
The pre-trained models and the other files we have provided are licensed under the MIT License.
