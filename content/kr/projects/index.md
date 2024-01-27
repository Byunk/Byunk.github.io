+++
title = "Projects"
readingTime = false
Toc = true
+++

## 진행중인 프로젝트

### KAIST 감사원 시스템 전산화 프로젝트

[_Github Repository (Backend) ⧉_](https://github.com/gdsc-kaist/board-of-audit-and-inspection-system)

KAIST의 학생 동아리 및 자치 단체의 감사 프로세스는 모두 서면으로 진행되고 있습니다. 본 프로젝트는 과정을 전산화하기 위해 GDSC KAIST에서 진행중인 프로젝트입니다. 본 프로젝트에서 백엔드 팀 리드를 맡아 전반적인 백엔드 시스템 개발과 배포과정에 참여했습니다. 2024년 상반기부터 시범 운영을거쳐 2024년 하반기에 도입이 예정되어 있습니다.

**Project Spec**

| Name | Email |
|------|-------|
|  프레임워크    |  `Express.js`     |
|  데이터베이스    |    `postgreSQL`, `Redis` (session DB)    |
|  테스트 커버리지     |  Unit test, Integration test    |
|  배포 환경    | AWS EC2 |

{{< figure src="bai-architecture.jpg" alt="BAI architecture" position="center" style="border-radius: 8px;" caption="감사원 프로젝트 백엔드 아키텍쳐" >}}

### Dotfiles

[dotfiles ⧉ ](https://github.com/Byunk/dotfiles)

개발환경 설정 아카이브 레포지토리

**Goal**

- bash script를 이용한 패키지 설치 및 설정 자동화

## 완료한 프로젝트

### The Voice

[_Github Repository ⧉_](https://github.com/KAIST-Google-Solution-Challenge)

AI 기반 보이스피싱 탐지 어플리케이션입니다. 노인들을 대상으로 하는 보이스피싱 범죄를 예방하기 위한 목적으로 제작하였으며, 구글에서 주최한 2023 솔루션 챌린지에서 [_Global TOP 100_](https://developers.google.com/community/gdsc-solution-challenge/winners)를 수상하였습니다.

사용자가 전화를 하거나 메시지를 주고 받는 것을 모니터링하여 보이스피싱일 확률이 기준 이상일 경우 경고 알림과 보호자에게 메시지를 자동 전송하는 기능을 개발하였습니다. 안드로이드로 제작하였으며 GCP의 `Speech-to-Text` 서비스와 `KoBERT` 모델을 이용하여 통화 내용을 기반으로 보이스피싱 확률을 예측하였습니다.

추가로, 노인을 대상으로 한 어플리케이션이므로 사용자의 접근성을 높이기 위한 UI/UX, 큰 글 모드 등을 적용하였으며 Google의 개발자와 멘토링을 진행하며 어플리케이션을 완성했습니다.

[UI 데모](/demo/the-voice)

{{< figure src="the-voice-figma.png" position="center" style="border-radius: 8px; width:50%; margin: 0 auto;" caption="The Voice UI" captionPosition="center" captionStyle="color: black;">}}

### 체스 게임 승률 예측

[_Github Repository ⧉_](https://github.com/Byunk/Winning-rate-prediction-of-the-chess-game-result-with-GNN)

CS471 (Graph Machine Learning and Mining)의 과제로 제출한 GNN을 활용한 체스 게임 승률 예측입니다. GATv2를 활용한 모델을 고안하고 [Lichess Dataset](https://database.lichess.org/)을 활용하여 ELO와의 성능 비교를 진행했습니다.

### 한국어 혐오 표현 감지

[_Github Repository ⧉_](https://github.com/Byunk/Korean-Hate-Speech-Detection)

CS470 (Introduction to Artificial Intelligence)의 과제로 제출한 Cross-lingual Transfer Learning Embedding Model을 이용한 한국어 혐오 표현 감지 프로젝트입니다. [Korean Hate Speech](https://github.com/kocohub/korean-hate-speech)에서 주최한 Kaggle competition을 벤치마크로 두고 학습 및 hyperparameter tuning을 진행했습니다.

### Treevial

탄소배출권 중개 플랫폼입니다. 소규모 산주들의 산림에서 탄소배출권 할당을 돕고, 이를 기업에 중개하는 비즈니스 모델을 제시하였습니다. 6명의 개발자로 이루어진 팀의 리더를 맡아 KAIST APP 창업 프로그램에서 결선에 진출 및 창업 지원금을 수령하였습니다.
