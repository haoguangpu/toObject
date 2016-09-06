# toObject
JS面向对象的程序设计
### 第一种：基于Object对象
      var person = new Object();   
      person.name = 'My Name';
      person.age = 18;
      person.getName = function(){
          return this.name;
      }
### 第二种：对象字面量方式（比较清楚的查找对象包含的属性及方法）
      var person = {
          name:'My name',
          age:18,
          getName:function(){
              return this.name;
          }
      }
### JS的对象可以使用'.'操作符动态的扩展其属性，可以使用'delete'操作符将其属性设置为'undefined'来删除属性。
      person.newAtt = 'new Attr';//添加属性
      alert(person.newAtt);//new Attr
      delete person.age;
      alert(person.age);//undefined(删除属性后值为undefined);   

# 二.对象的属性类型
ECMA-262第五版定义了JS对象属性中特征（用于JS引擎，外部无法直接访问）。ECMAscript中有两种属性：数据属性和访问器属性
### 1.数据属性：
      [[configurable]]:表示能否使用delete操作符删除从而重新定义，或能否修改为访问器属性。默认为true；
      [[Enumberable]]:表示是否可通过for-in循环返回属性。默认true;
      [[Writable]]:表示是否可修改属性的值。默认true;
      [[Value]]:包含该属性的数据值。读取/写入都是该值。默认为undefined;如上面实例对象person中定义了name属性，其值为’My name’
          对该值的修改都反正在这个位置
      要修改对象属性的默认特征（默认都为true)，可调用Object.defineProperty()方法，它接收三个参数：属性所在对象，属性名和一
          个描述符对象（必须是：configurable、enumberable、writable和value，可设置一个或多个值）。
##### 如下：（浏览器支持：IE9+、Firefox 4+、Chrome、Safari5+）
      var person = {};
      Object.defineProperty(person,'name',{
        configurable:false,
        writable:false,
        value:'jack'
      });
      alert(person.name);//jack
      delete person.name;
      person.name = 'lily';
      alert(person.name);//jack
  可以看出，delete及重置person.name的值都没有生效，这是因为调用defineProperty函数修改了对象属性的特征；值得注意的是一旦将
  configurable设置为false，则无法在使用defineProperty将其修改问哦true（执行会报错：can't redefine non-configurable property）;
###2.访问器属性
      它主要包括一对getter和setter函数，在读取访问器属性时，会调用getter返回有效值；写入访问器属性时，调用setter，写入新值；
      该属性有以下4个特征：
      [[Configurable]]:是否可通过delete操作符删除重新定义属性；
      [[Numberable]]:是否可通过for-in循环查找该属性；
      [[Get]]:读取属性时调用，默认：undefined;
      [[Set]]:写入属性时调用，默认：undefined;
      访问器属性不能直接定义，必须使用defineProperty()来定义，如下：
      var person = {
            _age:18
      };
      Object.defineProperty(person,'isAdult',{
            get:function(){
                  if(this._age >= 18){
                        return true;
                  }else{
                        return false;
                  }
            }
      });
      alert(person.isAdult?'成年'：'未成年')；//成年
从上面可知，定义访问器属性时gerrer与setter函数不是必须的，并且，在定义getter与setter时不能制定属性configurable及writable特性；
此外，ECMA-262(5)还提供了一个Object.defineProperties()方法，可以用来一次性定义多个属性的特性：        
             var person = {};        
             Object.defineProperties(person,{    
              _age:{    
                   value:19     
              },   
             isAdult:{    
                   get:function(){   
                         if(this._age >= 18){    
                                return true;     
                          }elsr{    
                                return false;    
                          }     
                        }    
             }    
            });     
      alert(person.isAdult?'成年'：'未成年')；//成年       
上述代码使用Object.defineProperties()方法同时定义了_age及isAudlt两个属性的特性此外，使用Object.getOwnPropertyDescriptor()      
方法可以取得给定属性的特性：      
      var desciptor = Object.getOwnPropertyDescriptor(person,'_age');       
      alert(descriptor.value);//19      
