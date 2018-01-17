navigation bar에 로고를 넣고싶다면

1. 로고의 이미지 크기가 적절치 않을경우

```
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.
    let imageView = UIImageView(frame: CGRect(x: 0, y: 0, width: 20, height: 20))
    imageView.contentMode = .ScaleAspectFit
    let image = UIImage(named: "YOURIMAGE")
    imageView.image = image
    navigationItem.titleView = imageView
}
```

2. 로고의 이미지 크기를 적절하게 제작하였을 경우

```
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.
    let image = UIImage(named: "YOURIMAGE")
    navigationItem.titleView = UIImageView(image: image)
}
```

``

이런식으로 쓰면 된다 :)