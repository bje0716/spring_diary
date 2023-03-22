> 기획서를 혼잣말 하는 말투로 작성했습니다.  
글 읽는데 방해될수 있다고 생각하여 서두에 양해를 구합니다.  
기획서가 완성하는대로 차차 수정하겠습니다 😅

# 일기장

SNS 타임라인 형식 일기장  
특징은 나만 쓰고 나만 볼수 있음 댓글도 나만 달수있음  
한마디로 나 혼자 북치고 장구치는 서비스다.  
그렇다. 내가 만들어서 나 혼자 쓸려고 기획했다.  

## 요구사항
차후 업데이트. 당장 생각나는게 없음.

## 사용 기술
이름 | 버전 | 개발사 | 사용용도 | URL | 비고
--- | ---| --- | --- | --- | ---
`Windows` | 10 | `Microsoft` | 운영체제 | https://microsoft.com/ko-kr/windows
`Zulu OpenJDK` | 11 | `Azul` | 자바 개발 도구 | https://azul.com
`IntelliJ IDEA` | 2022.3.3 | `Jetbrains` | IDE | https://jetbrains.com/idea
`Spring Boot` | 2.7.9 | `VMWare` | 웹 백엔드 프레임워크 | https://spring.io
`MariaDB` | 10.11.2 | `MariaDB Foundation` | RDBMS | https://mariadb.org
`HeidiSQL` | 12.4.0 | `Ansgar Becker` | DBMS 툴 | https://heidisql.com
`Bootstrap` | 5.2.3 | `Twitter` | CSS 프레임워크 | https://getbootstrap.com | 사용할지 안할지는 몰?루
`Font Awesome` | 6.3.0 | `FontIcons` | CSS 아이콘 프레임워크 | https://fontawesome.com
`JQuery` | 1.11.1 | `OpenJS Foundation` | JavaScript 프레임워크 | https://jquery.com | 사용할지 안할지는 몰?루
`Github` | | `Github` | Git | https://github.com | 소스 저장소
`Visual Studio Code` | 1.76.2 | `Microsoft` | 텍스트 편집기 | https://code.visualstudio.com | 기획서 작성에 사용

## DB 설계

~~최대한 단순하게 설계했음.~~ 단순한거 맞음? 🤔  
구현은 JPA 사용.

### 회원 테이블 (Member)
이름 | 타입 | 설명 | 예시 | 비고
--- | --- | --- | --- | ---
`member_id` | `Long` | 회원 ID (Index) | | `Primary Key`
`email` | `varchar` | 이메일 | admin@admin.com
`password` | `varchar` | 비밀번호 | (대충 sha256 긴거) | 2자 이상 8자 이하 특수문자 포함, 암호화 필수
`profile_img_url` | `varchar` | 프로필 이미지 URL | http://localhost:8080/img/profile/{email}-{uuid}.png | 이미지 사이즈 64x64
`last_login_date` | `Date` | 마지막 로그인 시간 | 2023-03-22 오전 10:00
`oauth_type` | `varchar` | 회원가입 경로 타입 | local | `local`, `google`, `naver`, `twitter`, `kakao`, `github`, `facebook` (enum 방식)
`login_ip` | `varchar` | 접속 IP | 192.168.0.1
`device_name` | `varchar` | 기기 | Android-{모델명} | 기기 등록해야 쓸수있음. 등록되지 않는 기기는 사용불가능. 최대 5개 등록 가능 (보안 요구 사항)
`member_exit_yn` | `boolean` | 회원탈퇴 여부 | n (기본값)

### 게시글 테이블 (Post)
이름 | 타입 | 설명 | 예시 | 비고
--- | --- | --- | --- | ---
`post_id` | `Long` | 글 ID | | `Primary Key`
`content` | `varchar` | 글 내용 | 똥!ㅋㅋ | 최대 2000 글자 제한
`author` | `varchar` | 글 작성자 | 페페 | `Foreign Key (member.email)`
`comment` | `List<Comment>` | 댓글 |
`post_imgs` | `List<Image>` | 게시글 이미지
`like_post_yn` | `boolean` | 관심글 체크여부
`post_update_yn` | `boolean` | 게시글 수정여부 | n (기본값)
`post_write_date` | `Date` | 게시글 작성날짜 | 2023-03-22 오전 10:00
`share_link` | `varchar` | 공유 URL 주소 | {URL}
`like_count` | `int` | 좋아요 수 | 0 | ~~근데 일기장인데 좋아요 숫자가 필요함?~~ 구현만 해놓고 기능 닫을꺼임
``hashtag` | `varchar` | 해시태그 | `#퇴근`, `#똥`, `#직장인` | #포함 해서 저장할지 미포함으로 저장할지 고민 중

### 댓글 테이블 (Comment)
이름 | 타입 | 설명 | 예시 | 비고
--- | --- | --- | --- | ---
`comment_id` | `Long` | 댓글 ID | | `Primary Key`
`post` | `Post` | 게시글 테이블 | | `Foreign Key (post)`
`author` | `varchar` | 댓글 작성자 | 페페 | `Foreign Key (member.email)`
`content` | `varchar` | 댓글 내용 | 똥!ㅋㅋ | 최대 200자 제한
`comment_img_url` | `varchar` | 댓글 이미지 URL | https://localhost:8080/img/commment/{comment_id}-{author}-{uuid}.png | 이미지 사이즈 32x32 (추후 변경)
`comment_write_date` | `Date` | 댓글 작성날짜 | 2023-03-22 오전 10:00
`comment_update_yn` | `boolean` | 댓글 수정여부 | n (기본값)
`comment_like_yn` | `boolean` | 댓글 좋아요여부 | n (기본값) | ~~이것도 솔직히 필요없는 기능인듯~~ 개발해놓고 막음
