# 设计模式：
##单例模式
单件模式用途：
单件模式属于工厂模式的特例，只是它不需要输入参数并且始终返回同一对象的引用。
单件模式能够保证某一类型对象在系统中的唯一性，即某类在系统中只有一个实例。它的用途十分广泛，打个比方，我们开发了一个简单的留言板，用户的每一次留言都要将留言信息写入到数据库中，最直观的方法是没次写入都建立一个数据库的链接。这是个简单的方法，在不考虑并发的时候这也是个不错的选择。但实际上，一个网站是并发的，并且有可能是存在大量并发操作的。如果我们对每次写入都创建一个数据库连接，那么很容易的系统会出现瓶颈，系统的精力将会很多的放在维护链接上而非直接查询操作上。这显然是不可取的。
如果我们能够保证系统中自始至终只有唯一一个数据库连接对象，显然我们会节省很多内存开销和cpu利用率。这就是单件模式的用途。当然单件模式不仅仅只用于这样的情况。在《设计模式：可复用面向对象软件的基础》一书中对单件模式的适用性有如下描述：
1、当类只能有一个实例而且客户可以从一个众所周知的访问点访问它时。
2、当这个唯一实例应该是通过子类化可扩展的，并且客户应该无需更改代码就能使用一个扩展的实例时。

下面对单件模式的懒汉式与饿汉式进行简单介绍：
1、饿汉式：在程序启动或单件模式类被加载的时候，单件模式实例就已经被创建。
2、懒汉式：当程序第一次访问单件模式实例时才进行创建。
如何选择：如果单件模式实例在系统中经常会被用到，饿汉式是一个不错的选择。

反之如果单件模式在系统中会很少用到或者几乎不会用到，那么懒汉式是一个不错的选择。

饿汉模式demo：

public Simple(){
private static Single s=new Single();        
private Single(){

}  
public static Simple getSimple(){
return s;
} 

}    
一般用于枚举法：
enum Single {
Single;

private Single() {

}

public void print(){
System.out.println("hello world");
}
}
public class SingleDemo {
public static void main(String[] args) {
Single a = Single.Single;
a.print();
}

懒汉模式 demo：

class Single{
private static Single s = null;

public Single() {
if (s == null)
　　s = new Single();
　　return s;
　　　 }
　　　 }

　　　 懒汉模式在使用时，容易引起不同步问题，所以应该创建同步"锁",demo如下
　　　 

　　　 class Single1 {
　　　 private static Single1 s = null;
　　　 
　　　 public Single1() {
　　　 
　　　 }
　　　 //同步函数的demo
　　　 public static synchronized Single1 getInstance() {
　　　 if (s == null)
　　　 s = new Single1();
　　　 
　　　 return s;
　　　 }
　　　 //同步代码快的demo加锁，安全高效
　　　 public static Single1 getInStanceBlock(){
　　　 if(s==null)
　　　 synchronized (Single1.class) {
　　　 if(s==null)
　　　 s = new Single1();
　　　 }
　　　 
　　　 return s;
　　　 
　　　 }
　　　 }

