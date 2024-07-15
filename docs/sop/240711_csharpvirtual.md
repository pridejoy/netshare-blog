# C# 虚方法virtual详解

`virtual` 关键字用于声明一个虚方法，可以在派生类中被重写（override）。虚方法允许基类定义一个方法的默认实现，但允许派生类提供自己的实现。

1. **声明虚方法**：在基类中，使用 `virtual` 关键字声明一个方法，表示该方法可以在派生类中被重写。

```csharp
public class BaseClass
{
   public virtual void Display()
   {
    Console.WriteLine("Display method of BaseClass");
   }
}
```

2. **重写虚方法**：在派生类中，使用 `override` 关键字来重写基类中的虚方法。

```csharp
public class DerivedClass : BaseClass
{
   public override void Display()
   {
      Console.WriteLine("Display method of DerivedClass");
   }
}
```

3. **调用虚方法**：当你调用一个对象的虚方法时，实际调用的方法取决于对象的运行时类型，即使对象是作为基类类型引用的。

```csharp
  BaseClass obj = new DerivedClass();
  obj.Display(); // 输出 "Display method of DerivedClass"
```

4. **调用基类的虚方法**：在派生类的重写方法中，可以使用 `base` 关键字调用基类的实现。

```csharp
public override void Display()
{
    base.Display(); // 调用基类的 Display 方法
    Console.WriteLine("Additional functionality in DerivedClass");
}
```

5. **密封方法**：如果不想允许进一步的派生类重写某个方法，可以使用 `sealed` 关键字。

```csharp
public class BaseClass
{
    public virtual void Method()
    {
        // ...
    }
}

public class DerivedClass : BaseClass
{
    public sealed override void Method()
    {
        // 其他派生类不能重写这个方法
    }
}
```

6. **抽象类和虚方法**：抽象类可以包含虚方法，但抽象类本身不能被实例化。

```csharp
public abstract class AbstractClass
{
  public virtual void Method() { }
  public abstract void AnotherMethod();
}
```

7. **虚属性和索引器**：除了方法，C# 还允许属性和索引器是虚的。

8. **多态性**：虚方法是实现多态性的一种方式。多态性允许你编写通用的代码，该代码可以操作不同类型的对象，而具体使用哪个对象的实现则在运行时确定。

### 示例

override是重写
overload是重载

```
//转自：https://www.cnblogs.com/zhaoshujie/p/10502404.html
using System;

namespace VirtualMethodTest
{
    class A
    {
        public virtual void Func() // 注意virtual,表明这是一个虚拟函数
        {
            Console.WriteLine("Func In A");
        }
    }

    class B : A // 注意B是从A类继承,所以A是父类,B是子类
    {
        public override void Func() // 注意override ,表明重新实现了虚函数
        {
            Console.WriteLine("Func In B");
        }
    }

    class C : B // 注意C是从A类继承,所以B是父类,C是子类
    {
    }

    class D : A // 注意B是从A类继承,所以A是父类,D是子类
    {
        public new void Func() // 注意new ，表明覆盖父类里的同名类，而不是重新实现
        {
            Console.WriteLine("Func In D");
        }
    }

    class program
    {
        static void Main()
        {
            A a = new A(); // 实例化a对象,A是a的实例类
            A b= new B(); // 实例化b对象,B是b的实例类
            A c = new C(); // 实例化b对象,C是b的实例类
            A d= new D(); // 实例化b对象,D是b的实例类

  
            a.Func();    // 执行a.Func：1.先检查申明类A 2.检查到是虚拟方法 3.转去检查实例类A，就为本身 4.执行实例类A中的方法 5.输出结果 Func In A
            b.Func();    // 执行b.Func：1.先检查申明类A 2.检查到是虚拟方法 3.转去检查实例类B，有重载的 4.执行实例类B中的方法 5.输出结果 Func In B
            c.Func();    // 执行c.Func：1.先检查申明类A 2.检查到是虚拟方法 3.转去检查实例类C，无重载的 4.转去检查类C的父类B，有重载的 5.执行父类B中的Func方法 5.输出结果 Func In B
            d.Func();    // 执行d.Func：1.先检查申明类A 2.检查到是虚拟方法 3.转去检查实例类D，无重载的（这个地方要注意了，虽然D里有实现Func()，但没有使用override关键字，所以不会被认为是重载） 4.转去检查类D的父类A，就为本身 5.执行父类A中的Func方法 5.输出结果 Func In A
            D d1 = new D();
            d1.Func(); // 执行D类里的Func()，输出结果 Func In D
            Console.ReadLine();
        }
    }
}
```
