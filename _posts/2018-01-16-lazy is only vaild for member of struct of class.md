### lazy is only vaild for member of struct of class



swift로 개발중에 메모리 낭비 방지를 위해 테이블의 이미지를 늦게 출력하게 하려고 lazy 키워드를 사용하려고 했다

하지만 에러가 났다

```
lazy is only vaild for member of struct of class
```

검색해보니 해당 lazy키워드는 func안에 넣을 수 없고 클래스 안에 글로벌하게만 쓸수 있다..

