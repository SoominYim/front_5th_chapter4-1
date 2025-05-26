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
> (업데이트: 2025-05-26 16:26 KST)

| 항목 | S3 | CloudFront |
|------|------------------|--------------------|
| Performance | 🟨 87 | 🟩 98 |
| Accessibility | 🟩 100 | 🟩 100 |
| Best Practices | 🟩 92 | 🟩 100 |
| SEO | 🟩 100 | 🟩 100 |
| PWA | 🟥 33 | 🟥 33 |
| FCP | 2271.6235049999996 ms (2.3s) | 1167.4879999999998 ms (1.2s) |
| LCP | 3039.4621499999994 ms (3.0s) | 2109.488 ms (2.1s) |
| Speed Index | 4761.051886 ms (4.8s) | 2906.9211191695667 ms (2.9s) |
| TTI | 7157.316729999999 ms (7.2s) | 2034.4879999999998 ms (2.0s) |
| TBT | 133 ms (0.1s) | 4 ms (0.0s) |
| CLS | 0.04571302421445905 | 0 |
| TTFB | 0 ms (0.0s) | 229.676 ms (0.2s) |
| Total Requests | 19 | 17 |
| Total Transfer Size | 397525 bytes (0.38MB) | 164241 bytes (0.16MB) |
<!-- end -->

> - **FCP/LCP/Speed Index/TTI**: 페이지 렌더링 및 반응속도를 보여주는 지표 (낮을수록 빠름)
> - **TBT**: 사용자가 입력할 때 대기해야 하는 총 시간 (낮을수록 쾌적)
> - **CLS**: 페이지가 갑자기 움직이는 현상 정도 (0~0.1이면 매우 우수)
> - **TTFB**: 서버 응답의 초기속도 (ms, 낮을수록 빠름)
> - **Total Requests/Size**: 요청 수와 전체 다운로드 크기 (적을수록 가벼움)
>   **참고:**
>   S3 정적 웹호스팅은 서버 타이밍 정보를 제공하지 않아,
>   Lighthouse 등 일부 도구에서는 TTFB(Time To First Byte)가 0ms 또는 N/A로 표시될 수 있음
>   실제 TTFB는 네트워크 환경과 AWS 리전, 사용자 위치에 따라 수십~수백 ms일 수 있음
>   정확한 TTFB 측정이 필요하다면 브라우저 개발자도구, curl, WebPageTest, Pingdom 등의 별도 네트워크 도구를 활용
