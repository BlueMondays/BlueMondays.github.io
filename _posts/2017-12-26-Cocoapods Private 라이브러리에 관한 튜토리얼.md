# Cocoapods 이란?
라이브러리 의존성 관리 매니저입니다. 
최근에는 수많은 xcode 프로젝트 라이브러리들이 cocoapods으로 관리되어집니다. 
사용할 라이브러리 목록을 Podfile에 저장해 두면 `pod install` or `pod update`로 신세계를 경험하게 됩니다!

현재 할리퀸만화 앱도 cocoapod을 이용하고 있습니다! 
안드로이드와 비교하자면 gradle과 같은 역할을 맡고 있습니다.
공홈 짜잔 ~ [https://cocoapods.org/]



# Cocoapods private 라이브러리를 사용해볼까요?

## cocoapods 설치
터미널에 아래와 같은 명령어를 입력합니다.
cocoapods을 먼저 설치하는 명령어입니다.
```javascript
sudo gem install cocoapods
```

## Podfile 설정
먼저 코코아팟을 적용할 프로젝트를 하나 만들고, 터미널로 해당 프로젝트를 타고 들어갑니다.
그리고 `pod init`이라는 매직 키워드를 입력해줍니다.
(사실 공홈에서는 Podfile을 하나 만든 후, 내용을 복붙하라고 하는데 저는 이게 더 깔끔한 것 같아요) 

그러면 이제 코코아팟을 사용할 수 있도록 기본 디렉토리 셋팅 및 프로젝트 설정을 셋팅해줍니다.
그리고 프로젝트를 닫고, 해당 프로젝트 타고 들어가면 못보던 `PROJECT.workspace` 가 하나 생기는데요.
이제 코코아팟을 사용하기 때문에 개발할때도 `.workspace` 파일로 오픈해야합니다.
왜냐면 빌드할 때, 코코아 팟으로 사용하는 외부 라이브러리도 함께 빌드하기 때문이죠!

```javascript 
TIP 
- workspace : 1개 이상의 프로젝트를 한꺼번에 개발할 때 사용
- project : 1개의 프로젝트
```

그리고 Podfile을 수정합니다.
저는 써보고싶던 hero를..
```javascript 
target 'testProduct' do
  # Comment the next line if you're not using Swift and don't want to use dynamic frameworks
  use_frameworks!

  # Pods for testProduct
  pod 'Hero'
end
```

그리고 터미널에서 `pod init`을 입력해줍니다.

```javascript 
TIP 
- pod init : 맨 처음 프로젝트에서 팟 파일을 설치시 사용
- pod update : 팟 파일을 최초 설치후, 업데이트나 추가 라이브러리를 설치할 때 사용
```

끝입니다!! 참 쉽죠?



# 이제 Cocoapods Private 라이브러리를 만들어볼까요?

## git Project 생성
저희 회사는 Yona를 사용하고있으니 요나로 해볼게요~
일단 요나에서 프로젝트를 하나 만들어줍니다.

깃을 사용하셔도 무방하고, bitbucket을 사용하여도 무방합니다.

저는 http://yobi.회사도메인.com/jiyeonpark/JYHelloTestLib 이렇게 만들었습니다.

그리고 이거와 별개로 맥에서 라이브러리를 만들 위치로 가서 
```javascript 
pod lib create [Name] 
```
이렇게 매직 키워드를 치면 기본 셋팅에 필요한 옵션들을 체크해줍니다.
- 언어선택 : Swift
- 데모 앱 생성 여부 : Yes
  : 나중에 테스트 데모앱 자체에서 테스트 가능하니 원하시는 옵션 선택하세요.
- 테스트 프레임워크 사용 여부 : None
- View based 테스팅 여부(single view) : No

알아서 셋팅이 되고 빠밤 xcode가 실행됩니다.

아래 네비게이션에서 우리가 건드리는 부분은 
![118-201712-26-1529-56.png](/images/118-201712-26-1529-56.png) 

* [Name].podspec
* Podfile
* /Classes/*

입니다.

우선 설명을 하자면 
* [Name].podspec 
  : 오픈소스 연동과 관련된 정보들
* Podfile
  : 데모 앱에서 테스트 하거나, 외부 라이브러리를 사용하여 또다른 라이브러리를 만들때 사용
* /Classes/* 
  : 이제 여기다가 원하는 라이브러리 파일 및 클래스를 추가합니다..!! 

## .podspec 수정
```javascript
#
# Be sure to run `pod lib lint JYHelloTestLib.podspec' to ensure this is a
# valid spec before submitting.
#
# Any lines starting with a # are optional, but their use is encouraged
# To learn more about a Podspec see http://guides.cocoapods.org/syntax/podspec.html
#

Pod::Spec.new do |s|
  s.name             = 'JYHelloTestLib'
  s.version          = '0.1.0'
  s.summary          = '짧은 설명을 입력해줍니다. description보다 짧아야해요.'

# This description is used to generate tags and improve search results.
#   * Think: What does it do? Why did you write it? What is the focus?
#   * Try to keep it short, snappy and to the point.
#   * Write the description between the DESC delimiters below.
#   * Finally, don't worry about the indent, CocoaPods strips it!

  s.description      = '긴 설명을 입력해줍니다. summary보다 길어야해요.'

  s.homepage         = 'http://jiyeonpark@yobi.회사도메인.com/jiyeonpark/JYHelloTestLib'
  # s.screenshots     = 'www.example.com/screenshots_1', 'www.example.com/screenshots_2'
  s.license          = { :type => 'MIT', :file => 'LICENSE' }
  s.author           = { 'jiyeonpark.1001@gmail.com' => 'jiyeonpark@회사도메인.com' }
  s.source           = { :git => '요비 주소를 입력해주세요', :tag => s.version.to_s }
  # s.social_media_url = 'https://twitter.com/<TWITTER_USERNAME>'

  s.ios.deployment_target = '8.0'

  s.source_files = 'JYHelloTestLib/Classes/**/*'
  
  # s.resource_bundles = {
  #   'JYHelloTestLib' => ['JYHelloTestLib/Assets/*.png']
  # }

  # s.public_header_files = 'Pod/Classes/**/*.h'
  # s.frameworks = 'UIKit', 'MapKit'
  # s.dependency 'AFNetworking', '~> 2.3'
end
```

그리고 올바르게 정보가 입력이 되었는지 검증합니다. 
`pod lib lint`

뽜봠!!! 이제 검증이 완료 되었다면 pod private repository에 추가합니다.
```javascript
pod repo add [Name] [git주소]
```

-> 여기서 궁금한것 !
git repository 와 cocoapod repository는 다른건가요


### git Repository와 cocoapods Repository (2018-01-09 업데이트)
```javascript
우선 repository라는 것은 다들 알고계시듯이 '저장소'라는 뜻입니다.

git repositry 줄여서 git repo는 소스를 저장하는 저장소입니다.
여기서 repo는 local과 remote로 구분되는데요.
이때 local는 말 그대로 로컬 머신에 있는 저장소입니다.
이것을 git에 push하기 전까지 로컬머신에만 존재하고 원격 저장소에는 반영이 안된 상태입니다.
이것을 커맨드라인으로는 `git push origin [브랜치 이름]` 로 원격 저장소(서버 저장소)에 올린다면 해당 소스는 다른 사람도 땡겨서 볼 수 있는 소스가 됩니다.

cocoapods repo라는 것도 동일한 개념입니다.
git에 push하기 전까지는 `~/.cocoapods/[라이브러리 이름]` 에 로컬별로 버전이 관리되고 있습니다.
여기서 해당 원격 저장소로 버전을 배포한다고 소스에 태그를 달고, 푸시를 하면 해당 라이브러리에 버전이 배포된 것입니다.
cocoapods repo는 단지 라이브러리 버전별 관리를 위해 .podspec 파일만을 담고있을 뿐입니다!
```

그리고 git에 소스를 푸시 할게요.
이것 역시 커맨드 라인으로 합니다.

```javascript
 git add *
 git commit -m “message”
 git remote add origin [git주소]
 git push origin master
```

짜잔 그럼 모든 소스가 깃의 원격 저장소로 이동했습니다.
그럼 기존에 있던 프로젝트를 삭제한 후 원격 저장소에서 소스트리를 이용해서 소스를 내려받습니다.

그러면 이제 private 라이브러리 개발 준비는 끝났습니다!!!!



## 제대로 되었는지 테스트 코드를 작성해볼까요?

/Classes 하위에 파일 하나를 만들고 테스트 코드를 작성합니다.
**(중요!!!) 이때 주의할 점은 public 접근제한자를 붙여 주어야 한다는 것입니다.**
안그러면 이 라이브러리를 사용하는 프로젝트에서 해당 class를 사용할 수 없습니다.

```javascript
import Foundation

public class HelloTest: NSObject {
	public func hello() {
		print("hello im JYHelloTestLib!!!!!!!!!!!")
	}
}

```

테스트 코드 작성후 cocoapod의 버전을 올려주어야겠지요!
.podspec 파일의 version을 변경하여줍니다.
```javascript
  s.version          = '0.7.0'
```

그리고 깃에 태그를 매겨줍니다.
코코아팟은 해당 라이브러리의 버전별로도 관리가 가능한데, 이것을 태그값으로 구분합니다.
![494-201712-26-1554-38.png](/images/494-201712-26-1554-38.png)

그리고 이제 버전정보를 담은 podspec 파일을 cocoapods private repository에 올려줍니다. 
```javascript
pod repo push [Name] [Name].podspec
```

이제 해당 잘 올라갔는지 확인해볼게요
~/.cocoapods/repos/[Name] 하위로 들어가보면 해당 버전별로 올라가있습니다.

## 이제 다른 프로젝트에서 내가 만든 private 라이브러리를 사용해봅시다.
다른 프로젝트의 Podfile을 열어줍니다.

```javascript
pod 'JYHelloTestLib', '~> 1.0.7', :git => 'http://jiyeonpark@yobi.회사도메인.com/jiyeonpark/JYHelloTestLib'
```

### 2018-1-9 추가한 내용
해당 주소만 있다면 팟 인스톨이 가능할 수 있습니다.
그렇다면 '프로젝트 멤버만 코드 및 관련 메뉴에 접근 가능' 옵션을 '예'로 변경 후
아래와 같이 작성하면 프로젝트 멤버로 들어간 사용자만 해당 팟을 설치할 수 있습니다!!!!!!!!!

```javascript
source 'https://github.com/CocoaPods/Specs.git'
...
pod '팟이름', :git => 'https://아이디:패스워드@깃허브주소.git'
pod 'JYHelloTestLib', '~> 1.0.7', :git => 'http://jiyeonpark:내비밀번호@yobi.회사도메인.com/jiyeonpark/JYHelloTestLib'
```


이렇게 작성해 줍니다.
private 라이브러리기때문에 해당 저장소가 어디있는지 명시를 해주어야합니다.
만약 버전을 기입하지 않을 경우 가장 최신버전으로 설치됩니다.

```javascript
Tip
pod repo remove [name]
```



# 마무리하며....

따흐흑 이 삽질을 거의 일주일동안했는데 너무 힘드네요..