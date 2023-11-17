# 환경변수
- COOKIE_SECRET=example
- JWT_SECRET=example
- MYSQL_USERNAME=example
- MYSQL_PASSWORD=example
- MYSQL_DATABASE=example
- MYSQL_HOST=example

# API 명세서 URL

- https://docs.google.com/spreadsheets/d/1KzjRW5NdnXmnmYq0XqWFGtpDWQfL7Oy3_jlX0poeph4/edit?usp=sharing

# ERD URL

![erd](https://velog.velcdn.com/images/wlduq0150/post/1c0e57b8-5427-4e09-831b-32eb3c40b6d5/image.png)

# 더 고민해 보기

1. **암호화 방식**
- 비밀번호를 DB에 저장할 때 Hash를 이용했는데, Hash는 `단방향 암호화`와 `양방향 암호화` 중 어떤 암호화 방식에 해당할까요?
- 답 : Hash는 단방향 암호화 방식입니다.
- 비밀번호를 그냥 저장하지 않고 Hash 한 값을 저장 했을 때의 좋은 점은 무엇인가요?
- 답 : 데이터베이스가 유출되더라도 최악의 경우는 피할 수 있습니다.

2. **인증 방식**
- JWT(Json Web Token)을 이용해 인증 기능을 했는데, 만약 Access Token이 노출되었을 경우 발생할 수 있는 문제점은 무엇일까요?
- 답: Access Token이 유출될 경우 다른 사용자가 내 아이디로 로그인을 한 것처럼 위장할 수 있어 개인정보 유출 및 금전적 피해가 발생할 수 있습니다.
- 해당 문제점을 보완하기 위한 방법으로는 어떤 것이 있을까요?
- 답: Access Token의 만료 기간을 아주 짧게 하고 Refresh Token을 도입해 자주 새로운 토큰을 발급시켜 줍니다.

3. **인증과 인가**
- 인증과 인가가 무엇인지 각각 설명해 주세요.
- 답: 인증은 로그인 등 사용자를 검증하는 행위를 의미하고, 인가는 사용자 인증 후에 어떤 리소스에 접근할 수 있는지를 확인하는 것입니다.
- 과제에서 구현한 Middleware는 인증에 해당하나요? 인가에 해당하나요? 그 이유도 알려주세요.
- 답: 과제에서 구현한 Middleware는 로그인 후에 토큰의 유효성을 검증하는 역할로 인가에 해당합니다.

4. **Http Status Code**
- 과제를 진행하면서 `사용한 Http Status Code`를 모두 나열하고, 각각이 `의미하는 것`과 `어떤 상황에 사용`했는지 작성해 주세요.
- 200 : 요청의 성공을 의미합니다. GET 또는 PATCH, DELETE가 성공적으로 처리됐음을 반환할때 사용했습니다.
- 201 : 요청의 성공과 동시에 새로운 리소스의 생성을 의미합니다. POST를 통해 새로운 리소스가 생성됐을때 사용했습니다.
- 400 : 클라이언트의 요청이 잘못되었음을 의미합니다. 요청 parameter 혹은 body, query가 형식에 맞지 않을 경우에 사용했습니다.
- 401 : 인증되지 않은 요청을 의미합니다. 인증 Middleware에서 인증에 실패했을 경우에 사용했습니다.
- 403 : 접근 권한이 없음을 의미합니다. 인증은 성공했지만 자신의 상품이 아닌 타인의 상품을 수정하거나 삭제할려고 시동한 경우에 사용했습니다.
- 404 : 해당 리소스가 존재하지 않음을 의미합니다. 상품 조회에 실패했을 경우에 사용했습니다.

5. **리팩토링**
- MongoDB, Mongoose를 이용해 구현되었던 코드를 MySQL, Sequelize로 변경하면서, 많은 코드 변경이 있었나요? 주로 어떤 코드에서 변경이 있었나요?
- 답 : 대부분은 모델을 통한 메서드에 변화가 있었습니다. ex) find -> findAll 또한, 메서드에 매개변수로 조건을 넣는 것과 특정 컬럼만 불러오는 등도 거의 달라서 수정한 부분이 있었습니다.
- 만약 이렇게 DB를 변경하는 경우가 또 발생했을 때, 코드 변경을 보다 쉽게 하려면 어떻게 코드를 작성하면 좋을 지 생각나는 방식이 있나요? 있다면 작성해 주세요.
- 답 : 테이블 마다 관리하는 모듈을 따로 생성해서 추후에 데이터베이스를 변경하더라도 라우터들을 일일히 수정하는 것이 아닌 해당 모듈만 수정해주면 될 것입니다.

6. **서버 장애 복구**
- 현재는 PM2를 이용해 Express 서버의 구동이 종료 되었을 때에 Express 서버를 재실행 시켜 장애를 복구하고 있습니다. 만약 단순히 Express 서버가 종료 된 것이 아니라, AWS EC2 인스턴스(VM, 서버 컴퓨터)가 재시작 된다면, Express 서버는 재실행되지 않을 겁니다. AWS EC2 인스턴스가 재시작 된 후에도 자동으로 Express 서버를 실행할 수 있게 하려면 어떤 조치를 취해야 할까요?
(Hint: PM2에서 제공하는 기능 중 하나입니다.)
- 답 : pm2 startup / save 기능을 활용합니다.

7. **개발 환경**
- nodemon은 어떤 역할을 하는 패키지이며, 사용했을 때 어떤 점이 달라졌나요?
- 답 : 개발 도중 코드에 변화가 생기면 서버를 재실행해주는 패키지로, 메번 서버를 재실행하는 번거로운 작업을 줄여주었습니다.
- npm을 이용해서 패키지를 설치하는 방법은 크게 일반, 글로벌(`--global, -g`), 개발용(`--save-dev, -D`)으로 3가지가 있습니다. 각각의 차이점을 설명하고, nodemon은 어떤 옵션으로 설치해야 될까요?
- 답 : 일반은 해당 프로젝트에, 글로벌은 시스템상에 모든 프로젝트에, 개발용은 해당 프로젝트에 devDependencies에 설치됩니다. nodemon은 개발환경에서 작업을 도와주는 패키지로 개발용으로 설치하는 것이 적절합니다.
