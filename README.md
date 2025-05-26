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
## 📊 S3 vs CloudFront Lighthouse 비교
> 아래 표는 **최신 배포 시마다 자동으로 업데이트**됩니다.
> (업데이트: 2025-05-26 16:09 KST)

| 항목 | S3 | CloudFront |
|------|------------------|--------------------|
| Performance | 🟨 88 | 🟩 100 |
| Accessibility | 🟩 100 | 🟩 100 |
| Best Practices | 🟩 92 | 🟩 100 |
| SEO | 🟩 100 | 🟩 100 |
| PWA | 🟥 33 | 🟥 33 |
| FCP | 2181.62223 ms (2.2s) | 1162.281 ms (1.2s) |
| LCP | 2910.8889 ms (2.9s) | 1162.281 ms (1.2s) |
| Speed Index | 4185.464358000025 ms (4.2s) | 1955.7479140000714 ms (2.0s) |
| TTI | 6953.45558 ms (7.0s) | 2050.281 ms (2.1s) |
| TBT | 171.5 ms (0.2s) | 20 ms (0.0s) |
| CLS | 0.04571302421445905 | 0.04571302421445905 |
| TTFB | 0 ms (0.0s) | 392.75399999999996 ms (0.4s) |
| Total Requests | 19 | 17 |
| Total Transfer Size | 397525 bytes (0.38MB) | 164257 bytes (0.16MB) |
<!-- end -->

> - **FCP/LCP/Speed Index/TTI**: 페이지 렌더링 및 반응속도를 보여주는 지표 (낮을수록 빠름)
> - **TBT**: 사용자가 입력할 때 대기해야 하는 총 시간 (낮을수록 쾌적)
> - **CLS**: 페이지가 갑자기 움직이는 현상 정도 (0~0.1이면 매우 우수)
> - **TTFB**: 서버 응답의 초기속도 (ms, 낮을수록 빠름)
> - **Total Requests/Size**: 요청 수와 전체 다운로드 크기 (적을수록 가벼움)
