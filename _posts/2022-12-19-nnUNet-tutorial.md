---
layout: post
title: "nnU-Net tutorial(for LiTS)"
author: keemsir
date: 2022-12-19 11:33:00 +0800
categories: [Blogging, Tutorial]
tags: [nnUNet, deep-learning, machine-learning, medical, image, segmentation]
pin: true
---

<!-- categories: [Deep-learning, Machine-learning, nnUNet] -->


# 1. Enviroment setting
---

> nnU-Net을 학습시키기 위해서 최소 10GB 의 GPU memory가 필요

> nnUNet 라이브러리를 설치하기 전에 pytorch, cuda를 우선적으로 설치하는것을 권장

> 가상 환경 생성을 추천

```bash
conda create -n [eviroment_name] [python=3.8 or python=3.10]
```

## nnU-Net official github
<https://github.com/MIC-DKFZ/nnUNet>

### 공식 github에서 nnUNet 라이브러리 설치


1. github에서 직접 다운받기

    ```bash
    git clone https://github.com/MIC-DKFZ/nnUNet.git
    cd nnUNet
    pip install -e .
    ```

`sample content`

2. _pip install nnunet_ 으로 설치하기

    ```bash
    pip install nnunet
    ```
<br><br><br>

## LiTS 데이터를 기준의 메뉴얼
<br>

nnU-Net을 사용하기위해서 세가지 환경변수를 설정해야 한다.<br>

    1. nnUNet_raw_data_base
    2. nnUNet_kpreprocessed
    3. RESULTS_FOLDER


nnU-Net은 `Ubuntu / Linux` 환경에서 개발되어 다른 운영체제는 지원하지않는다.

일반적으로 home directory에 있는 `.bashrc` 파일에 경로를 설정한다.

`torch /home/keemsir/.bashrc`




# 2. Data preprocessing
---


-
-
-



# 3. Data Training
---
```bash
nnUNet_training 2d 
```


# 4. Data prediction
---

# 5. Ensemble
---

