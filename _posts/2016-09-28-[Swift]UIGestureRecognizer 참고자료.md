참고 : http://minsone.github.io/mac/ios/uigesturerecognizer

참고하여 나는 이런식으로 썼다!

```swift
 override func viewDidLoad() {

        super.viewDidLoad()

        initUi()

        let swipeUp = UISwipeGestureRecognizer(target: self, action: #selector(self.didSwipe(swipeGes:)))

        let swipeDown = UISwipeGestureRecognizer(target: self, action: #selector(self.didSwipe(swipeGes:)))

        swipeUp.direction = .up

        swipeDown.direction = .down

        self.view.gestureRecognizers = [swipeUp, swipeDown]

        

        initUi()

    }

    override func didReceiveMemoryWarning() {

        super.didReceiveMemoryWarning()

    }


    func didSwipe(swipeGes : UISwipeGestureRecognizer) {

      

        switch (swipeGes.direction) {

            case UISwipeGestureRecognizerDirection.down:

                print("Swiped down")

            case UISwipeGestureRecognizerDirection.up:

                print("Swiped up")

            default:

                break
        }

    }

```

