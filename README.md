# Repo
Make fake repositories with AutoMoq more easily

Big thanks to the creators of these:<br />
Moq: https://github.com/Moq/moq4<br />
AutoFixture: https://github.com/AutoFixture/AutoFixture
 
To use this tool: 
 
Include the file in your test project (or wherever your C# unit tests are).

In the file, add a using statement to include your database repositories.

Also in the file, replace every instance of "Context1, Context2, etc" with your own useful, descriptive names.<br />
_The reason these names aren't parameterized to the class is because 
if you're making a good number of tests in your project, including the extra step will get cumbersome._

Once that's completed, the usage in a test class is simple:

    using Repo.cs
    using System.Linq

    namespace.Test {

      //Arrange
      var repo = new Repo<SomeEntity>() //construct your fake repo 
        .Add("Name","Aaron","Age",27)   //add records to it
        .Add("Name","Feng","Age",26)
        .C1();                          //initialize it
  
      SomeController ctrl = new Controller(repo);
  
      //Act
      var a = ctrl.GetPersonByName("Aaron");
      var b = ctrl.GetAllPeopleWithName("Bob");
  
      //Assert
      Assert.AreEqual(a.Age, 27);
      Assert.AreEqual(0, b.Count());
    }
    
Each add statement adds a record to your fake database, and can have up to 5 attributes of any type.
_To add more attributes, add override methods to Repo.cs_

The constructor will create 3 fake records by default, but adding a number as an input will control how many if 3 doesn't work for you.

You can also add a boolean to the constructor to tell AutoFixture to ignore circular references, although good practice says to get rid of those circles in your code.

There's also an .Empty method that will delete all records from you database if you find that useful. (Say you wanted more than one run of tests in the same file).
