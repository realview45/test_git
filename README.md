## 💬 dbdb Talk 서비스 데이터 모델 $\text{README}$

안녕하세요\! **dbdb** 팀에서 설계한 $\text{Porail}$ 서비스의 **실시간 채팅 및 사용자 관리**를 위한 데이터베이스 $\text{ERD}$ 분석 결과입니다.

-----

## 💡 개요

이 $\text{ERD}$는 **사용자 관리**, **채팅방 및 메시지 교환**, **신고/제재**, **알림** 기능을 지원하도록 설계되었습니다. 실시간 상호작용과 관리 기능을 위한 핵심 데이터 구조를 정의합니다.

-----

## 📋 $\text{ERD}$ 핵심 테이블 분석

### 👥 사용자 및 제재 관리

| 테이블명 | 한글명 | 설명 | 주요 필드 |
| :--- | :--- | :--- | :--- |
| **`User`** | 사용자 테이블 | 서비스 이용자 정보. | `user_id` (PK), `nickname`, `email`, `is_active` (활성화 상태), `roles` (역할). |
| **`BanLog`** | 제재처리 전체 로그 | 관리자에 의한 사용자 제재 기록. | `id` (PK), `ban_type`, `reason`, `ban_start_time`, `ban_end_time`. |
| **`Report`** | 신고 테이블 | 사용자 신고 내역 기록. | `id` (PK), `reporter_user_id`, `reported_object_id` (신고 대상), `reason`, `process_status` (처리 상태). |
| **`Forbidden_words`** | 금지어 테이블 | 채팅 메시지 검열에 사용될 금지어 목록. | `id` (PK), `forbidden_word`, `chat_room_id` (특정 채팅방 금지어 지정 가능). |

### 💬 채팅방 및 메시지 관리

| 테이블명 | 한글명 | 설명 | 주요 필드 |
| :--- | :--- | :--- | :--- |
| **`ChatRoom`** | 채팅방 테이블 | 채팅방 자체의 정보. | `id` (PK), `topic`, `start_time`, `end_time` (방 종료 시간). |
| **`RoomParticipant`** | 채팅방 참여자 테이블 | 어떤 사용자가 어떤 채팅방에 참여했는지 연결 (N:M 해소). | `id` (PK), `room_id` (FK), `user_id` (FK), `join_time`, `exit_time`. |
| **`ChatMessage`** | 키뮤니케이션 테이블 | 채팅방 내에서 교환된 메시지 내용. | `id` (PK), `room_id` (FK), `user_id` (FK), `content`, `created_at`. |
| **`MessageRead`** | 메시지 읽음 여부 | 사용자가 메시지를 읽었는지 확인. **'읽음' 처리** 기능 구현에 활용. | `id` (PK), `message_id` (FK), `user_id` (FK), `is_read`. |

### 🔔 알림 관리

| 테이블명 | 한글명 | 설명 | 주요 필드 |
| :--- | :--- | :--- | :--- |
| **`Notification`** | 알림 테이블 | 시스템 알림 또는 이벤트 알림 내용. | `id` (PK), `title`, `content`, `created_time`. |
| **`NotificationUser`** | 알림 수신 테이블 | 특정 사용자가 특정 알림을 받았는지 관리. | `id` (PK), `notification_id` (FK), `user_id` (FK), `is_read`, `delivered_time`. |

-----

## 🔗 주요 관계 및 특징

1.  **채팅방 참여 ($\text{N:M}$)**: `ChatRoom`과 `User`는 **`RoomParticipant`** 테이블을 통해 다대다 관계로 연결됩니다.
2.  **메시지 전송**: `ChatMessage`는 `ChatRoom`과 `User` 모두에 연결되어 **누가($\text{User}$)** **어떤 방에($\text{ChatRoom}$)** 메시지를 보냈는지 명확히 합니다.
3.  **읽음 처리**: `MessageRead` 테이블은 `ChatMessage`와 `User`를 연결하여 각 사용자가 메시지를 읽었는지 추적합니다.
4.  **신고 및 제재**: `Report` 테이블은 `User`를 신고자 및 피신고자로 연결하며, `BanLog`는 제재 기록을 남깁니다. $\text{BanLog}$는 `User`와 `Report` 테이블 모두와 연결될 수 있습니다.
5.  **금지어 범위**: `Forbidden_words`는 `ChatRoom` $\text{ID}$를 포함하여 **전역 금지어** 또는 **특정 채팅방에만 적용되는 금지어**를 관리할 수 있습니다.

-----

## 🛠️ 스택
<p>
<img src="https://img.shields.io/badge/mariaDB-003545?style=for-the-badge&logo=mariaDB&logoColor=white">
<img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white">
<img src="https://img.shields.io/badge/visualstudiocode-007ACC?style=for-the-badge&logo=visualstudiocode&logoColor=white">
<img src="https://img.shields.io/badge/git-F05032?style=for-the-badge&logo=git&logoColor=white">
<img src="https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white">
<img src="https://img.shields.io/badge/notion-000000?style=for-the-badge&logo=notion&logoColor=white">
<img src="https://img.shields.io/badge/discord-5865F2?style=for-the-badge&logo=discord&logoColor=white">
</p>


### 팀명: dbdb

-----

이 $\text{ERD}$는 $\text{dbdb}$ 서비스 내에서 실시간 소통 및 커뮤니티 관리 기능을 효과적으로 지원하기 위한 기반 구조입니다.
