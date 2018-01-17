앱이 시작되는 과정에서 셋팅해야 되는 부분이 있어서, 스플래시 이미지를 길게 보여줘야 할 때가 있다.

앱이 시작되는 과정에서 셋팅하는 부분은 앱이 동작하고 있어야 하므로 이런식으로 표현하면 앱이 동작하지 않으므로 적절한 방법이 아니다.

```
sleep(5)
```



일단 AppDelegate내에 splash이미지 뷰를 추가 한후, 윈도우에 띄워주고 없애주면된다.

코드로 보자.








```swift
class AppDelegate: UIResponder, UIApplicationDelegate {
	var window: UIWindow?
	private var splash : UIImageView!

	func application(_ application: UIApplication, didFinishLaunchingWithOptionslaunchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 	{
	/* 앱이 실행될 때 필요한 셋팅을 합니다. */
	return true
  }

	func splashDidShow() {
	/* 앱에서 작동하는 LunchImage 이외에 스플래시 이미지를 따로 만들어 주어 보여주는 부분입니다. */
           /* 사용자에게는 그저 LunchScreen이 길게 보이는 것처럼 보여집니다. */

	func getSplashImageName() -> String {
		/* 여러 해상도에 맞는 스플래시 이미지의 이름을 가져오는 부분입니다. 
                       이부분을 사용하지 않고 
                       splash.image = UIImage(named: "LunchImage")
                       이렇게 하여도 동작하긴 하나, Default이미지가 가져와지므로 해상도 별로 적절한 이미지가 보여지지 않습니다 ( 이미지가 늘어져 보이는 이슈가 생길 수 있습니다. )
                    */

		let viewSize = UIScreen.main.bounds.size
		
		guard let imagesDict = Bundle.main.infoDictionary as [String: AnyObject]?,
			  let imagesArray = imagesDict["UILaunchImages"] as? [[String: String]] else {
				return "LaunchImage"
		}
		
		var viewOrientation: String
		switch UIDevice.current.orientation { 
                   // 이부분은 https://stackoverflow.com/questions/25796545/getting-device-orientation-in-swift 를 참고하세요
		case .portrait:
			viewOrientation = "Portrait"
		case .portraitUpsideDown:
			viewOrientation = "PortraitUpsideDown"
		default:
			viewOrientation = "Portrait"
		}
		
		for dict in imagesArray {
			if let sizeString = dict["UILaunchImageSize"], let imageOrientation = dict["UILaunchImageOrientation"] {
				let imageSize = CGSizeFromString(sizeString)
				if imageSize.equalTo(viewSize) && viewOrientation == imageOrientation {
					if let imageName = dict["UILaunchImageName"] {
						return imageName
					}
				}
			}
		}
		
		return "LaunchImage"
	}
	
	UIApplication.shared.isStatusBarHidden = true
            /* 상단에 status바가 보이지 않도록 합니다. */
	splash = UIImageView(frame: self.window!.frame)
	splash.image = UIImage(named: getSplashImageName())
	self.window!.addSubview(splash)
	self.window?.bringSubview(toFront: splash)
}

func splashDidRemove() {
	print(#function)
	UIView.animate(withDuration: 0.4,
	               animations: { self.splash.alpha = 0 },
	               completion: { _ in
					self.splash.removeFromSuperview()
					UIApplication.shared.isStatusBarHidden = false    
                                             /* 상단에 status바가 보이도록 합니다. */
					self.isSplashDidRemove = true
	})
}
```



그럼 즐코!