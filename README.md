# Sarang Univ Admin

사랑의교회 대학부 **수련회 통합 관리 시스템**의 관리자 웹 프런트엔드 스냅샷입니다.

> **이 저장소에 대하여**
> - 팀 조직(`Sarang-Univ`)에서 개발·운영 중인 관리자 프런트엔드를 개인 계정에 백업한 사본입니다. 프런트엔드는 팀원들이 주로 작성했습니다.
> - **본인(@Jkim1647) 담당은 백엔드**입니다 — Node.js · PostgreSQL 기반 REST API 설계와 성능 최적화. 백엔드 코드는 비공개 저장소에 있습니다.

## 주요 기능

| 영역 | 기능 |
|---|---|
| 신청 · 입금 | 수련회 · 부서별 신청 관리, 수련회비 · 셔틀버스비 입금 확인, 일정 변경 요청 |
| 숙소 | 숙소 배정(배정 알고리즘 · 미리보기), GBS별 숙소 엑셀 다운로드, 변경 이력 |
| GBS 라인업 | GBS 편성 · 장소 배정, 라인업 변경 이력, 다수 관리자 동시 편집 |
| 셔틀버스 | 버스 일정 관리, 일정 변경 요청 · 이력, 탑승 체크 |
| 현장 운영 | 식사 체크, 셔틀 체크, 일정 변경 이력 |

## 기술 스택

- **Frontend**: Next.js 14 (App Router) · React 18 · TypeScript
- **UI**: Tailwind CSS · shadcn/ui (Radix UI)
- **State / Data**: Zustand · SWR · Axios
- **Table**: TanStack Table v8 · TanStack Virtual
- **Backend (비공개)**: Node.js · PostgreSQL · REST API

## 성능 개선 기록

GBS 라인업 페이지는 여러 관리자가 동시에 편집하기 때문에 2초 간격 polling을 쓰고 있었는데, `user-lineups` API 응답이 평균 **2.87초**로 polling 간격보다 길어 요청이 겹치는 문제가 있었습니다. 원인 분석과 WebSocket 전환 계획은 [WEBSOCKET_MIGRATION_PLAN.md](./WEBSOCKET_MIGRATION_PLAN.md)에 정리했습니다.

그 밖의 기술 검토 문서는 [`docs/`](./docs)에 있습니다 — TanStack Table 셀 병합, 동적 컬럼 생성, 모바일 반응형 테이블, Zustand 상태 관리 분석 등.

## 디렉터리 구조

```text
src/
├─ app/
│  ├─ login/
│  └─ (main)/retreat/[retreatSlug]/   # 수련회별 관리 화면 (숙소, 라인업, 셔틀, 식사 체크 …)
├─ components/   # shadcn/ui + 기능별 컴포넌트
├─ hooks/        # SWR · 테이블 훅
├─ lib/ · utils/
├─ store/        # Zustand
└─ types/
```

## 로컬 실행

```bash
yarn install

# HTTP로 바로 실행
yarn dev:http          # http://localhost:3000

# 운영과 같은 도메인(HTTPS)으로 실행하려면 hosts 등록 후
# 127.0.0.1 local.admin.sarang-univ.com
yarn dev               # sudo node server.mjs
```

> 백엔드 API 서버가 비공개이므로 이 저장소만으로는 실제 데이터가 조회되지 않습니다.
