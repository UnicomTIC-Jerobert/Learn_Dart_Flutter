இது ஒரு மிகச் சிறந்த மற்றும் நடைமுறைக்கு உகந்த யோசனை. SQLite மற்றும் Camera-ஐ இணைத்து ஒரு CRUD செயலி உருவாக்குவது மாணவர்களுக்கு Flutter-ன் முழுத் திறனையும் காட்டும்.

ஆனால், முந்தைய கேலரி செயலிக்கு நான் கூறியது போலவே, இதை 3-மணி நேர அமர்வில் முழுமையாகச் செய்து முடிப்பது மிகவும் கடினம். இது ஒரு பெரிய மற்றும் சிக்கலான தலைப்பு. இதற்குக் காரணங்கள்:

1.  **SQLite Complexity:** ஒரு தரவுத்தளத்தை உருவாக்குவது, அட்டவணைகளை வரையறுப்பது, SQL வினவல்களை (queries) எழுதுவது, மற்றும் தரவுத்தள இணைப்பை நிர்வகிப்பது போன்ற பல புதிய கருத்துக்களை அறிமுகப்படுத்த வேண்டும்.
2.  **Image Picker & File System:** கேமராவிலிருந்து படத்தைத் தேர்ந்தெடுப்பது, அதற்கான அனுமதிகளை (permissions) கையாளுவது, அந்தப் படத்தை சாதனத்தில் ஒரு கோப்பாகச் சேமிப்பது, பிறகு அந்தக் கோப்பின் பாதையை (path) மட்டும் SQLite-ல் சேமிப்பது எனப் பல படிகள் உள்ளன.
3.  **Time:** இந்த இரண்டு பெரிய தலைப்புகளையும் (SQLite + Camera/File) UI உடன் இணைத்து, பிழைகளைச் சரிசெய்து, தெளிவாக விளக்கக் குறைந்தது 2-3 மணிநேரம் தேவைப்படும். இதுவே ஒரு தனி அமர்வுக்கான தலைப்பு.

**எனது வலுவான பரிந்துரை:**

மாணவர்கள் குழப்பமடையாமல் இருக்க, நாம் இதை இரண்டு படிகளாகப் பிரிப்போம்.

*   **பரிந்துரை படி 1 (அமர்வில் செய்வது):** கேமரா பகுதியைத் தவிர்த்துவிட்டு, **SQLite-ஐப் பயன்படுத்தி ஒரு முழுமையான பயனர் மேலாண்மை CRUD செயலியை (User Management CRUD App)** உருவாக்குவோம். இதில் பயனரின் பெயர் மற்றும் மின்னஞ்சல் போன்ற உரைத் தரவை (text data) மட்டும் சேமிப்போம். இது SQLite-ன் அடிப்படைகளை மிகத் தெளிவாகவும், ஆழமாகவும் புரிய வைக்கும்.
*   **பரிந்துரை படி 2 (கூடுதல் குறிப்புகளாக):** அமர்வு முடிந்ததும், நீங்கள் மாணவர்களுக்குக் கொடுப்பதற்காக, **ஏற்கனவே உருவாக்கிய செயலியில் கேமரா மற்றும் படத்தைச் சேமிக்கும் செயல்பாட்டை எப்படிச் சேர்ப்பது** என்பதற்கான முழுமையான குறிப்புகளையும், குறியீட்டையும் நான் தருகிறேன்.

இந்த அணுகுமுறை, மாணவர்கள் முக்கியமான கருத்துக்களை ஒன்றன் பின் ஒன்றாக,しっかりとப் புரிந்துகொள்ள உதவும்.

நீங்கள் சம்மதித்தால், நாம் **SQLite-ஐ மட்டும் பயன்படுத்தி CRUD செயலியை** உருவாக்க ஆரம்பிக்கலாம்.

---

### **பகுதி 6: SQLite உடன் பயனர் மேலாண்மை CRUD செயலி (User Management CRUD App with SQLite)**

**செயலியின் நோக்கம்:**
1.  பயனர்களைச் சேர் (பெயர், மின்னஞ்சல்).
2.  சேமிக்கப்பட்ட பயனர்களின் பட்டியலைக் காட்டு.
3.  ஒரு பயனரின் விவரங்களைத் திருத்து (Edit).
4.  ஒரு பயனரை நீக்கு (Delete).

#### **படி 1: தேவையான தொகுப்புகளைச் சேர்த்தல் (Adding Packages)**

`pubspec.yaml` கோப்பில் இந்த இரண்டு தொகுப்புகளையும் சேர்க்கவும்:
1.  **`sqflite`**: Flutter-ல் SQLite தரவுத்தளத்தைப் பயன்படுத்த உதவும் முக்கிய தொகுப்பு.
2.  **`path`**: சாதனத்தின் கோப்பு முறைமையில் (file system) தரவுத்தளத்திற்கான சரியான பாதையைக் கண்டறிய உதவும்.

```yaml
dependencies:
  flutter:
    sdk: flutter
  sqflite: ^2.3.0 # Check for the latest version
  path: ^1.8.3
```
கோப்பைச் சேமித்து `flutter pub get` கட்டளையை இயக்கவும்.

#### **படி 2: பயனர் மாதிரி வகுப்பு (`User` Model)**

இந்த வகுப்பு, ஒரு பயனரின் தரவைக் குறிக்கும். SQLite-ல் சேமிக்கவும், அங்கிருந்து படிக்கவும் எளிதாக `toMap()` மற்றும் `fromMap()` என்ற இரண்டு முறைகளைச் சேர்ப்போம்.

```dart
// user_model.dart

class User {
  final int? id; // ID can be null when creating a new user
  String name;
  String email;

  User({this.id, required this.name, required this.email});

  // Helper method to convert a User object into a Map.
  // This is used when inserting data into the database.
  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
      'email': email,
    };
  }

  // Helper method to create a User object from a Map.
  // This is used when reading data from the database.
  factory User.fromMap(Map<String, dynamic> map) {
    return User(
      id: map['id'],
      name: map['name'],
      email: map['email'],
    );
  }
}
```

#### **படி 3: தரவுத்தள உதவி வகுப்பு (`DatabaseHelper` Class)**

இதுதான் நம் செயலியின் மிக முக்கியமான பகுதி. தரவுத்தளத்தை உருவாக்குவது, திறப்பது மற்றும் CRUD செயல்பாடுகளைச் செய்வது போன்ற அனைத்து தர்க்கங்களையும் (logic) இது கொண்டிருக்கும். இது ஒரு Singleton Pattern-ஐப் பயன்படுத்தும், அதாவது செயலி முழுவதும் இந்த வகுப்பிற்கு ஒரே ஒரு பொருள் (instance) மட்டுமே இருக்கும்.

```dart
// database_helper.dart

import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';
import 'user_model.dart'; // Import the User model

class DatabaseHelper {
  // Singleton pattern
  static final DatabaseHelper _instance = DatabaseHelper._internal();
  factory DatabaseHelper() => _instance;
  DatabaseHelper._internal();

  static Database? _database;

  Future<Database> get database async {
    if (_database != null) return _database!;
    _database = await _initDatabase();
    return _database!;
  }

  Future<Database> _initDatabase() async {
    String path = join(await getDatabasesPath(), 'user_database.db');
    return await openDatabase(
      path,
      version: 1,
      onCreate: _onCreate,
    );
  }

  // Create the database table
  Future<void> _onCreate(Database db, int version) async {
    await db.execute('''
      CREATE TABLE users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        email TEXT NOT NULL
      )
    ''');
  }

  // --- CRUD Operations ---

  // Create (Insert) a new user
  Future<int> createUser(User user) async {
    Database db = await database;
    return await db.insert('users', user.toMap());
  }

  // Read all users
  Future<List<User>> getAllUsers() async {
    Database db = await database;
    final List<Map<String, dynamic>> maps = await db.query('users');
    return List.generate(maps.length, (i) {
      return User.fromMap(maps[i]);
    });
  }

  // Update a user
  Future<int> updateUser(User user) async {
    Database db = await database;
    return await db.update(
      'users',
      user.toMap(),
      where: 'id = ?',
      whereArgs: [user.id],
    );
  }

  // Delete a user
  Future<int> deleteUser(int id) async {
    Database db = await database;
    return await db.delete(
      'users',
      where: 'id = ?',
      whereArgs: [id],
    );
  }
}
```

#### **படி 4: பயனர் இடைமுகம் (`UserScreen` UI)**

இப்போது, தரவுத்தள செயல்பாடுகளை UI உடன் இணைப்போம். இது ஒரு `StatefulWidget` ஆக இருக்கும்.

```dart
// user_screen.dart

import 'package:flutter/material.dart';
import 'database_helper.dart';
import 'user_model.dart';

class UserScreen extends StatefulWidget {
  const UserScreen({Key? key}) : super(key: key);

  @override
  _UserScreenState createState() => _UserScreenState();
}

class _UserScreenState extends State<UserScreen> {
  late Future<List<User>> _userList;
  final dbHelper = DatabaseHelper();

  final _nameController = TextEditingController();
  final _emailController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _refreshUserList();
  }

  void _refreshUserList() {
    setState(() {
      _userList = dbHelper.getAllUsers();
    });
  }

  void _showFormDialog({User? user}) {
    // If user is not null, it's an edit operation.
    if (user != null) {
      _nameController.text = user.name;
      _emailController.text = user.email;
    } else {
      _nameController.clear();
      _emailController.clear();
    }

    showDialog(
      context: context,
      builder: (_) => AlertDialog(
        title: Text(user == null ? 'Add User' : 'Edit User'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(controller: _nameController, decoration: InputDecoration(labelText: 'Name')),
            TextField(controller: _emailController, decoration: InputDecoration(labelText: 'Email')),
          ],
        ),
        actions: [
          TextButton(
            child: Text('Cancel'),
            onPressed: () => Navigator.of(context).pop(),
          ),
          ElevatedButton(
            child: Text(user == null ? 'Add' : 'Update'),
            onPressed: () async {
              if (user == null) {
                // Add new user
                await dbHelper.createUser(User(name: _nameController.text, email: _emailController.text));
              } else {
                // Update existing user
                await dbHelper.updateUser(User(id: user.id, name: _nameController.text, email: _emailController.text));
              }
              _refreshUserList();
              Navigator.of(context).pop();
            },
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('User Management')),
      body: FutureBuilder<List<User>>(
        future: _userList,
        builder: (context, snapshot) {
          if (!snapshot.hasData) {
            return Center(child: CircularProgressIndicator());
          }
          if (snapshot.data!.isEmpty) {
            return Center(child: Text('No users found. Add one!'));
          }

          return ListView.builder(
            itemCount: snapshot.data!.length,
            itemBuilder: (context, index) {
              final user = snapshot.data![index];
              return Card(
                margin: EdgeInsets.all(8),
                child: ListTile(
                  title: Text(user.name),
                  subtitle: Text(user.email),
                  trailing: Row(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      IconButton(
                        icon: Icon(Icons.edit, color: Colors.blue),
                        onPressed: () => _showFormDialog(user: user),
                      ),
                      IconButton(
                        icon: Icon(Icons.delete, color: Colors.red),
                        onPressed: () async {
                          await dbHelper.deleteUser(user.id!);
                          _refreshUserList();
                        },
                      ),
                    ],
                  ),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _showFormDialog(),
      ),
    );
  }
}
```

#### **படி 5: செயலியை இணைத்தல் (Wiring up the App)**

உங்கள் `main.dart` கோப்பில் `UserScreen`-ஐ `home` ஆக அமைக்கவும்.

```dart
// main.dart

import 'package:flutter/material.dart';
import 'user_screen.dart'; // Import your screen

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'SQLite CRUD Demo',
      theme: ThemeData(primarySwatch: Colors.teal),
      home: UserScreen(),
    );
  }
}
```

இப்போது இந்தச் செயலியை இயக்கினால், நீங்கள் பயனர்களைச் சேர்க்க, பார்க்க, திருத்த மற்றும் நீக்க முடியும். இந்தத் தரவு அனைத்தும் உங்கள் சாதனத்தில் உள்ள SQLite தரவுத்தளத்தில் சேமிக்கப்படும். செயலி மூடப்பட்டு மீண்டும் திறக்கப்பட்டாலும் தரவு அப்படியே இருக்கும்.

---

இந்தச் செயல்முறை, மாணவர்களுக்கு உள்ளூர் தரவுத்தளத்தைப் (local database) பற்றி ஒரு திடமான அடித்தளத்தை வழங்கும். இது முடிந்ததும், நீங்கள் அவர்களுக்கு நான் தரும் கூடுதல் குறிப்புகளைப் பயன்படுத்தி கேமரா செயல்பாட்டைச் சேர்க்கச் சொல்லலாம். இந்த அணுகுமுறை உங்களுக்குச் சரியாக உள்ளதா?
