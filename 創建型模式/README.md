# 創建型模式
顧名思義，這種類型主要是在處理物件在創建時的機制，基本上是通過控制創建時的流程來解決問題。

### 主要有這四種設計模式
* 建構子模式(Constructor Pattern)
* 工廠模式(Factory Pattern)
* 原型模式(Prototype Pattern) 
* 單例模式(Singleton Pattern)


## 建構子模式(Constructor Pattern)
example:
```js
// 傳統JS寫法
function Programmer(name,programming_language){
    // 設定屬性值
    this.name = name
    this.language = programming_language
    // 在物件中宣告Method
    this.getDetails = function(){
        return `${this._name} 會寫 ${this._language} 程式`
    }
}

// ES6寫法
class Programmer{
    constructor(name,programming_language){
        // 設定屬性值
        this._name = name
        this._language = programming_language
        // 在物件中宣告Method
        this.getDetails = function(){
            return `${this._name} 會寫 ${this._language} 程式`
        }
    }
} 
// 創建一個Programmer實例
const AFa = new Programmer("AFa", "JavaScript")
console.log(AFa.getDetails()) // AFa 會寫 JavaScript 程式
```
建構子模式是以類別(class)為基底的設計模式，建構子是一種特殊的function可以用來實例化新物件所定義的方法&屬性

**這不是一種典型的設計模式，這在許多物件導向的語言中是再常見不過的基礎語言架構**，
但在JavaScript中，物件可以很自由不需要建構子去做功能或類別的定義，
因此需要制定這樣的設計模式來做好基礎建設。

在此範例中，我們定義了一個具有name及programming_language屬性，以及getDetail方法的Programmer類別，
接著用new給予個別的屬性值當作參數，實例化了一個會寫Javascript且名為AFa的Programmer。
  
  


## 工廠模式(Factory Pattern)
example:
```js
    class BallFactory{
        constructor(){
            this.createBall = function(type){
                let ball
                if(type==="football" || type==="soccer"){
                    ball = new Football()
                }else if(type === "basketball"){
                    ball = new Basketball()
                }
                ball.roll = function(){
                    return `這顆 ${this._type} 正在滾動`
                }

                return ball
            }
        }
    }

    class Football{
        constructor(){
            this._type = "足球"
            this.kick = function(){
                return "你踢了這顆足球..."
            }
        }
    }

    class Basketball{
        constructor(){
            this._type = "籃球"
            this.shoot = function(){
                return "你投出了這顆籃球..."
            }
        }
    }

    const factory = new BallFactory()
    const football = factory.createBall("football")
    const basketball = factory.createBall("basketball")

    console.log(football.roll()) // 這顆 足球 正在滾動
    console.log(football.kick()) // 你踢了這顆足球...
    console.log(basketball.roll()) // 這顆 籃球 正在滾動"
    console.log(basketball.shoot()) // 你投出了這顆籃球...
```
工廠模式是另一種以類別為基底的設計模式，
**這種模式通常運用在需要同時管理或操作多個有相似特性卻仍不同的物件集合**，

例子中我們創建了一個工廠類別"BallFactory"，
它有一個createBall的method能夠根據傳入不同的type來建立對應的ball物件。