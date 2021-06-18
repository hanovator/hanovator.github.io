---
layout: post
title:  "BERT"
subtitle:   "BERT"
categories: dl
tags: nlp
comments: true
---

### 목차
1. BERT 개요
2. Model Architecture
3. Model Pre-training
4. Task specific-Models
5. BERT vs GPT vs ELMo
6. Reference

## 1. BERT 개요
- 2018년 여러 NLP task에서 SOTA를 기록한 모델
- 그림1과 같이 semi-supervised learning step과 특정 처리를 위한 supervised training으로 나뉜다.
<p align="center" style="color:gray;">
  <img style="margin:50px 0 0 0" src="https://images.velog.io/images/hanovator/post/d4884494-3ba5-47e9-9cc0-a61a7f7dec1f/image.png" alt="" />
(그림1)
</p>

#### * Example: Sentence Classification
- BERT를 이해하는데 있어서 가장 쉬운방법은 single text를 classification하는 것이다. (그림2)
- 그림2는 pre-training(semi-supervised)된 BERT모델에 classification을 fine-tuning(supervised training)한 형식이다.
<p align="center" style="color:gray">
  <img style="margin:50px 0 10px 0" src="http://jalammar.github.io/images/BERT-classification-spam.png" alt="" />
  (그림2)
</p> 

## 2. Model Architecture 
### (Sentence Classification을 예시로한 전체적인 구조)

1) BERT는 size에 따라서 base와 large모델로 나뉘게 된다.(그림3)
- BERT BASE: 다른 transformer 모델들과의 비교를 위해 만들어진 모델
- BERT LARGE: SOTA모델을 위해 만든 모델
<p align="center" style="color:gray">
  <img style="margin:50px 0 10px 0" src="http://jalammar.github.io/images/bert-base-bert-large-encoders.png" alt="" />
  (그림3)
</p> 


2) BERT는 Transformer Encoder stack만을 이용한 모델이다.(그림4)
<p align="center" style="color:gray">
  <img style="margin:50px 0 10px 0" src="https://images.velog.io/images/hanovator/post/882435b6-6e5a-4012-a311-6b65a230e6fd/image.png" alt="" />
  (그림4)
</p> 

3) Model Inputs (그림5)
- [CLS]로 시작 임베딩임을 알린다.
- Encoder는 stack으로 계속 쌓일 수 있으며 계속 self-attention된다.
<p align="center" style="color:gray">
  <img style="margin:50px 0 10px 0" src="https://images.velog.io/images/hanovator/post/86849db8-8378-4749-99ed-5d0cd4f78ac1/image.png" alt="" />
  (그림5)
</p> 

4) Model Outputs
- 각 위치에서 hidden_size(BERT Base의 경우 768)크기의 벡터를 출력한다.
- 위 예시로 봤을때 [CLS]의 출력에만 초점을 맞춘다. (그림6)
<p align="center" style="color:gray">
  <img style="margin:50px 0 10px 0" src="https://images.velog.io/images/hanovator/post/4608fa95-3353-47ab-93e1-70504746528e/image.png" alt="" />
  (그림6)
</p> 

## 3. Model Pre-training

1) Masked Language Model (그림7)
- BERT는 forward와 backward 양쪽을 동시에 학습하고자 하였다.
- 15% 를 랜덤으로 [MASK]로 두었으며 이를 학습 시켰다.
- 이를 “masked language model”이라 한다.
- 이 외에도 fine-tuning이 더 잘 되도록 [MASK]가 아닌 아에 다른 단어를 넣기도 하였다.
<p align="center" style="color:gray">
  <img style="margin:50px 0 10px 0" src="https://images.velog.io/images/hanovator/post/71882bb0-8b58-4cc7-8bc9-37b7a4434802/image.png" alt="" />
  (그림7)
</p> 

2) Two-sentence Tasks (그림8)
- 두개의 문장이 [SEP]로 구분된다
- 첫번째 문장 다음으로 오는 문장이 두번째 문장인지에대해 학습한다.
<p align="center" style="color:gray">
  <img style="margin:50px 0 10px 0" src="https://images.velog.io/images/hanovator/post/6cfc93fb-4bdc-4767-b716-6c82b97e19fe/image.png" alt="" />
  (그림8)
</p> 

## 4. Task specific-Models
- Pre-training된 BERT모델은 그림9와 같은 Task를 할 수 있다.
<p align="center" style="color:gray">
  <img style="margin:50px 0 10px 0" src="https://images.velog.io/images/hanovator/post/b5367c58-8425-4511-bd2f-ed3a73d0f7b6/image.png" alt="" />
  (그림9)
</p> 

## 5. BERT vs GPT vs ELMo
![](https://images.velog.io/images/hanovator/post/a5abe9cb-e089-43b4-9e49-dfdc4bae8fe3/image.png)

## 6. Reference
> http://jalammar.github.io/illustrated-bert/
> https://arxiv.org/pdf/1810.04805.pdf&usg=ALkJrhhzxlCL6yTht2BRmH9atgvKFxHsxQ

