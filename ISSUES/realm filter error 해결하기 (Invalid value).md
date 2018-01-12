# 🛠 realm filter error 해결하기 (Invalid value)

메인 테이블에서 셀을 선택했을 때, 다음 페이지에서 해당 셀에 해당하는 총 정보를 가져오는 상황이었다.
다음 페이지에 ID를 넘겨주는 것까지 완성한 상태인데,
미리 primaryKey로 설정한 ID로 저장된 데이터를 찾고자 했더니 에러가 났다.

#### Date 형태

``` swift
class Diary: Object {
  dynamic var id = 0
  dynamic var timeStamp:TimeInterval = 0.0
  dynamic var content = ""
    
  override class func primaryKey() -> String? {
    return "id"
  }
    
}
```

#### 찾는 메소드

``` swift
func getDiary(id:Int) {
  var seletedDiary = realm.objects(Diary.self).filter("id = '\(id)'")
  log.info(message: "\(seletedDiary)")
}
```

#### 에러 메세지
    Terminating app due to uncaught exception 'Invalid value',
    reason: 'Expected object of type int for property 'id'
    on object of type 'Diary', but received: 1'


공식 사이트에 나와있는데로 쿼리를 썼는데 에러가 나서 당황했지만,

찾아보니 타입문제를 해결하면 되었다.

    If you use: id = 'YOUR_VAR_OR VALUE' => means that id is a String (Ex: id ='4')
    But if you use: id = YOUR_VAR_OR VALUE => means that id is an integer (Ex: id = 4)   
    
출처 - [Realm error: Invalid Value, expecting int and receiving](http://stackoverflow.com/questions/30724918/realm-error-invalid-value-expecting-int-and-receiving-0)

 
내가 id를 애초에 Int type으로 설정해놓아서, 찾을 때에도 Int로서 찾아야 한다고 한다.

realm 쿼리에 해당되는 ' ' 는 String을 뜻하는 것이니, ''을 빼면 되겠다.

 
#### 수정한 메소드

``` swift
func getDiary(id:Int) {
  var seletedDiary = realm.objects(Diary.self).filter("id = \(id)")
  log.info(message: "\(seletedDiary)")
}
```

위와 같이 수정해서 사용해보니 정상 작동 되었다.