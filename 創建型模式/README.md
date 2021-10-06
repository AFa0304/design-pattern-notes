# 創建型模式
顧名思義，這種類型主要是在處理物件在創建時的機制，基本上是通過控制創建時的流程來解決問題。

### 主要有這四種設計模式
* 建構子模式(Constructor Pattern)
* 工廠模式(Factory Pattern)
* 原型模式(Prototype Pattern)
* 單例模式(Singleton Pattern)
  
<br/>

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
  
  
<br/>

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



## 原型模式(Prototype Pattern)
example:
```js
    const iPhone = {
        brand: "apple",
        turnOn(){
            return "手機開機"
        },
        turnOff(){
            return "手機關機"
        }
    }

    const myIPhone = Object.create(iPhone,{owner:{value:"AFa"}})
    console.log(myIPhone.__proto__ === iPhone) // true
    console.log(myIPhone.owner) // AFa
    console.log(myIPhone.turnOn()) // 手機開機
```
在此範例中，我們有一個iPhone作為prototype，並使用了原生JS的Object.create去創建了另一個物件，並定義了新的屬性owner。

<br/>

## 單例模式(Singleton Pattern)
example:
```js
    class FrontEnd_Engineer{
        constructor(name){
            if(FrontEnd_Engineer.exists){
                return FrontEnd_Engineer.instance
            }
            this._name = name
            FrontEnd_Engineer.instance = this
            FrontEnd_Engineer.exists = true
            return this
        }
        getName(){
            return this._name
        }
        setName(name){
            this._name = name
        }
    }

    const AFa = new FrontEnd_Engineer("AFa")
    console.log(AFa.getName()) // AFa

    const Alan = new FrontEnd_Engineer("Alan")
    console.log(Alan.getName()) // AFa

    AFa.setName("Tom")
    console.log(AFa.getName()) // Tom
    console.log(Alan.getName()) // Tom
```
**單例模式是一種特別的設計模式，該類別只能存在唯一的實例；**
**邏輯大概是如果該類別沒有實例物件的話，則會創建一個新的實例回傳，**
**若該實例物件已經存在，則會回傳該實例物件的參考。**

在這個範例中，我們有一個名叫FrontEnd_Engineer的類別，
首先我們先使用new建立了一個物件叫AFa，這時因為沒有任何實例物件所以AFa就被創建出來了；
而我們第二次調用new創建一個物件叫Alan時並沒有物件被實例，取而代之的是回傳了原本已經被實例物件AFa的參考。