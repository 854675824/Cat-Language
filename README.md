# Cat-Language
Cat 编程语言是用Java开发的，他的语法类似于Shell、Smali
# Cat #

**CatJava**是基于**Java**开发的轻量级***脚本编程语言***，他的语法类似于**Shell**、**Smali**。

## Ca Beta ##
_注:此项目备份已丢失_
此项目作为Cat的测试版本，意义重大，此版本与后面版本不同的是，它是将Java的 ***Runnable*** 作为函数，与后面版本相同的是，同样采用了 **HashMap** ***<String,Value>*** 作为全局表。

当运行代码时，会先使用方法 **getWord** 拆分字符串，然后获取并变量全局表的所有变量名称，如果相等就获取对应的Value变量，然后执行 Value 变量的run方法。  

***Main.java 部分代码***

    this.Global.put("echo",new Runnable(){  
        public void run() {
            System.out.println(this.args);
        }  
    })  
    Ca.loadString("echo \\"Hello World !\\"")

***Value.java 代码***
```
import java.lang.reflect.Constructor;
public class Value extends Object{
    private Class c;
    private Object obj;
    public String Tag;
    public <V> Value(V value) {
        c = value.getClass();
        obj = value;
    }

    @Override
    public String toString() {
        return get().toString();
    }

    @Override
    public boolean equals(Object obj) {
        return get().equals(obj);
    }

    @Override
    protected <V> V clone() {
        return convertType((Object)get(), this.c);
    }

    public <V> V get() {
        return convertType(this.obj, this.c);
    }
    
    public static <T> T convertType(Object obj, Class<T> newObjClass)  {
        T newObject = null;
        try {
            newObject = newObjClass.cast(obj);
        } catch (ClassCastException e) {
            try {
                Constructor<T> constructor = newObjClass.getConstructor(obj.getClass());
                newObject = constructor.newInstance(obj);
            } catch (Exception e2) {} 
        }
        return newObject;
    }
    public static <T> Value parseValue(T value){
        return new Value(value);
    }
    
    // API for Runnable
    public static Runnable run(Value v){
        Runnable r=(Runnable)v;
        r.run();
        return r;
    }
    public Runnable run(){
        Runnable r=(Runnable)this;
        r.run();
        return r;
    }
}

```
