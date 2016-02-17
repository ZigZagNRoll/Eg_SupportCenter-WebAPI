##Shortlist

*Compiler-as-a-service (Roslyn)
*Import of static type members into namespace
*Exception filters
*Await in catch/finally blocks
*Auto property initializers
*Default values for getter-only properties
*Expression-bodied members
*Null propagator (Succinct null checking)
*String Interpolation
*nameof operator
*Dictionary initializer




## Initializers for auto-properties  --- Geen idee of dit impact heeft op supportcenter

```C#
 public class Customer
{
    public string First { get; set; } = "Jane";
    public string Last { get; set; } = "Doe";
}
```

## Getter-only auto-properties --- Hetzelfde als het vorige maar de set kan weg gelaten worden
```C#
 public class Customer
 {
    public string First { get; } = "Jane";
    public string Last { get; } = "Doe";
 }
 ```

### Kan ook direct in het volgende field declared worden
```C#
 public class Customer
 {
    public string Name { get; }
    public Customer(string first, string last)
    {
        Name = first + " " + last;
    }
 }
```

## Methods as well as user-defined operators and conversions can be given an expression body by use of the “lambda arrow”
Het samenvoegen van methods met behulp van lambdas. Hier wordt gezegt dat zolang de methode een enkele return heeft deze ook gewoon toegevoegt kan worden mbv een lambda. 
```C#
 public Point Move(int dx, int dy) => new Point(x + dx, y + dy); 
 public static Complex operator +(Complex a, Complex b) => a.Add(b);
 public static implicit operator string(Person p) => p.First + " " + p.Last;
```

### For void-returning methods – and Task-returning async methods – the arrow syntax still applies
Hetzelfde werkt voor void returning methods. Al moet de expression na de lambda pijl wel een statement expression zijn.
```C#
public void Print() => Console.WriteLine(First + " " + Last);
```

## Expression bodies on property-like function members -- Uitzoeken wat dit betekent
Properties and indexers can have getters and setters. Expression bodies can be used to write getter-only properties and indexers where the body of the getter is given by the expression body:
```C#
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id); 
```

## Using static - static is uitgebreid: door static te gebruiken worden ineens alle onderliggende objecten bruikbaar en moeten en geen tussen stappen meer worden geschreven
```C#
using static System.Console;
using static System.Math;
using static System.DayOfWeek;
class Program
{
    static void Main()
    {
        WriteLine(Sqrt(3*3 + 4*4)); 
        // Geen Math.Example.Whatever.Sqrt() meer
        WriteLine(Friday - Monday); 
    }
}
```
