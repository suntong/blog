# Random SSN Generator

[category Tech][tags programming,text,tool,C#]

Nothing fancy, just a normal Random SSN Generator in C# code.

<!--more-->

Post it because I can't find a decent C# SSN number random generator code. I thought I would have found plenty, but all that I found are of low quality ones, some are even using the wrong algorithm, i.e., the generated could well be an invalid SSN number. Maybe I didn't find that good one, but anyway, here is one more.

```csharp
        static void TestSSN()
        {
            Console.WriteLine("\n== Test SSN");
             for (int x = 1; x <= 3; x++)
                Console.WriteLine(RandomSSN("9"));
            for (int x = 1; x <= 3; x++)
                Console.WriteLine(RandomSSN("98"));
            for (int x = 1; x <= 3; x++)
                Console.WriteLine(RandomSSN("987"));
            Console.WriteLine(RandomSSN("77","-"));
            Console.WriteLine(GenerateSSN("-"));
        }

        public static string RandomSSN(string thePrefix, string delimiter = "")
        {
            string generatedSSN = GenerateSSN(delimiter);
            return thePrefix + generatedSSN.Substring(thePrefix.Length);
        }

        public static string GenerateSSN(string delimiter)
        {
            int iThree = GetRandomNumber(132, 921);
            int iTwo = GetRandomNumber(12, 83);
            int iFour = GetRandomNumber(1423, 9211);
            return iThree.ToString() + delimiter + iTwo.ToString() + delimiter + iFour.ToString();
        }

        //Function to get random number
        private static readonly Random getrandom = new Random();
        public static int GetRandomNumber(int min, int max)
        {
            return getrandom.Next(min, max);
        }
```

As you can see, it is

- using the correct algorithm, so the generated SSN will always be valid.
- versatile enough to allow a fixed prefix. This is very important to us, because we need to put people into different groups. Each group having an unique SSN prefix will make things much more clearer for us.
- able to generate SSN with or without the "-" delimiter, and that delimiter character is even configurable too.
- most importantly, using the correct C# code to do the random number generating. I thought it should be a no-brainer, but if you search for it, you will find that the C# randomization is confusing enough to cause lots of problems for people. Lots of posted C# randomization code are being criticized for doing it in the wrong way.

Still, nothing fancy, just easy to use, and generic enough to incorporate all kinds of usages, Random SSN Generator.



