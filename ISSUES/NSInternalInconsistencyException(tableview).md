# 🛠 An NSInternalInconsistencyException occurred when tableview have only one section and cell and try deleteRows

TableView에서 데이터가 마지막 1개 일 때, deleteRows 하면 NSInternalInconsistencyException이 난다.

### 🍔 해당 코드

``` swift
override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
  let seletedDiaryID = SharedMemoryContext.setAndGet(key: "seletedDiaryID"
            , setValue: getSelectedDiaryID(section: indexPath.section, row: indexPath.row))
        
  if editingStyle == .delete {
    diaryRepository.delete(id: seletedDiaryID as! Int)
    self.tableView.deleteRows(at: [indexPath], with: .automatic)
    UIView.transition(with: self.tableView, duration: 1.0, options: .transitionCrossDissolve, animations: {
      let diarys = self.diaryRepository.findAll()
      self.sortedDate = Array(diarys.keys).sorted(by: >)
      self.tableView.reloadData()
      }, completion: nil)
    }
}

```

### 🍔 error가 나는 부분
``` swift
self.tableView.deleteRows(at: [indexPath], with: .automatic)
```

### 🍔 error massage

    Terminating app due to uncaught exception 'NSInternalInconsistencyException'
    reason: 'UITableView internal bug: unable to generate a new section map
    with old section count: 1 and new section count: 0'


tableview는 section이 하나 남은 상태에서 마지막 남은 row를 지우면 error가 발생하는 듯 하다.
검색해본 방법 [stackoverflow](http://stackoverflow.com/questions/27757170/error-when-deleting-item-from-tableview-in-swift) 은 core data에 대한 것만 있어서.. Not in my case.. 나한텐 해당이 안되고.. 다른 방법으로 해결했다. I solved the problem differently.

#### 🐥 *마지막 데이터* 일 때에는 셀을 지우지 않고, 해당 데이터를 realm에서 지운 후, 비어있는 realm 데이터로 tableview reload를 하는 것이다. (데이터가 하나도 없는 App 처음 상태와 동일해지는 것. 이러면 section도 0, row도 0이 된다.)

### 🐬 fix code

``` swift

override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
  let seletedDiaryID = SharedMemoryContext.setAndGet(key: "seletedDiaryID", setValue: getSelectedDiaryID(section: indexPath.section, row: indexPath.row))

  if editingStyle == .delete {
    diaryRepository.delete(id: seletedDiaryID as! Int)
    // 삭제 후, 다이어리를 찾았을 때
    let diarys = self.diaryRepository.findAll()
    /* 마지막 Diary 일 때 row를 지우면 NSInternalInconsistencyException이 일어남
      -> 마지막 diary일 땐 그냥 비어있는 diary 데이터로 tableView reload data */
    if false == isLastDairy(diarys: diarys) {
      // 마지막 diary가 아니면 deleteRow를 한다.
      self.tableView.deleteRows(at: [indexPath], with: .automatic)
    }
    UIView.transition(with: self.tableView, duration: 1.0, options: .transitionCrossDissolve, animations: {
      self.sortedDate = Array(diarys.keys).sorted(by: >)
      self.tableView.reloadData()
      }, completion: nil)
    }
}
    
func isLastDairy(diarys : [String : Array<Diary>]) -> Bool {
  if 1 < diarys.count {
    return false
  }
  return true
}

```

한참 찾다가 잔머리 굴리고 해본 건데, 뭔가 간단하게 되서 찝찝하다.