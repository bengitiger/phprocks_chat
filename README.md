phprocks_chat
=============

PHPROCKS.COM CHAT Ver 0.2
/************************************************************
 *                                                          *
 * PHPROCKS.COM 채팅 서버, 클라이언트  Version 0.2          *
 * Copyright(C) 2014, 만든이 이용교(LEE, YONGGYO)           *
 * 이 소스는 GPL 라이센스로 배포됩니다.                     *
 * 수정 및 상업적인 용도로 사용하셔도 무방하오나,           *
 * 출처 및 만든이 관련 주석은 제거하지 마십시오.            *
 * 만든이 이메일 - lyonggyo@gmail.com                       *
 *                                                          *
 * 사용법                                                   *
 * 아래의 주소를 참고 하세요.                               *
 *  http://phprocks.com/?module=post&action=view&seq=428    *
 *                                                          *
 *                                                          *
 ************************************************************
 */


PHPROCKS 채팅 Ver 0.2 

1. 특징 소개 

  기존의 채팅 소스에서 귓속말 기능 및 관리기능을 추가 하였습니다. 

  관리기능이라고 해서 특별한 것은 없고 지난 대화 기록 조회, 삭제, 관리자 추가 정도만 있습니다. 

  관리기능이 추가되어 데이터 베이스(mysql) 연결이 있어야 정상 작동 합니다. 


  

2. 설치 방법 

    A. 소스 코드를 다운로드 받습니다. 
    github - git clone https://github.com/yonggyo00/phprocks_chat 
  
    github 사용이 여의치 않을 경우 여기에 첨부된 소스를 다운로드 하세요. 
  
    B. 필수 모듈을 설치 합니다. 
        node.js 모듈을 말합니다. 
        node.js가 설치 되어 있지 않다면, http://www.nodejs.org에서 다운 받아 설치 하세요. 

        forever 모듈을 제외한 모든 모듈은 package.json에 이미 기입 되어 있습니다. 따라서 아래의 방법 
        으로 필수 모듈을 설치 하세요. 
        npm install 
        npm install -g forever 
  
    C. 사용할 데이터 베이스를 생성하고 chat.sql 스키마 파일을 포팅 합니다. 
        기본적으로 데이터 베이스는 chat으로 되어 있습니다. 만약 다른 데이터베이스 이름으로 생성한다면, 
        server.js와 admin/database.php의 database 정보를 변경하세요. 
        
        # CREATE DATABASE chat CHARATER SET UTF8 COLLATE UTF8_GENERAL_CI; 
        # exit; 
        $ mysql -u사용자명 -p비밀번호 chat < chat.sql 
      
  
    D. server.js의 파일에서 db 연결 정보와 채팅 서버 포트, 주소를 변경합니다. 
        var connection = mysql.createConnection({ 
            host: '127.0.0.1',  // DB 주소 
            user : 'user',    // 데이터 베이스 사용자 
            password : 'password',  // 데이터 베이스 비번 
            insecureAuth: true 
        }); 
  
          // 만약 데이터베이스가 chat이 아니라면 chat 부분을 사용하시는 데이터 베이스로 변경하세요. 
          connection.query("USE chat");  
    
        var io = require('socket.io')(3000);  // 원하는 포트로 변경합니다. 
      
    E. chat.php / chat_room.php 
        localhost:3000으로 설정 되어 있는 부분을 
        server.js가 실행될 서버의 주소(같은 서버가 될 수도 있습니다.)와 포트로 변경하세요. 
  
    F. admin/database.php 
        define('HOST', 'localhost');  // DB 주소 
        define('USER', 'user');  // 사용자 
        define('PASS', 'password');  // 비밀번호 
        define('DB', 'chat');  // 데이터 베이스 
      
  
    G. 설정이 완료되면 server.js를 forever 모듈을 사용하여 실행 합니다. 
        forever 모듈을 사용하는 이유는 server.js가 어떤 에러에 의해 다운이 되더라도 계속 재 실행 되도록 하기 위해서 입니다. 
        forever start server.js 
  



  
3. 각 홈페이지의 사용자와 연동 방법. 

    클라이언트 스크립트 chat.php와 chat_room.php를 직접 수정하여 커스터마이징 하는 경우 
    $in['username']과 $in['nickname']을 변경 해 주시면 됩니다. 
    그누보드의 경우 $_member 배열이나 $_SESSION에서 로그인 정보를 가져올 수 있으므로 그 부분으로 변경하시면 됩니다. 
  
    직접 수정 하지 않고 로그인 사용자 연동하는 경우 iframe을 사용하는 경우가 가장 좋을 듯 합니다. 
    이런 경우 다음과 같이 쿼리 스트링 형태로 로그인 사용자 정보를 넘겨 주면 됩니다. 
      서버주소/chat.php?username=아이디&nickname=닉네임 




4. 초기 관리자 계정 
    chat.sql에는 관리자 계정 및 비밀번호가 설정 되어 있습니다. 

    모든 설치 완료후 관리자화면  접속시 admin/123456 을 사용하셔서 접속하시면 됩니다. 

    비밀번호 변경은 관리자 페이지에서 하실 수 있습니다. 

  

5.  데모 사이트 
 채팅 사이트  http://syboard.phprocks.com/chat/chat.php 
 채팅 관리자 http://syboard.phprocks.com/chat/admin 
  
 관리자 테스트 계정 
 admin / 123456 

 좀더 자세한 설명은  http://phprocks.com/?module=post&action=view&seq=428 에서 확인 하시면 됩니다. 



P.S 
데모 사이트의 어드민의 경우 데이터베이스 인코딩에 조금 문제가 있는 듯 합니다. 최신 버전(mysql 5.6 버전에서는 인코딩 깨짐 없이 잘 작동 합니다.) 

회원 로그인 정보(아이디, 닉네임)와 연동하여 활용하는 부분은 http://phprocks.com 에 적용해 놓았습니다. 

다만 이 부분은 로그인 했을 떼 나타나는 상단 탭에 있으므로 이부분 확인을 원하시는 경우 아래의 테스트 계정으로 사이트 접속해 주세요. 

테스트 계정 
아이디 : rockyroad 
비밀번호 : 4394018 

감사합니다.
