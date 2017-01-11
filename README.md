#设计模式

-------------------

###设计模式是什么？
所谓的**设计模式**，一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。 
###设计的原则
- **开放封闭原则：** 对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码。
- **单一职责原则:**一个类只能有一个让它变化的原因。即，将不同的功能隔离开来，不要都混合到一个类中。
- **接口隔离原则:** 每个接口都实现单一的功能。添加新功能时，要增加一个新接口，而不是修改已有的接口，禁止出现“胖接口”。符合单一职责原则和开放封闭原则。
- **依赖倒置原则:** 具体依赖于抽象，而非抽象依赖与具体。即，要把不同子类的相同功能抽象出来，依赖与这个抽象，而不是依赖于具体的子类。


###设计模式的分类
- **创建型模式** ：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。
- **结构型模式** ：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
- **行为型模式** ：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

-------------------



###工厂方法模式

工厂模式方法：`普通工厂模式，多个工厂方法`

 **1.普通工厂模式：**是建立一个工厂类，对实现了同一接口的一些类进行实例的创建。
 
 **在java中**：
 ![enter image description here](http://dl.iteye.com/upload/attachment/0083/1180/421a1a3f-6777-3bca-85d7-00fc60c1ae8b.png?_=3023236)
 
 **java例子：**

 ```java
  
    //1.创建二者的共同接口：
    public interface Sender {  
        public void Send();  
    } 
    //创建实现类：
    public class MailSender implements Sender {   
        public void Send() {  
             System.out.println("this is mailsender!");  
        }  
    }
    public class SmsSender implements Sender {  
  
 
	    public void Send() {  
	        System.out.println("this is sms sender!");  
	    }  
	}
	//建工厂类
	public class SendFactory {  
  
    public Sender produce(String type) {  
        if ("mail".equals(type)) {  
            return new MailSender();  
        } else if ("sms".equals(type)) {  
            return new SmsSender();  
        } else {  
            System.out.println("请输入正确的类型!");  
            return null;  
        }  
      }  
    }  
    //调用
    public class FactoryTest {  
  
    public static void main(String[] args) {  
        SendFactory factory = new SendFactory();  
        Sender sender = factory.produce("sms");  
        sender.Send();  
      }  
    }        

**javascript：**
```javascript

//Speedster车型号类
function Speedster(){
	this.name = "Speedster";
}
Speedster.prototype.print = function(){
      alert(this.name);
}
//Lowrider车型号类
function Lowrider(){
	this.name = "Lowrider";
}
Lowrider.prototype.print = function(){
      alert(this.name);
}
//Cruiser车型号类
function Cruiser(){
	this.name = "Cruiser";
 }
 Cruiser.prototype.print = function(){
      alert(this.name);
}


var BicycleShop = function(){};
BicycleShop.prototype = {
   sellBicycle : function( model ){
        var bicycle;
        switch( model ){
            case "The Speedster":
                bicycle = new Speedster();
                break;
            case "The Lowrider":
                bicycle = new Lowrider();
                break;
            case "The Cruiser":
            default:
                bicycle = new Cruiser();
                break;
        }
        return bicycle;
    }
}

//sellBicycle 方法根据所提供的自行车型号来进行自行车的实例创建。那么对于一家ABC店铺，需要Speedster车型我只需要
    var ABC = new BicycleShop();
    var myBike = ABC.sellBicycle("The Speedster");
    myBike.print();  


```
**问题：**需要添加一些自行车款式的时候我就必须修改 BicycleShop 的 switch 部分，修改就可能会带来bug。
**解决：**将这部分生成实例的代码单独的提出来分工交给一个简单的工厂对象。

```javascript

//工厂类
var BicycleFactory = {
    createBicycle : function( model ){
        var bicycle;
        switch( model ){
            case "The Speedster":
                bicycle = new Speedster();
                break;
            case "The Lowrider":
                bicycle = new Lowrider();
                break;
            case "The Cruiser":
            default:
                bicycle = new Cruiser();
                break;
        }
        return bycicle;
    }
}
```
**效果：** BicycleFactory 是一个脱离于BicycleShop的单体。当需要添加新的类型的时候，不需要动 BicycleShop 只需修改工厂单体对象就可以。

```javascript

//Speedster类
function Speedster(){
	this.name = Speedster;
}
Speedster.prototype.print = function(){
      alert(this.name);
}
//Lowrider类
function Lowrider(){
	this.name = Lowrider;
}
Lowrider.prototype.print = function(){
      alert(this.name);
}
//Cruiser类
function Cruiser(){
	this.name = Cruiser;
 }
 Cruiser.prototype.print = function(){
      alert(this.name);
}
//工厂类
var BicycleFactory = {
    createBicycle : function( model ){
        var bicycle;
        switch( model ){
            case "The Speedster":
                bicycle = new Speedster();
                break;
            case "The Lowrider":
                bicycle = new Lowrider();
                break;
            case "The Cruiser":
            default:
                bicycle = new Cruiser();
                break;
        }
        return bycicle;
    }
}
var BicycleShop = function(){};
   BicycleShop.prototype = {
    sellBicycle : function( model ){
        var bicycle = BicycleFactory.createBicycle(model);    
        return bicycle;
    }
}
//调用
var Shop = new BicycleShop () ;
var Lowrider =  Shop.sellBicycle ("The Lowrider");

```



**总结：**其实这个就是一个简单工厂模式：BicycleFactory就是简单工厂。把成员对象的创建工作转交给一个外部对象BicycleFactory来完成。


**2.多个工厂方法模式**
**定义：**提供多个工厂方法，分别创建对象。

**思考：**在上面的例子里面，当字符输入错误，可能就无法处理，或是处理成错误的方式。


**解决：**
```javascript

var Factory  = function() {

	 this.creatSpeedster = function(){
		 return new Speedster();
	 }
	 this.creatLowride  = function(){
		 return new Lowride();
	 }
	 this.creatCruiser = function(){
		 return new Cruiser();
	 }
    
}
//Speedster
function Speedster(){
	this.name = "Speedster";
}
Speedster.prototype.print = function(){
      alert(this.name);
}
//Lowrider
function Lowrider(){
	this.name = "Lowrider";
}
Lowrider.prototype.print = function(){
      alert(this.name);
}
//Cruiser
function Cruiser(){
	this.name = "Cruiser";
 }
 Cruiser.prototype.print = function(){
      alert(this.name);
}
//使用
var factory = new Factory();
var Speedster  = factory.creatSpeedster();
Speedster.print();  


```


###抽象工厂模式
**思考：**工厂方法模式有一个问题就是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了闭包原则。
**解决：**创建多个工厂类，这样一旦需要增加新的功能，直接增加新的工厂类就可以了，不需要修改之前的代码。
```javascript

(function() {
// 定义Company接口，
var Company = function() {
    this.name = 'Company';
    this.methods = [ 'bulidComputer', 'bulidTelephone' ];
};
// 定义Company接口下的实现方法验证，保证实现其接口的实现类必须有对应的方法.
Company.ensureImplements = function(CompanyImplement) {
    var company = new Company();
    var implement = new CompanyImplement();
    for (var j = 0, methodsLen = company.methods.length; j < methodsLen; j++) {
        var method = company.methods[j];
        if (!implement[method]
        || typeof implement[method] !== 'function') {
        throw new Error("强制性实现检测: 对象 " + implement + "没有实现 "
        + company.name + " 接口。方法 " + method + " 没有找到");
        }
    }
};
// 定义Computer抽象类，
function Computer(){};
Computer.prototype.doUse = function(){};
// 定义PersonalComputer类.
function PersonalComputer(){};
PersonalComputer.prototype = new Computer();
PersonalComputer.prototype.doUse = function(){
document.write("这是个人计算机的功能.<br>");
};
// 定义NotebookComputer类.
function NotebookComputer(){};
NotebookComputer.prototype = new Computer();
NotebookComputer.prototype.doUse = function(){
document.write("这是笔记本电脑的功能.<br>");
};
// 定义Telephone抽象类.
function Telephone(){};
Telephone.prototype.doUse = function(){};
// 定义Mobile类.
function Mobile(){};
Mobile.prototype = new Telephone();
Mobile.prototype.doUse = function(){
	document.write("这是手机的功能.<br>");
};
// 定义DesktopPhone类.
function DesktopPhone(){};
DesktopPhone.prototype = new Telephone();
DesktopPhone.prototype.doUse = function(){
document.write("这是座机电话的功能.<br>");
};
// 定义CompanyA实现类.
var CompanyA = function() { };
CompanyA.prototype = {
  bulidComputer : function(parameter) {
      var computer = null ;
      if (parameter =='个人电脑') {
        computer = new PersonalComputer();
        return computer; 
      }
      else if (
        parameter =='笔记本电脑') {
        computer = new NotebookComputer();
        return computer;
      }
      else return computer;
  },
  bulidTelephone : function(parameter) {
      var telephone = null ;
      if (parameter =='座机电话') {
      telephone = new DesktopPhone();
      return telephone; }
      else if (parameter =='手机') {
      telephone = new Mobile();
      return telephone;}
      else return telephone;
      return telephone;
  }
};
// 保证CompanyA实现类实现Company接口
Company.ensureImplements(CompanyA);
// 程序开始运行
var company = new CompanyA();
// 根据传入的参数得到ProductA产品
var computer1 = company.bulidComputer('个人电脑');![Alt text](./设计模式 3.html)

computer1.doUse();
var computer2 = company.bulidComputer('笔记本电脑');
computer2.doUse();
// 根据传入的参数得到ProductB产品
var telephone1 = company.bulidTelephone('座机电话');
telephone1.doUse();
var telephone2 = company.bulidTelephone('手机');
telephone2.doUse();
}());

```
**好处：**只需做一个实现类，实现Company接口，同时做一个工厂类，就OK了，无需去改动现成的代码。这样做，拓展性较好！




###单例模式 
**定义：** 只能实例化一次的对象叫做单例。
 

**需求：**点击某个按钮的时候需要在页面弹出一个遮罩层
```javascript
var createMask = function(){
   return document,body.appendChild(  document.createElement(div)  );
}
( 'button' ).click( function(){
   Var mask  = createMask();
   mask.show();
})

```
**问题：**这个遮罩层是全局唯一的, 那么每次调用createMask都会创建一个新的div, 虽然可以在隐藏遮罩层的把它remove掉. 但显然这样做不合理.
**改进：**
```javascript
var mask = document.body.appendChild( document.createElement( ''div' ) );
$( ''button' ).click( function(){
   mask.show();
} )

```
**问题：**这样确实在页面只会创建一个遮罩层div, 但是另外一个问题随之而来, 也许我们永远都不需要这个遮罩层, 那又浪费掉一个div, 对dom节点的任何操作都应该非常吝啬.
**改进：**
```javascript
var mask;
var createMask = function(){ 
	if ( mask ) return mask; 
	else{ 
		mask = document,body.appendChild(  document.createElement(div)      );
		return mask;
	}
}
```
**结果：**完成了一个产生单列对象的函数，只是在函数体体内改变了外界变量mask的引用, 在多人协作的项目中, createMask是个不安全的函数. 另一方面, mask这个全局变量并不是非需不可. 再来改进一下.
**改进：**
```javascript
var createMask = function(){
  var mask;
  return function(){
       return mask || ( mask = document.body.appendChild( document.createElement('div') ) )
  }
}()
```
简单的闭包把变量mask包起来, 至少对于createMask函数来讲, 它是封闭的。
###建造者模式
**定义：**将一个复杂对象的 构造 与它的表示相分离，使同样的创建过程可有不同的表示，这就叫做建造者模式。（所谓对象就是指某个类具有不同的属性）。`其实建造者模式就是一个指挥者，一个建造者，一个使用指挥者调用具体建造者工作、并得从具体建造者得出结果的客户`


**例子：**
```javascript
    function student(){  
    }  
  
    function Baogongtou(){  
        this.gaifangzi=function(work){  
          work;
        }  
    }  
  
    function Gongren(){
        var studentXin =new student();

        this.setName=function(name){  
           studentXin.name = name;
        }  
        this.setAge=function(age){  
            studentXin.age = age; 
        }  
        this.setemail=function(email){  
            studentXin.setemail = email; 
        } 


        //...也可以有更多的属性
        this.jiaogong=function(){  
            return studentXin;  
        }  
    }  
    var gongren=new Gongren;  
    var baogongtou=new Baogongtou(gongren);  
    baogongtou.gaifangzi(gongren.setName("shuliqi"));  
    baogongtou.gaifangzi(gongren.setAge(23)); 
    //主人要房子  
    var student=gongren.jiaogong();  
    console.log(student); 
```

###原型模式
**方法：**我们创建的每个函数都有prototype（原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。使用原型对象的好处就是可以让所有对象实例共享它所包含的属性及方法。

```javascript
function Blog() {
}
 
Blog.prototype.name = 'wuyuchang';
Blog.prototype.url = 'http://tools.jb51.net/';
Blog.prototype.friend = ['fr1', 'fr2', 'fr3', 'fr4'];
Blog.prototype.alertInfo = function() {
  alert(this.name + this.url + this.friend );
}
 
// 以下为测试代码
var blog = new Blog(),
  blog2 = new Blog();
blog.alertInfo();  // wuyuchanghttp://tools.jb51.net/fr1,fr2,fr3,fr4
blog2.alertInfo();  // wuyuchanghttp://tools.jb51.net/fr1,fr2,fr3,fr4
 

```

```javascript
function Blog(name, url, friend) {
  this.name = name;
  this.url = url;
  this.friend = friend;
}
 
Blog.prototype.alertInfo = function() {
  alert(this.name + this.url + this.friend);
}
 
var blog = new Blog('wuyuchang', 'http://tools.jb51.net/', ['fr1', 'fr2', 'fr3']),
  blog2 = new Blog('wyc', 'http://**.com', ['a', 'b']);
 
blog.friend.pop();  //移除最后一个元素
blog.alertInfo();  // wuyuchanghttp://tools.jb51.net/fr1,fr2
blog2.alertInfo();  // wychttp://**.coma,b
```
| 测试网址			     |  压缩前的大小       | 压缩后的大小  |
| --------               	| -----:            | :----: |
| https://sina.cn/       	| 185.119bytes      |  156.878bytes|
| https://news.sina.cn/  	| 69.043bytes       |  60.693bytes |
| http://finance.sina.cn/	| 10.551bytes       |  8.863bytes  |
| https://sports.sina.cn/	| 114.869bytes      |  102.167bytes|
| https://ent.sina.cn/		| 60.807bytes       |  54.473bytes |
| http://mil.sina.cn/		| 46.616bytes       |  41.556bytes |
| http://video.sina.cn/		| 44.631bytes       |  37.566bytes |

具体如下：
没有去掉空格和换行
