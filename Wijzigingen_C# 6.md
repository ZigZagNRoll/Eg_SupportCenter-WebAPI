## Shortlist

* Compiler-as-a-service (Roslyn)
* Import of static type members into namespace
* Exception filters
* Await in catch/finally blocks
* Auto property initializers
* Default values for getter-only properties
* Expression-bodied members
* Null propagator (Succinct null checking)
* String Interpolation
* nameof operator
* Dictionary initializer




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

## Extension methods
Dit heeft meer uitleg nodig
```C#
using static System.Linq.Enumerable; // The type, not the namespace
class Program
{
    static void Main()
    {
        var range = Range(5, 17);                // Ok: not extension
        var odd = Where(range, i => i % 2 == 1); // Error, not in scope
        var even = range.Where(i => i % 2 == 0); // Ok
    }
}
```

## Null-Conditional operators
Extra null checking op een duidelijkere manier 
```C#
int? length = customers?.Length; // null if customers is null
Customer first = customers?[0];  // null if customers is null
```
Null kan over meerdere niveaus gebruikt worden
```C#
int length = customers?.Length ?? 0; // 0 if customers is null
```
Delegates kunnen aangeroepen worden mbv de Invoke methode
```C#
if (predicate?.Invoke(e) ?? false) { … }
```
Triggers:
```C#
PropertyChanged?.Invoke(this, args);
```

## String interpolation
```C#
var s = String.Format("{0} is {1} year{{s}} old", p.Name, p.Age);
==> 
var s = $"{p.Name} is {p.Age} year{{s}} old";
var s = $"{p.Name} is {p.Age} year{(p.Age == 1 ? "" : "s")} old";
var s = $"{p.Name,20} is {p.Age:D3} year{{s}} old";
```

## Nameof expressions
Nameof kan gebruikt worden om objecten te volgen ook al zoude deze van naam wijzigen dit is vooral handig voor het oproeken van exceptions
```C#
if (x == null) throw new ArgumentNullException(nameof(x));
```

## Index initializers

```C#
var numbers = new Dictionary<int, string> {
    [7] = "seven",
    [9] = "nine",
    [13] = "thirteen"
};
```

## Exception filters
If the parenthesized expression evaluates to true, the catch block is run, otherwise the exception keeps going.

Exception filters are preferable to catching and rethrowing because they leave the stack unharmed. If the exception later causes the stack to be dumped, you can see where it originally came from, rather than just the last place it was rethrown.

```C#
try { … }
catch (MyException e) when (myfilter(e))
{
    …
}
```
Wordt voornamelijk gebruikt om iets te doen voor de exception zelf runt
```C#
private static bool Log(Exception e) { /* log it */ ; return false; }
…
try { … } catch (Exception e) when (Log(e)) {}
```

## Await in catch and finally blocks
Await kan nu ook in de catch en finally blocks gebruikt worden. niet zo boeiend maar een fix

```C#
Resource res = null;
try
{
    res = await Resource.OpenAsync(…);       // You could do this.
    …
} 
catch(ResourceException e)
{
    await Resource.LogAsync(res, e);         // Now you can do this …
}
finally
{
    if (res != null) await res.CloseAsync(); // … and this.
}
```
