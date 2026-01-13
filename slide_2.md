### **பகுதி 2: Dart மொழியின் அடிப்படைகள் (Dart Language Fundamentals)**

**குறிப்பு:** Dart என்பது C++, Java, மற்றும் JavaScript போன்ற மொழிகளிலிருந்து பல கருத்துக்களைப் பெற்றிருந்தாலும், அதற்கென சில தனித்துவமான அம்சங்களும் உள்ளன.

#### **1. மாறிகள் (Variables) மற்றும் தரவு வகைகள் (Data Types)**

ஒரு நிரலில் தரவுகளைச் சேமிக்க மாறிகள் பயன்படுகின்றன. Dart ஒரு "type-safe" மொழி, அதாவது ஒவ்வொரு மாறிக்கும் ஒரு குறிப்பிட்ட தரவு வகை இருக்கும்.

*   `var`: ஒரு மாறியின் வகையை Dart தானாகவே யூகித்துக் கொள்ளும். அதன் மதிப்பு பின்னர் மாற்றப்படலாம்.
*   `final`: ஒரு மாறிக்கு ஒரு முறை மட்டுமே மதிப்பு ஒதுக்க முடியும். இது இயங்கும் நேரத்தில் (runtime) தீர்மானிக்கப்படும்.
*   `const`: இதுவும் `final` போன்றதுதான், ஆனால் இதன் மதிப்பு நிரல் தொகுக்கப்படும் போதே (compile-time) தீர்மானிக்கப்பட வேண்டும். இது ஒரு மாறாத மதிப்பு.

**முக்கிய தரவு வகைகள்:**
*   `int`: முழு எண்கள் (e.g., 10, -5, 0)
*   `double`: தசம எண்கள் (e.g., 3.14, -0.5)
*   `String`: எழுத்துகளின் தொடர் (e.g., 'வணக்கம்', "Flutter")
*   `bool`: சரி அல்லது தவறு (`true` or `false`)
*   `List`: பொருட்களின் வரிசைப்படுத்தப்பட்ட தொகுப்பு (an ordered collection, like an Array).
*   `Map`: `key-value` ஜோடிகளின் தொகுப்பு.

```dart
// Variable Declaration
var name = 'Suresh'; // Type inferred as String
String language = 'Tamil'; // Explicitly typed as String

final int age = 25; // Cannot be changed
const double pi = 3.14; // A compile-time constant

// Basic Data Types
int score = 100;
double percentage = 92.5;
bool isLoggedIn = true;
String greeting = 'Hello Dart!';

// Collections
List<String> colors = ['Red', 'Green', 'Blue'];
Map<String, String> capitals = {
  'India': 'New Delhi',
  'USA': 'Washington D.C.'
};
```

#### **2. Collections-ஐ Loop செய்தல்**

ஒரு `List` அல்லது `Map`-ல் உள்ள ஒவ்வொரு பொருளையும் அணுக சுழற்சிகள் (Loops) பயன்படுகின்றன.

```dart
List<String> fruits = ['Apple', 'Banana', 'Mango'];
Map<int, String> students = {1: 'Arun', 2: 'Bala', 3: 'Chitra'};

// Looping through a List using for-in loop
print('--- Fruits ---');
for (String fruit in fruits) {
  print(fruit);
}

// Looping through a Map using forEach
print('\n--- Students ---');
students.forEach((rollNumber, name) {
  print('Roll No: $rollNumber, Name: $name');
});
```

#### **3. பொருள் சார்ந்த நிரலாக்கம் (Object-Oriented Programming - OOP)**

OOP என்பது சிக்கலான மென்பொருட்களை எளிதாக நிர்வகிக்கக்கூடிய பகுதிகளாக (பொருள்கள்) பிரித்து உருவாக்கும் ஒரு முறையாகும்.

**அ) வகுப்புகள் (Classes) மற்றும் பொருள்கள் (Objects)**

*   **Class:** ஒரு பொருளை உருவாக்குவதற்கான வரைபடம் (blueprint). இது properties (தகவல்கள்) மற்றும் methods (செயல்கள்) ஆகியவற்றைக் கொண்டிருக்கும்.
*   **Object:** ஒரு வகுப்பின் நிஜ உலகப் பிரதிநிதி (instance).

```dart
// A blueprint for a Car
class Car {
  String brand;
  String model;
  int year;

  // A method (a function inside a class)
  void drive() {
    print('$brand $model is now driving.');
  }
}

void main() {
  // Creating an object (instance) of the Car class
  var myCar = Car();
  myCar.brand = 'Toyota';
  myCar.model = 'Fortuner';
  myCar.year = 2023;
  
  // Calling the method on the object
  myCar.drive(); // Output: Toyota Fortuner is now driving.
}
```

**ஆ) Constructors**

Constructors என்பவை ஒரு பொருளை உருவாக்கும் போது தானாகவே அழைக்கப்படும் சிறப்பு முறைகள். இவை பொருளின் αρχικές மதிப்புகளை அமைக்கப் பயன்படுகின்றன.

```dart
class Student {
  String name;
  int age;

  // 1. Parameterized Constructor (with 'this' syntax shortcut)
  Student(this.name, this.age);

  // 2. Named Constructor
  // ஒரு வகுப்பிற்கு பல வழிகளில் பொருள்களை உருவாக்க இது உதவுகிறது.
  Student.fromBirthYear(String name, int birthYear) {
    this.name = name;
    this.age = DateTime.now().year - birthYear;
  }
  
  void displayInfo() {
    print('Name: $name, Age: $age');
  }
}

void main() {
  // Using the parameterized constructor
  var student1 = Student('Anitha', 21);
  student1.displayInfo(); // Output: Name: Anitha, Age: 21

  // Using the named constructor
  var student2 = Student.fromBirthYear('Kumar', 2002);
  student2.displayInfo(); // Output: Name: Kumar, Age: 21 (assuming current year is 2023)
}
```

**இ) Access Modifiers, Getters, மற்றும் Setters**

Dart-ல் ஒரு property அல்லது method-ன் பெயரை அடிக்கோடிட்டு (`_`) ஆரம்பித்தால், அது அந்த கோப்பிற்கு (file) மட்டும் தனிப்பட்டதாக (private) ஆகிவிடும். இந்த தனிப்பட்ட மதிப்புகளைப் பாதுகாப்பாக அணுக `getters` மற்றும் `setters` பயன்படுகின்றன.

```dart
class BankAccount {
  String accountHolder;
  double _balance; // Private property

  BankAccount(this.accountHolder, this._balance);

  // Getter: To read the private _balance property
  double get balance {
    // We can add logic here, e.g., check for user permissions
    return _balance;
  }

  // Setter: To modify the private _balance property with validation
  set deposit(double amount) {
    if (amount > 0) {
      _balance = _balance + amount;
    } else {
      print('Invalid amount. Deposit must be positive.');
    }
  }
}

void main() {
  var myAccount = BankAccount('Gopi', 5000.0);

  // Using the getter to read the balance
  print('Initial Balance: ${myAccount.balance}'); // Notice we don't use ()

  // Using the setter to deposit money
  myAccount.deposit = 2000;
  print('New Balance: ${myAccount.balance}');

  myAccount.deposit = -100; // This will print the error message
}
```

**ஈ) மரபுரிமை (Inheritance)**

ஒரு வகுப்பு (`child class`) மற்றொரு வகுப்பின் (`parent class`) பண்புகளையும் செயல்களையும் பெறுவதற்குப் பெயர் மரபுரிமை. இது குறியீட்டை மீண்டும் பயன்படுத்துவதை (`code reusability`) ஊக்குவிக்கிறது. `extends` என்ற சொல் பயன்படுத்தப்படுகிறது.

```dart
// Parent Class
class Animal {
  String name;

  Animal(this.name);

  void eat() {
    print('$name is eating.');
  }
}

// Child Class inheriting from Animal
class Dog extends Animal {
  // The child constructor calls the parent constructor using 'super'
  Dog(String name) : super(name);

  void bark() {
    print('Woof! Woof!');
  }
}

void main() {
  var myDog = Dog('Buddy');
  myDog.eat(); // Method from the parent class (Animal)
  myDog.bark(); // Method from the child class (Dog)
}
```

**உ) Abstract Classes மற்றும் Mixins (சுருக்கமான அறிமுகம்)**

*   **Abstract Class:** இது ஒரு முழுமையற்ற வகுப்பு. இதை நேரடியாகப் பொருள் உருவாக்கப் பயன்படுத்த முடியாது. ஆனால் மற்ற வகுப்புகள் இதை `extend` செய்து அதன் முறைகளைச் செயல்படுத்தலாம். இது ஒரு பொதுவான கட்டமைப்பை வழங்குகிறது.
*   **Mixin:** இது மரபுரிமை இல்லாமல் ஒரு வகுப்பிற்குள் சில செயல்களை மீண்டும் பயன்படுத்தும் ஒரு வழியாகும். `with` என்ற சொல் பயன்படுத்தப்படுகிறது.

```dart
// Abstract Class example
abstract class Shape {
  void draw(); // Abstract method with no implementation
}

class Circle extends Shape {
  @override // Annotation to indicate we are implementing the parent's method
  void draw() {
    print('Drawing a circle.');
  }
}

// Mixin example
mixin Flyer {
  void fly() {
    print('I am flying!');
  }
}

class Bird with Flyer {}

class Superhero with Flyer {}

void main() {
  var circle = Circle();
  circle.draw();

  var bird = Bird();
  bird.fly();

  var superman = Superhero();
  superman.fly();
}
```

#### **4. ஒத்திசைவற்ற நிரலாக்கம் (Asynchronous Programming)**

செயலிகளில், நெட்வொர்க்கில் இருந்து தரவைப் பெறுவது, கோப்பைப் படிப்பது போன்ற சில செயல்களுக்கு அதிக நேரம் ஆகலாம். இந்த நேரத்தில் செயலி முடங்கிவிடாமல் (freeze) இருக்க, ஒத்திசைவற்ற நிரலாக்கம் பயன்படுகிறது.

*   **`Future`:** இது எதிர்காலத்தில் ஒரு மதிப்பைத் தரும் ஒரு பொருளாகும். (உதாரணம்: நீங்கள் ஆன்லைனில் உணவு ஆர்டர் செய்கிறீர்கள். உங்களுக்கு உடனடியாக உணவு கிடைக்காது, ஆனால் அது வரும் என்ற "உறுதி" (`Future`) கிடைக்கும்).
*   **`async` மற்றும் `await`:** இவை `Future`-களை எளிதாகக் கையாள உதவுகின்றன.
    *   `async`: ஒரு செயல்பாட்டை (function) ஒத்திசைவற்றது என்று குறிக்கிறது.
    *   `await`: ஒரு `Future` முடியும் வரை காத்திருக்கச் சொல்கிறது, ஆனால் UI-ஐ முடக்காது.

```dart
// This function simulates fetching user data from a server, which takes time.
// It returns a Future that will complete with a String value.
Future<String> fetchUserData() {
  return Future.delayed(Duration(seconds: 2), () => 'John Doe');
}

// 'async' marks the function as asynchronous
Future<void> printUserData() async {
  print('Fetching user data...');
  
  // 'await' pauses the execution of this function until the Future is complete
  String userData = await fetchUserData();
  
  print('User data fetched!');
  print(userData);
}

void main() {
  print('Program started.');
  printUserData();
  print('Program finished? Not yet. Waiting for async operation.');
}

// Output:
// Program started.
// Fetching user data...
// Program finished? Not yet. Waiting for async operation.
// (after 2 seconds)
// User data fetched!
// John Doe
```

---
