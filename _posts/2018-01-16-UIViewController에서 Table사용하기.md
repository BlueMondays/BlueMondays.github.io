### UIViewController에서 Table사용하기



UIViewController에서 Table사용하는 법은 UITableViewController에서 사용하는 것과 다를 바가 없다.

그저 override키워드를 빼는 것일뿐! 

```swift
import UIKit

class BookListController : UIViewController{

    let list = (try! Realm().objects(User.self).last?.products)! //사용자의 보관함 목록을 불러옴

    

    override func viewDidLoad(){
        super.viewDidLoad()

    }

    

    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.list.count //갯수반환
    }

    
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {

        let cell = tableView.dequeueReusableCellWithIdentifier("BookListCell")!
        let row = self.list[indexPath.row] //주어진 행에 맞는 데이터 소스를 가져옴
        cell.textLabel?.text = (row.title)!
        return cell

    }

```



  

주의!!!!!!

이런식으로 UIViewController에서 Table을 쓸때 TableView의 datasource와 delegate가 해당 컨드롤러로 설정되어야 한다.

예를 들어 나는 지금 BookListController이므로 

![img](http://cfile10.uf.tistory.com/image/227EF33857C635F51D493A)

### 이런식으로 연결이 되어야 한다는 이야기다 

만약 무슨말인지 모르겠다면

![img](http://cfile3.uf.tistory.com/image/2629924357C6375D224ED0)

이런식으로 tableview를 변수선언을 하고

```swift
tableView.delegate = self

tableView.dataSource = self
```



를 viewDidLoad에 붙여 넣도록 하자.      



나도 ios개발 초보인지라 이런걸 한번 했어도 까먹고 까먹고 하다보니 

테이블이 왜 안나오나 하루종일 고민하다 삽질끝에 깨달았다..ㅠㅠ

