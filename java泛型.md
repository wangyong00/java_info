## class<T>和 class<?>类型 有什么区别

平时看java源代码的时候，如果碰到泛型的话，我想? T K V E这些是经常出现的，但是有时想不起来代表什么意思，今天整理下： 

- ？ 表示不确定的java类型。 
- T  表示java类型。 
- K V 分别代表java键值中的Key Value。 
- E 代表Element。 

Object跟这些东西代表的java类型有啥区别呢？ 
Object是所有类的根类，是具体的一个类，使用的时候可能是需要类型强制转换的，但是用T ？等这些的话，在实际用之前类型就已经确定了，不需要强制转换。
追问：
也就是说，这个方法能知道返回的是哪种类型（父类），就用T行了？如果完全不知道的就用？用T的得到的对象就不需要类型转换了，而用？的就必需用强转了！
追答：
第一种是固定的一种泛型，第二种是只要是Object类的子类都可以，换言之，任何类都可以，因为Object是所有类的根基类
固定的泛型指类型是固定的，比如：Interge,String. 就是<T extends Collection>

- <？ extends Collection> 这里？代表一个未知的类型，
但是，这个未知的类型实际上是Collection的一个子类，Collection是这个通配符的上限.
举个例子
class Test <T extends Collection> { }

- <T extends Collection>其中,限定了构造此类实例的时候T是一个确定类型(具体类型)，这个类型实现了Collection接口，
但是实现 Collection接口的类很多很多，如果针对每一种都要写出具体的子类类型，那也太麻烦了，干脆还不如用
Object通用一下。
* <? extends Collection>其中,?是一个未知类型,是一个通配符泛型,这个类型是实现Collection接口即可。



``` java

public class Demo <T extends Animal>{

    private T ob;

    public T getOb() {
        return ob;
    }

    public void setOb(T ob) {
        this.ob = ob;
    }

    public Demo(T ob) {
        super();
        this.ob = ob;
    }
    
    public void print(){
        System.out.println("T的类型是："+ob.getClass().getName());
    }
}

```