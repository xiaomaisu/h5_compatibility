###prototype问题

####单纯使用prototype
* ECMAScript 描述了原型链的概念，并将原型链作为实现继承的主要方法。其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。实现原型链有一种基本模式，其代码大致如下:
		
		function SuperType() {
			this.property = true;
		}

		function SubType() {

		}

		SubType.prototype = new SuperType();
		var instance1 = new SubType();
		
		
 上述代码是SubType 继承了SuperType,而继承是通过创建SuperType的实例，并将该实例赋给SubType.prototype实现的。现在存在于supertype的实例中的所有属性和方法，现在也存在于subtype.prototype了。此时instance.constructor因为被重写指向了supertype。
 
 
 那么以上代码有没有问题，这种继承方式是不是ok的呢。我们知道包含引用类型值的原型属性会被所有实例共享，而这也是为什么要在构造函数中，而不是原型对象中定义属性的原因。在通过原型来实现继承时，原型实际上会变成另一个类型的实例，于是，原先的实例属性顺理成章的变成原型属性。
 
 		
		function SuperType(){	
    		this.colors = ["red","yellow"];
		}
		function SubType(color){
    		//color?this.colors.push(color):noop();
		}
		function noop(){}
		SubType.prototype = new SuperType();
		var instance1 = new SubType();
		instance1.colors.push("black");
		console.log(instance1.colors);
		var instance2 = new SubType();
		console.log(instance2.colors);
		
这个例子中的supertype构造函数定义了一个colors属性，该属性包含一个数组为引用类型值，supertype的每个实例都会有各自包含的colors属性，当subtype通过原型链继承supertype后，subtypr.prototype就变成了supertype的一个实例，因此它也拥有了一个自己的colors属性，但是这个属性为subtype的所有实例共享，因此我们对instance1.colors的修改可以通过instance2.colors反映出来，因为这个问题，实例中很少会单独使用原型链

####借用构造函数

* 这种技术的思想比较简单，即在子类型构造函数中的内部调用超类型构造函数。【此处使用apply或者call实现】

		function SuperType(){	
    		this.colors = ["red","yellow"];
		}
		function SubType(color){
    		SuperType.call(this);
		}
		function noop(){}
		SubType.prototype = new SuperType();
		var instance1 = new SubType();
		instance1.colors.push("black");
		console.log(instance1.colors);
		var instance2 = new SubType();
		console.log(instance2.colors);
		
	此时subtype的每个实例都会有自己的colors属性的副本，但此时方法都在构造函数中定义，函数复用无从谈起。而且，在超类型的原型中定义的方法，对子类型而言也是不可见的，结果所有类型都只能使用构造函数模式
	
	
####组合继承方式

指的是将原型链和借用构造函数的技术结合到一块，从而发挥两者之长的一种继承模式。背后思路为使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承，这样，即通过在原型链上定义方法实现了函数的复用，又保证了每个实例都有它自己的属性。
比较好的demo示例为:<https://github.com/mrdoob/three.js/blob/dev/src/lights/PointLight.js>
