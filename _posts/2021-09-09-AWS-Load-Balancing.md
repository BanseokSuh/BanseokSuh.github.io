---
title: AWS Load Balancing에 대하여
author: Banny
date: 2021-09-09 21:11:00 +0900
categories: [AWS]
tags: [AWS, Load Balancing]
---

## ELB(Elastic Load Balancing)

<center>
<img alt="AWS-LoadBalancer" src="https://user-images.githubusercontent.com/62047302/132688116-63969777-f471-484a-8da0-e50fbdf37aba.png">
</center>

<br>

로드 밸런서는 클라이언트로부터 오는 트래픽을 허용하고, EC2 인스턴스와 같은 등록된 대상으로 요청을 분산하여 라우팅합니다. 단 하나의 인스턴스에도 부하가 걸리지 않도록 트래픽을 최적으로 라우팅을 합니다. 또한 그 대상의 상태를 모니터링하는데, 비정상적인 대상을 감지하면 로드 밸런서는 라우팅을 중단합니다.

<br>

## ELB의 특징

- 트래픽 분산: 로드 밸런서가 하나의 단일 진입점이 되고, 등록되어 있는 EC2 인스턴스에 트래픽을 분산합니다.
- 자동 확장: AWS에서 ELB 스케일을 관리해주고 있습니다. <a href="https://docs.aws.amazon.com/ko_kr/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html" target="_blank">AWS Auto Scaling</a>이라고 불립니다.
- 인스턴스의 상태를 자동 감지해서 오류가 있는 시스템은 배제: EC2 인스턴스에 장애가 발생했을 경우 그것이 회복될 때까지 배제합니다.
- 사용자 세션을 특정 인스턴스에 고정: 사용자가 특정 EC2에 접속했다면, 그 다음 접속시에도 같은 EC2로 접속하게 됩니다.
- SSL 암호화 지원
- IPv4, IPv6 지원
- <a href="https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html" target="_blank">CloudWatch를 통한 모니터링</a>

<br>

## 대상 등록하기 및 트래픽 분산

<center>
<img alt="AWS-Target-Group" src="https://user-images.githubusercontent.com/62047302/132692049-ae90e09a-fc5c-4747-97a1-a5d9de8dd1f9.png">
</center>

<br>

위의 그림과 같이 여러 개의 EC2 인스턴스를 로드 밸런서의 대상 그룹(Target Group)에 등록시킵니다. 그러면 ELB에 설정해둔 포트로 요청을 들어왔을 때, 대상 그룹에 저장되어 있는 EC2로 자동적으로 트래픽이 분산되게 됩니다.

<br>

## SSL 암호화

SSL 암호화를 지원하는 것은 ELB의 주요 특징 중 하나입니다.

로드 밸런서에서 80번 포트에 대한 리스너를 수정해줍니다. 아래 그림과 같이 redirection에 대한 설정을 해주는데, HTTPS로 리다이렉트를 진행해줍니다.

<center>
<img alt="AWS-Redirect to HTTPS" src="https://user-images.githubusercontent.com/62047302/132698545-d0ce55eb-31b9-4055-ad98-1d4fa0e4c867.png">
</center>

<br>

그러긴 위해서는 SSL 인증서가 필요합니다. 저 같은 경우에는 아래 그림에서와 같이 ACM(Amazon Certificate Manager)룰 통해서 인증서를 발급받았습니다.
이 작업을 완료하면 HTTP 프로토콜(80번 포트)로 요청이 들어왔을 시에 HTTPS 프로토콜(443번 포트)로 리다이렉트가 됩니다. 아마존에서 발급받은 인증서를 통해서 말이죠.

<br>

<center>
<img alt="AWS-Redirect to HTTPS(2)" src="https://user-images.githubusercontent.com/62047302/132785859-f9d36b92-452b-43d5-bd3b-42779d1023bd.png">
</center>

<!-- <br>

## AWS CloudWatch - 모니터링 시스템

<center>
<img alt="AWS-Redirect to HTTPS(2)" src="https://user-images.githubusercontent.com/62047302/132785859-f9d36b92-452b-43d5-bd3b-42779d1023bd.png">
</center> -->

<br>
<br>
참고자료<br>
- <a href="https://docs.aws.amazon.com/ko_kr/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html" target="_blank">Elastic Load Balancing의 작동 방식</a><br>
- <a href="https://aws.amazon.com/ko/premiumsupport/knowledge-center/elb-redirect-http-to-https-using-alb/" target="_blank">Application Load Balancer를 사용하여 HTTP 요청을 HTTPS로 리디렉션하려면 어떻게 해야 합니까?</a><br>
