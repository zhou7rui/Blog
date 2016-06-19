title: java接口的解释
date: 2015-10-07 16:54:00
tags: java
---
接口只是一个规范，所以里面的方法都是空的。
假如我开了一个宠物粮店，声明所有宠物都可以来我这里买粮食，这就相当于一个接口，
``` JAVA
public interface PetRestaurant {
public void buy();
}

```
<!-- more -->
当一只狗看到了，知道自己是宠物，所以它去实现这个接口

``` JAVA
public class DogPet implements PetRestaurant {
@Override
public void buy() {
System.out.println("我是狗，我要买狗粮");
}
}
```
当一只猫看到了，知道自己也是宠物，所以也去实现这个接口
<!-- more -->
``` JAVA
public class CatPet implements PetRestaurant {
@Override
public void buy() {
System.out.println("我是猫，我要买猫粮");
}
}
```

当狗和猫来我的店之前，我是不知道他们到底是什么，但是当他们来到我的店，我就知道一个要猫粮食，一个要狗粮食。因为他们都实现了 我这个接口，都可以买。下面这个类相当于一个接待顾客的类，即店小二，他接待所有实现了我这个宠物店接口的动物，传进来一个PetRestaurant 类型的宠物，注意，这个PetRestaurant是接口

``` JAVA
public class test {
public void buy(PetRestaurant pet){
	pet.buy();
	}
}
```

好了，这个时候我这个老板出现了，我可以给他们卖粮食了，相当于测试类

``` JAVA
public class Tests {
public static void main(String[] args) {
PetRestaurant dog = new DogPet(); //实例化一个狗，相当于把狗顾客实例化
PetRestaurant cat = new CatPet();//实例化一个猫，相当于把猫顾客实例化

test t = new test(); //实例化一个店小二
t.buy(cat); //把猫交给店小二
t.buy(dog); //把狗交给店小二
}
}
```

这样运行的结果就是
我是猫，我要买猫粮
我是狗，我要买狗娘

你知道吗，整个过程我这个店主其实根本不知道来的到底是猫是狗还是其他什么，我只要有一个店小二，把这些来的不知什么动物都全部交给店小二，店小二就知道怎么去卖了，因为这些狗啊猫啊都实现了我这个宠物店的接口，而店小二就负责接待所有实现了我这个接口的动物。这就有一个好处，假如明天来了一头小猪，只要它实现了我这个接口，我只管交给店小二处理就OK了，我这个店小二根本不需要变化，我这个店主也只需要实例化一下这个动物就OK
你想，假如没有接口，会怎么办，来一个猫，我要去创造一个猫，还要实例化，来一只狗，我要创建一只狗，同样要实例化，还要配备专门的店小二去接待，就会相当麻烦

