This To-Do app will allow users to:
1.  See a list of tasks.
2.  Add a new task.
3.  Mark a task as completed (by tapping on it).
4.  Delete a task.

---

### **பகுதி 4: ஒரு எளிய To-Do செயலியை உருவாக்குதல் (Building a Simple To-Do App)**

#### **படி 1: திட்டத்தின் அமைப்பு (Project Structure)**

நாம் இந்தச் செயலியை ஒரே கோப்பில் (`main.dart`) உருவாக்கப் போகிறோம். இது மாணவர்களுக்குப் புரிந்துகொள்ள எளிதாக இருக்கும்.

இந்தச் செயலிக்கு மூன்று முக்கிய பகுதிகள் தேவை:
1.  **`Task` Model:** ஒவ்வொரு To-Do பணியின் தரவையும் வைத்திருக்க ஒரு வகுப்பு.
2.  **`TodoScreen`:** செயலியின் முக்கியத் திரை (UI). இது ஒரு `StatefulWidget` ஆக இருக்கும், ஏனெனில் பணிகளின் பட்டியல் மாறும்.
3.  **`main` function:** செயலியைத் தொடங்கும் இடம்.

---

#### **படி 2: `Task` மாதிரி வகுப்பை உருவாக்குதல் (Creating the `Task` Model)**

இது ஒரு எளிய Dart வகுப்பு. இது ஒவ்வொரு பணியின் தரவையும் (அதன் பெயர் மற்றும் அது முடிக்கப்பட்டதா இல்லையா) வைத்திருக்கும். இது OOP-ன் ஒரு சிறிய பயன்பாடு.

```dart
// main.dart

// A simple class to represent a single to-do task.
class Task {
  String name;
  bool isDone;

  // Constructor with a default value for isDone.
  Task({required this.name, this.isDone = false});

  // A helper method to toggle the 'done' state.
  void toggleDone() {
    isDone = !isDone;
  }
}
```
**விளக்கம்:**
*   `name`: பணியின் பெயர்.
*   `isDone`: பணி முடிக்கப்பட்டதா இல்லையா என்பதைக் குறிக்கும் ஒரு `bool`.
*   `toggleDone()`: `isDone`-ன் மதிப்பை `true`-லிருந்து `false`-ஆகவும், `false`-லிருந்து `true`-ஆகவும் மாற்றும் ஒரு சிறிய செயல்பாடு.

---

#### **படி 3: முக்கிய UI-ஐ (`TodoScreen`) உருவாக்குதல்**

இதுதான் நம் செயலியின் இதயம். இது ஒரு `StatefulWidget` ஆக இருக்கும், ஏனெனில் பணிகளின் பட்டியல் (`tasks` list) பயனர் செயல்களால் மாறும்.

**ஆரம்ப கட்டமைப்பு (`Initial Structure`):**

```dart
// main.dart

import 'package:flutter/material.dart';

// (Task class from above should be here)

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter To-Do App',
      theme: ThemeData(
        primarySwatch: Colors.indigo,
      ),
      home: TodoScreen(),
    );
  }
}

// This is our main screen widget.
class TodoScreen extends StatefulWidget {
  const TodoScreen({Key? key}) : super(key: key);

  @override
  _TodoScreenState createState() => _TodoScreenState();
}

class _TodoScreenState extends State<TodoScreen> {
  // This list will hold all our Task objects. This is our 'state'.
  final List<Task> _tasks = [
    Task(name: 'Buy milk'),
    Task(name: 'Finish Flutter session notes', isDone: true),
    Task(name: 'Go for a walk'),
  ];
  
  // Controller to get text from the TextField.
  final TextEditingController _taskController = TextEditingController();

  // We will add methods to add, toggle, and delete tasks here.

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('My To-Do List'),
      ),
      body: Column(
        children: [
          // We will build the task list UI here.
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // We will open a dialog to add a new task here.
        },
        child: Icon(Icons.add),
        tooltip: 'Add Task',
      ),
    );
  }
}
```

---

#### **படி 4: பணிகளின் பட்டியலைக் காட்டுதல் (Displaying the List of Tasks)**

நாம் `ListView.builder`-ஐப் பயன்படுத்தி பணிகளின் பட்டியலைக் காட்டப் போகிறோம். இது செயல்திறன் மிக்கது, ஏனெனில் இது திரையில் தெரியும் உருப்படிகளை (items) மட்டுமே உருவாக்கும்.

`_TodoScreenState`-ன் `build` методуக்குள் இருக்கும் `Column`-ஐ மாற்றவும்:
```dart
// Inside _TodoScreenState's build method
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text('My To-Do List'),
    ),
    body: Column(
      children: [
        // This Expanded widget makes the ListView take up all available vertical space.
        Expanded(
          child: ListView.builder(
            itemCount: _tasks.length,
            itemBuilder: (context, index) {
              final task = _tasks[index];
              return ListTile(
                title: Text(
                  task.name,
                  style: TextStyle(
                    decoration: task.isDone
                        ? TextDecoration.lineThrough // Strike-through if done
                        : TextDecoration.none,
                  ),
                ),
                leading: Checkbox(
                  value: task.isDone,
                  onChanged: (value) {
                    // We will add the logic to toggle the task's state here.
                  },
                ),
                trailing: IconButton(
                  icon: Icon(Icons.delete, color: Colors.red),
                  onPressed: () {
                    // We will add the logic to delete the task here.
                  },
                ),
              );
            },
          ),
        ),
      ],
    ),
    // ... (FloatingActionButton remains the same)
  );
}
```
**விளக்கம்:**
*   `ListView.builder`: பட்டியலை உருவாக்கப் பயன்படுகிறது.
*   `itemCount`: பட்டியலில் எத்தனை உருப்படிகள் உள்ளன.
*   `itemBuilder`: ஒவ்வொரு உருப்படியையும் எப்படி வரைய வேண்டும் என்பதை இது கூறுகிறது.
*   `ListTile`: ஒரு அழகான, முன்-வடிவமைக்கப்பட்ட பட்டியல் வரிசை. இதில் `title`, `leading` (இடதுபுறம்), மற்றும் `trailing` (வலதுபுறம்) போன்ற பண்புகள் உள்ளன.
*   `TextDecoration.lineThrough`: பணி முடிந்தால், உரையின் மீது ஒரு கோடு வரைகிறது.

---

#### **படி 5: செயல்பாடுகளைச் சேர்த்தல் (Adding the Functionality)**

இப்போது, பணிகளைச் சேர்க்க, மாற்ற, மற்றும் நீக்க செயல்பாடுகளை (`methods`) `_TodoScreenState`-க்குள் சேர்ப்போம். **முக்கியமாக, UI-ஐப் புதுப்பிக்க நாம் `setState()`-ஐ அழைக்க வேண்டும்.**

**முழுமையான `_TodoScreenState` வகுப்பு:**
```dart
class _TodoScreenState extends State<TodoScreen> {
  final List<Task> _tasks = [
    Task(name: 'Buy milk'),
    Task(name: 'Finish Flutter session notes', isDone: true),
    Task(name: 'Go for a walk'),
  ];
  
  final TextEditingController _taskController = TextEditingController();

  // Method to add a new task
  void _addTask(String name) {
    if (name.isNotEmpty) {
      setState(() {
        _tasks.add(Task(name: name));
      });
      _taskController.clear(); // Clear the text field after adding
      Navigator.of(context).pop(); // Close the dialog
    }
  }

  // Method to toggle the 'done' status of a task
  void _toggleTask(int index) {
    setState(() {
      _tasks[index].toggleDone();
    });
  }

  // Method to delete a task
  void _deleteTask(int index) {
    setState(() {
      _tasks.removeAt(index);
    });
  }

  // Method to show the dialog for adding a new task
  void _showAddTaskDialog() {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text('Add New Task'),
          content: TextField(
            controller: _taskController,
            autofocus: true,
            decoration: InputDecoration(hintText: 'Enter task name'),
          ),
          actions: [
            TextButton(
              child: Text('Cancel'),
              onPressed: () => Navigator.of(context).pop(),
            ),
            ElevatedButton(
              child: Text('Add'),
              onPressed: () => _addTask(_taskController.text),
            ),
          ],
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('My To-Do List'),
      ),
      body: ListView.builder(
        itemCount: _tasks.length,
        itemBuilder: (context, index) {
          final task = _tasks[index];
          return ListTile(
            title: Text(
              task.name,
              style: TextStyle(
                fontSize: 18,
                decoration: task.isDone ? TextDecoration.lineThrough : TextDecoration.none,
                color: task.isDone ? Colors.grey : Colors.black,
              ),
            ),
            leading: Checkbox(
              value: task.isDone,
              onChanged: (value) {
                _toggleTask(index); // Call the toggle method
              },
            ),
            trailing: IconButton(
              icon: Icon(Icons.delete, color: Colors.red[400]),
              onPressed: () {
                _deleteTask(index); // Call the delete method
              },
            ),
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _showAddTaskDialog, // Call the method to show the dialog
        child: Icon(Icons.add),
        tooltip: 'Add Task',
      ),
    );
  }
}
```

---

#### **இறுதி முழுமையான குறியீடு (`main.dart`)**
```dart
import 'package:flutter/material.dart';

// --- Main App Entry Point ---
void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter To-Do App',
      theme: ThemeData(
        primarySwatch: Colors.indigo,
        // Optional: Improve UI look
        appBarTheme: AppBarTheme(
          elevation: 4,
          backgroundColor: Colors.indigo,
          titleTextStyle: TextStyle(color: Colors.white, fontSize: 22, fontWeight: FontWeight.bold)
        ),
        floatingActionButtonTheme: FloatingActionButtonThemeData(
          backgroundColor: Colors.indigo,
        ),
      ),
      debugShowCheckedModeBanner: false, // Hides the debug banner
      home: TodoScreen(),
    );
  }
}

// --- Task Model Class ---
class Task {
  String name;
  bool isDone;

  Task({required this.name, this.isDone = false});

  void toggleDone() {
    isDone = !isDone;
  }
}

// --- To-Do Screen Widget ---
class TodoScreen extends StatefulWidget {
  const TodoScreen({Key? key}) : super(key: key);

  @override
  _TodoScreenState createState() => _TodoScreenState();
}

class _TodoScreenState extends State<TodoScreen> {
  final List<Task> _tasks = [
    Task(name: 'Buy milk'),
    Task(name: 'Finish Flutter session notes', isDone: true),
    Task(name: 'Go for a walk'),
  ];
  
  final TextEditingController _taskController = TextEditingController();

  void _addTask(String name) {
    if (name.isNotEmpty) {
      setState(() {
        _tasks.add(Task(name: name));
      });
      _taskController.clear();
      Navigator.of(context).pop();
    }
  }

  void _toggleTask(int index) {
    setState(() {
      _tasks[index].toggleDone();
    });
  }

  void _deleteTask(int index) {
    setState(() {
      _tasks.removeAt(index);
    });
  }

  void _showAddTaskDialog() {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text('Add New Task'),
          content: TextField(
            controller: _taskController,
            autofocus: true,
            decoration: InputDecoration(hintText: 'Enter task name'),
            onSubmitted: (value) => _addTask(value), // Allows adding with keyboard enter key
          ),
          actions: [
            TextButton(
              child: Text('Cancel'),
              onPressed: () {
                _taskController.clear();
                Navigator.of(context).pop();
              }
            ),
            ElevatedButton(
              child: Text('Add'),
              onPressed: () => _addTask(_taskController.text),
            ),
          ],
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('My To-Do List'),
      ),
      body: _tasks.isEmpty
          ? Center(
              child: Text(
                'No tasks yet. Add one!',
                style: TextStyle(fontSize: 20, color: Colors.grey),
              ),
            )
          : ListView.builder(
              itemCount: _tasks.length,
              itemBuilder: (context, index) {
                final task = _tasks[index];
                return Card( // Using Card for better UI
                  margin: EdgeInsets.symmetric(horizontal: 10, vertical: 5),
                  child: ListTile(
                    title: Text(
                      task.name,
                      style: TextStyle(
                        fontSize: 18,
                        decoration: task.isDone ? TextDecoration.lineThrough : TextDecoration.none,
                        color: task.isDone ? Colors.grey[600] : Colors.black87,
                      ),
                    ),
                    leading: Checkbox(
                      value: task.isDone,
                      onChanged: (value) => _toggleTask(index),
                    ),
                    trailing: IconButton(
                      icon: Icon(Icons.delete_outline, color: Colors.red[700]),
                      onPressed: () => _deleteTask(index),
                    ),
                  ),
                );
              },
            ),
      floatingActionButton: FloatingActionButton(
        onPressed: _showAddTaskDialog,
        child: Icon(Icons.add, color: Colors.white),
        tooltip: 'Add Task',
      ),
    );
  }
}
```

---
