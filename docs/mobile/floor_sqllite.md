# Floor Database Example

This repository demonstrates how to use the Floor database in a Flutter application. Floor is an SQLite abstraction for Flutter, inspired by Room for Android.


## 1. Setup Dependencies
Add the runtime dependency floor as well as the generator floor_generator to your pubspec.yaml. 
```yml
dependencies:
  flutter:
    sdk: flutter
  floor: ^1.4.0

dev_dependencies:
  floor_generator: ^1.4.0
  build_runner: ^2.1.2

```

## 2. Create an Entity¶
```dart
// entity/person.dart

import 'package:floor/floor.dart';

@entity
class Person {
  @primaryKey
  final int id;

  final String name;

  Person(this.id, this.name);
}

```

##  3. Create a DAO (Data Access Object)¶

```dart
// dao/person_dao.dart

import 'package:floor/floor.dart';

@dao
abstract class PersonDao {
  @Query('SELECT * FROM Person')
  Future<List<Person>> findAllPersons();

  @Query('SELECT * FROM Person WHERE id = :id')
  Stream<Person?> findPersonById(int id);

  @insert
  Future<void> insertPerson(Person person);
}

```

##  4. Create the Database¶

```dart
// database.dart

// required package imports
import 'dart:async';
import 'package:floor/floor.dart';
import 'package:sqflite/sqflite.dart' as sqflite;

import 'dao/person_dao.dart';
import 'entity/person.dart';

part 'database.g.dart'; // the generated code will be there

@Database(version: 1, entities: [Person])
abstract class AppDatabase extends FloorDatabase {
  PersonDao get personDao;
}

```

## 5. Run the Code Generator¶

```cmd
flutter packages pub run build_runner build
```

6. Use the Generated Code¶

```dart
final database = await $FloorAppDatabase.databaseBuilder('app_database.db').build();

final personDao = database.personDao;
final person = Person(1, 'Frank');

await personDao.insertPerson(person);
final result = await personDao.findPersonById(1);

```

## 7. reference 
For more detailed instructions, visit the official documentation: [Floor Documentation](https://pinchbv.github.io/floor/getting-started/)
