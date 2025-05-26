### [S3 배포 링크](http://soominss-buket.s3-website.ap-northeast-2.amazonaws.com/)

### [CloudeFront 배포 링크](https://d3jxcj7xvwc1i5.cloudfront.net)

---

# 기본 과제

## 사용 기술

- **Next.js**: 정적 페이지 생성 프레임워크
- **Yarn**: 패키지 매니저 (yarn install, yarn build 사용)
- **GitHub Actions**: 자동화된 워크플로우 실행
- **AWS S3**: 정적 파일 업로드 대상
- **AWS CloudFront**: CDN을 통한 글로벌 캐싱 및 배포
- **IAM / GitHub Secrets**: 자격 정보 관리

---

## 배포 흐름

1. main 브랜치에 코드가 푸시되거나 수동으로 워크플로우 실행
2. [.github/workflows/deployment.yml](./.github/workflows/deployment.yml)에 정의된 작업 실행
3. yarn install로 의존성 설치 → yarn build로 정적 파일 생성
4. out/ 디렉토리를 S3 버킷에 업로드
5. CloudFront 캐시를 무효화하여 최신 콘텐츠 반영

---

## 아키텍처 다이어그램

![배포 파이프라인 도식](./assets/deploy_pipeline.png)

---

## 정리

- GitHub Actions로 자동 배포 파이프라인 구축
- 정적 파일을 S3에 업로드 후 CloudFront로 제공
- 캐시 무효화를 통해 실시간 콘텐츠 반영 처리
- 배포에 필요한 자격 정보는 GitHub Secrets에 저장
- 전체 빌드 및 배포 과정은 yarn 명령어 기반으로 구성

---

# 심화 과제

<!-- 측정표 -->

## 📊 S3 & CloudFront Lighthouse 리포트

> 아래 표는 **최신 배포 시마다 자동으로 업데이트**됩니다.
> (업데이트: 2025-05-26 15:31 KST)

### ⚡️ Lighthouse report: CloudFront

| Category          | Score |
| ----------------- | ----- |
| 🟩 Performance    | 100   |
| 🟩 Accessibility  | 100   |
| 🟩 Best Practices | 100   |
| 🟩 SEO            | 100   |
| 🟥 PWA            | N/A   |

| Metric                         | Value (ms) | Value (s) |
| ------------------------------ | ---------- | --------- |
| First Contentful Paint (FCP)   | 1187       | 1.2       |
| Largest Contentful Paint (LCP) | 1250       | 1.3       |
| Speed Index                    | 1211       | 1.2       |
| Time to Interactive (TTI)      | 1444       | 1.4       |
| Total Blocking Time (TBT)      | 45         | 0.0       |
| Cumulative Layout Shift (CLS)  | 0.01       | -         |
| Time to First Byte (TTFB)      | 100        | 0.1       |
| Total Requests                 | 18         | -         |
| Total Transfer Size (bytes)    | 512000     | 0.49 MB   |

### ⚡️ Lighthouse report: S3

| Category          | Score |
| ----------------- | ----- |
| 🟨 Performance    | 86    |
| 🟩 Accessibility  | 100   |
| 🟨 Best Practices | 92    |
| 🟩 SEO            | 100   |
| 🟥 PWA            | N/A   |

| Metric                         | Value (ms) | Value (s) |
| ------------------------------ | ---------- | --------- |
| First Contentful Paint (FCP)   | 2244       | 2.2       |
| Largest Contentful Paint (LCP) | 2451       | 2.5       |
| Speed Index                    | 2300       | 2.3       |
| Time to Interactive (TTI)      | 2678       | 2.7       |
| Total Blocking Time (TBT)      | 143        | 0.1       |
| Cumulative Layout Shift (CLS)  | 0.01       | -         |
| Time to First Byte (TTFB)      | 120        | 0.1       |
| Total Requests                 | 23         | -         |
| Total Transfer Size (bytes)    | 1048576    | 1.00 MB   |

<!-- end -->
