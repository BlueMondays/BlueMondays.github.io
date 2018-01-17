# QA 프로세스 정리
저희 회사는 하나의 앱을 QA팀에게 넘기기 위해서 많은 공수가 들어가고 있습니다.
testflight를 사용하지 않고 웹서버로 올려 다운로드 하도록 하고 있는데요.

1. .plist 파일 수정후 업로드
2. archive한 .ipa 파일 추가
3. .html 파일 수정
4. 수정 내역 메일링

QA가 한번으로 끝날수도 있으나, 디바이스를 추가해달라는 요청, 추가 수정사항이 있을 때는 위와 같은 프로세스를 한번 더 반복해야 되는 상황이 있습니다.
그러므로 이 공수를 좀 줄이기 위해 우선적으로 QA 프로세스 자동화를 목적으로 R&D 를 진행하였습니다.

# 튜토리얼
## fastlane 설치
우선적으로 fastlane을 설치를 진행합니다.

Xcode 커맨드라인툴을 설치 후, fastlane gem을 설치합니다.
기본적으로 fastlane은 커맨드라인툴이기 때문에 거의 모든 작업은 터미널을 통해 이루어집니다.
```javascript
xcode-select --install
sudo gem install fastlane -NV
```

설치가 완료 되었다면 fastlane을 적용하고자 하는 프로젝트 폴더로 디렉토리를 변경하여 이니셜라이징을 합니다.
```javascript
fastlane init
```

그럼 해당 프로젝트 폴더에 fastlane을 사용할 준비가 완료되었습니다!
![304-20181-11-956-49.png](./assets/img/304-20181-11-956-49.png) 
![964-20181-11-957-3.png](./assets/img/964-20181-11-957-3.png) 

지금 제가 사용하고자 하는 앱은 출시가 이미 된 앱이기 때문에, itunesconnect에 등록해 놓은 스크린샷과 정보들이 모두 다운로드 되어집니다.
(fastlane은 앱 출시 과정또한 자동화가 될 수 있기 때문이죠.)

파일별 설명은 다음과 같습니다.
- Appfile : lane을 빠르게 수행하기 위해 앱에 대한 정보를 담고 있습니다. 번들 아이디, 애플 아이디, 팀 아이디 등등..
      만약 아이튠즈커넥트와 아이디가 다르다거나, 베타 버전과 출시 버전의 번들 아이디가 다르다거나 하는 특수한 상황에서만 수정을 하고, 그 이외에는 수정하지 않습니다.
      (https://docs.fastlane.tools/advanced/#appfile 을 통해 해당 파일에 대한 관련 정보를 확인하세요.)
- Deliverfile : 아이튠즈 커넥트에 스크린샷을 올리거나, 메타데이터 등을 업로드 할때 필요한 계정정보를 담고있습니다.
- Fastfile : 가장 많은 수정을 하게 될 파일입니다. 
         custom으로 lane이름을 만들고, 앱이 lane을 수행하기 전에 해야 할 일, 수행하고 난 후에 할 일등을 정의합니다.
     ​    
## fastfile 수정
기본적으로 default로 fastfile이 작성이 되있습니다.
그럼 우선 default로 작성된 lane을 수행해볼까요?
```javascript
fastlane release or fastlane ios release
```
이렇게 터미널에 치기만 하면 작성된 플로우 대로 실행이 됩니다.

코드를 작성하기 전에 환경변수를 이용하기 위해 추가로 gem을 설치해보도록 하겠습니다.
ruby에서는 dotenv라는 gem을 이용하여 환경변수를 관리합니다.
```javascript
gem install dotenv
```

그리고 .env 파일을 생성합니다.
```javascript
touch .env
```

그리고 .env 파일에 환경변수를 추가하기 위해서는 .env 파일을 열고 수정합니다.
.env 파일에서는 여러 데이터 타입을 지원하지 않고 오직 String 타입만을 지원합니다.

작성할 때는 이렇게
```javascript
BUNDLE_VERSION = "1.1.1"
QA_VERSION = "1"

FIX_LIST = "지,연,짱"
NEW_LIST = "새,로,운,것"
```
사용할 때는 이렇게!
```javascript
ENV[BUNDLE_VERSION]
```

```javascript
touch .env
```

이제 위에서 작성한 QA 플로우 대로 lane을 정의해보도록 하겠습니다.

```javascript
  .env 파일
	# 번들 버전
	BUNDLE_VERSION = "1.1.1"

	# QA를 진행한다면 몇차 QA 인지를 지정
	QA_VERSION = "1"

	# 수정 사항과 새로운 사항을 ,로 구분하여 String으로 작성합니다.
	FIX_LIST = "지,연,짱"
	NEW_LIST = "새,로,운,것"

	# 다운로드 센터에 올리기 위한 계정정보, 도메인
	CONTENT_SERVER_DOMAIN_NAME = #서버 도메인
	CONTENT_SERVER_FTP_LOGIN = #로그인 아이디
	CONTENT_SERVER_FTP_PASSWORD = #로그인 패스워드


  Fastfile.rb

  # html 파일을 수정하기 위한 gem
  require 'nokogiri'
  require 'open-uri'
  # 디렉토리를 한꺼번에 삭제하기위한 gem
  require 'fileutils' 
  require 'mailgun'

  lane :QA do

  	# 아카이브 전에 가장 최신의 provisioning profile을 받습니다.
	sigh(adhoc: true, force: false)
	
	# 그리고 QA를 진행할 때 네이밍을 1.1.1.beta1 이런식으로 수정하기 때문에 조합하여 네이밍을 환경변수에 저장합니다.
	ENV["QA_NAME"] = ENV["BUNDLE_VERSION"] + ".beta" + ENV["QA_VERSION"]
	ipaName = ENV["QA_NAME"] + ".ipa"
	plistName = ENV["QA_NAME"] + ".plist"
	
	# ipa파일 .plist파일을 저장하기 위한 폴더를 하나 만듭니다. 
	# 모든 과정이 완료 되었을 때, 해당 폴더를 한꺼번에 삭제하기 위함입니다.
	directory_name = "fastlane_output"
	Dir.mkdir(directory_name) unless File.exists?(directory_name)
	
	# 프로젝트를 arcive합니다.
	# .ipa파일을 아까 만든 디렉토리에 저장합니다.
	 gym( workspace: "워크스페이스.xcworkspace",
 		 scheme: #스킴,
 		 export_method: "ad-hoc",
 		 silent: true,
 		 clean: true,
 		 include_bitcode: true,
 		 include_symbols: false,
 		 output_directory: Dir.pwd + "/" + directory_name , # output 디렉토리
 		 output_name: ipaName #네이밍
 	  )
 	
	# 해당 ipa파일을 object로 가져옵니다.
	ipaFile = File.new(Dir.pwd + "/" + directory_name + "/" + ipaName)
	
	# .plist 파일을 생성합니다.
	data = {'items' => [{
				'assets' => [{
					'kind' => 'software-package',
					'url' => 'https://웹서버도메인/appdist/build/ios-hq/' + ipaName
				}],
				'metadata' => {
					'bundle-identifier' => 번들아이디,
					'bundle-version' => ENV["BUNDLE_VERSION"],
					'kind' => 'software',
					'title' => 앱이름
				}
	}]}
	
	# 아까 만든 디렉토리(fastlane_output)로 워킹 디렉토리 변경합니다.
	Dir.chdir directory_name
	
	# .plist 파일을 워킹 디렉토리에 저장합니다.
	plist = CFPropertyList::List.new
	plist.value = CFPropertyList.guess(data)
	plist.save(plistName, CFPropertyList::List::FORMAT_BINARY)
	
	# 해당 plist파일을 object로 가져옵니다.
	plistfile = File.new(plistName)
	
	# .ipa 파일과 .plist을 다운로드 센터의 build/ios-hq에 올립니다.
	Net::FTP.open(ENV["CONTENT_SERVER_DOMAIN_NAME"], ENV["CONTENT_SERVER_FTP_LOGIN"], ENV["CONTENT_SERVER_FTP_PASSWORD"]) do |ftp|
	  ftp.chdir('build/ios-hq')
	  ftp.putbinaryfile(ipaFile)
	  ftp.putbinaryfile(plistfile)
	  ftp.close
	end
	
	# html 파일을 수정하기 위해 파일을 서버에서 가져옵니다.
	Net::FTP.open(ENV["CONTENT_SERVER_DOMAIN_NAME"], ENV["CONTENT_SERVER_FTP_LOGIN"], ENV["CONTENT_SERVER_FTP_PASSWORD"]) do |ftp|
	  ftp.getbinaryfile(수정코자하는 html 파일)
	  ftp.close
	end
	
	# 추가할 html을 정의합니다.
	addHtml = "
				<section class='section-version'>
					<h3>Version #{ENV["BUNDLE_VERSION"]} Beta#{ENV["QA_VERSION"]} (#{Date.today.to_s})</h3>
					<div class='section-install'>
						<a href='itms-services://?action=download-manifest&url=다운로드 url/#{plistName}'>
						<span class='label install'>Install Now!</span>
						</a>
					</div>
					
	" 
	

	if ENV["QA_VERSION"] = "1" 
	 # 만약에 첫번째 QA 버전이라면 수정사항을 추가합니다.
	
		addHtml += "
				<ul>
						
		"
		
		
		ENV["NEW_LIST"].split(',').each do |list|		
			addHtml += "
								<li><span class='label new'>New!</span>#{list}	</li>
			"
		end
		
		ENV["FIX_LIST"].split(',').each do |list|		
			addHtml += "
								<li><span class='label fix'>Fixed</span>#{list}	</li>
			"
		end
		
		addHtml += "
						
					</ul>
				</section>
		"
	elsif
	
	 #나머지는 수정내역을 추가하지 않습니다.
	  addHtml += "
				</section>
		"
	end
	
	
	# 수정된 html 파일을 object로 저장합니다.
	mainHtml = Nokogiri::HTML.parse(File.new(Dir.pwd + "/html 파일"))
	
	# 해당 다운로드 리스트 div를 찾아서 최 상단에 html을 추가합니다.
	div = mainHtml.at('.downloadList')
	div.first_element_child.before(addHtml)
	File.write("html 파일", mainHtml.to_html)
	
	uploadHtml = File.new(Dir.pwd + "/html 파일")
	
	# 서버에 html 파일을 올립니다.
	Net::FTP.open(ENV["CONTENT_SERVER_DOMAIN_NAME"], ENV["CONTENT_SERVER_FTP_LOGIN"], ENV["CONTENT_SERVER_FTP_PASSWORD"]) do |ftp|
	  ftp.putbinaryfile(uploadHtml)
	  ftp.close
	end
	
	# fastlane-output 디렉토리를 삭제합니다.
	FileUtils.rm_r Dir.pwd

	# 다운로드 센터에 파일이 올라갔다는 것을 알리기 위해 메일링 합니다. mailgun을 이용합니다.
	mg_client = Mailgun::Client.new '메일건 키'

	# 메일 내용은 새로운 기능과 수정된 기능을 포함합니다.
	text = ""
	ENV["NEW_LIST"].split(',').each do |list|		
		text += "
		- 새로운 기능! " + list
	end
		
	ENV["FIX_LIST"].split(',').each do |list|		
		text += "
		- 수정된 기능!" + list
	end
	
	message_params =  { from: 'postmaster@sandbox메일건키.mailgun.org',
						to:   '받는 사람 이메일',
						subject: "iOS 앱이 #{ENV["BUNDLE_VERSION"]} Beta #{ENV["QA_VERSION"]} 가 다운로드 센터에 업로드 되었습니다.",
						text: "
iOS 앱이 #{ENV["BUNDLE_VERSION"]} Beta #{ENV["QA_VERSION"]} 가 다운로드 센터에 업로드 되었습니다. 
아래 주소로 다운로드 센터로 이동하여 다운로드 해주세요.

#{text}
						
https://다운로드 url
						"
					  }

	# Send your message through the client
	mg_client.send_message '메일건 도메인', message_params
  
	# 모든 프로세스가 완료되었습니다!
  end
```

![444-20181-11-1051-59.png](./assets/img/444-20181-11-1051-59.png) 

## 모든 프로세스 완료!
이제 저 lane을 작성해 두었으니,
팀원들은 .env 파일의 번들 버전, QA 버전, 수정사항, 새로운사항을 수정 후, `fastlane QA` 만 입력하면 QA를 위한 프로세스가 완료될 것입니다.
QA 요청 메일은 구체적인 설명이 필요하므로 이 과정에 추가하지 않았습니다.
