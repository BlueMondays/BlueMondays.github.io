## iOS9 App Transport Security 설정



iOS9 보안정책 변경으로 웹뷰를 사용할때, 다음과 같은 오류를 뿌려주며 이미지가 제대로 로딩이 안됬었다.

```
App Transport Security has blocked a cleartext HTTP resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.
```



Info.plist를 가서 뭐 설정을 하라는데..

구글링 결과 **ATS(App Transport Security)**가 문제였다.

ATS는 응용프로그램과 웹 서비스간의 안전한 연결을 위해 사용한 것으로, 활성화되면 HTTP를 통해 통신을 할 수 없다. 

그러므로 웹뷰를 정상적으로 사용하길 원한다면 info.plist에 예외설정을 해야한다.

아래는 [덴타럴님의 블로그(http://blowmj.tistory.com/entry/iOS-iOS9-App-Transport-Security-%EC%84%A4%EC%A0%95%EB%B2%95)](http://blowmj.tistory.com/entry/iOS-iOS9-App-Transport-Security-%EC%84%A4%EC%A0%95%EB%B2%95)에 쓰여있는 글이다!

**1. 전체의 HTTP를 허용하는 방법(비추천이라고 합니다)**

```
<key> NSAppTransportSecurity </ key>
<dict>
    <key> NSAllowsArbitraryLoads </ key>
    <true />
</ dict>
```

****

![img](http://cfile28.uf.tistory.com/image/260DD642567266C315B38D)

**2. ATS를 제외시킬 도메인을 Info.plist에 기재하는 방법**

```
<key> NSAppTransportSecurity </ key>
<dict>
    <key> NSExceptionDomains </ key>
    <dict>
        <key> www.xxx.com </ key>
        <dict>
            <key> NSTemporaryExceptionAllowsInsecureHTTPLoads </ key>
            <true />
        </ dict>
    </ dict>
</ dict>
```

![img](http://cfile21.uf.tistory.com/image/223B2446567268441E49A3)

몇가지 더 설정을 알아 보겠습니다.

- `NSExceptionMinimumTLSVersion`: TLS 최소 버전을 문자열로 입력합니다. 아래 값들 중 하나를 넣을 수 있거나 생략할 수 있습니다.

- - `TLSv1.0`
  - `TLSv1.1`
  - `TLSv1.2` (생략할 경우의 기본값)

- `NSExceptionRequiresForwardSecrecy`: forward secrecy 라는 비밀키 암호화와 관련된 설정입니다.

- `NSExceptionAllowsInsecureHTTPLoads`: HTTPS(SSL) 연결이 아니더라도 통신을 허용할 것인가를 YES 혹은 NO로 설정 할 수 있습니다.

- `NSIncludesSubdomains`: 이 사이트의 하부도메인들에도 이 설정을 적용할 것인가를 YES 혹은 NO로 설정 할 수 있습니다.

- `NSThirdPartyExceptionMinimumTLSVersion`: 써드파티 TLS 버전을 입력 할 수 있습니다.

- `NSThirdPartyExceptionRequiresForwardSecrecy`: 역시 써드파티 Forward Secrecy 설정할 수 있습니다.

- `NSThirdPartyExceptionAllowsInsecureHTTPLoads`: 역시나 써드파티 HTTPS 연결 강제를 설정합니다.

------

 iOS 입문자로서 info.plist 설정 팁? 을 주자면

info.plist 파일을 클릭하면 해당 화면처럼 나오는데 구글링을 하면 다 태그형식으로 되어있다..

예를 들어 <key>NSAppTransportSecurity**</key> **이런식으로..

아니 이걸 어떻게 하란 소리지?!

![img](http://cfile21.uf.tistory.com/image/232CD53557BBBB4709C00A)

그럴때는 source code로 보기를 하면 된다!

![img](http://cfile6.uf.tistory.com/image/2162353357BBBC6224AD03)

쨔쨘!!!!

![img](http://cfile30.uf.tistory.com/image/277A403A57BBBC8B21F5BE)

