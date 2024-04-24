#### One to Many Relationships ( A.K.A Parent-Child )

Don't forget to delcare one-to-many relationships in models as an empty list:

```
public class Author
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public List<Book> Books { get; set; } = new List<Book>();
}
```

Then in this table, an AuthorId table will automatically be created to indicate the relationship:

```
public class Book
{
    public int BookId { get; set; }
    public string Title { get; set; }
    public DateTime PublishDate { get; set; }
    public decimal BasePrice { get; set; }
    public Author Author { get; set; }      // know since Author has a list of type <Book> that it will need an AuthorId column, and generates it.
}
```

Don' forget you can use EF PowerTools to view the flowchart for all of these (requires EntityFrameworkCore.Design to be installed in the project with the context).

To define a custom field as a foreign key, you can do this during `OnModelCreating` with:

```
modelBuilder.Entity<Author>()
            .HasMany(a => a.Books)
            .WithOne(b => b.Author)
            .HasForeignKey(b => b.AuthorFK);
```            

If you had done the above, the AuthorId field would not be created automatically and an AuthorFK one would instead.

**Requiring fields**

In the above example, if a book is created without an author an exception will be thrown.  The `AuthorId` key generated in `Book` is not nullable by default.  If do want this to be available, make sure you explicitly declare the field, and make it nullable.  You can also specify a mapping of this by appending `.IsRequired(false)` to its definition in the model builder.