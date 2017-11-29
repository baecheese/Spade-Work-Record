# 📚 단어 개수 찾기
공백이 포함된 문장에서 단어의 개수를 찾는 방법.
“안녕하세요. 저는 cheese 입니다.”라고 하면 4를 리턴해야한다.

## 고려사항
1. 문장 맨 앞과 맨 뒤에 공백이 있을 경우 제외
2. 띄어쓰기가 연속 적으로 되있을 때 제외 ex `“안녕      나는 치즈”`

## 풀이
``` Swift
func wordNumberCount(word:String) -> Int {
    let str = word.trimmingCharacters(in: .whitespaces)
    var count = 0
    var beforeSpace = false
    for char in str.characters {
        if " " == char && false == beforeSpace {
            count += 1
            beforeSpace = true
        }
        else {
            beforeSpace = false
        }
    }
    return count + 1
}
```

###  추신
* 문장의 앞/뒤 공백을 없애려면 `.trimmingCharacters(in: .whitespaces)`를 사용
* [CharacterSet - Foundation | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/characterset)

#Development/study