---

---

## 1.函数

### 1.`ObjectOutputStream`序列化

```java
 // 创建序列化对象oss
 ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("ser.bin"));
 // 创建好反序列化对象oss后使用ObjectOutputStream类自带的writeObject方法,
 //将指定对象序列化写入输出流  writeObject方法传入的对象会转化 为字节流将对象的状态保存到文件中
 oos.writeObject(obj);

```

### 2.`ObjectInputStream`反序列化

```java
   ObjectInputStream ois = new ObjectInputStream(new FileInputStream(Filename));
        // 调用反序列化对象中的readObject从输入流中读取对象的字节流，并将其反序列化为对象。
        // 读取对象的字节表示形式，并将其转换为Java对象
        Object obj = ois.readObject();
        // 返回 反序列化后的对象
        return obj;

```

### 3.`Serializable`接口

有实现了`Serializable`或者`Externalizable`接口的**类的对象**才能被序列化为字节序列。（不是则会抛出异常; 实现`Serializable`接口，类表明它是可序列化的，可以被`ObjectOutputStream`序列化，以及被`ObjectInputStream`反序列化

```java
class person implements Serializable
{
    int age;
     String name="John";
     public person()
     {

     }

     public person(int age, String name)
     {
         this.age=age;
         this.name=name;
     }

     public String toString()
     {
         return "Name: "+name+" Age: "+age;
     }
}

```

### 4.Externalizable接口序列化

使用该接口之后，之前基于`Serializable`接口的序列化机制就将失效。`Externalizable `的序列化机制优先级要高于 `Serializable`

实现此接口后，类中属性字段使用 `transient` 和不使用没有任何区别,都会生效

### 5.完整的序列化代码示例

```java
import java.io.*;

class person implements Serializable
{
    int age;
     String name="John";
     public person()
     {

     }

     public person(int age, String name)
     {
         this.age=age;
         this.name=name;
     }

     public String toString()
     {
         return "Name: "+name+" Age: "+age;
     }
}

public class Main {
    public static void serialize(Object obj, String filename) throws IOException {
        FileOutputStream file = new FileOutputStream(filename);
        ObjectOutputStream out = new ObjectOutputStream(file);
        out.writeObject(obj);
        out.close();
        file.close();
        System.out.println("Object has been serialized");
    }
    
    public static person deserialize(String filename) throws IOException, ClassNotFoundException {
        FileInputStream file = new FileInputStream(filename);
        ObjectInputStream in = new ObjectInputStream(file);
        person p = (person) in.readObject();
        in.close();
        file.close();
        System.out.println("Object has been deserialized");
        return p;
    }
    
    public static void main(String[] args) throws IOException {

        person Person =new person();
        Person.age = 20;
        serialize(Person, "person.ser");
        person p = null;
        try {
            p = deserialize("person.ser");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        System.out.println("Name: " + p.name);
        System.out.println("Age: " + p.age);

    }
}
```

## 2.注意点

- 反序列化过程中，它的父类如果没有实现序列化接口，那么将需要提供无参构造函数来重新创建对象

- 实现 `Serializable`接口的子类也是可以被序列化的

- 静态成员变量是不能被序列化 序列化是针对对象属性的，而静态成员变量是属于类的

- `transient `标识的对象成员变量不参与序列化

一个对象的成员变量被标记为transient时，在对象进行序列化时，这些被标记的成员变量将不会被序列化，即它们的值不会被保存到序列化的数据流中。只会参与这个过程并输出对应数据类型的默认值,int类型输出**0** ,string类型输出`null`，但原始对象中的值并没有改变,仍然是**30**，只要添加了transient，序列化运行时会跳过该字段的处理。

```java
package a;

import java.io.*;

// 实现 Serializable接口
class Person implements Serializable {
    private String name;
    private transient int age; // 使用transient关键字标记age成员变量不被序列化

    // 构造函数
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

public class SerializationExample {
    public static void main(String[] args) {
        // 实例化类person
        Person person = new Person("Alice", 30);

        try {
            // 将对象序列化到文件
            ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("person.ser"));
            out.writeObject(person);
            out.close();

            // 从文件中反序列化对象
            ObjectInputStream in = new ObjectInputStream(new FileInputStream("person.ser"));
            Person deserializedPerson = (Person) in.readObject();
            in.close();

            // 输出反序列化后的对象
            System.out.println(deserializedPerson);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}


--------------------------------------

Person{name='Alice', age=0}

// 0 就是age默认的的值,但实际的数据并没有受到影响
// 这在某些情况下很有用，比如某些敏感信息或临时数据不需要被序列化保存
```

- Externalizable 进行反序列化时，需要有默认的构造方法,有参的构造器是不够的,还需要自己手写一个默认无参的构造器, 因为使用 Externalizable 进行反序列化时，需要有默认的构造方法，通过反射先创建出该类的实例，然后再把解析后的属性值，通过反射赋值。

  Serializable接口则不需要单独写默认无参的构造器
  

## 3.漏洞实现

### 1.原理

只要服务端反序列化数据，客户端传递类的readObject中代码会自动执行，可能包含危险代码

可能得形式：

1. 入口类的readObject直接调用
2. 入口类参数中包含可控类，该类有危险方法，readObject时调用
3. 入口类参数中包含可控类，该类又调用其他有危险方法的类，readObject时调用

### 2.使用

#### URLDNS链