---
title: Linux 압축 및 해제
date: 2025-07-01 10:00:00 +0800
author: keemsir
categories: [Linux]
tags: [Linux, tar, zip]
pin: false
published: true
---


# Linux에서 압축 및 압축 해제

리눅스 환경에서는 파일이나 디렉토리를 효율적으로 관리하기 위해 다양한 압축 및 압축 해제 명령어를 사용합니다. 이 포스트에서는 **tar**, **gzip**, **zip** 등 자주 쓰는 명령어와 실전 예시, 주요 옵션까지 한눈에 정리합니다.

## 1. tar: 여러 파일을 하나로 묶기 (아카이브)

`tar`는 여러 파일과 디렉토리를 **하나의 파일로 묶는** 명령어입니다. 자체적으로 압축은 아니지만, gzip 등과 결합해 압축도 가능합니다.

### 기본 사용법

```bash
# 디렉토리(abc)를 aaa.tar로 묶기
tar -cvf aaa.tar abc
```

- `-c`: 새 아카이브 생성
- `-v`: 진행 상황 출력(선택)
- `-f`: 파일 이름 지정

### 압축 해제

```bash
tar -xvf aaa.tar
```

- `-x`: 압축 해제(Extract)

## 2. tar + gzip: tar.gz(.tgz)로 압축 및 해제

`tar`와 `gzip`을 결합하면 `.tar.gz` 또는 `.tgz` 확장자를 가진 파일을 만들 수 있습니다.

### 압축

```bash
# 디렉토리(abc)를 aaa.tar.gz로 압축
tar -zcvf aaa.tar.gz abc
```

- `-z`: gzip 압축 추가

### 압축 해제

```bash
tar -zxvf aaa.tar.gz
```

## 3. gzip, gunzip: 개별 파일 압축/해제

`gzip`은 **단일 파일**을 압축할 때 사용합니다. 압축하면 원본 파일은 사라지고 `.gz` 파일만 남습니다.

### 압축

```bash
gzip example.txt
# 결과: example.txt.gz
```

### 압축 해제

```bash
gunzip example.txt.gz
# 결과: example.txt
```

## 4. zip, unzip: 윈도우 호환 압축

`zip`은 윈도우와 호환되는 압축 파일을 만들 때 주로 사용합니다.

### 압축

```bash
# 디렉토리(abc)를 aaa.zip으로 압축
zip -r aaa.zip abc
```

- `-r`: 하위 디렉토리까지 모두 압축

### 압축 해제

```bash
unzip aaa.zip
# 특정 폴더에 압축 해제
unzip aaa.zip -d ./target
```

## 5. 주요 명령어 옵션 정리

| 옵션 | tar | zip | gzip |
|------|-----|-----|------|
| -c   | 새 아카이브 생성 | - | - |
| -x   | 압축 해제 | - | -d (압축 해제) |
| -v   | 진행 상황 출력 | -v (자세히) | -v (자세히) |
| -f   | 파일 이름 지정 | - | -f (강제) |
| -z   | gzip 압축/해제 | - | - |
| -r   | - | 하위 디렉토리 포함 | -r (재귀) |
| -d   | - | 지정 폴더에 압축 해제 | -d (압축 해제) |

## 6. 실전 예시 모음

- **현재 디렉토리 전체를 tar.gz로 압축**
    ```bash
    tar -zcvf backup.tar.gz ./*
    ```
- **tar 파일을 특정 경로에 해제**
    ```bash
    tar -xvf backup.tar -C /home/user/restore/
    ```
- **zip 파일을 암호 설정하여 압축**
    ```bash
    zip -e secure.zip secret_folder
    ```

## 7. 기타 압축 포맷

리눅스에서는 이 외에도 `bzip2`, `xz`와 같은 고압축률 명령어도 자주 사용합니다.

| 확장자 | 설명 | 특징 |
|--------|------|------|
| .tar.gz, .tgz | tar + gzip | 빠르고 범용적 |
| .tar.bz2      | tar + bzip2 | 압축률 높음, 속도 느림 |
| .tar.xz       | tar + xz | 압축률 가장 높음 |

