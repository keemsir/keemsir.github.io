---
title: TOPAS 3.9 tutorials
date: 2024-01-29 10:00:00 +0800
author: keemsir
categories:
  - Medical Physics
  - Tutorial
tags:
  - TOPAS
  - WSL
  - MC
  - Monte-Carlo
  - Simulation
pin: true
---

# TOPAS 요약
추가 Reference: _TOPAS: An innovative proton Monte Carlo platform for research and clinical applications_
URL: https://aapm.onlinelibrary.wiley.com/doi/10.1118/1.4758060

추가 Reference2: _Implementation of a double scattering nozzle for Monte Carlo recalculation of proton plans with variable relative biological effectiveness_
URL: https://iopscience.iop.org/article/10.1088/1361-6560/abc12d

---

TOPAS는 geant4를 사용자 관점에서 C++등의 코드 변경없이 사용할수있는 툴로 이해하면 좋겠다.
때문에 TOPAS에서 사용하는 parameter는 다 훑어보기

TOPAS monte carlo simulation for double scattering 

# Index
## Introduction to Parameter System
### 1. Design Philosophy
OpenGL graphic을 유지하고 싶으면 다음 옵션을 설정해라
`b:Ts/PauseBeforeQuit = "True" # defaults to "False"`

기본값은 single CPU thread를 지원하지만, 다음 코드를 통해 multi thread를 지원 할 수 있다. 
`i:Ts/NumberOfThreads = "True" # defaults to "False"`


### 2. Syntax
아래는 10가지 기본 parameters 설정이다.
```
d:Ge/Phantom/HLX                           = 10. cm     # Dimensioned Double
u:Ge/Magnet/Dipole/MagneticFieldDirectionX = 1.0        # Unitless Double
i:Sc/DoseScorer/ZBins                      = 100        # Integer
b:Sc/DoseScorer/Active                     = "True"     # Boolean
s:Ge/Phantom/Material                      = "G4_WATER" # String
dv:Ge/RMW_Track1/Angles         = 4 69.1 92.2 111.0 126.0 deg      # Dimensioned Double Vector
uv:Ma/Phantom_Plastic/Fractions = 3 0.05549 0.75575 0.18875        # Unitless Double Vector
iv:Gr/Color/yellow              = 3 225 255 0                      # Integer Vector
bv:Tf/ScoringOnOff/Values       = 4 "true" "false" "true" "false"  # Boolean Vector
sv:Ma/MyPlastic/Components      = 3 "Hydrogen" "Carbon" "Oxygen"   # String Vector
```

