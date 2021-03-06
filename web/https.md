#### HTTPS
* HTTPS = Hypertext Transfer Protocol Over Secure Socket Layer
* 보안이 강화된 HTTP

#### HTTPS와 SSL
* SSL(Secure Socket Layer) 프로토콜은 처음에 Netscape사에서 웹서버와 브라우저 사이의 보안을 위해 만들었다.
* 보통 HTTPS와 SSL을 같은 의미로 이해하고 있는 경우가 많다.
* HTTPS는 SSL 프로토콜 위에서 돌아가는 프로토콜이다.

#### SSL과 TLS
* 같은 말이다.
* 처음에 SSL이 네스케이프에 의해 발명되었고 나중에 표준화기구 IETF의 관리로 변경되면서 TLS라는 이름으로 바뀌었다.

#### SSL 디지털 인증서
* SSL 인증서는 서버간의 통신을 제3자가 보증해주는 전자화된 문서다.
* 클아이언트가 서버에 접속한 직후에 서버는 클아이언트에게 이 인증서 정보를 전달한다.
* 클라이언트는 이 인증서 정보가 신뢰할 수 있는 것인지를 검증한 후 다음 절차를 수행한다.
* SSL과 SSL 디지털 인증서를 이용했을 떄의 이점은 아래와 같다.
    * 통신 내용이 공격자에게 노출되는 것을 막을 수 있다.
    * 클아이언트가 접속하려는 서버가 신뢰 할 수 있는 서버인지를 판단 할 수 있다.
    * 통신 내용의 악의적인 변경을 방지할 수 있다.
* 인증서의 핵심 기능
    * 클라이언트가 접속한 서버가 신뢰 할 수 있는 서버임을 보장한다.
    * SSL 통신에 사용할 공개키를 클아이언트에게 제공한다.
    
#### CA
* Certificate authority
* 인증서의 역할은 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지를 보장하는 역할을 한다.
* 브라우저는 공인된 CA 정보를 가지고 있다.

#### 대칭키 방식
* 대칭키를 이용해서 암호화하면 복호화하기 위해 대칭키가 반드시 필요하다.

#### 공개키 방식
* 공개키로 암호화를 하면 비공개키로 복호화 할 수 있고 비공개키로 암호화하면 공개키로 복호화 할 수 있다.

#### SSL 동작과정
1. 브라우저는 SSL로 암호화된 페이지(https://)를 요청한다.
2. 서버는 Public Key를 인증서와 함께 전송한다.
3. 브라우저는 인증서가 자신이 신용있다고 판단한 CA로부터 서명된 것인지, 유효한 날짜인지 등을 확인한다.
4. 브라우저는 Public Key를 사용해서 랜덤 대칭 암호화키와 URL, http 데이터들을 암호화해서 전송한다.
5. 서버는 Private Key를 이용해서 랜덤 대칭 암호화키와 URL, http 데이터들을 복호화 한다.
6. 서버는 요청받은 URL에 대한 응답을 브라우저로부터 받은 랜덤 대칭 암호화키를 이용하여 암호화해서 브라우저로 전송한다.
7. 브라우저 대칭 키를 이용해서 http 데이터와 html문서를 복호화하고 화면에 정보를 뿌려준다.
