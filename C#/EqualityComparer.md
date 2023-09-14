If you are trying to compare the contents of objects - typically in LINQ queries - you will need to implement a custom equality comparer override.  This is a class which extends `EqualityComparer<NameOfTheObject>` and must override `Equals` and `GetHashCode`.

`Equals` returns a bool, just have it check that the desired fields on each object are equal then return true.

`GetHashCode` is an int used in Dictionaries and HashTables.  Some ways to implement this are:
```
public override int GetHashCode(NameOfTheObject obj)
    {
      string value = obj.ProductID.ToString() + 
        obj.Name + 
        obj.Color + 
        obj.ListPrice.ToString() + 
        obj.StandardCost.ToString();

      return value.GetHashCode();
    }
```
Or another suggesting is to multiply everything by prime numbers like this:
```
public override int GetHashCode()
{
    unchecked // Overflow is fine, just wrap
    {
        int hash = 3049; // Start value (prime number).

        // Suitable nullity checks etc, of course
        hash = hash * 5039 + field1.GetHashCode();
        hash = hash * 883 + field2.GetHashCode();
        hash = hash * 9719 + field3.GetHashCode();
        return hash;
    }
}
```

A fully implemented example of `PersonComparer`:

```
public class PersonComparer : EqualityComparer<Person>
    {
        public override bool Equals(Person? x, Person? y)
        {
            if (x.FirstName == y.FirstName &&
                x.LastName == y.LastName &&
                x.Age == y.Age) return true;
            return false;
        }

        public override int GetHashCode([DisallowNull] Person obj)
        {
            int hash = 4327; //skipping null checks to save space.
            hash = hash * 5399 + obj.FirstName.GetHashCode();
            hash = hash * 2221 + obj.LastName.GetHashCode();
            hash = hash * 1049 + obj.Age.GetHashCode();
            return hash;
        }
    }
```

And it in use:
```
using System.Linq;
using LINQPractice;

List<Person> people1 = new List<Person>
{
    new Person("Bob", "Ross", 50),
    new Person("Nick", "B", 30),
};
List<Person> people2 = new List<Person>
{
    new Person("Bob", "Ross", 50),
    new Person("Nick", "B", 30),
};
List<Person> people3 = new List<Person>
{
    new Person("Somebody", "Else", 44),
    new Person("Old", "Folkerson", 90),
};

PersonComparer pc = new PersonComparer();

Console.WriteLine($"Are people1 and people2 equal? {people1.SequenceEqual(people2, pc)}");
Console.WriteLine($"Are people1 and people3 equal? {people1.SequenceEqual(people3, pc)}");
```
