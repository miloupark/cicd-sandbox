# 🚀 웹 프론트엔드 프로젝트에 CD/CD 적용하기

> 현업 프로젝트 배포 과정을 실습하며 정리한 기록

<br>

## 1️⃣ 프로젝트 초기 세팅

### 프로젝트 생성 & Git 연결

```bash
$ npm create vite@latest cicd-sandbox -- --template react

$ git init
$ git remote add origin <해당-repo-url>

```

<br>

## 2️⃣ Git Branch 전략

```bash
$ git checkout -b dev
$ git push -u origin dev
```

- `main`: 안정적인 배포용 브랜치
- `dev`: 개발용 브랜치

<br>

## 3️⃣ Issue 관리

작업 단위를 최대한 작게 나누어 충돌을 줄인다.  
충돌을 방지하기 위해 가능한 한 독립적인 컴포넌트나 기능 단위로 작업을 나누는 것이 좋다.

만약 프로젝트 기간이 3주라면,

- 1~2주차: 기능 개발
- 3일: 기능 마무리 + 배포 준비
- 3~4일: 배포 및 안정화

<br>

## 4️⃣ S3 버킷 생성 및 설정

- 배포 환경 (실제 사용자용): prod-cicd-sandbox
- 개발 환경 (테스트용): dev-cicd-sandbox

설정 방법:

1. 권한 > 버킷 정책
2. 속성 > 정적 웹 사이트 호스팅 > 활성화

<br>

## 5️⃣ CloudFront + 도메인 연결

![](./public/cloudfront3.png)

- 프로덕션 / 개발용 각각 따로 CloudFront 배포 생성

<br>

![](./public/domain.png)

- 도메인 연결 시에도 prod, dev 따로 매핑

실습 :

- [S3-prod](http://prod-cicd-sandbox.s3-website.ap-northeast-2.amazonaws.com/)
- [S3-dev](http://dev-cicd-sandbox.s3-website.ap-northeast-2.amazonaws.com/)
- [CloudFront-prod](https://d36iaucl5xrtt4.cloudfront.net/)
- [CloudFront-dev](https://d24oa7dahq7ss9.cloudfront.net/)

<br>

## 6️⃣ ACM 인증서 발급 & 적용

### (1) 인증서 발급

#### CloudFront > 설정 > 편집 > ACM 발급 요청

🚀 배포 환경

![](./public/acm.png)

👩🏻‍💻 개발 환경

![](./public/acm2.png)

<br>

### (2) 도메인에 CNAME 추가

#### 발급 후, ACM에서 제공하는 CNAME 레코드를 도메인에 등록한다.

🚀 배포 환경

![](./public/acm-domain.png)

👩🏻‍💻 개발 환경

![](./public/acm-domain2.png)

<br>

### (3) CloudFront에 인증서 적용

#### CloudFront 콘솔에서 `설정 > 편집 > 인증서 적용` 진행

🚀 배포 환경

![](./public/cloudfront.png)

👩🏻‍💻 개발 환경

![](./public/cloudfront2.png)

<br>

### (4) 결과 확인

- [배포 환경: cicdsandbox.o-r.kr](https://cicdsandbox.o-r.kr/)
- [개발 환경: dev.cicdsandbox.o-r.kr](https://dev.cicdsandbox.o-r.kr/)

<br>

### (5) CI/CD

배포, 개발 각 파일 생성

- deploy-dev.yml
- deploy-prod.yml

<br>

### (6) IAM

#### IAM > 사용자 > 생성

🚀 배포 환경

![]()

👩🏻‍💻 개발 환경

![](./public/iam.png)

<br>

#### IAM > 사용자 > 보안 자격 증명 > 액세스 키 발급

![](./public/iam2.png)
![](./public/iam3.png)

<br>

#### GitHub Secrets에 엑세스 키 등록

🚀 배포 환경 & 👩🏻‍💻 개발 환경

![](./public/githubsecrets.png)
