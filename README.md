### [S3 배포 링크](http://soominss-buket.s3-website.ap-northeast-2.amazonaws.com/)

### [CloudeFront 배포 링크](https://d3jxcj7xvwc1i5.cloudfront.net)

---

# 기본 과제

## 사용 기술

- **Next.js**: 정적 페이지 생성 프레임워크
- **Yarn**: 패키지 매니저 (`yarn install`, `yarn build` 사용)
- **GitHub Actions**: 자동화된 워크플로우 실행
- **AWS S3**: 정적 파일 업로드 대상
- **AWS CloudFront**: CDN을 통한 글로벌 캐싱 및 배포
- **IAM / GitHub Secrets**: 자격 정보 관리

---

## 배포 흐름

1. `main` 브랜치에 코드가 푸시되거나 수동으로 워크플로우 실행
2. [`.github/workflows/deployment.yml`](./.github/workflows/deployment.yml)에 정의된 작업 실행
3. `yarn install`로 의존성 설치 → `yarn build`로 정적 파일 생성
4. `out/` 디렉토리를 S3 버킷에 업로드
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





