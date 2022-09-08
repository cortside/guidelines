# Rounding

While it might seem obvious how to round, there are actually multiple methods to handle rounding.  The default rounding method without specifying a specific method in C# using Math.Round is "banker's" rounding, or rounding to the nearest even.  This may not result in the rounding that most might assume if not particularly aware of different rounding methods and reasons for each.

For better understanding of the differing methods, see here:

* <https://en.wikipedia.org/wiki/IEEE_754#Roundings_to_nearest>

Rounding away from zero is the most widely known form of rounding, while rounding to nearest even is the standard in financial and statistical operations. It conforms to IEEE Standard 754, section 4.

To illustrate the difference, the following code can be run using [.NET Fiddle](https://dotnetfiddle.net/):

```csharp
using System;
					
public class Program
{
	public static void Main()
	{
		// default "banker's rounding" (rounding to nearest even)
		Console.WriteLine(Math.Round(0.50005, 4));
		Console.WriteLine(Math.Round(1.50005, 4));

		// specifying specific midpoint rounding method, away from zero
		Console.WriteLine(Math.Round(0.50005, 4, MidpointRounding.AwayFromZero));
		Console.WriteLine(Math.Round(1.50005, 4, MidpointRounding.AwayFromZero));
	}
}
```

and results in the following output:

```
0.5
1.5001
0.5001
1.5001
```

My general recommendation for rounding is to explicitly use the most common understanding of how to round, away from zero, instead of the default banker's rounding.  This will cause least questions when doing simple and single rounding operations.  For example, calculating a ratio that will not be part of an aggregate.

When used in multiple rounding operations, rounding to nearest even reduces the rounding error that is caused by consistently rounding midpoint values in a single direction. In some cases, this rounding error can be significant.  Care should be taken to explicitly chose a rounding method when the calculation will be used in an aggregate.  For example, calculating the sum of interest accrued over a period of time by daily interest calculations.

For an example of the rounding drift and more information, see the page on [Math.Round](https://docs.microsoft.com/en-us/dotnet/api/system.math.round?redirectedfrom=MSDN&view=net-6.0#midpoint-values-and-rounding-conventions).
