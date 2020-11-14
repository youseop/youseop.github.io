---
layout: post
title: SON Detection
subtitle: Python - Colab
cover-img: /assets/img/posting/mountain.jpg
thumbnail-img: /assets/img/posting/ML1.JPG
share-img: /assets/img/posting/mountain.jpg
tags: [DeepLearning]
comments: true
---

## SON Detection

어제 팀원들과 함께 손흥민 하이라이트 영상에서 와이드 뷰 부분만 모두 캡쳐해서 바운딩 박스를 쳤었다.

이 데이터를 가지고 오늘 실험삼아 (epoch = 100) 으로 가볍게 학습시켜봤는데 생각보다 결과가 긍정적이었다.

라벨링 하면서도 손흥민이 어디있는지 못찾겠는 그런 사진들이 꽤 있었는데 다시 한번 머신러닝의 위대함을 실감할 수 있었다.

- SON detection in wide view (Epoch = 100)

![Crepe](https://i.imgur.com/vOTqA0J.jpg)

- Valid Img

![Crepe](https://i.imgur.com/90barw1.jpg)

- 학습 과정에서 전혀 사용되지 않은 이미지

![Crepe](https://i.imgur.com/SDd64TH.jpg)

확률은 조금 낮게 나왔지만 그래도 한줄기 희망이 보이는 결과인것 같다.

다음주에는 라벨링된 데이터를 추가하고 epoch도 1000까지 키워서 학습시켜 보자.

<br>
