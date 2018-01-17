String의 부분 부분 속성을 바꾸고 싶다면!

```swift
let attributedString = NSMutableAttributedString(string: "총 12 건")

attributedString.addAttribute(NSForegroundColorAttributeName, value: UIColor.rgba(123, 124, 125, 255), range: NSRange(location: 0, length: 1))

attributedString.addAttribute(NSForegroundColorAttributeName, value: UIColor.rgba(123, 124, 125, 255), range: NSRange(location: attributedString.length - 1 , length: 1))

lbCount.attributedText = attributedString

```



이런식으로 쓰면 됩니당

