With C# 6 release, I've seen that a lot of syntax changes have occurred since its inception. Because of this, I deemed correct to discuss a bit and present all the features for people stumbling upon this or searching about this topic.

It's useful to make a list of the most essential C# 6 features with simple code examples that would make it both easy to understand and simple to copy/paste a sample into a new console app to try it. Let's jump in. Some of those excerpts are from concrete examples or from other leaders and real mentors in the IT world, I am sharing hoping it might help somebody to move forward.


Auto-Property Initializers
--------------------------

With C# 5, we may have created our properties with a getter and setter and initialized our constructor:

```csharp
using System;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            Owner owner = new Owner();
            Console.WriteLine(owner.OwnerId);
            Console.ReadLine();
        }
    }

    public class Owner
    {
        public Owner()
        {
            OwnerId = Guid.NewGuid();
        }

        public Guid OwnerId { get; set; }
    }

}
```

With C# 6, we can modify the `Owner` class and populate the property called `OwnerId` inline:

```csharp
using System;
using static System.Console;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            Owner owner = new Owner();
            WriteLine(owner.OwnerId);
            ReadLine();
        }
    }

    public class Owner
    {
        public Guid OwnerId { get; set; } = Guid.NewGuid();
    }

}
```

Dictionary Initializers
-----------------------

In C# 5, you would initialize the `Dictionary` with a `{"Key", "Value"}` syntax:

```csharp
using System.Collections.Generic;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            var stars = new Dictionary<string, string> ()
            {
                 { "Adriano Palmieri", "Pallacanestro" },
                 { "Damiano Palmieri", "Calcio" },
                 { "Giuliano Palmieri", "Rugby" }
            };

            foreach (KeyValuePair<string, string> keyValuePair in stars)
            {
                Console.WriteLine(keyValuePair.Key + ": " +
                keyValuePair.Value + "\\n");
            }

            Console.ReadLine();
        }
    }
}
```

In C# 6, you just place the key between two square brackets `["Key"]` and then set the value of the key `["Key"] = "value"`, way cleaner and less error prone:

```csharp
using System.Collections.Generic;
using static System.Console;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            var stars = new Dictionary<string, string> ()
            {
                ["Adriano Palmieri"] = "Pallacanestro",
                ["Damiano Palmieri"] = "Calcio",
                ["Giuliano Palmieri"] = "Rugby"
            };

            foreach (KeyValuePair<string, string> keyValuePair in stars)
            {
                WriteLine(keyValuePair.Key + ": " +
                keyValuePair.Value + "\\n");
            }

            ReadLine();
        }
    }
}
```

Static Using Syntax
-------------------

With C# 5 Print something to the Console:

```csharp
using System;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello my name is Adriano!");
        }
    }
}
```

With C# 6, you can add the `using static` qualifier and reference the `WriteLine` method by itself:

```csharp
using static System.Console;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            WriteLine("Hello my name is Adriano!");
        }
    }
}
```

This also works for classes.

```csharp
using static System.Console;
using static CSharp6CS.BrandNewStaticClass;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            WriteLine("Hello my name is Adriano!");
            HelloBack();
        }
    }
    static class BrandNewStaticClass
    {
        public static void HelloBack()
        {
            WriteLine("Hello Computer!");
        }
    }
}
```

Since we declared `BrandNewStaticClass` with the `static` keyword, we can run the method by just calling the method name:

```csharp
Hello my name is Adriano!
Hello Computer!
```

Exception Filters
-----------------

With C# 6 we now reach the level VB was already on this specific topic. Exception filters allow you to specify a condition for a catch block:

```csharp
using System;
using static System.Console;


namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpStatusCode = 404;
            Write("HTTP Error: ");

            try
            {
                throw new Exception(httpStatusCode.ToString());
            }
            catch (Exception ex)
            {
                if (ex.Message.Equals("500"))
                    Write("Bad Request");
                else if (ex.Message.Equals("401"))
                    Write("Unauthorized");
                else if (ex.Message.Equals("402"))
                    Write("Payment Required");
                else if (ex.Message.Equals("403"))
                    Write("Forbidden");
                else if (ex.Message.Equals("404"))
                    Write("Not Found");
            }

            ReadLine();
        }
    }
}
```

Noew we can filter ahead of time if a specific condition is met before entering the catch block:

```csharp
using System;
using static System.Console;


namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            var httpStatusCode = 404;
            Write("HTTP Error: ");

            try
            {
                throw new Exception(httpStatusCode.ToString());
            }
            catch (Exception ex) when (ex.Message.Equals("400"))
            {
                Write("Bad Request");
                ReadLine();
            }
            catch (Exception ex) when (ex.Message.Equals("401"))
            {
                Write("Unauthorized");
                ReadLine();
            }
            catch (Exception ex) when (ex.Message.Equals("402"))
            {
                Write("Payment Required");
                ReadLine();
            }
            catch (Exception ex) when (ex.Message.Equals("403"))
            {
                Write("Forbidden");
                ReadLine();
            }
            catch (Exception ex) when (ex.Message.Equals("404"))
            {
                Write("Not Found");
                ReadLine();
            }

            ReadLine();
        }
    }
}
```

Await in a Catch and Finally Block
----------------------------------

With C# 6, you can write asynchronous code inside in a catch/finally; this will help developers to log exceptions to a file or database without blocking the current thread:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using static System.Console;


namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            Task.Factory.StartNew(() => GetPage());
            ReadLine();
        }

        private async static Task GetPage()
        {
            HttpClient client = new HttpClient();
            try
            {
                var result = await client.GetStringAsync("http://www.telerik.com");
                WriteLine(result);
            }
            catch (Exception exception)
            {
                try
                {
                    //This asynchronous request will run if the first request failed. 
                    var result = await client.GetStringAsync("http://www.progress.com");
                    WriteLine(result);
                }
                catch
                {
                    WriteLine("Entered Catch Block");
                }
                finally
                {
                    WriteLine("Entered Finally Block");
                }
            }
        }
    }
}
```

String Interpolation
--------------------

One of my favourite for code readability and maintenance. With C# 5, we typically concatenated two or more strings together or better, use a StringBuilder object to compose a multi-part string:

```csharp
using System;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            string firstName = "Adriano";
            string lastName = "Palmieri";

            Console.WriteLine("Name : " + firstName + " " + lastName);
            Console.WriteLine("Name : {0} {1}", firstName, lastName);

            Console.ReadLine();
        }
    }
}
```

In C# 6.0, we have string interpolation, a lot cleaner; just make sure you use the `$` before the start of the string:

```csharp
using static System.Console;

namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            string firstName = "Adriano";
            string lastName = "Palmieri";

            WriteLine($"My name is {firstName} {lastName}!");

            ReadLine();
        }
    }
}
```

The output for this example is :

```csharp
Adriano Palmieri is my name!
```

nameOf Expression
-----------------

This is a very helpful feature, instead of typing the argument name that might be the reason for a message or exception handling, you can now use a new syntax to get the name from the parameter itself; this is extremely valuable during refactoring especially in large and complex enterprise applications:

```csharp
using System;
using static System.Console;


namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {

            NewActivity("Hi");
            ReadLine();
        }

        public static void NewActivity(string name)
        {
            if (name == null) throw new Exception("Name is null");
        }
    }
}
```

If we change the name of the parameter, the previous error will be misleading to the developer; with C# 6 we have a new amazing way to handle this automatically:

```csharp
using System;
using static System.Console;


namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {

            NewActivity("Hi");
            ReadLine();
        }

        public static void NewActivity(string newName)
        {
            if (newName == null) throw new Exception(nameof(newName) + " is null");
        }
    }
}
```

Expression Bodied Function & Property
-------------------------------------

Functions and properties in lambda expressions save you from defining your function and property statement block:

```csharp
using static System.Console;

namespace CSharp6CS
{
    class Program
    {
        private static double SumNumbers(double num1, double num2) => num1 + num2;

        static void Main(string[] args)
        {
            double num1 = 5;
            double num2 = 10;

            WriteLine(SumNumbers(num1, num2));
            ReadLine();
        }
    }
}
```

The result of the above program would be 15.

Null Conditional Operator
-------------------------

Time saver and great help on handling `NullReferenceException` errors:

```csharp
using System;
using static System.Console;


namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            Person person = new Person();
            if (person.Name == String.Empty)
            {
                person = null;
            }

            WriteLine(person != null ? person.Name : "Field is null.");

            ReadLine();
        }
    }

    public class Person
    {
        public string Name { get; set; } = "";
    }
}
```

This returns "Field is null". If we enter some data into name, then the console prints out whatever is contained in the name.

Using C# 6.0, we can use `?.` to check if an instance is null or not:

```csharp
using System;
using static System.Console;


namespace CSharp6CS
{
    class Program
    {
        static void Main(string[] args)
        {
            Person person = new Person();
            if (person.Name == String.Empty)
            {
                person = null;
            }

            WriteLine(person?.Name ?? "Field is null.");

            ReadLine();
        }
    }

    public class Person
    {
        public string Name { get; set; } = "";
    }
}
```

Conclusions
-----------

Thankfully C# 6.0 is final and can be used in production environments now for company that are real and ready to save time and money and help their internal teams be less stressed and more productive! A great leadership shall have the power to list the benefits of switching to the new C# 6 version, show them to the client and gather trust consequentially to those decisions.

Production environments could be more resilient to the changes, especially in poor architected or highly patched situation that requires a lot of time to do simple tasks.

I truly hope you enjoy the article, I put some bit and pieces from other great sources but I deemed necessary to put up this article due to high number of "wannabe" high ranks individual that actually have no clue about real development for the real world!!!

God bless, Hope this help.
