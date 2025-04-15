# Java 中 `equals` 与 `==` 运算的区别

## 1. `==` 运算符

- **比较内容**：
  - 用于比较**基本数据类型**的值是否相等。
  - 用于比较**引用数据类型**时，比较的是两者的**内存地址**（引用是否指向同一个对象）。
- **适用场景**：
  - 比较基本数据类型的值。
  - 比较两个引用是否指向同一个对象。

### 示例代码

```java
public class Main {
    public static void main(String[] args) {
        // 基本数据类型比较
        int a = 10;
        int b = 10;
        System.out.println(a == b); // 输出: true

        // 引用数据类型比较
        String str1 = new String("hello");
        String str2 = new String("hello");
        System.out.println(str1 == str2); // 输出: false （不同对象）

        String str3 = "world";
        String str4 = "world";
        System.out.println(str3 == str4); // 输出: true （字符串池优化，同一对象）
    }
}
```

---

## 2. `equals` 方法

- **比较内容**：
  - 默认情况下，`Object` 类中的 `equals` 方法与 `==` 运算符的行为相同（比较内存地址）。
  - 不过，许多类（比如 `String`、`Integer` 等）重写了 `equals` 方法，用来比较**对象的内容**是否相等。
- **适用场景**：
  - 比较两个对象的内容是否相等。
  - 通常需要重写 `equals` 方法以实现自定义的比较逻辑。

### 示例代码

```java
public class Main {
    public static void main(String[] args) {
        // 引用数据类型比较
        String str1 = new String("hello");
        String str2 = new String("hello");
        System.out.println(str1.equals(str2)); // 输出: true （内容相同）

        // 自定义类比较
        Person p1 = new Person("Alice", 25);
        Person p2 = new Person("Alice", 25);
        System.out.println(p1.equals(p2)); // 输出: false （未重写 equals 方法）
    }
}

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

---

## 3. 重写 `equals` 方法的场景

为了让自定义类对象可以按内容比较，需要重写 `equals` 方法。例如：

```java
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true; // 同一对象
        if (obj == null || getClass() != obj.getClass()) return false; // 类型检查

        Person person = (Person) obj;
        return age == person.age && name.equals(person.name); // 按内容比较
    }
}
```

重写后：

```java
public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("Alice", 25);
        Person p2 = new Person("Alice", 25);
        System.out.println(p1.equals(p2)); // 输出: true （按内容比较）
    }
}
```

---

## 4. `equals` 与 `==` 的区别对比表

| **比较项**         | **`==` 运算符**                              | **`equals` 方法**                          |
|---------------------|---------------------------------------------|--------------------------------------------|
| **比较内容**        | 基本类型比较值，引用类型比较内存地址         | 比较对象内容（需重写 `equals` 方法）        |
| **适用场景**        | 基本类型值比较、判断两个引用是否是同一对象   | 判断两个对象的内容是否相等                 |
| **默认行为**        | 对于引用类型，比较内存地址                   | 默认行为与 `==` 相同（`Object` 类中）       |
| **重写需求**        | 不能重写                                    | 可通过重写实现自定义的内容比较逻辑          |

---

**总结**：

- `==` 运算符主要用于比较基本数据类型的值或引用是否指向同一个对象。
- `equals` 方法用于比较对象的逻辑内容，通常需要重写以实现自定义的比较逻辑。
- 如果需要比较自定义类的内容是否相同，需要同时重写 `equals` 和 `hashCode` 方法以确保一致性。