# 📚 DeveryTime
> [cite_start]**"JSP Model 2 기반의 학습 관리 및 체계적인 스터디 그룹/일정 관리 플랫폼"** [cite: 1829, 1830, 1831]
---

## 📅 프로젝트 기간
- [cite_start]**분석 및 구현:** 2026. 03. 19 ~ 2026. 03. 31 [cite: 1860]

---

## 🛠️ Tech Stacks & Environment
- [cite_start]**Language:** Java 21 [cite: 1862]
- [cite_start]**Framework/Library:** Servlet, JSP (Model 2) [cite: 1861]
- [cite_start]**Frontend:** HTML5, JavaScript, Tailwind CSS, DaisyUI, FullCalendar [cite: 1746, 3310]
- [cite_start]**Database & Server:** Oracle 21c, Tomcat 9.0 [cite: 1862]
- [cite_start]**VCS & Tools:** Git, GitHub, Figma, ERDCloud, Notion [cite: 1937, 2039, 2624, 3347]

---

## 👥 Contributors 및 요구사항 분석
- [cite_start]**김민도 (스터디 파트 담당):** 스터디, 스터디 일정, 스터디 할 일 관리 CRUD 로직 구현[cite: 1886, 1887]. [cite_start]스터디장 위임/추방 및 중복 가입 방지 등 권한 제어 로직 설계[cite: 1906]. [cite_start]DB 객체 정의서 작성 및 TABLE 제약조건 검수[cite: 1888].
- [cite_start]**황윤재 (회원/메인):** 로그인/로그아웃, 회원가입, 이메일 2차 인증 구현[cite: 1876, 1877].
- [cite_start]**박지명 (게시판):** 다중 도메인 게시판 CRUD 설계 및 필터/권한 처리[cite: 1889, 1890].
- [cite_start]**이찬희 (학습계획):** 학습계획 및 기록 관리, Tailwind 공통 CSS 및 페이징 클래스 설계[cite: 1882, 1883, 1885].

---

## 1. 🏗️ System Architecture & Tech Achievement

### 🔹 트리거(Trigger) 기반 데이터 무결성 제어
- [cite_start]**스터디 구성원 동기화 (`trg_member_withdraw_sync`):** 회원이 탈퇴할 때 데이터를 즉시 삭제하지 않고 상태(status)만 변경하는 시스템 특성상, 기존의 `ON DELETE CASCADE`를 사용할 수 없었습니다[cite: 3189]. [cite_start]이를 해결하기 위해 회원 상태가 변경될 때 해당 회원의 스터디 가입 정보를 자동으로 삭제(`DELETE FROM study_member`)하는 트리거를 구현하여 데이터 무결성을 완벽히 보장했습니다[cite: 3189, 3196, 3197].
- [cite_start]**정원 초과 방지 (`trg_check_study_capacity`):** 스터디 가입 시 최대 정원(capacity)과 현재 가입 인원을 비교하여 정원 초과 가입을 원천 차단하는 복합 트리거를 설계 및 적용했습니다[cite: 3207, 3208]. [cite_start]이 과정에서 데이터 증가에 따른 부하를 인지하고, 향후 성능 최적화를 위한 비정규화(현재 인원수 컬럼 추가) 아이디어를 도출하기도 했습니다[cite: 3208, 3209].

### 🔹 비동기 통신과 프론트엔드 연동을 통한 UX 최적화
- [cite_start]**Fetch API 도입:** 세부 할 일(To-Do)의 완료 상태를 변경할 때 새로고침 없이 Fetch API를 활용하여 비동기로 백엔드와 통신(`todo-status.do`)하도록 설계했습니다[cite: 1800]. 
- [cite_start]**즉각적인 시각적 피드백:** 비동기 요청 후 응답받은 JSON 결과에 따라 JavaScript로 즉각적인 DOM 조작(취소선 및 배경색 처리)을 수행하여 사용자 경험(UX)을 크게 향상시켰습니다[cite: 1801, 1802].
- [cite_start]**모던 UI 및 시각화:** Tailwind CSS와 DaisyUI를 활용해 일관된 디자인 시스템을 구축하고, FullCalendar 라이브러리를 연동하여 스터디 일정을 직관적인 달력 형태로 제공했습니다[cite: 1746, 1774, 3310].

### 🔹 Servlet/JSP (Model 2) 계층 분리와 방어적 프로그래밍
- [cite_start]**SRP (단일 책임 원칙) 적용:** 요청 제어(Controller), 비즈니스 로직(Service), 데이터 접근(DAO) 계층을 엄격히 분리하여 결합도를 낮췄습니다[cite: 2893, 2993]. 
- **데이터 정합성 검증:** 스터디 정보 수정 시 현재 가입된 인원 수(`totalCountM`)보다 정원을 낮게 설정할 수 없도록 Java 백엔드 단에서 강력한 예외 처리 방어벽을 구축했습니다.

---

## 2. 🤖 Retrospective: 견고한 시스템과 협업의 가치

### "사전 설계와 규칙이 디버깅의 시간을 결정한다"
[cite_start]이번 프로젝트를 통해 Servlet이 Java와 Tomcat 사이에서 어떻게 동작하고, JSP와 Parameter를 어떻게 주고받는지 그 본질적인 흐름을 체득했습니다[cite: 3306].

- [cite_start]**설계와 명명 규칙의 중요성:** 프로젝트 초기, 메서드와 파라미터의 명명법을 명확하게 통일하지 않아 디버깅 과정에서 적지 않은 어려움을 겪었습니다[cite: 3308]. [cite_start]다른 사람의 코드를 유지보수해야 하는 상황을 시뮬레이션하며, **'코드 작성 전 팀원 간의 완벽한 컨벤션 합의'**가 선택이 아닌 필수임을 깊이 깨달았습니다[cite: 3308, 3309].
- [cite_start]**풀스택 관점과 DB 최적화:** 단순한 데이터 삽입을 넘어, 오라클(Oracle) 고급 문법을 적극적으로 찾아 활용하며 복잡한 테이블 제약조건을 위반하지 않는 정교한 데이터를 생성해 냈습니다[cite: 3316, 3317]. [cite_start]이를 통해 데이터베이스 설계와 무결성 유지에 대한 이해도를 크게 높일 수 있었습니다[cite: 3317].
- [cite_start]**협업과 형상 관리 (Git):** 처음 접하는 Git-Hub의 브랜치(branch) 전략과 병렬 작업이 초기에는 낯설고 힘들었지만, 사용법을 익히고 나니 **"형상 관리 시스템 없이는 진정한 협업 개발이 불가능하다"**는 확신을 얻었습니다[cite: 3313, 3314].
- [cite_start]**결론:** 단순히 백엔드 로직을 짜는 것에 그치지 않고, Tailwind와 DaisyUI를 활용한 프론트엔드 작업까지 주도하며 전체 애플리케이션의 유기적인 결합을 경험했습니다[cite: 3310, 3311]. 앞으로도 백엔드의 견고함을 기본으로, 프론트엔드와의 원활한 소통과 체계적인 아키텍처 설계 능력을 갖춘 주도적인 개발자로 성장해 나갈 것입니다.
