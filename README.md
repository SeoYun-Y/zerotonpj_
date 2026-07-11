# 🌱 EcoStep (제로톤 프로젝트)

> 날씨에 따라 실천 가능한 환경 보호 챌린지를 추천하고, 인증을 통해 포인트를 적립하는 지속가능한 생활 실천 앱


https://github.com/user-attachments/assets/198157be-5eda-4682-a273-5b95e3895bd1
---

## 📖 프로젝트 소개

"환경을 지켜야 한다"는 건 알지만, 막상 오늘 뭘 해야 할지 막막할 때가 많습니다.
**EcoSeed**는 사용자의 현재 위치 날씨 데이터를 기반으로 실천 가능한 친환경 챌린지를 추천하고, 인증 사진을 업로드하면 포인트를 적립해주는 앱입니다. 예를 들어 맑은 날엔 "빨래 햇볕에 말리기", 비 오는 날엔 "빗물로 식물에 물주기"처럼 날씨와 연결된 행동을 제안해 실천 장벽을 낮췄습니다.

> 📌 이 리포지토리는 팀원의 원본 리포지토리를 fork하여 진행한 팀 프로젝트입니다. 백엔드 API 서버는 별도 리포지토리([zerothon](https://github.com/SeoYun-Y/zerothon))에서 직접 개발했습니다.

## 🧑‍🤝‍🧑 팀 구성

| 이름 | 역할 |
|---|---|
| 염서윤 | Backend & Flutter |
| 박채영 | Backend & Flutter |
| 주동광 | 기획 |
| 조혜진 | 디자인 |
| 김재민 | 기획 |

## 🙋‍♀️ 본인 담당 부분

디자이너가 전달한 디자인 시안을 바탕으로 아래 화면들을 Flutter로 구현하고 백엔드 API와 연동했습니다.

- 로그인 / 회원가입 화면
- 홈 / 날씨 기반 챌린지 추천 화면
- 챌린지 상세 / 인증 업로드 화면


## 🛠️ 기술 스택

**Frontend**
- Flutter (iOS / Android / Web 크로스플랫폼)
- Geolocator (위치 기반 날씨 조회)

**Backend**
- Node.js + Express.js — RESTful API 서버
- Firebase Firestore — NoSQL 실시간 데이터베이스
- Firebase Authentication — 이메일/비밀번호 인증
- Firebase Admin SDK — 서버 측 DB 제어
- OpenWeatherMap API — 실시간 날씨 데이터 수집

## 🏗️ 아키텍처

```
[Flutter App]
   ├── Geolocator → 사용자 현재 위치 획득
   ├── HTTP 요청 → [Node.js/Express 서버]
   │                    └── OpenWeatherMap API 호출 → 날씨 태그 산출
   │                    └── Firestore 조회 → 날씨 태그 매칭 챌린지 반환
   └── Firebase Auth/Firestore 직접 연동 → 로그인, 인증(Proof) 업로드, 포인트 갱신
```

- 사용자 인증은 클라이언트에서 Firebase Auth SDK로 직접 처리
- 날씨 기반 챌린지 추천은 별도 Express 서버를 통해 OpenWeatherMap → Firestore 조회 순으로 처리
- 인증/포인트 적립은 Firestore 트랜잭션으로 즉시 반영

## 🔑 주요 기능

### 회원가입 / 로그인
- Firebase Authentication 기반 이메일/비밀번호 인증
- 가입 시 `users` 컬렉션에 `email`, `nickname`, `totalPoint`, `joinedChallenges` 초기화

### 날씨 기반 챌린지 추천
- 위치 좌표(위도/경도)를 서버로 전달 → OpenWeatherMap으로 날씨 조회 → `weatherTag`로 챌린지 필터링
- 예: 맑음 → 빨래 말리기 / 비 → 빗물 재활용

### 챌린지 참여 & 인증
- 참여 제한 없이 다중 챌린지 참여 가능, `users.joinedChallenges`에 기록
- 인증 사진 + 설명 업로드 → `proofs` 컬렉션 저장, 동일 챌린지 중복 인증 방지
- 인증 제출과 동시에 포인트 자동 적립

### 마이페이지
- 누적 포인트, 참여 챌린지 목록, 인증 이력 조회

## 📁 Firestore 데이터 구조

```
users (collection)
 └─ {userId}
     ├─ email, nickname
     ├─ totalPoint
     └─ joinedChallenges: [challengeId, ...]

proofs (collection)
 └─ {proofId}
     ├─ userId, challengeId
     ├─ imageUrl, description
     └─ status (기본: 승인)
```

## 🚧 트러블슈팅

### Flutter 패키지 버전 호환성 이슈
Firebase 관련 패키지(firebase_core, firebase_auth, cloud_firestore 등)와 Flutter SDK 버전 간 호환성 문제로 빌드 오류가 반복적으로 발생했습니다. 패키지 버전을 하나씩 맞춰가며 `pubspec.yaml`의 의존성 버전을 조정하고, `flutter pub get` / `flutter clean` 을 반복 실행하며 원인을 좁혀 해결했습니다.


## 🚀 향후 개선 사항

- 관리자 인증 승인/반려 기능
- 사용자 간 랭킹/경쟁 요소
- 챌린지 상세 페이지 댓글 기능
- Firebase Cloud Messaging 푸시 알림

## 📦 실행 방법

```bash
flutter pub get
flutter run
```



https://github.com/user-attachments/assets/ad88f009-84fe-43d1-b1a3-6973fddbfbfa

