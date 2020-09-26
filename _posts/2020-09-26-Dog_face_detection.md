---
layout: post
title: 강아지 얼굴인식
subtitle: Python
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/ML.JPG
share-img: /assets/img/posting/mountain.jpg
tags: [딥러닝]
comments: true
---

## 강아지 얼굴인식

빵형의 개발도상국님 코드를 사용했습니다. [Reference](https://www.youtube.com/watch?v=yRH5by6IiEE&t=1s), [Github_code](https://github.com/kairess/dog_face_detector)
<br>

# [Colab_link](https://colab.research.google.com/notebooks/intro.ipynb)

Google에서 제공하는 Colab을 이용하면 jupyter notebook과 별도의 Library들을 설치할 필요없이 바로 이용가능하다. 이때 파일들을 받아와 사용하기 위해서는 사용할 파일을 우선 google drive에 업로드한 뒤 drive를 Colab에 마운트 시켜주어야 한다.

```
from google.colab import drive
drive.mount('/content/drive')
```

이 코드를 입력하고 shift+enter를 눌러 실행시키면 링크가 등장한다. 링크를 타고 들어가 구글 계정을 선택한 후 코드를 복사해 빈칸에 복붙해주면 마운트가 완료된다.
파일을 불러오거나 사용할 때는 왼쪽의 세번째 파일 모양 아이콘을 클릭해 원하는 파일을 찾아준 후 오른쪽 클릭을 하면 뜨는 경로복사를 눌러 활용할 수 있다.

나의 경우에는 강아지 사진의 경로를 아래와 같이 활용해 이미지를 불러올 수 있었다.

```python
img_path = '/content/drive/My Drive/Deeplearning_pract/findDOGface/img/31.jpg'
img = cv2.imread(img_path)
```

### 이미지 불러오기

![Crepe](https://imgur.com/uP9WzGi)

원하는 이미지를 구글 드라이브에 업로드한 후 코드에서 img_path 경로 부분만 수정해주면 작동 될 것이다. 오류가 생긴다면 경로가 올바른지 혹은 마운트가 제대로 되었는지 확인해보자.

## Face detection

이제 강아지 얼굴이 어디에 위치해 있는지 알아보자.

```python
detector = dlib.cnn_face_detection_model_v1('/content/drive/My Drive/Deeplearning_pract/findDOGface/dogHeadDetector.dat')
predictor=dlib.shape_predictor('/content/drive/My Drive/Deeplearning_pract/findDOGface/landmarkDetector.dat')
#경로는 구글 드라이브 내 파일 경로에 맞게 수정
```

detector를 사용해 강아지 얼굴의 위치를 찾아낸 후 predictor를 사용해 강아지의 눈,코, 귀 그리고 정수리까지 총 6개의 점을 탐지한다.

### dog head detector

![Crepe](https://imgur.com/ODPJaFI)

### landmark detector

![Crepe](https://imgur.com/z0BnFwd)

### Add glasses

빵형의 개발도상국 유트브 [영상](https://www.youtube.com/watch?v=yRH5by6IiEE&t=1s)에서는 루돌프 사슴 코와 뿔을 달아주는 작업을 진행했다. 이를 활용해 코와 뿔 대신 안경을 씌워주도록 코드를 약간 수정했다. 이미지는 되도록 png 확장자 파일을 사용하는 것이 좋다. jpg확장자 파일을 사용하면 사각형 이미지 전체가 덧씌워지게 된다.

```python
glasses = cv2.imread('/content/drive/My Drive/Deeplearning_pract/findDOGface/img/glasses2.png',  cv2.IMREAD_UNCHANGED)
g_height, g_width= glasses.shape[:2]
print(g_height,g_width) #png파일의 높이 너비 확인
plt.imshow(glasses) # 이미지를 출력해 제대로 받아졌는지 확인
```

다음의 코드를 추가해 안경 이미지를 불러오고 확인해본다.

![Crepe](https://imgur.com/xtQdojz)

landmark detection한 이미지에서 강아지의 눈에 해당하는 점은 2번과 5번이다.
따라서, 눈의 위치에 대한 정보는 shape[2]와 shape[5]에 들어있다.

```python
eye_left=shape[5]
eye_right=shape[2]

glasses_size = np.linalg.norm(shape[5] - shape[2]) * 4
#위에서 4는 임의로 지정한 숫자로 이 숫자를 키우면 안경의 크기가 커진다.

img_result2 = overlay_transparent(img_result2,glasses,(eye_right[0]+eye_left[0]+6)//2,(eye_right[1]+eye_left[1]+20)//2,overlay_size=(int(glasses_size),int(glasses_size)))
```

(eye_right[0]+eye_left[0]+6)//2,(eye_right[1]+eye_left[1]+20)//2
이 값들은 임의로 정한 값이며 정확하지 않아서 사진별로 약간 안경이 뒤틀려 보일 수 있습니다...

### Result

마루

![Crepe](https://imgur.com/EOlD5Y6)

아궁이

![Crepe](https://imgur.com/ljmzskj)

### 개선 사항

아직 강아지 눈의 기울어진 각도에 따라 안경을 기울이는 코드를 작성하지 못했다.
기존 코드에 있는 문장들을 활용해보고, 이리저리 시도해 보았는데 수 많은 오류들 때문에 막히고 있다.
