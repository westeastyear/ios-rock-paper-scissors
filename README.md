>프로젝트 기간 2022-02-14 ~ 2022-02-18
>
>팀원 : [Donnie](https://github.com/westeastyear), [Tiana](https://github.com/Kim-TaeHyun-A) / 리뷰어 : [숲재](https://github.com/forestjae)


## 목차
- [프로젝트 소개](#프로젝트-소개)
- [Flow Chart](#Flow-Chart)
- [키워드](#키워드)
- [구현사항](#구현사항)
- [고민한점](#고민한점)
- [배운개념](#배운개념)


## 프로젝트 소개
`xcode`의 콘솔을 활용하여 컴퓨터와 가위바위보 게임 후 묵찌빠 게임을 할 수 있게 만든 프로젝트입니다.


## Flow Chart
<img src="https://i.imgur.com/znFuJFY.png" width="80%">


## 개발환경 및 라이브러리
[![swift](https://img.shields.io/badge/swift-5.6-orange)]()
[![xcode](https://img.shields.io/badge/Xcode-13.3-blue)]()


## 키워드
`enumeration` `error handling` `flow control`


## 구현사항
* `enum Game` : 가위바위보게임 or 묵찌빠게임

* `enum RockPaperScissors` : 묵찌빠를 숫자 문자로 구분

* `enum GameResult` : win, loss, tie

* `enum Player` : computer or user or nobody
    * nobody : 가위바위보에서 아직 turn이 정해지지 않은 상태이므로 이를 구별하기 위해 사용
    
* `enum GameError` : 잘못된 입력을 에러처리

* `operateGames()` : 전체적인 게임진행(가위바위보 게임 후 묵찌빠 게임)을 실행시키는 구문

* `startGame(of game: Game, _ firstTurn: Player) -> Player` : 가위바위보, 묵찌빠 게임을 실행하는 메서드
    * 게임 메뉴 출력
    * user와 computer 수 생성
    * winner가 결정될 떄까지 반복 : 잘못된 입력 예외처리
    
* `printGameMenu(of game: Game, turnValue: String)` : 들어오는 Game매개변수에 알맞는 게임출력 메소드 구분

* `printRockPaperScissorsGameMenu()` : 콘솔창에 가위바위보의 게임메뉴`가위(1), 바위(2), 보(3)! <종료 : 0>` 출력

* `printMukChiPaGameMenu(turnValue: String)` : 콘솔창에 묵찌빠의 게임메뉴`묵(1), 찌(2), 빠(3)! <종료: 0>` 출력

* `generateValues(at game: Game) -> (computerValue: String, userValue: String)`
    * 가위바위보 : 컴퓨터는 랜덤 수 생성, 사용자의 입력 받음
    * 묵찌빠 : 컴퓨터는 랜덤 수 생성, 사용자의 입력 받음 -> 묵찌빠 순서로 바꿔줌

* `generateRandomComputerNumber() -> Int` : 1과 3사이의 수 랜덤 생성

* `convertMukChiPa(_ number: Int) -> String`

* `convertType(of selectedNumber: String) throws -> RockPaperScissors`

* `findGameResult(computerValue: RockPaperScissors, userValue: RockPaperScissors) -> GameResult` : 가위바위보 결과 반환

* `printGameResult(of game: Game, _ gameResult: GameResult)` : 가위바위보 게임 결과를 출력

* `selectGameMenuUserNumber() -> String` : user의 입력값을 받음

* `stopGameLoop(of game: Game, _ gameResult: GameResult) -> Bool` : 게임의 종류 별로 종료 조건이 상이
    * 가위바위보는 비기지 않은 경우
    * 묵찌빠는 비긴 경우

* `changeTurn(_ currentValue: Player, _ gameResult: GameResult) -> Player` : 게임 결과에 따라 turn이 변경

* `printCurrentTurn(of game: Game, _ turn: Player)` : 가위바위보 게임의 경우 turn이 없으므로 바로 return


## 고민한점
* magic number
* enum initializer
* enum rawValue
* error handling
* CustomStringConvertible
* map
* commit message


**자세한 고민 보기**
[STEP 1 PR](https://github.com/yagom-academy/ios-rock-paper-scissors/pull/111)
[STEP 2 PR](https://github.com/yagom-academy/ios-rock-paper-scissors/pull/121)


## 배운개념
### magic number
enumeration의 case를 사용해서 하드코딩을 지양하였습니다.


### enum initializer
[enum init 공식문서](https://developer.apple.com/documentation/swift/rawrepresentable/1538354-init)
원시값을 사용해서 매칭되는 열거형의 인스턴스를 생성하여 활용하였습니다.
```swift=
guard let value = RockPaperScissors(rawValue: selectedNumber) else {
    throw InputError.wrongInputError
}
```


### enum rawValue

enum의 원시값으로는 리터럴만 사용가능합니다.

* 적절하지 원시값 타입을 작성한 경우 xcode에서 나타난 에러
: (사용자 정의 타입(enum GameResult)을 원시값의 타입으로 세팅하려고 한 경우
```swift=
1. 'Game' declares raw type 'GameResult', 
but does not conform to RawRepresentable and conformance 
could not be synthesized
2. Do you want to add protocol stubs?
3. Raw type 'GameResult' is not expressible by a string, 
integer, or floating-point literal
4. Raw value for enum case must be a literal
```

* 리터럴
<img src="https://i.imgur.com/R8MXixr.png" width="80%">

### error handling
Error프로토콜을 채택한 enum을 만들고 do-catch를 활용하여 에러를 처리하였습니다.
```swift=
enum InputError: Error {
    case wrongInputError
}
```


### CustomStringConvertible
[CustomStringConvertible 공식문서](https://developer.apple.com/documentation/swift/customstringconvertible)
```swift=
protocol CustomStringConvertible
```
이 프로토콜을 따르는 타입은 사용자 정의에 따른 텍스트 출력이 가능해집니다.
위 프로토콜을 채택하는 대신에, 이번 프로젝트에서는 enum의 rawValue를 사용했습니다.


### map
같은 대상에 대해서 2번 변환이 필요한 경우에 고차함수를 활용하였습니다.
```swift=
let userNumber = Int(selectGameMenuUserNumber())
       .map( { convertMukChiPa($0) } )
```


### commit message
"body에는 how보다 what과 why에 중점을 맞춰서" 진행하려고 했습니다.
