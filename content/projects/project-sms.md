---
title: "Student Management Service"
startDate: "2023-04"
team: "MSG 동아리"
---

> AWS S3, Redis, Spring Boot, Hexagonal Architecture

백엔드 개발자로 참여한 프로젝트로, 학교 내부에서 기업에 제출할 목적으로 사용하던 기존의 종이책자를 브라우저와 앱으로 사용할 수 있게 하는 것을 목적으로 개발되었습니다.

# 출시 과정에서 생긴 문제

출시 전에 테스트를 하는 과정에서 JPA Entity에서는 이미 삭제된 필드임에도 불구하고, 프로덕션 데이터베이스에서는 계속 필드가 남아있어 Nullable 관련 오류가 발생하였고, 코드와 비교해서 사용되지 않는 필드를 제거하는 작업을 진행하였습니다.

또한 학교 선생님들과 요구사항 타협 도중 변경된 사항이 있어, 학생이 제출한 인증제 혹은 보고서 목록을 확인하고 승인할 수 있는 것은 모든 선생님에서 각 반의 담임선생님으로 수정하였습니다.

[🔁 제출된 인증제 목록을 각 담임 선생님 반의 인증제만 반환하도록 함](https://github.com/GSM-MSG/SMS-BackEnd/pull/419)

# 데이터베이스 해킹 이슈

데이터베이스를 해킹당해서 학생 정보, 포트폴리오 등의 개인정보가 탈취당하는 일이 발생하였습니다. 예전에 성능과 보안 대신 비용 최적화를 위해 많은 것들을 축소했었고, 그 과정에서 너무 광범위한 보안 그룹 포트 노출, 취약한 비밀번호 등의 문제가 드러나게 되었습니다.

예정했던 출시일에 정상적으로 진행할 수 없게 되었고, 기존에 EC2에서 컨테이너로 실행되던 MySQL을 주기적인 백업을 위해서 RDS로 마이그레이션하고, 보안 그룹의 포트를 수정해서 이번 사건을 재번복하지 않기 위해 노력하였습니다.

# 신기능 개발

기업에서 포트폴리오에 아무 제한없이 접근할 수 있도록 허용하면 개인정보 침해가 발생할 수 있기 때문에, 디스코드와 유사하게 만료기간이 있는 링크를 생성하고, 해당 기간에만 특정 학생의 포트폴리오를 확인할 수 있도록 하는 기능을 Redis TTL을 이용해서 개발하였습니다.

[🔁 학생정보 조회 링크 생성 API 개발](https://github.com/GSM-MSG/SMS-BackEnd/pull/314)

포트폴리오 파일 업로드 기능을 개발하기 위해서 AWS S3를 사용하였고, 확장자를 PDF로 제한하는 함수를 추가하였습니다.

[🔁 포트폴리오 PDF 업로드 기능 개발](https://github.com/GSM-MSG/SMS-BackEnd/pull/363)