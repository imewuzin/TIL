# AWS로 Frontend 배포하기

| AWS Amplify             | AWS S3 + CloudFront  |
| ----------------------- | -------------------- |
| Vercel처럼 Github 배포      | Github Pages처럼 정적    |
| Git push -> 자동 배포       | HTML, CSS, JS 단일 배포  |
| JavaScript / TypeScript | Build한 뒤 배포 필요       |
| SSR 등 지원                | SSR 등 불가능            |
| 종합적으로 Vercel 느낌         | 종합적으로 GH Pages 느낌    |
| Global CDN(CF) 간접 배포    | Global CDN(CF) 직접 배포 |

---

## AWS Amplify (추천)

- 그냥 AWS Amplify 홈페이지 들어가서 Github 레포 올리면 됨

---

## AWS S3 + CloudFront

- 파일 저장소 = S3

- CDN = CloudFront

- 사용자는 웹 주소를 방문 -> 가장 가까운 CDN(=CF)에 연결 -> CDN에 있는 Cache를 전송

- 만약 CDN Cache가 없다면 S3에 있는 원본을 CDN에 가져옴

- S3는 지역(ex. Seoul), CloudFront는 Global (지구 전체)
1. AWS access key, secret key 발급
   
   - [링크](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-northeast-2#/users/create)
   
   - 사용자 신규 생성해서 만들기
   
   - 정책 직접 연결, AWS S3 Full access 부여 (모든 S3 권한 줌. 개발용으로만 사용)
   
   . 사용자 클릭 - 보안 자격 증명 - 액세스 키 만들기 - Command Line Interface

2. AWS CLI 설치
   
   - `pip install awscli`

3. `aws config`
   
   - AWS 설정 - Access Key, Secret Key 생성 필요 -> 앞서 만든 키 사용
   
   - 지역(서울) = `ap-northeast-2`
   
   - 기본 타입 = `JSON`
   
   - 키 입력 시 아무것도 나타나지 않는 것처럼 보여도 실제로 잘 입력됨

4. AWS S3 버킷 만들기
   
   - 이름 고유해야 함

5. `npm run build`로 빌드하기

6. 빌드한 폴더 S3에 올리기
   
   - `aws s3 cp --recursive dist/ s3://ssafy-2024-frontend`

7. S3 버킷의 정적 웹 사이트 호스팅 활성화하기 (추천X. 보안 이유 생길수도. 비용 발생)

8. 혹은 AWS CloudFront에서 호스팅 활성화
   
   - S3 bucket 이름으로 검색 시 자동으로 버킷명.s3~url 생성
   
   - 원본 엑세스 제어 설정 사용
   
   - 신규 OAC 생성
   
   - 기본값 두고 create (Redirect HTTPS, GET/HEAD/OPTIONS 추천)
   
   - WAF 추가비용 드니 끄기
   
   - HTTP/2, 3 모두 활성
   
   - 기본값 루트 객체에 `index.html` 필수
   
   - 정책 복사하기

9. S3로 넘어가서 권한 - 정책 - 편집 - 정책 복사했던거 붙여넣기
