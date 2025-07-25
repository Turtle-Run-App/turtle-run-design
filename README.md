# Turtle Run - 러닝 기반 영토 점령 게임

이 문서는 Turtle Run 앱의 기획 사항을 기록하고 있습니다.

## 1. 앱 개요

- **앱 이름**: Turtle Run
- **플랫폼**: iOS Native App
- **백엔드**: Java Spring
- **주요 기능**: 러닝 기반 영토 점령 게임

## 2. 핵심 컨셉

- 러닝 세션을 통해 Shell(영토)을 점령하는 게임적 요소
- 러닝 경로를 기반으로 한 다각형 Shell 확장
- 종족 시스템을 통한 러닝 버프 제공

## 3. 주요 기능

### 3.1 Shell 점령 시스템

- 러닝 경로 기반 다각형 Shell 생성
- Shell 크기 계산 및 점령
- Shell 시각화 (지도 표시)
- Shell 점령 히스토리

### 3.2 종족 시스템

#### 3.2.1 종족 개요

- 3가지 종족 중 선택 가능
- 각 종족별 고유 특성 및 러닝 버프 제공
- 러닝 스타일에 따른 최적화된 종족 선택 지원
- 종족 변경은 30일마다 1회 가능

#### 3.2.2 종족별 특성

**붉은귀거북 (Red-eared Slider)**

- 컨셉: 빠른 속도와 민첩성 특화
- 특징: 스피드 러너에게 최적화
- 시각적 특성: 빨간색 테마

**사막거북 (Desert Tortoise)**

- 컨셉: 장거리와 지구력 특화
- 특징: 마라톤 러너에게 최적화
- 시각적 특성: 노란색 테마

**그리스거북 (Greek Tortoise)**

- 컨셉: 다양한 경로 탐색 특화
- 특징: 탐험형 러너에게 최적화
- 시각적 특성: 파란색 테마

#### 3.2.3 종족별 버프 시스템

각 종족은 총 100%의 Shell layer 버프를 제공하며, 버프 구성은 다음과 같습니다:

**붉은귀거북 버프 (총 100%)**

- 기본 버프: 3km 이상 달릴 시 Shell layer 50%
- 페이스 버프: 평균 페이스 6'30"/km 이하 달성 시 Shell layer 35%
- 거리 버프: 5km 이상 달릴 시 Shell layer 15%

**사막거북 버프 (총 100%)**

- 기본 버프: 3km 이상 달릴 시 Shell layer 50%
- 페이스 버프: 평균 페이스 6'30"/km 이하 달성 시 Shell layer 20%
- 거리 버프: 5km 이상 달릴 시 Shell layer 30%

**그리스거북 버프 (총 100%)**

- 기본 버프: 3km 이상 달릴 시 Shell layer 50%
- 페이스 버프: 평균 페이스 6'30"/km 이하 달성 시 Shell layer 25%
- 거리 버프: 5km 이상 달릴 시 Shell layer 15%
- 탐사 버프: 새로운 경로 30% 이상 포함 시 Shell layer 10%

#### 3.2.4 종족 선택 및 관리

**초기 설정**

- 기존 러닝 데이터 분석을 통한 Shell 계산
- 종족별 버프 적용 시뮬레이션
- SP 환산 과정의 시각적 표현
- 개인 랭크 자동 결정

**종족 변경 시스템**

- 30일마다 1회 변경 가능
- 변경 시 현재 버프 획득 현황 표시
- 러닝 스타일 분석 기반 추천 종족 제시
- 변경 시 기존 SP는 유지

### 3.3 개인 SP 시스템

- SP(Shell Point): 개인이 기여한 Shell layer의 총 누적치
- SP 기반 개인 랭킹 시스템
- 랭크 구분
  - 해츨링 (Hatchling): 0~999 SP
  - 주니어 (Juvenile): 1,000~4,999 SP
  - 서브어덜트 (Subadult): 5,000~19,999 SP
  - 어덜트 (Adult): 20,000~49,999 SP
  - 매츄어 (Mature): 50,000~99,999 SP
  - 엘더 (Elder): 100,000~249,999 SP
  - 레전드 (Legend): 250,000+ SP
- SP 누적에 따른 업적 및 보상 시스템

### 3.4 랭크 시스템

#### 3.4.1 랭크 구분

SP(Shell Point) 기반으로 다음과 같이 7단계로 구분:

- **해츨링 (Hatchling)**: 0~999 SP
  - 새로 태어난 거북이
  - 기본 프로필 배지
- **주니어 (Juvenile)**: 1,000~4,999 SP

  - 활발한 젊은 거북이
  - 브론즈 배지 및 색상

- **서브어덜트 (Subadult)**: 5,000~19,999 SP

  - 성장하는 거북이
  - 실버 배지 및 색상

- **어덜트 (Adult)**: 20,000~49,999 SP

  - 성숙한 거북이
  - 골드 배지 및 색상

- **매츄어 (Mature)**: 50,000~99,999 SP

  - 지혜로운 거북이
  - 플래티넘 배지 및 색상

- **엘더 (Elder)**: 100,000~249,999 SP

  - 존경받는 원로
  - 다이아몬드 배지 및 색상

- **레전드 (Legend)**: 250,000+ SP
  - 전설적인 러너
  - 특별 레전드 배지 및 효과

#### 3.4.2 랭크별 혜택

**공통 혜택**

- 프로필 배지 및 색상 변경
- 랭크에 따른 프로필 테마 해금
- 커뮤니티 내 권한 확대

**고급 랭크 전용 혜택**

- 특별 Shell 패턴 해금 (Adult 이상)
- 이벤트 우선 참여권 (Mature 이상)
- 멘토링 시스템 참여 자격 (Elder 이상)
- 베타 기능 우선 체험권 (Legend)

#### 3.4.3 SP 환산 시스템

**Shell layer → SP 환산**

- Shell layer 면적을 SP로 1:1 환산
- 종족별 버프가 적용된 최종 Shell layer 기준
- 실시간 SP 누적 및 랭크 업데이트
- 랭크 승급 시 특별 애니메이션 효과

## 4. 기술 스택

### 4.1 프론트엔드 (iOS)

**Core Framework**

- Swift (최신 버전)
- SwiftUI (주요 UI) + UIKit (세부 컴포넌트)
- Combine (반응형 프로그래밍)

**위치 및 헬스**

- Core Location (GPS 추적)
- HealthKit (러닝 데이터 연동 및 워크아웃 종료 감지)
- MapKit (지도 및 Shell 시각화)
- Core Motion (동작 감지)
- HKObserverQuery (HealthKit 데이터 변경 감지)

**데이터 및 네트워킹**

- Core Data (로컬 데이터 저장)
- URLSession + Alamofire (네트워킹)
- Codable (JSON 파싱)

**UI/UX 향상**

- Core Animation (애니메이션)
- Core Graphics (커스텀 Shell 드로잉)

### 4.2 백엔드 (Java Spring)

**Core Framework**

- Java 17+ (LTS)
- Spring Boot 3.x
- Spring WebFlux (비동기 처리)
- Spring Security (인증/인가)

**데이터 처리**

- Spring Data JPA (ORM)
- Spring Data Redis (캐싱 및 세션)
- PostgreSQL (메인 데이터베이스)
- Redis (캐시, 실시간 랭킹)
- Jackson (JSON 파싱 - HealthKit 데이터 처리)

**지리 정보 처리**

- PostGIS (공간 데이터 확장)
- GeoTools (지리 계산)
- JTS Topology Suite (다각형 계산)
- Spatial4j (공간 데이터 인덱싱)

**실시간 및 메시징**

- Spring WebSocket (실시간 통신)
- Apache Kafka (이벤트 스트리밍)
- Firebase Cloud Messaging (푸시 알림)

**모니터링 및 분석**

- Spring Actuator (헬스 체크)
- Micrometer (메트릭 수집)
- ELK Stack (로그 분석)

### 4.3 인프라 및 DevOps

**클라우드 플랫폼**

- AWS (주요 플랫폼)
  - EC2 (서버 호스팅)
  - RDS (PostgreSQL 관리)
  - ElastiCache (Redis 관리)
  - S3 (파일 저장)
  - CloudFront (CDN)

**컨테이너 및 오케스트레이션**

- Docker (컨테이너화)
- Kubernetes (오케스트레이션)
- AWS EKS (관리형 K8s)

**CI/CD**

- GitHub Actions (CI/CD 파이프라인)
- SonarQube (코드 품질)
- AWS CodeDeploy (배포 자동화)

**모니터링**

- Prometheus + Grafana (메트릭 모니터링)
- AWS CloudWatch (시스템 모니터링)
- Sentry (에러 추적)

## 5. 데이터 모델

### 5.1 사용자 (User)

- TBW

### 5.2 러닝 세션 (Running Session)

**HealthKit 연동 데이터**

- 워크아웃 ID (HKWorkout identifier)
- 시작/종료 시간
- 총 거리 및 소요 시간
- 평균 페이스
- GPS 경로 데이터 (CLLocation 배열)
- 칼로리 소모량

**Shell 계산 데이터**

- 경로 기반 다각형 좌표
- 계산된 Shell 면적
- 종족별 적용 버프
- 최종 Shell layer 값
- 서버 전송 상태

### 5.3 Shell (쉘)

**기본 정보**

- Shell ID (고유 식별자)
- 사용자 ID (소유자)
- 생성 시간
- 러닝 세션 ID (연관 워크아웃)

**지리 정보**

- 다각형 좌표 (GeoJSON 형식)
- Shell 면적 (㎡)
- 중심점 좌표
- 경계 박스 (bounding box)

**게임 데이터**

- 기본 Shell layer 값
- 종족별 적용 버프
- 최종 Shell layer (SP 환산용)
- 버프 적용 내역

### 5.5 종족 (Species)

**종족 정보**

- 종족 ID
- 종족 이름 (한글/영문)
- 종족 설명
- 테마 색상

**버프 정보**

- 기본 버프 비율
- 페이스 버프 비율
- 거리 버프 비율
- 탐사 버프 비율 (그리스거북만)
- 버프 조건 정보

**통계**

- 종족별 사용자 수
- 종족별 평균 SP
- 종족별 활성 사용자 비율

## 6. UI/UX 디자인 가이드라인

### 6.1 디자인 테마

- 모던한 도시 야경을 연상시키는 어두운 초록색 메인 테마
- 게임적 요소와 도시적 감각이 조화된 UI
- 직관적인 지도 인터페이스
- 동기부여를 주는 시각적 요소

### 6.2 색상 팔레트

**메인 테마**

- 메인 컬러: #1A3A2F (어두운 초록색)
- 보조 컬러: #2C4F43 (중간 톤 초록색)
- 강조 컬러: #4A9D7F (밝은 초록색)
- 배경색: #121212 (어두운 배경)
- 텍스트: #FFFFFF (흰색)
- 보조 텍스트: #B3B3B3 (회색)

**종족별 테마 색상**

_붉은귀거북 (Red-eared Slider)_

- 주요 색상: #E53E3E (선명한 빨강)

_사막거북 (Desert Tortoise)_

- 주요 색상: #D69E2E (황금색)

_그리스거북 (Greek Tortoise)_

- 주요 색상: #3182CE (선명한 파랑)

**랭크별 색상**

- 해츨링: #718096 (회색)
- 주니어: #CD7F32 (브론즈)
- 서브어덜트: #C0C0C0 (실버)
- 어덜트: #FFD700 (골드)
- 매츄어: #E5E4E2 (플래티넘)
- 엘더: #B9F2FF (다이아몬드)
- 레전드: #9F7AEA (퍼플/레전드)

### 6.3 주요 화면

- 스플래시 화면
- 로그인/회원가입 화면
- 메인 대시보드
- 지도/Shell 화면
- 리그/랭킹 화면
- 프로필/설정 화면

## 7. 개발 로드맵

### 7.1 Phase 1 (MVP) - 기본 기능 구현

**인증 및 사용자 관리**

- 이메일/소셜 로그인 시스템
- 사용자 프로필 기본 설정
- 기존 러닝 데이터 연동 (HealthKit)

**Shell 점령 시스템**

- HealthKit 워크아웃 종료 이벤트 감지
- GPS 기반 러닝 경로 데이터 수집
- Shell 다각형 생성 알고리즘
- Shell 크기 계산 및 서버 전송
- 기본 지도 UI 및 Shell 시각화

**종족 시스템**

- 3가지 종족 선택 기능
- 종족별 버프 시스템 구현
- 종족별 Shell layer 계산
- 종족 변경 30일 제한 로직

**SP 및 랭크 시스템**

- Shell layer → SP 환산 시스템
- 7단계 랭크 구분 및 승급 로직
- 개인 프로필 및 통계 화면
- 기본 랭크 배지 시스템

### 7.2 Phase 2 - 소셜 기능 및 고도화

**Nest (그룹) 시스템**

- Nest 생성 및 관리
- 멤버 초대 및 권한 관리
- Nest 전용 Shell 통합
- Nest 랭킹 시스템

**소셜 및 커뮤니티 기능**

- 친구 추가 및 친구 목록
- 러닝 세션 공유 기능
- Shell 점령 알림 시스템
- 사용자 간 메시징

**고급 게임 요소**

- 업적 및 배지 시스템
- 도전 과제 (Daily/Weekly/Monthly)
- 특별 이벤트 및 시즌 시스템
- 리더보드 및 경쟁 시스템

## 9. 핵심 용어 정의

### 9.1 Shell (쉘)

- 러닝 경로를 기반으로 생성되는 다각형 영토
- 개인의 기본 점령 단위
- 거북이의 등껍질을 모티브로 한 디자인
- 크기와 모양이 러닝 경로에 따라 다양하게 생성
- 개인 또는 종족의 소유가 가능

### 9.2 Shell layer (쉘 레이어)

- Shell의 실제 면적을 나타내는 단위
- 종족별 버프가 적용되어 최종 Shell layer 계산
- SP 환산의 기준이 되는 핵심 지표
- 러닝 성과에 따라 다양한 버프로 증가 가능

### 9.3 종족 (Species)

- 사용자가 선택할 수 있는 3가지 거북이 종족
- 각 종족별 고유한 버프 시스템 제공
- 30일마다 1회 변경 가능

**종족별 특성**

- **붉은귀거북**: 속도 특화, 빠른 페이스에 유리한 버프
- **사막거북**: 지구력 특화, 장거리 러닝에 유리한 버프
- **그리스거북**: 탐험 특화, 새로운 경로 개척에 유리한 버프

### 9.4 버프 (Buff)

- 종족별로 제공되는 Shell layer 증가 효과
- 러닝 조건 달성 시 자동으로 적용
- 총 100%까지 Shell layer 증가 가능

**버프 종류**

- **기본 버프**: 3km 이상 러닝 시 기본 제공 (50%)
- **페이스 버프**: 평균 페이스 6'30"/km 이하 달성 시
- **거리 버프**: 5km 이상 장거리 러닝 시
- **탐사 버프**: 새로운 경로 30% 이상 포함 시 (그리스거북 전용)

### 9.5 SP (Shell Point)

- 개인이 기여한 Shell layer의 총 누적치
- 랭킹 시스템의 기본 단위
- Shell layer 1㎡ = SP 1점으로 환산
- 종족 버프가 적용된 최종 Shell layer 기준으로 계산

### 9.6 랭크 (Rank)

- SP 기반 7단계 등급 시스템
- 각 랭크별 고유한 배지 및 혜택 제공

**랭크 구분**

- 해츨링 (0~999 SP): 새로 태어난 거북이
- 주니어 (1,000~4,999 SP): 활발한 젊은 거북이
- 서브어덜트 (5,000~19,999 SP): 성장하는 거북이
- 어덜트 (20,000~49,999 SP): 성숙한 거북이
- 매츄어 (50,000~99,999 SP): 지혜로운 거북이
- 엘더 (100,000~249,999 SP): 존경받는 원로
- 레전드 (250,000+ SP): 전설적인 러너

### 9.8 Shell 점령

**HealthKit 기반 세션 종료 감지**

- Apple 운동 앱 또는 다른 러닝 앱의 워크아웃 종료 이벤트 감지
- HealthKit에서 새로 생성된 러닝 데이터 자동 수집
- 워크아웃 완료 시점에 Shell 계산 프로세스 시작

**Shell 계산 및 서버 전송**

- GPS 좌표를 기반으로 한 다각형 생성
- 종족별 버프 적용하여 Shell layer 계산
- 계산된 Shell 데이터를 Turtle Run 서버에 전송
- 기존 Shell과의 중복 검사 및 병합 처리
- 점령자 정보 및 시간 기록
