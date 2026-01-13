### **பயிற்சி 1: உடல் நிறை குறியீட்டெண் (BMI) கால்குலேட்டர்**

**நோக்கம்:**
பயனரிடமிருந்து அவர்களின் எடை (கிலோகிராமில்) மற்றும் உயரத்தை (மீட்டரில்) உள்ளீடாகப் பெற்று, அவர்களின் BMI-ஐ கணக்கிட்டு, அவர்கள் எந்த வகையைச் சேர்ந்தவர்கள் (குறைந்த எடை, சாதாரண எடை, முதலியன) என்பதைக் காட்ட வேண்டும்.

**தேவையான கருத்துக்கள்:**
*   பயனர் உள்ளீடு (`stdin.readLineSync`)
*   தரவு வகை மாற்றம் (`double.parse`)
*   கணித செயல்பாடுகள்
*   `if-else if-else` நிபந்தனைகள்

**குறியீடு (Code):**
```dart
import 'dart:io';

void main() {
  print('--- BMI Calculator ---');

  // Get weight from the user
  print('Enter your weight in kilograms (e.g., 70):');
  String? weightInput = stdin.readLineSync();
  // Convert string input to a double. If input is null, default to 0.
  double weight = double.parse(weightInput ?? '0');

  // Get height from the user
  print('Enter your height in meters (e.g., 1.75):');
  String? heightInput = stdin.readLineSync();
  double height = double.parse(heightInput ?? '0');

  if (weight > 0 && height > 0) {
    // Calculate BMI: weight / (height * height)
    double bmi = weight / (height * height);
    
    print('\nYour BMI is: ${bmi.toStringAsFixed(2)}'); // Show only 2 decimal places

    String category;
    if (bmi < 18.5) {
      category = 'Underweight (குறைந்த எடை)';
    } else if (bmi >= 18.5 && bmi < 24.9) {
      category = 'Normal weight (சாதாரண எடை)';
    } else if (bmi >= 25 && bmi < 29.9) {
      category = 'Overweight (அதிக எடை)';
    } else {
      category = 'Obesity (உடல் பருமன்)';
    }
    
    print('Category: $category');
  } else {
    print('Invalid input. Please enter valid weight and height.');
  }
}
```

**மாதிரி உள்ளீடு/வெளியீடு (Sample Input/Output):**
```
--- BMI Calculator ---
Enter your weight in kilograms (e.g., 70):
75
Enter your height in meters (e.g., 1.75):
1.8

Your BMI is: 23.15
Category: Normal weight (சாதாரண எடை)
```

---

### **பயிற்சி 2: எளிய நாணய மாற்றி (Simple Currency Converter)**

**நோக்கம்:**
ஒரு `Map`-ல் சில நாணயங்களின் மாற்று விகிதங்களை (exchange rates) சேமித்து வைக்கவும். பயனரிடமிருந்து ஒரு தொகையையும், எந்த நாணயத்திலிருந்து மாற்ற வேண்டும் என்பதையும் கேட்டு, அதன் இந்திய ரூபாய் மதிப்பைக் கணக்கிட்டுக் காட்ட வேண்டும்.

**தேவையான கருத்துக்கள்:**
*   `Map` (தரவுகளைச் சேமிக்க)
*   பயனர் உள்ளீடு
*   `if-else` (நாணயம் Map-ல் உள்ளதா எனச் சரிபார்க்க)
*   `String` முறைகள் (`toUpperCase`)

**குறியீடு (Code):**
```dart
import 'dart:io';

void main() {
  // Exchange rates with respect to INR
  Map<String, double> exchangeRates = {
    'USD': 83.50,
    'EUR': 90.25,
    'GBP': 105.70,
    'AED': 22.75,
  };
  
  print('--- Simple Currency Converter (to INR) ---');
  print('Available currencies: ${exchangeRates.keys.join(', ')}');
  
  // Get the currency code from the user
  print('\nEnter the currency code you want to convert from (e.g., USD):');
  String? currencyCode = stdin.readLineSync()?.toUpperCase(); // Convert to uppercase for consistency

  // Check if the currency code is valid
  if (currencyCode != null && exchangeRates.containsKey(currencyCode)) {
    // Get the amount from the user
    print('Enter the amount:');
    String? amountInput = stdin.readLineSync();
    double amount = double.parse(amountInput ?? '0');

    if (amount > 0) {
      double rate = exchangeRates[currencyCode]!; // '!' asserts that the key exists
      double convertedAmount = amount * rate;
      
      print('\n$amount $currencyCode is equal to ₹${convertedAmount.toStringAsFixed(2)} INR');
    } else {
      print('Invalid amount. Please enter a positive number.');
    }
  } else {
    print('Invalid currency code. Please choose from the available currencies.');
  }
}
```

**மாதிரி உள்ளீடு/வெளியீடு (Sample Input/Output):**
```
--- Simple Currency Converter (to INR) ---
Available currencies: USD, EUR, GBP, AED

Enter the currency code you want to convert from (e.g., USD):
usd
Enter the amount:
150

150.0 USD is equal to ₹12525.00 INR
```

---

### **பயிற்சி 3: மாணவர் மதிப்பெண் பகுப்பாய்வி (Student Grade Analyzer)**

**நோக்கம்:**
பயனரிடம் எத்தனை பாடங்கள் உள்ளன என்று கேட்டு, ஒவ்வொரு பாடத்தின் மதிப்பெண்ணையும் உள்ளீடாகப் பெறவும். அந்த மதிப்பெண்களை ஒரு `List`-ல் சேமித்து, மொத்த மதிப்பெண், சராசரி, மற்றும் தரத்தை (Grade) கணக்கிட்டுக் காட்ட வேண்டும்.

**தேவையான கருத்துக்கள்:**
*   `List` (மதிப்பெண்களைச் சேமிக்க)
*   `for` loop (உள்ளீடுகளைப் பெற)
*   கணித செயல்பாடுகள் (கூட்டல், வகுத்தல்)
*   `if-else if-else` (தரம் கணக்கிட)

**குறியீடு (Code):**
```dart
import 'dart:io';

void main() {
  print('--- Student Grade Analyzer ---');
  
  List<int> marks = [];
  int totalMarks = 0;
  
  // Get the number of subjects
  print('Enter the number of subjects:');
  int numberOfSubjects = int.parse(stdin.readLineSync() ?? '0');
  
  if (numberOfSubjects > 0) {
    // Loop to get marks for each subject
    for (int i = 1; i <= numberOfSubjects; i++) {
      print('Enter mark for subject $i:');
      int mark = int.parse(stdin.readLineSync() ?? '0');
      marks.add(mark);
      totalMarks += mark; // Add mark to total
    }
    
    // Calculate average
    double average = totalMarks / numberOfSubjects;
    
    // Determine the grade
    String grade;
    if (average >= 90) {
      grade = 'A';
    } else if (average >= 80) {
      grade = 'B';
    } else if (average >= 70) {
      grade = 'C';
    } else if (average >= 60) {
      grade = 'D';
    } else {
      grade = 'F (Fail)';
    }
    
    print('\n--- Results ---');
    print('Entered Marks: $marks');
    print('Total Marks: $totalMarks');
    print('Average: ${average.toStringAsFixed(2)}%');
    print('Grade: $grade');
  } else {
    print('Please enter a valid number of subjects.');
  }
}
```

**மாதிரி உள்ளீடு/வெளியீடு (Sample Input/Output):**
```
--- Student Grade Analyzer ---
Enter the number of subjects:
5
Enter mark for subject 1:
85
Enter mark for subject 2:
92
Enter mark for subject 3:
78
Enter mark for subject 4:
88
Enter mark for subject 5:
95

--- Results ---
Entered Marks: [85, 92, 78, 88, 95]
Total Marks: 438
Average: 87.60%
Grade: B
```

---

### **பயிற்சி 4: வார்த்தை எண்ணி (Word Counter)**

**நோக்கம்:**
பயனரிடமிருந்து ஒரு வாக்கியத்தைப் பெற்று, அந்த வாக்கியத்தில் ஒவ்வொரு வார்த்தையும் எத்தனை முறை வந்துள்ளது என்பதைக் கணக்கிட்டு ஒரு `Map`-ல் காட்ட வேண்டும்.

**தேவையான கருத்துக்கள்:**
*   `String` முறைகள் (`toLowerCase`, `split`)
*   `List` மற்றும் `for-in` loop
*   `Map` (வார்த்தை எண்ணிக்கையைச் சேமிக்க)
*   `if` நிபந்தனை (Map-ல் ஒரு key உள்ளதா எனச் சரிபார்க்க)

**குறியீடு (Code):**
```dart
import 'dart:io';

void main() {
  print('--- Word Counter ---');
  print('Enter a sentence:');
  
  String sentence = stdin.readLineSync()?.toLowerCase() ?? '';
  
  // Remove punctuation and split the sentence into words
  List<String> words = sentence.replaceAll(RegExp(r'[^\w\s]'), '').split(' ');
  
  Map<String, int> wordCount = {};
  
  for (String word in words) {
    // If the word is not empty
    if (word.isNotEmpty) {
      if (wordCount.containsKey(word)) {
        // If word is already in the map, increment its count
        wordCount[word] = wordCount[word]! + 1;
      } else {
        // If word is not in the map, add it with a count of 1
        wordCount[word] = 1;
      }
    }
  }
  
  print('\n--- Word Count ---');
  // Print the results
  wordCount.forEach((key, value) {
    print('$key: $value');
  });
}
```

**மாதிரி உள்ளீடு/வெளியீடு (Sample Input/Output):**
```
--- Word Counter ---
Enter a sentence:
Flutter is great and Flutter is easy. Learning Flutter is fun.

--- Word Count ---
flutter: 3
is: 3
great: 1
and: 1
easy: 1
learning: 1
fun: 1
```

---

### **பயிற்சி 5: எண் யூகிக்கும் விளையாட்டு (Number Guessing Game)**

**நோக்கம்:**
நிரல் 1-க்கும் 100-க்கும் இடையில் ஒரு சீரற்ற எண்ணை (random number) உருவாக்கும். பயனர் அந்த எண்ணைக் கண்டுபிடிக்க வேண்டும். ஒவ்வொரு தவறான யூகிப்பிற்கும், நிரல் "மிக அதிகம்" (Too high) அல்லது "மிகக் குறைவு" (Too low) என்று ஒரு குறிப்பைக் கொடுக்கும்.

**தேவையான கருத்துக்கள்:**
*   சீரற்ற எண் உருவாக்கம் (`dart:math`)
*   `while` loop (சரியான விடை கிடைக்கும் வரை விளையாட்டைத் தொடர)
*   `if-else if-else` (குறிப்புகள் வழங்க)
*   பயனர் உள்ளீடு மற்றும் `break` statement

**குறியீடு (Code):**
```dart
import 'dart:io';
import 'dart:math';

void main() {
  print('--- Number Guessing Game ---');
  print('I have selected a random number between 1 and 100. Can you guess it?');

  final random = Random();
  int randomNumber = random.nextInt(100) + 1; // Generates a number from 1 to 100
  int attempts = 0;

  while (true) {
    attempts++;
    print('\nEnter your guess:');
    String? input = stdin.readLineSync();
    int guess = int.parse(input ?? '0');

    if (guess == randomNumber) {
      print('Congratulations! You guessed the number correctly in $attempts attempts.');
      break; // Exit the loop
    } else if (guess < randomNumber) {
      print('Too low! Try again.');
    } else {
      print('Too high! Try again.');
    }
  }
}
```

**மாதிரி உள்ளீடு/வெளியீடு (Sample Input/Output):**
```
--- Number Guessing Game ---
I have selected a random number between 1 and 100. Can you guess it?

Enter your guess:
50
Too high! Try again.

Enter your guess:
25
Too low! Try again.

Enter your guess:
37
Congratulations! You guessed the number correctly in 3 attempts.
```

---
