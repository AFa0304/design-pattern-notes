# 結構型模式
這種模式主要讓我們在不竄改原先功能的情況下，取得新的功能。

### 主要有這六種設計模式
* 配接器模式(Adapter Pattern)
* 組合模式(Composite Pattern)
* 裝飾者模式(Decorator Pattern)
* 表象模式(Facade Pattern)
* 享元模式(Flyweight Pattern)
* 代理人模式(Proxy Pattern)
  
<br/>

## 配接器模式(Adapter Pattern)
example:
```js
    // 舊有的interface
    class OldRobot{
        constructor(){
            this.location = 0
            this.operations = function(distance,operation){
                switch(operation){
                    case "forward":
                        return this.location += distance
                    case "back":
                        return this.location -= distance
                }
            }
            this.getLocation = function(){
                return this.location
            }
        }
    }
    // 新的interface
    class NewRobot{
        constructor(){
            this.location = 0
            this.forward = function(distance){
                this.location += distance
            }
            this.back = function(distance){
                this.location -= distance
            }
            this.getLocation = function(){
                return this.location
            }
        }
    }
    // 配接器
    class RobotAdapter{
        constructor(){
            const newRobot = new NewRobot()
            this.operations = function(distance,operation){
                switch(operation){
                    case "forward":
                        return newRobot.forward(distance)
                    case "back":
                        return newRobot.back(distance)
                }
            }
            this.getLocation = function(){
              return newRobot.getLocation()
            }
        }
    }
    // 舊interface的使用
    const oldRobot = new OldRobot()
    oldRobot.operations(5,"forward")
    oldRobot.operations(3,"back")
    console.log(oldRobot.getLocation()) // 2
    // 新interface的使用
    const newRobot = new NewRobot()
    newRobot.forward(10)
    newRobot.back(5)
    console.log(newRobot.getLocation()) // 5
    // 配接器的使用
    const robotAdapter = new RobotAdapter()
    robotAdapter.operations(2,"forward")
    robotAdapter.operations(1,"back")
    console.log(robotAdapter.getLocation()) // 1
```
**這種模式用於新舊API共用要相容時使用，通常被用於新開發或程式碼重構。**  
上面例子中我們有一個名為OldRobot的舊類別，及一個叫NewRobot的新類別；  
OldRobot有一個operations的方法來控制Robot的前進及後退，  
但到了NewRobot卻是把前進及後退分為兩個方法，在這情況下配接器類別RobotAdapter的功用就誕生了，  
它包裝了NewRobot並增加了operations作為公開的API，而operations裡面是使用NewRobot的前進及後退的方法；  
這讓工程師能夠在不影響舊有使用的前提上，繼續開發NewRobot的程式。