If a type needs to be nullable (to have null as an option) declare it with `?` after the type.  When referencing it later use `varName.Value`  

You can also use `??` to give a default value for a null entry

```
public class TestingNullable
    {
        public double? hourlyRate;

        public TestingNullable(double? rate)
        {
            hourlyRate = rate ?? 10;
        }

        public double GetWage(double hours)
        {
            return hours * hourlyRate.Value;
        }
    }
```

