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

<br/>

## 組合模式(Composite Pattern)
example:
```js
// 基礎元件Component
class Component {
    constructor(name) {
        this._name = name;
    }
    getNodeName() {
            return this._name;
        }
    // 需要覆寫的抽象方法
    getType() {}
    addChild(component) {}
    removeChildByName(componentName) {}
    removeChildByIndex(index) {}
    getChildByName(componentName) {}
    getChildByIndex(index) {}
    noOfChildren() {}
    // log樹狀結構
    static logTreeStructure(root) {
        let treeStructure = '';

        function traverse(node, indent = 0) {
            treeStructure += `${'--'.repeat(indent)}${node.getNodeName()}\n`;
            indent++;
            for (let i = 0, length = node.noOfChildren(); i < length; i++) {
                traverse(node.getChildByIndex(i), indent);
            }
        }
        traverse(root);
        return treeStructure;
    }
}
// 無子元件的Leaf類別
class Leaf extends Component {
    constructor(name) {
        super(name);
        this._type = 'Leaf Node';
    }
    getType() {
        return this._type;
    }
    noOfChildren() {
        return 0;
    }
}
// 能夠有子元件的Composite類別
class Composite extends Component {
    constructor(name) {
        super(name);
        this._type = 'Composite Node';
        this._children = [];
    }
    getType() {
        return this._type;
    }
    addChild(component) {
        this._children = [...this._children, component];
    }
    removeChildByName(componentName) {
        this._children = [...this._children].filter(component => component.getNodeName() !== componentName);
    }
    removeChildByIndex(index) {
        this._children = [...this._children.slice(0, index), ...this._children.slice(index + 1)];
    }
    getChildByName(componentName) {
        return this._children.find(component => component.name === componentName);
    }
    getChildByIndex(index) {
        return this._children[index];
    }
    noOfChildren() {
        return this._children.length;
    }
}
// 使用
const tree = new Composite('root'); 
tree.addChild(new Leaf('left'));
const right = new Composite('right');
tree.addChild(right);
right.addChild(new Leaf('right-left'));
const rightMid = new Composite('right-middle');
right.addChild(rightMid);
right.addChild(new Leaf('right-right'));
rightMid.addChild(new Leaf('left-end'));
rightMid.addChild(new Leaf('right-end'));
// log樹狀結構
console.log(Component.logTreeStructure(tree));
/*
root
--left
--right
----right-left
----right-middle
------left-end
------right-end
----right-right
*/
```
這是一種結構型設計模式，使用像是**樹狀結構**一樣的方式來組合這些物件，  
在這種模式下，**每一個節點可以是一個獨立物件，也可以是一個物件的組合**，  

這邊直接拿參考的範例來看，範例中創建了一個名為Component的基礎class，這個基礎類別實作了需要的通用功能及抽象功能，另外也有一個靜態方法去遍歷該類別底下的所有子類別組合的樹狀結構。  
另外範例中也創建了兩個子類別去繼承Component，一個是沒有子元件的Left，一個是可以有子元件且還有一些Method，能夠去處理增加、刪除及查詢子元件的功能，  
在這範例中這兩個子元件也被用來創建樹的結構。

<br/>

## 裝飾者模式(Decorator Pattern)
example:
```js
class Man{
    constructor(name,charisma){
        this._name = name
        this.charisma = charisma
    }
    getDetail(){
        return `一位叫做${this._name}的男性`
    }
}
// 一般男性
function normalMan(man){
    man.isNormalMan = true
    man.sleep = function(){
        return `${man.getDetail()} 睡著了。`
    }
    return man
}
// 完美的高富帥
function totalPackage(man){
    man.isTotalPackage = true
    man.charisma += 80
    man.throwParty = function(){
        return `${man.getDetail()} 參加了派對。`
    }
    return man
}

const AFa = normalMan(new Man("AFa",15))
console.log(AFa.isNormalMan) // true
console.log(AFa.charisma) // 15
console.log(AFa.sleep()) //一位叫做AFa的男性 睡著了。

const Jeff = totalPackage(new Man("Jeff",10))
console.log(Jeff.isTotalPackage)
console.log(Jeff.charisma) // 90
console.log(Jeff.throwParty()) //一位叫做Jeff的男性 參加了派對。
```
這種模式著重在對已存在的類別新增行為或功能，是使用子類別的另一種替代方案。  

Decorator Pattern的特性很容易用JavaScript去達成，因為JavasScript允許動態對物件增加方法或屬性。  

在這個範例中我們新增了一個男性Man的類別，另外也創建了兩個修飾功能的類別-一般男normalMan及高富帥totalPackage，  
它們接受Man物件並回傳"裝飾過"的Man物件，兩個類別分別新增了一個屬性及功能，另外totalPackage類別也編輯了已存在的屬性值。

<br/>

