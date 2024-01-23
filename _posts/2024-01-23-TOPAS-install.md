---
title: TOPAS 3.9 installation on Windows 10/11 using the WSL(Windows Subsystem for Linux)
date: 2024-01-23 10:00:00 +0800
author: keemsir
categories:
  - Computer
tags:
  - TOPAS
  - WSL
  - MC
  - Monte-Carlo
  - Simulation
pin: true
---

# WSL에서 TOPAS 설정하기

> [!info]
> 참고 사이트: https://www.topasmc.org/user-guides

윈도우 버전은 윈도우 11기준으로 18362 이상이어야 한다.


> [!note] 
> WSL가 Windows에 설치되어 있어야함



```shell
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y libexpat1-dev
sudo apt install -y libgl1-mesa-dev
sudo apt install -y libglu1-mesa-dev
sudo apt install -y libxt-dev
sudo apt install -y xorg-dev
sudo apt install -y build-essential
sudo apt install -y libharfbuzz-dev
```


다운을 받으면 분리되어있는 압축파일로 구성되어있는데 다음 명령을 통해 합친다. 
`cat topas_3_9_debian9.tar.gz.part_* > topas_3_9_debian9.tar.gz`
그리고 압축을 풀어준다.
`tar -xvf topas_3_9_debian9.tar.gz`

TOPAS 설치할 폴더를 만든다.
`mkdir G4Data`

해당 폴더로 이동해서 다운로드 후 압축을 해제한다.
```shell
cd G4Data
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4NDL.4.6.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4EMLOW.7.13.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4PhotonEvaporation.5.7.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4RadioactiveDecay.5.6.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4PARTICLEXS.3.1.1.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4SAIDDATA.2.0.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4ABLA.3.1.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4INCL.1.0.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4PII.1.3.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4ENSDFSTATE.2.3.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4RealSurface.2.2.tar.gz
wget -4 https://geant4-data.web.cern.ch/geant4-data/datasets/G4TENDL.1.3.2.tar.gz
tar -zxf G4NDL.4.6.tar.gz
tar -zxf G4EMLOW.7.13.tar.gz
tar -zxf G4PhotonEvaporation.5.7.tar.gz
tar -zxf G4RadioactiveDecay.5.6.tar.gz
tar -zxf G4PARTICLEXS.3.1.1.tar.gz
tar -zxf G4SAIDDATA.2.0.tar.gz
tar -zxf G4ABLA.3.1.tar.gz
tar -xzf G4INCL.1.0.tar.gz
tar -zxf G4PII.1.3.tar.gz
tar -zxf G4ENSDFSTATE.2.3.tar.gz
tar -zxf G4RealSurface.2.2.tar.gz
tar -zxf G4TENDL.1.3.2.tar.gz
```


`.bashrc`를 열어서 TOPAS 경로 설정을 한다. ()
```shell
export TOPAS_G4DATA_DIR=$HOME/USER/G4Data
export PATH=$HOME/USER/topas/bin:$PATH
```

그 후 TOPAS 설치 폴더로 이동해서 테스트 해본다.
`source rundemos.csh`

