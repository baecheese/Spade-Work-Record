# 🛠 fatal error: unexpectedly found nil while unwrapping an Optional value
# 자료가 1개 있을 때, 삭제 후, 저장하면 optional error

* 자료가 1개 저장 되어 있을 때, 해당 자료를 삭제 후, 다시 하나의 자료를 생성하려하면, 옵셔널 오류가 나타난다.

* 문제의 코드

``` Swift
func save(project:Project) -> Bool {
        var latestId = 0
        do {
            try realm.write {
                if (false == realm.isEmpty) {
                    latestId = (realm.objects(Project.self).max(ofProperty: "id") as Int?)! // ⚡️error
                    latestId += 1
                    project.id = latestId
                }
                else {
                    project.id = latestId
                }
                realm.add(project)
            }
        }
        catch ContentsSaveError.contentsIsEmpty {
            log.warn(message: "contentsIsEmpty")
            return false
        }
        catch {
            log.error(message: "project save error")
            return false
        }
        return true
    }
```
	* 오류가 발생 한 곳은 id를 생성하기 위해 만든 코드 부분.
		* 해당 부분은 realm이 비어있을 땐, id를 0으로 생성하고, 이후로는 마지막 id에서 +1을 하도록 만들어놓았다

## 문제 이유
* 안에 있는 자료를 다 지운다고 realm.isEmpty가 true가 되지 않았다.
	* 안에 있는 자료를 다 지우면 해당 모델의 상태는 다음처럼 그냥 비어있게 됨
```
(lldb) po realm.objects(Project.self)
Results<Project> (

)
```
#### (false == realm.isEmpty)를 (false == realm.objects(검사하고싶은모델Object.self).isEmpty)로 바꿔야 한다

##  해결

```Swift
func save(project:Project) -> Bool {
        var latestId = 0
        do {
            try realm.write {
                if (false == realm.objects(Project.self).isEmpty) // 수정! {
                    latestId = (realm.objects(Project.self).max(ofProperty: "id") as Int?)!
                    latestId += 1
                    project.id = latestId
                }
                else {
                    project.id = latestId
                }
                realm.add(project)
            }
        }
        catch ContentsSaveError.contentsIsEmpty {
            return false
        }
        catch {
            return false
        }
        return true
    }
```
