# Design_Patterns


### Inheritance & Encapsulation
Before diving to far into the many design patterns we should ensure we understand some of the basic concepts of inheritance and encapsulation. 

- Inheritance Rules 
  - Should a class inherit from another?
    - [x] IS A
      - Is a `Dog` an `Animal`
      - Is a `Bird` a `Dog`
    - [x] Has A
      - `Dog` has a `Height`
  - The subclass "IS A" superclass
  - Does the subclass **need** most of the methods of a superclass
  
  ```csharp
  public class Animal 
  {
    public string Name {get; set;}
    public double Height {get; set;}
    public double Weight {get; set;}
    
    public void Eat()
    {
      Console.WriteLine(Name + " is eating.");
    }
  }
  
  public class Dog : Animal
  {
    public void Dig()
    {
      //The Name Variable is Inherited from the Animal Class
      Console.WriteLine(Name + " is digging.");
    }
  }
  ```
  
  ### Strategy Pattern
  The Strategy Pattern forces encapsulated strategies to implement a paticular set of rules. Stategies can be swapped as necessary or appropriate. New Strategies can be defined and will be forced to implement all of the necessary rules to be a valid stategy.
  
  To illustrate this let's talk about how some animals have the ability to fly while others do not.
  
  ```csharp
  public interface IFly
  {
    public string Fly();
  }
  
  public class Animal 
  {
    //Defines a relationship between IFly and Animal
    public IFly FlyingType {get; set;}
    public string TryToFly() 
    {
      //Defers the Ability of Flying to the FlyingType ***Strategy***
      FlyingType.Fly();
    }
  }
  
  //IFly Strategies
  
  public CanFly : IFly
  {
    public string Fly()
    {
      return "Flying";
    }
  }
  
  public CantFly : IFly
  {
    public string Fly()
    {
      return "Unable to Fly";
    }
  }
  
  //Implement the 
  public class Dog : Animal 
  {
      public Dog()
      {
        FlyingType = new CantFly();
      }
  }
  
  
  public class Bird : Animal 
  {
      public Bird()
      {
        FlyingType = new CanFly();
      }
  }
  
  ```
  
  
  
