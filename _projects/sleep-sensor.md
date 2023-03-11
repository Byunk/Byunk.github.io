---
title: Sleep-Sensor
excerpt: Developed a server that analyzes pressure sensor data in Node.js for a smart mattress
layout: single
permalink: /sleep-sensor/
author_profile: true
header:
  image: /assets/images/project/sleep-sensor-teaser.jpg
  teaser: /assets/images/project/sleep-sensor-teaser.jpg
sidebar:
  nav: "sidebar"
---

## Overview

I participated in the sleep-sensor development team in [KAIST-ITC](https://itc.kaist.ac.kr/) as a undergraduate researcher and software engineer from *2022.07*. As a engineer, my main project is development of algorithm for a smart mattress.

Analyzing sensor data, extracting features from signal, and implementing algorithm are what I did. Worked with 1 senior software engineer and his role is implementation of imbed sensor logic and infrastructure.

**The product is going to be released within a year.**

## Methods

The main logic of this algorithm is divided into two steps: **extract features from signal and mapping to proper sleep levels (Nonsleep, REM sleep, Deepsleep)**. Sensor data is measured with 4HZ sample rate from pressure sensor which embeds below of the mattress. Each 60 sec long sensor data is sent to the server at every single minute.

Time domain features, Frequency domain features, Entropy and regularity features are calculated and used. Additionally, various anomaly detection algorithms are implemented and exploited.

## Timeline

***2022.07 - 2022.08***

___

- Onboarding period. Mini project which requires analyzing with Fourier Transform. 

- Participated in writing paper. Adapting FTT to analyze sensor data.

K. Choi et al., "Activity and environmental sensor application: Tonometry aided arrhythmia detection system," 2022 13th International Conference on Information and Communication Technology Convergence (ICTC), Jeju Island, Korea, Republic of, 2022, pp. 2022-2025, doi: 10.1109/ICTC55196.2022.9952394.

[https://ieeexplore.ieee.org/document/9952394/](https://ieeexplore.ieee.org/document/9952394/)

***2022.08 - 2022.12***

___

- Analyzing sensor data's features.

- Implementing algorithm.

***2022.12 - 2023***

___

- Testing algorithm with several participants.

## Results

{% include figure image_path="/assets/images/project/sleep-sensor-result.png" caption="analysis result" %}
