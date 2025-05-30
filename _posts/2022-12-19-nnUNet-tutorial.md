---
# layout: post
title: "nnU-Net tutorial(for LiTS)"
author: keemsir
date: 2022-12-19 11:33:00 +0800
categories: [Blogging, Tutorial]
tags: [nnUNet, deep-learning, machine-learning, medical, image, segmentation]
pin: true
---



![nnuent-flow](/assets/img/commons/nnunet_flow.png)


# Enviroment Setting
---

> nnU-Net을 학습시키기 위해서 최소 10GB 의 GPU memory가 필요

> nnUNet 라이브러리를 설치하기 전에 pytorch, cuda를 우선적으로 설치하는것을 권장

> nnU-Net은 `Ubuntu / Linux` 환경에서 개발되어 다른 운영체제는 공식적으로 지원하지않는다.

> 가상 환경 생성을 추천

## Create virtual enviroment

```bash
conda create -n [eviroment_name] [python=3.8 or python=3.10]
```

> eviroment_name: 자유롭게 설정 <br>
python version: 3.8이나 3.10 권장
{: .prompt-info }
 

### nnU-Net official reference

github.ref: <https://github.com/MIC-DKFZ/nnUNet> <br>
paper.ref: <https://cardiacmr.hms.harvard.edu/files/cardiacmr/files/isensee_etal_nature2021_nnunet.pdf><br>


## Install nnU-Net library


1. github에서 직접 다운받기

    ```bash
    git clone https://github.com/MIC-DKFZ/nnUNet.git
    cd nnUNet
    pip install -e .
    ```



2. _pip install nnunet_ 으로 설치하기

    ```bash
    pip install nnunet
    ```
<br><br><br>

## Environment path setting
<br>

nnU-Net을 사용하기위해서 세가지 환경변수를 설정해야 한다.<br>



1. nnUNet_raw_data_base
    - raw data의 구조는 밑의 Example tree structure에 따라야한다.<br>

    ```
    Example tree structure:
    
    nnUNet_raw_data_base/nnUNet_raw_data/Task507_LiTS
    ├── dataset.json
    ├── imagesTr
    │   ├── train_0_0000.nii.gz
    │   ├── train_1_0000.nii.gz
    │   ├── ...
    ├── imagesTs
    │   ├── test_101_0000.nii.gz
    │   ├── test_102_0000.nii.gz
    │   ├── ...
    └── labelsTr
        ├── train_0.nii.gz
        ├── train_1.nii.gz
        ├── ...
    ```


2. nnUNet_preprocessed
    - 전처리가 완료된 데이터가 저장되는 곳이다. 이 폴더에서 데이터를 읽어서 학습데이터로 쓰인다. 따라서 엑세스의 대기시간이 짧고 처리량이 높은 드라이브에 위치하는것이 좋다.(ssd 권장)
3. RESULTS_FOLDER
    - 학습된 모델의 weight를 저장하는 경로



일반적으로 home directory에 있는 `.bashrc` 파일에 경로를 설정한다.

`touch /home/keemsir/.bashrc`의 파일 가장 하단에 편집기를 이용해서 다음과 같이 편집한다.

- ex)
    ```bash
    export nnUNet_raw_data_base="/media/keemsir/nnUNet_raw_data_base"
    export nnUNet_preprocessed="/media/keemsir/nnUNet_preprocessed"
    export RESULTS_FOLDER="/media/keemsir/nnUNet_trained_models"
    ```

편집한 후에는 다음과같이 `source /home/keemsir/.bashrc` .bashrc 를 실행하여 재로드 시켜야 한다.

<br>




> nnUNet_preprocessed는 SSD에 위치하는것을 권장
{: .prompt-warning }

> `.bashrc` 파일 수정없이 터미널에서 실행하여 일시적으로 사용할 수 있다.

다음과 같이 `echo $RESULTS_FOLDER`로 경로 설정이 잘 됐는지 확인할 수 있다.

```bash
> echo $RESULTS_FOLDER
media/keemsir/nnunet_trained_models
```

# Data Preprocessing
---

> `LiTS dataset`를 사용한 이 튜토리얼에서는 2개의 `label(liver, tumor)`에 대한 `3d_fullres`의 학습 및 추론이며,
`k-fold(k: 5)` 를 기준으로 학습했다.

경로 환경 설정과 데이터 준비가 끝났으면 nnU-Net 라이브러리를 이용한 전처리를 실행한다.

다운로드 받은 LiTS 경로(`INPUT_DATA_PATH`)로 다음과 같이 터미널에 입력한다.

> INPUT_DATA_PATH 경로내부에 데이터 구조를 다음과 같이 따라줘야한다.
{: .prompt-tip }

```console
Task007_LiTS/
├── dataset.json        <-- contains metadata of the dataset.
├── imagesTr            <-- contains the images belonging to the training cases.
├── (imagesTs)          <-- (optional) contains the images that belong to the test cases.
└── labelsTr            <-- contains the images with the ground truth segmentation maps for the tarining cases.
```



## Terminal command(convert decathlon task)

```bash
nnUNet_convert_decathlon_task -i INPUT_DATA_PATH -output_task_id TASK_NUM
```
> TASK_NUM: 임의의 정수인 세자리 숫자로 설정 (중복가능성으로 인해 500 이상 권장)

- ex)
    ```bash
    nnUNet_convert_decathlon_task -i media/keemsir/input/Task07_LiTS/ -output_task_id 507
    ```
> 

## Terminal command(plan and preprocess)

```bash
nnUNet_plan_and_preprocess -t TASK_NUM
```

- ex)
    ```bash
    nnUNet_plan_and_preprocess -t 507
    ```
> 위에서 설정한 `output_task_id`인 `507`가 `TASK_NUM`



# Data Training
---
학습 가능한 네트워크는 [2d, 3d_fullres, 3d_lowres, 3d_cascade_fullres] 로 구성되어있고, fold(default: 5-fold) 별 학습이 가능하다. 여기서 예제로 3d_fullres에 대한 예시이다.

## Other parmeter setting

nnUNet package 설치 경로에서 다양한 parameter들을 변경 할 수 있다.

> `~/nnunet/training/network_training/nnUNetTrainerV2.py` 에서 `epoch`, `learning rate`, `optimizer` 등을 수정 할 수 있다.

<br>

## Terminal command(traning)

```bash
nnUNet_training 2d nnUNetTrainerV2 TASK_NUM FOLD --npz
nnUNet_training 3d_fullres nnUNetTrainerV2 TASK_NUM FOLD --npz
nnUNet_training 3d_lowres nnUNetTrainerV2 TASK_NUM FOLD --npz
nnUNet_training 3d_cascade_fullres nnUNetTrainerV2CascadeFullRes TASK_NUM FOLD --npz
```

- ex)
    ```bash
    nnUNet_training 3d_fullres nnUNetTrainerV2 507 0 --npz
    nnUNet_training 3d_fullres nnUNetTrainerV2 507 1 --npz
    nnUNet_training 3d_fullres nnUNetTrainerV2 507 2 --npz
    nnUNet_training 3d_fullres nnUNetTrainerV2 507 3 --npz
    nnUNet_training 3d_fullres nnUNetTrainerV2 507 4 --npz
    ```

### k-fold(k=5)에 대한 learning curve

- blue line: train loss function
- red line: validation loss function
- green line: evaluation metric

![Kfold_0](/assets/img/commons/sample/0.png)
![Kfold_1](/assets/img/commons/sample/1.png)
![Kfold_2](/assets/img/commons/sample/2.png)
![Kfold_3](/assets/img/commons/sample/3.png)
![Kfold_4](/assets/img/commons/sample/4.png)

<br>

# Data Prediction
---
해당 네트워크에 대한 전체 fold 학습이 끝나면 다음과 같이 Cross-Validation metrics 를 추출할 수 있다.

## Terminal command(determine postprocessing)

```bash
nnUNet_determine_postprocessing -tr nnUNetTrainerV2 -t TASK_NUM -m 2d
nnUNet_determine_postprocessing -tr nnUNetTrainerV2 -t TASK_NUM -m 3d_fullres
nnUNet_determine_postprocessing -tr nnUNetTrainerV2 -t TASK_NUM -m 3d_lowres
nnUNet_determine_postprocessing -tr nnUNetTrainerV2CascadeFullRes -t TASK_NUM -m 3d_cascade_fullres
```

- ex)
    ```bash
    nnUNet_determine_postprocessing -tr nnUNetTrainerV2 -t 507 -m 3d_fullres
    ```

<br>

<br>

## Terminal command(predict)

```bash
nnUNet_predict -i INPUT_FOLDER -o OUTPUT_FOLDER -t TASK_NUM -m 3d_fullres
```

> INPUT_FOLDER의 파일 형식은 Data Preprocessing - convert 단계에서 생성된 rawdata와 같은 파일 형식



- ex)
    ```bash
    nnUNet_predict -i media/keemsir/dnnUNet_raw_data_base/nnUNet_raw_data/Task507_LiTS/imagesTs/ -o OUTPUT_FOLDER/ -t 507 -m 3d_fullres
    ```
> 



# Ensemble
---

위에선 3d_fullres에 대해서만 학습했지만, 2개 이상의 네트워크를 이용해서 학습을 한다면 `ensemble method`의 적용이 가능하다.

가능한 학습 네트워크는 `[2d, 3d_fullres, 3d_lowres, 3d_cascade_fullres]`가 있다.

<br>

<br>

- ex) 두가지 이상의 모델로 학습을 하고 각각 추론한 경로가 (OUTPUT_FOLDER1, OUTPUT_FOLDER2, OUTPUT_FOLDER3, ...) 일때,

## Terminal command(ensemble)

```bash
nnUNet_ensemble -f OUTPUT_FOLDER1 OUTPUT_FOLDER2 OUTPUT_FOLDER3 ... -o ENSEMBLE_FOLDER
```
앙상블 결과는 ENSEMBLE_FOLDER에 저장

+ `ps.` 모든 훈련`[2d, 3d_fullres, 3d_lowres, 3d_cascade_fullres]`이 완료된다면 가장 최적의 앙상블 조합을 찾을 수 있다.

foreground dice average로 각 앙상블 방법에 따른 `dice score`를 볼 수 있다.

```bash
nnUNet_find_best_configuration -t 529
```

<br>

---

nnunet용 snipet code의 라이브러리

<https://github.com/keemsir/nnUNet_utilities>

