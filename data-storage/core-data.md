# Core Data

**Core Data** It is an object graph and persistence framework provided by Apple. It provides generalized and automated solutions to common tasks associated with object life cycle and object graph management, including persistence

**Ưu điểm:**

1. Much better memory management
2. When you have changes, you can save only the changed objects, not the entire data set.
3. You can read/write your model objects directly instead of converting them to/from something like an NSDictionary
4. Built-in sorting of objects when you fetch them from the data store.
5. Rich system of predicates for searching your data set
6. Others.[https://stackoverflow.com/a/6377850](https://stackoverflow.com/a/6377850)

**Nhược điểm:**

1. Có thể là 1 framework khá khó học cho người mới
2. Liên quan đến cơ chế của Core Data. Muốn thay đổi dữ liệu của object thì phải load object vào memory ⇒ Muốn xoá 1000 records thì phải load 1000 records đó vào memory để xoá ⇒ Dư thừa

#### NSPersistentContainer

* Tools to load your data model.
* Tools to read and write your data to persistent storage.
* Tools that create a space for you to change your data before saving it.

#### NSManagedObjectContext

* Enables you to fetch specific data objects from persistent storage
* Manages object changes in memory until written to the persistent store
* Allows you to undo changes
* Has the ability to perform data object work in the background
