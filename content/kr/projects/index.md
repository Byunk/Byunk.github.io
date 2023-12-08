+++
title = "Projects"
readingTime = false
Toc = true
+++

## 진행중인 프로젝트

### KAIST 감사원 시스템 전산화 프로젝트

[_Github Repository (Backend) ⧉_](https://github.com/gdsc-kaist/board-of-audit-and-inspection-system)

GDSC KAIST에서 진행하는 KAIST 감사원의 회계 감사 프로세스 전산화 프로젝트입니다. Backend Team Lead를 맡아 총 5명으로 이루어진 팀의 개발을 주도했습니다.

`Node.js`, `postgreSQL`, `Redis`를 활용해 시스템을 구축했고 dockerization, `Github actions`, AWS 배포 등의 작업을 진행했습니다.

`TDD`에 기반한 개발을 진행하고 있으며, 2024년 상반기부터 교내 서비스를 운영할 계획입니다.

### Dotfiles

[dotfiles ⧉ ](https://github.com/Byunk/dotfiles)

어떤 환경에서도 동일한 개발환경을 쉽게 구성할 수 있도록 `Neovim`, `Tmux`, `zsh` 등의 설정 파일을 모아둔 레포지토리입니다. 기존에 bash script로 짜여진 설치 스크립트를 `Go`로 이전하기 위한 프로젝트를 기획 중입니다.

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
