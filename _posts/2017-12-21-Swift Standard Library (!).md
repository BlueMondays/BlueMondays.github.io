### Swift Standard Library (!)



# **서론**

점점 많아지는 라이브러리!

어떤 라이브러리를 사용해야 내맘에 쏙 드는 코드를 만들 수 있을까..!!

모든 개발자의 고민이 아닐까..

어찌어찌 동작은 되는데, 개발자 스스로 보기에 만족스럽고 예쁜 코드가 아니라면 보람차지 않다.. (경험상ㅋㅋㅋ)

수많은 라이브러리 속에 대부분 많이 사용하는 라이브러리 5개만 뽑아보고자 한다..!!



## Alamofire

****

![img](http://cfile4.uf.tistory.com/image/9979553359CB587D36B778)

HTTP 네트워킹 라이브러리 입니다.

기본적으로 Apple에도 네트워킹을 위한 메소드와 클래스를 제공하고있습니다.

하지만 사용하기에는 굉장히 복잡하기 그지없습니다.

코드 라인수도 엄청나고, 알아보기도 힘들죠..

Alamofire를 사용하면, 보다 깔끔한 코드 작성이 가능해집니다.



```swift
Alamofire.request("https://httpbin.org/get").responseJSON { response in
  print("Request: \(String(describing: response.request))")   // original url request
  print("Response: \(String(describing: response.response))") // http url response
  print("Result: \(response.result)")                         // response serialization result

  if let json = response.result.value {
      print("JSON: \(json)") // serialized json response
  }

  if let data = response.data, let utf8Text = String(data: data, encoding: .utf8) {
      print("Data: \(utf8Text)") // original server data as UTF8 string
  }
}
```




## [**Realm**](https://realm.io/kr/)

![img](http://cfile6.uf.tistory.com/image/99EC953359CC42A42267B2)

****

Realm은 모바일 환경에 최적화된 데이터 베이스 입니다.

처음에 리얼엠이라고 발음했던 렘 ㅋㅋㅋㅋㅋㅋ

구글링 해보니, realm은 덴마크 왕국(Danish Realm)이란 단어에서 이름을 따왔다고 한다..

오.. 처음알게된 사실!



## **RxSwift**

![img](http://cfile2.uf.tistory.com/image/9978783359CC42E42388CA)

****

Observer패턴, Iterator패턴 그리고 Functional Programming의 장점만 모아모아 만든 라이브러리 입니다.

비동기 작업을 위한 코드를 작성할 때, 쉽게 closure를 다룰 수 있어 가독성을 높이고 골칫거리와 버그를 줄여준다고 합니다!



## SwiftyJson

![img](http://cfile2.uf.tistory.com/image/999C253359CC4311184A59)

****

Swift에서 Json을 손쉽게 다루기 위한 라이브러리입니다.

SwiftyJson을 사용하기 전 코드

```swift
  if let statusesArray = try? JSONSerialization.jsonObject(with: data, options: .allowFragments) as? [[String: Any]],
    let user = statusesArray[0]["user"] as? [String: Any],
    let username = user["name"] as? String {
    // Finally we got the username
}
```



SwiftJson을 사용한 후 코드

```swift
let json = JSON(data: dataFromNetworking)
if let userName = json[0]["user"]["name"].string {
  //Now you got your value
}
```

## [**HERO**](https://github.com/lkzhao/Hero)

****

이건 한번 써보고 싶어서 넣은 라이브러리..

유명하기도 하고!! ㅎㅎ

이건 UI를 위한 라이브러리인데 깃헙에 가면 정말 뿅갈만한 트랜지션을 보여주고 있어서 탐나는 라이브러리다..

꼭 한번 써보고 말테다!

