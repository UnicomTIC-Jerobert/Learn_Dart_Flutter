### **பயிற்சி 6: மாணவர் மேலாண்மை அமைப்பு (Student Management System)**

**நோக்கம்:**
ஒரு மாணவர் மேலாண்மை அமைப்பை உருவாக்குவது. இந்த அமைப்பில், நாம் புதிய மாணவர்களைச் சேர்க்கலாம், அவர்களுக்குப் பாட மதிப்பெண்களைச் சேர்க்கலாம், மேலும் அவர்களின் மொத்த மதிப்பெண் மற்றும் சராசரியைக் காணலாம். இது `List` மற்றும் `Map` ஐ OOP உடன் இணைத்து ஒரு முழுமையான தீர்வை உருவாக்கும்.

**தேவையான OOP கருத்துக்கள்:**
*   **Class & Object:** `Student` என்ற ஒரு வகுப்பு மற்றும் அதன் பொருள்கள்.
*   **Encapsulation:** மாணவர் ஐடி (`_id`) மற்றும் மதிப்பெண்களை (`_marks`) private ஆக வைத்து, அவற்றை `getters` மூலம் அணுகுவது.
*   **Constructor:** மாணவரின் பெயர் மற்றும் ஐடியுடன் ஒரு புதிய பொருளை உருவாக்குவது.
*   **Methods:** மதிப்பெண்களைச் சேர்ப்பது, சராசரியைக் கணக்கிடுவது போன்ற செயல்பாடுகள்.
*   **Data Structures in OOP:** ஒரு `List<Student>` ஐப் பயன்படுத்தி அனைத்து மாணவர்களையும் நிர்வகிப்பது மற்றும் ஒரு `Map<String, int>` ஐப் பயன்படுத்தி ஒரு மாணவரின் மதிப்பெண்களை நிர்வகிப்பது.

**குறியீடு (Code):**

**1. மாணவர் வகுப்பு (`Student` Class):**
```dart
// Student Class: Represents a single student's data.
class Student {
  // Private properties - cannot be accessed directly from outside the file.
  final String _id;
  String name;
  final Map<String, int> _marks = {}; // Subject Name -> Mark

  // Constructor to initialize a student object.
  Student(this._id, this.name);

  // Getter to safely access the private student ID.
  String get id => _id;

  // Getter to get an unmodifiable view of the marks map.
  Map<String, int> get marks => Map.unmodifiable(_marks);

  // Method to add a mark for a subject.
  void addMark(String subject, int score) {
    if (score >= 0 && score <= 100) {
      _marks[subject] = score;
      print('Mark added for $subject.');
    } else {
      print('Invalid score. Please enter a value between 0 and 100.');
    }
  }

  // Method to calculate the average mark.
  double calculateAverage() {
    if (_marks.isEmpty) {
      return 0.0;
    }
    int total = _marks.values.reduce((sum, mark) => sum + mark);
    return total / _marks.length;
  }

  // Method to display student's details.
  void displayDetails() {
    print('--- Student Details ---');
    print('ID: $id'); // Using the getter
    print('Name: $name');
    if (_marks.isEmpty) {
      print('Marks: No marks added yet.');
    } else {
      print('Marks:');
      _marks.forEach((subject, score) {
        print('  - $subject: $score');
      });
      print('Average: ${calculateAverage().toStringAsFixed(2)}%');
    }
    print('-----------------------');
  }
}
```

**2. முக்கிய நிரல் (`main` function):**
```dart
import 'dart:io';

// Include the Student class definition here or import it from another file.

void main() {
  List<Student> students = []; // List to hold all student objects
  int nextStudentId = 101;

  while (true) {
    print('\n--- Student Management System ---');
    print('1. Add New Student');
    print('2. Add Mark for a Student');
    print('3. View Student Details');
    print('4. View All Students');
    print('5. Exit');
    print('Choose an option:');

    String? choice = stdin.readLineSync();

    switch (choice) {
      case '1':
        print('Enter student name:');
        String name = stdin.readLineSync() ?? 'Unknown';
        String id = 'S${nextStudentId++}';
        students.add(Student(id, name));
        print('Student "$name" added successfully with ID: $id');
        break;
      
      case '2':
        print('Enter student ID to add mark (e.g., S101):');
        String id = stdin.readLineSync() ?? '';
        Student? student = findStudentById(students, id);

        if (student != null) {
          print('Enter subject name:');
          String subject = stdin.readLineSync() ?? '';
          print('Enter mark for $subject:');
          int score = int.parse(stdin.readLineSync() ?? '0');
          student.addMark(subject, score);
        } else {
          print('Student with ID $id not found.');
        }
        break;
        
      case '3':
        print('Enter student ID to view details:');
        String id = stdin.readLineSync() ?? '';
        Student? student = findStudentById(students, id);

        if (student != null) {
          student.displayDetails();
        } else {
          print('Student with ID $id not found.');
        }
        break;
        
      case '4':
        if (students.isEmpty) {
          print('No students in the system yet.');
        } else {
          print('\n--- All Students ---');
          for (var student in students) {
            print('ID: ${student.id}, Name: ${student.name}');
          }
        }
        break;
        
      case '5':
        print('Exiting system. Goodbye!');
        return; // Exit the main function
        
      default:
        print('Invalid option. Please try again.');
    }
  }
}

// Helper function to find a student in the list by their ID.
Student? findStudentById(List<Student> students, String id) {
  for (var student in students) {
    if (student.id == id) {
      return student;
    }
  }
  return null;
}
```

---

### **பயிற்சி 7: எளிய வங்கிச் செயலி (Simple Bank Application)**

**நோக்கம்:**
ஒரு வங்கிச் செயலியை உருவாக்குவது. இதில் வெவ்வேறு வகையான கணக்குகளை (Savings, Current) உருவாக்கலாம். ஒவ்வொரு கணக்கிற்கும் டெபாசிட் மற்றும் வித்டிரா செய்ய முடியும். இது மரபுரிமை (Inheritance) மற்றும் சுருக்கமான வகுப்புகள் (Abstract Classes) போன்ற மேம்பட்ட OOP கருத்துக்களை விளக்கும்.

**தேவையான OOP கருத்துக்கள்:**
*   **Abstract Class:** `Account` என்ற ஒரு பொதுவான கட்டமைப்பை உருவாக்குவது.
*   **Inheritance:** `SavingsAccount` மற்றும் `CurrentAccount` வகுப்புகள் `Account`-ஐ `extend` செய்வது.
*   **Polymorphism:** வெவ்வேறு கணக்கு வகைகளுக்கு ஒரே மாதிரியான செயல்பாடுகளை (உதாரணமாக, `withdraw`) வெவ்வேறு வழிகளில் செயல்படுத்துவது.
*   **Encapsulation:** கணக்கு எண் (`_accountNumber`) மற்றும் இருப்பு (`_balance`) போன்றவற்றை private ஆக வைத்து `getters` மூலம் பாதுகாப்பாக அணுகுவது.
*   **Data Structures in OOP:** `Map<String, Account>` ஐப் பயன்படுத்தி அனைத்து கணக்குகளையும் நிர்வகிப்பது.

**குறியீடு (Code):**

**1. சுருக்கமான கணக்கு வகுப்பு (`Abstract Account` Class):**
```dart
// Abstract class defines a template for an account.
// It cannot be instantiated directly.
abstract class Account {
  final String _accountNumber;
  String accountHolder;
  double _balance;

  Account(this._accountNumber, this.accountHolder, this._balance);

  // Getters for private properties
  String get accountNumber => _accountNumber;
  double get balance => _balance;

  // A regular method shared by all account types
  void deposit(double amount) {
    if (amount > 0) {
      _balance += amount;
      print('Deposited $amount. New balance: $_balance');
    } else {
      print('Invalid deposit amount.');
    }
  }

  // An abstract method that MUST be implemented by child classes
  void withdraw(double amount);

  // A method that can be overridden by child classes
  void displayDetails() {
    print('Account Number: $_accountNumber');
    print('Account Holder: $accountHolder');
    print('Balance: $_balance');
  }
}
```

**2. சேமிப்புக் கணக்கு வகுப்பு (`SavingsAccount` Class):**
```dart
// SavingsAccount inherits from Account
class SavingsAccount extends Account {
  double interestRate;
  static const double _minBalance = 500.0;

  // Constructor calls the parent constructor using 'super'
  SavingsAccount(String accNum, String holder, double bal, this.interestRate)
      : super(accNum, holder, bal);

  // Implementation of the abstract method
  @override
  void withdraw(double amount) {
    if (amount <= 0) {
      print('Invalid withdrawal amount.');
      return;
    }
    if ((_balance - amount) >= _minBalance) {
      _balance -= amount;
      print('Withdrew $amount. New balance: $_balance');
    } else {
      print('Withdrawal failed. Minimum balance of $_minBalance must be maintained.');
    }
  }

  // Overriding the displayDetails method to add extra info
  @override
  void displayDetails() {
    print('\n--- Savings Account Details ---');
    super.displayDetails(); // Call the parent's method first
    print('Interest Rate: $interestRate%');
    print('-----------------------------');
  }
}
```

**3. நடப்புக் கணக்கு வகுப்பு (`CurrentAccount` Class):**
```dart
// CurrentAccount also inherits from Account
class CurrentAccount extends Account {
  double overdraftLimit;

  CurrentAccount(String accNum, String holder, double bal, this.overdraftLimit)
      : super(accNum, holder, bal);

  // A different implementation of the withdraw method
  @override
  void withdraw(double amount) {
    if (amount <= 0) {
      print('Invalid withdrawal amount.');
      return;
    }
    if ((_balance - amount) >= -overdraftLimit) {
      _balance -= amount;
      print('Withdrew $amount. New balance: $_balance');
    } else {
      print('Withdrawal failed. Overdraft limit of $overdraftLimit exceeded.');
    }
  }
  
  @override
  void displayDetails() {
    print('\n--- Current Account Details ---');
    super.displayDetails();
    print('Overdraft Limit: $overdraftLimit');
    print('----------------------------');
  }
}
```

**4. முக்கிய நிரல் மற்றும் வங்கி வகுப்பு (`Bank` Class & `main`):**
```dart
import 'dart:io';

// Include all class definitions here or import them

// Bank class to manage all accounts
class Bank {
  final Map<String, Account> _accounts = {};

  void createAccount(Account account) {
    _accounts[account.accountNumber] = account;
    print('Account created successfully for ${account.accountHolder} with number ${account.accountNumber}.');
  }

  Account? findAccount(String accNum) {
    return _accounts[accNum];
  }
}

void main() {
  Bank myBank = Bank();
  int nextAccNum = 1001;
  
  while (true) {
    print('\n--- Simple Bank Application ---');
    print('1. Create Savings Account');
    print('2. Create Current Account');
    print('3. Deposit');
    print('4. Withdraw');
    print('5. View Account Details');
    print('6. Exit');
    print('Choose an option:');

    String? choice = stdin.readLineSync();

    switch (choice) {
      case '1':
      case '2':
        print('Enter account holder name:');
        String name = stdin.readLineSync() ?? 'Unknown';
        print('Enter initial deposit:');
        double balance = double.parse(stdin.readLineSync() ?? '0');
        String accNum = 'ACC${nextAccNum++}';

        if (choice == '1') {
          var acc = SavingsAccount(accNum, name, balance, 3.5);
          myBank.createAccount(acc);
        } else {
          var acc = CurrentAccount(accNum, name, balance, 5000.0);
          myBank.createAccount(acc);
        }
        break;
      
      case '3':
      case '4':
        print('Enter account number:');
        String accNum = stdin.readLineSync() ?? '';
        Account? account = myBank.findAccount(accNum);

        if (account != null) {
          print('Enter amount:');
          double amount = double.parse(stdin.readLineSync() ?? '0');
          if (choice == '3') {
            account.deposit(amount);
          } else {
            account.withdraw(amount);
          }
        } else {
          print('Account not found.');
        }
        break;

      case '5':
        print('Enter account number:');
        String accNum = stdin.readLineSync() ?? '';
        Account? account = myBank.findAccount(accNum);
        if (account != null) {
          account.displayDetails();
        } else {
          print('Account not found.');
        }
        break;
        
      case '6':
        return;
        
      default:
        print('Invalid option.');
    }
  }
}
```
---
