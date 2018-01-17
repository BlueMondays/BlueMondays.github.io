## 1. 새로운 프로젝트 생성

![img](http://cfile29.uf.tistory.com/image/990B894A5A3B43F016D7BC)

![img](http://cfile27.uf.tistory.com/image/994F194A5A3B43F00351BD)

![img](http://cfile26.uf.tistory.com/image/998EB6395A3B44530B436F)

아 저  .a 파일은 빌드를 진행하면 생성된다!

저건 처음에 없는게 정상입니당

 이건 밑에 설명!

자 이렇게 트리가 만들어졌으면 이제 테스트 코드를 넣어보자



## 2. 테스트 코드작성

staticLibraryTest2.h

![img](http://cfile4.uf.tistory.com/image/99554A3C5A3B44A03C65F1)

staticLibraryTest2.m

![img](http://cfile22.uf.tistory.com/image/99770E3C5A3B44A0362A9D)

자 이렇게 작성을 하고 run을 해보자...

이건 뭐 시뮬레이터랑 실제 디바이스랑 달라서 한가지로 통합해 줘야 한다는데 이건 차후에!

그럼 위에서 봤던 .a 파일이 생성되었을 것이다

그러면 이제 파인더에서 봐볼까?



## 3. 다른 프로젝트에 적용

![img](http://cfile29.uf.tistory.com/image/992A91375A3B458412021D)

다른프로젝트에서 사용하려면 저 include 폴더와 .a 파일 두개다 필요하다.

적용할 새로운 프로젝트에 저 두 파일을 드래그해서 넣어준다. 이런식으로!

![img](http://cfile21.uf.tistory.com/image/99EF873E5A3B45D91D45FE)

나는 Swift를 사용하는 프로젝트에 사용할꺼기 때문에 옵씨를 같이 사용할수 있게 해주는 브릿징 헤더를 만들어준다.

![img](http://cfile3.uf.tistory.com/image/99AD84345A3B462C10A8BD)



그리고 테스트 코드

```swift
override func viewDidLoad() {

        super.viewDidLoad()

        initUi()

		staticLibraryTest2.sayHello()
    }

```