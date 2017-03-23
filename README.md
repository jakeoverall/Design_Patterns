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
  
  To illustrate this let's talk about how some animals have the ability to fly while others do not. The major benefit of the Strategy pattern is the ability to choose an implementation of a behavior and creat new implementations without modifying existing code. One drawback to the strategy pattern is it increases the total number of classes in a project. 
  
  ```csharp
  public interface IFly
  {
    string Fly();
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
  

### Observer Pattern
The Observer pattern is used when several objects need to recieve updates when an event or change occurs. The Observer pattern is comprised of two types of objects a **Subject** and its list of dependents called **Observers**

```csharp
	public interface ISubject
	{
		void Register(IObserver observer);
		void UnRegister(IObserver observer);
		void Notify();
	}

	public interface IObserver
	{
		void Update();
	}

	public class Chatroom : ISubject
	{
		public List<IObserver> Users { get; set; } = new List<IObserver>();
		public List<string> Messages { get; set; } = new List<string>();
		public void Register(IObserver observer)
		{
			Users.Add(observer);
		}

		public void UnRegister(IObserver observer)
		{
			Users.Remove(observer);
		}

		public void Chat(string message)
		{
			Messages.Add(message);
			Notify();
		}

		public void Notify()
		{
			foreach (var observer in Users)
			{
				observer.Update();
			}
		}
	}
	public class User : IObserver
	{
		private Chatroom _room;
		public void Update()
		{
			//Have the client print the last message added to the chat
			Console.WriteLine(_room.Messages[_room.Messages.Count - 1]);
		}
		public void sendMessage(string message)
		{
			_room.Chat(message);
		}

        internal void join(Chatroom room)
        {
            _room = room;
        }
    }

	public class Program
	{
		public static void Main(string[] args)
		{
			var general = new Chatroom();

            var jake = new User();
            var tom = new User();

            general.Register(jake);
            general.Register(tom);
            jake.join(general);
            tom.join(general);
            
            jake.sendMessage("Hello My name is Jake");
            tom.sendMessage("Hi Jake, I'm Tom");
            Console.ReadLine();
		}
	}

```

### Factory Pattern
The Factory pattern is used when creating one of many possible classes that all share a common super class or base class. Classes can be instantiated at run time.

```csharp

	public abstract class Enemy
	{
		public EnemyTypes Type;
		public string Name { get; set; }
		public int Damage { get; set; }
		public int Health { get; set; }
		public bool Dead { get; set; } = false;

		public void Attack()
		{
			Console.WriteLine($"{Name} is dealing {Damage} damage");
		}
		public void TakeDamage(int amt)
		{
			Console.WriteLine($"{Name} is taking {amt} damage");
			Health -= amt;

			CheckDeath();
		}

		public void CheckDeath()
		{
			if (Health <= 0) { Dead = true; }
		}
	}

	public enum EnemyTypes
	{
		Goblin = 1,
		Orc,
		Troll
	}

	public class Troll : Enemy
	{
		public Troll(string name)
		{
			Name = name;
			Damage = 40;
			Health = 80;
			Type = EnemyTypes.Troll;
		}
	}

	public class Orc : Enemy
	{
		public Orc(string name)
		{
			Name = name;
			Damage = 25;
			Health = 35;
			Type = EnemyTypes.Orc;
		}
	}

	public class Goblin : Enemy
	{
		public Goblin(string name)
		{
			Name = name;
			Damage = 10;
			Health = 15;
			Type = EnemyTypes.Goblin;
		}
	}

	public class EnemyFactory
	{
		public List<Enemy> Enemies { get; set; } = new List<Enemy>();

		public void CreateEnemy()
		{
			int enemySelection;
			Console.WriteLine("What type of Enemy Do you want to create?");
			Console.WriteLine("1. Goblin");
			Console.WriteLine("2. Orc");
			Console.WriteLine("3. Troll");

			Console.Write("Select a Number: ");
			string choice = Console.ReadLine();
			int.TryParse(choice, out enemySelection);
			EnemyTypes type = NewMethod(enemySelection);

			switch (type)
			{
				case EnemyTypes.Goblin:
					Enemies.Add(new Goblin("Ugly Goblin"));
					break;
				case EnemyTypes.Orc:
					Enemies.Add(new Orc("Ugly Orc"));
					break;
				case EnemyTypes.Troll:
					Enemies.Add(new Troll("Ugly Troll"));
					break;
				default:
					break;
			}
			if (Enemies.Count < 3)
			{
				Console.Clear();
				CreateEnemy();
			}
		}

		private static EnemyTypes NewMethod(int enemySelection)
		{
			return (EnemyTypes)enemySelection;
		}
	}

	public class Program
	{
		public static void Main(string[] args)
		{
			var enemyFactory = new EnemyFactory();
			enemyFactory.CreateEnemy();

			foreach (var enemy in enemyFactory.Enemies)
			{
				Console.WriteLine($"{enemy.Type}. {enemy.Name}");
			}

			Console.ReadLine();
		}
	}
```

  
