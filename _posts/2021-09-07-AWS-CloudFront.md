---
title: AWS-CloudFront
author: Banny
date: 2021-09-07 00:41:00 +0900
categories: [AWS]
tags: [AWS, CloudFront]
---

<!-- testing -->

## CloudFront란?

HTML, CSS, JS, 이미지 파일 등 정적 및 동적 웹 컨텐츠를 더 빨리 배포할 수 있도록 지원해주는 서비스입니다.

사용자가 컨텐츠를 요청하면 지연시간이 가장 낮은 엣지 로케이션으로 요청이 라우팅됩니다. 그로 인해 최고 성능으로 컨텐츠가 제공됩니다.

CloudFront가 아닌 일반 웹 서버에서 이미지를 제공한다고 할 때, 해당 이미지의 URL로 이동해 이미지를 볼 수 있지만, 이미지가 발견될 때까지는 복잡한 네트워크 라우팅이 있습니다.

CloudFront는 AWS 백본 네트워크를 통해 콘텐츠를 가장 효과적으로 서비스할 수 있는 엣지로 각 사용자 요청을 라우팅하여 콘텐츠 배포 속도를 높입니다.

또한 파일(객체)가 전 세계 여러 엣지 로케이션에 캐싱되므로 안정성과 가용성이 향상됩니다.

<br>

## 컨텐츠를 전송하도록 CloudFront를 설정하는 방법

<center>
<img alt="AWS-how-you-configure-cf" src="https://user-images.githubusercontent.com/62047302/132225771-82ecbb3b-8115-4522-9c3a-90515ce2bb31.png">
</center>

<br>

위 그림은 CloudFront에 S3 버킷 혹은 HTTP 서버를 등록하고 설정할 때 일어나는 일련의 과정입니다. 설명은 다음과 같습니다.

1. Amazon S3 버킷 혹은 고유 HTTP 서버와 같은 오리진 서버로부터 가져온 파일을 전 세계 CloudFront의 엣지 로케이션에 배포합니다. 오리진 서버는 객체의 원본 버전을 저장합니다.

2. 오리진 서버에 파일(객체)을 업로드합니다. 웹 페이지, 이미지 및 미디어 파일을 포함합니다.

3. 사용자가 웹 사이트나 애플리케이션을 통해 파일을 요청할 경우에 어떤 오리진 서버에서 파일을 가져올지 알려 주는 CloudFront 배포를 만듭니다.

4. CloudFront는 새 배포에 도메인 이름을 할당합니다. 도메인 이름을 임의로 설정할 수도 있습니다.

5. CloudFront에서는 배포의 구성을 모든 해당 엣지 로케이션 또는 CloudFront가 파일의 사본을 캐싱하는 지리적으로 분산된 데이터 센터의 POP(Point of Presence) 서버 모음으로 보냅니다.

<br>

## CloudFront에서 컨텐츠를 제공하는 방법

<center>
<img alt="AWS-how-cloudfront-delivers-content" src="https://user-images.githubusercontent.com/62047302/132225298-e3b610d0-0a3b-4914-8185-2a649e1c11b1.png">
</center>

<br>

위 그림은 사용자가 CloudFront에 등록된 도메인에 요청을 보냈을 때 일어나는 과정입니다. 설명은 다음과 같습니다.

1. 사용자가 웹 사이트 또는 애플리케이션에 액세스하고 이미지 혹은 HTML 같은 파일을 요청합니다.

2. DNS가 요청을 최적으로 서비스할 수 있는 CloudFront POP(엣지 로케이션)로 요청을 라우팅합니다. 일반적으로 가장 가까운 POP로 라우팅이 됩니다.

3. POP에서 CloudFront는 요청 파일이 POP에 캐싱이 되어 있는지 확인합니다. 파일이 캐시에 있으면 CloudFront는 파일을 사용자에게 반환합니다. 파일이 캐시에 없으면 다음을 수행합니다.<br>

   a. CloudFront는 배포의 사양과 요청을 비교하고 파일에 대한 요청을 해당하는 파일 형식으로 사용자의 오리진 서버(이미지 파일의 경우 Amazon S3 버킷, HTML 파일의 경우 HTTP 서버)에 전달합니다.<br>
   b. 오리진 서버는 파일을 다시 엣지 로케이션으로 보냅니다.<br>
   c. 오리진에서 첫 번째 바이트가 도착하면 CloudFront가 파일을 사용자에게 전달하기 시작합니다. CloudFront는 다음에 다른 사용자가 해당 파일을 요청할 때 엣지 로케이션에 해당 파일을 캐싱합니다.
   <br>

<br>

## CloudFront의 Edge Location

Amazon CloudFront는 현재 47개국 90개 도시에서 225개가 넘는 PoP(Point of Presence, 엣지 로케이션 215개 이상, 리전별 중간 티어 캐시 13개)로 구성된 글로벌 네트워크를 사용하고 있다고 합니다. Amazon CloudFront 엣지 로케이션의 위치는 다음과 같습니다.

<center>
<img alt="AWS-Cloudfront-Map" src="https://user-images.githubusercontent.com/62047302/132236745-3346a0fd-3132-4d0a-bddd-587db7decbeb.png">
</center>

<br>
<br>

참고: <br>
<a href="https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/Introduction.html">Amazon CloudFront란 무엇입니까?</a>,<br>
<a href="https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/Introduction.html#HowCloudFrontWorksOverview">콘텐츠를 전송하도록 CloudFront를 설정하는 방법</a>,<br>
<a href="https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/HowCloudFrontWorks.html">CloudFront에서 콘텐츠를 제공하는 방법</a>,<br>
<a href="https://aws.amazon.com/ko/cloudfront/features/?whats-new-cloudfront.sort-by=item.additionalFields.postDateTime&whats-new-cloudfront.sort-order=desc">글로벌 엣지 네트워크</a>
