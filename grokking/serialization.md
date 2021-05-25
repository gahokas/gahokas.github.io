---
layout: default
---
# 8) How do you serialize an object in java? (page 86)


Serialization is the process of saving an object to a file (or transferring it across a network). The object can then be de-serialized at a later time by a different process to rebuild the object. 

## Default Java serialization mechanism

Java Objects (and primitive types) are serialized by writing them to an OutputStream. The object (and all fields of the object) must implement the Serializable interface. The Serlializable interface doesn't actually have any methods, rather it is just used as a "marker" to indicate the Object may be serialized. Java will encode the object by writing the class of the object and all fields, including references to other objects, to the Output stream so an exact copy of the object can be rebuilt. Static fields are not serialized, as they belong to the class itself and not the object instance. If there are specific non-static fields that you don't want serialized, the transient keyword can be used.

Here's an example of a simple Person object that is serialized to a file, then read from the same file (de-serialized).

```java
public class App {
    public static void main(String[] args) throws Exception {
        System.out.println("Hello, World!");
        
        FileOutputStream fos = new FileOutputStream("file.tmp");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
  
        Person person = new Person("Joe", "Shlabotnik", new Date());

        oos.writeObject(person);
        oos.close();
        fos.close();

        FileInputStream fis = new FileInputStream("file.tmp");
        ObjectInputStream ois = new ObjectInputStream(fis);

        Person person2 = (App.Person) ois.readObject();

        System.out.println("Got this from the file: " + person2.print());

        ois.close();
        fis.close();
    }

    public static class Person implements Serializable {
        private String firstName;
        private String lastName;
        private Date birthDate;

        static final long serialVersionUID = 1L;

        Person(String firstName, String lastName, Date birthDate) {
            this.firstName = firstName;
            this.lastName = lastName;
            this.birthDate = birthDate;
        }

        public String print() {
            return firstName + " " + lastName + " " + birthDate;
        }

    }

}

The app produces the following output:

```
Hello, World!
Got this from the file: Joe Shlabotnik Thu May 20 06:28:53 PDT 2021
```

It is strongly recommended that every object that gets serialized has a declared
```
static final long serialVersionUID
```
Though Java will give an object a default one. This is the "version" of the object, and is stored with the object when it is serialized. It is used to compare the version of the serialized object (in a file) with version defined in the class. If the versions are not the same, the object cannot be de-serialized. 

For example, if we were to serialize a Person object to a file, and then at a later time add a new field to Person, we would change the serialVersionUID of the new Person class. The current definition of Person wouldn't be able to read the old serialized Person files, since the class defintion and serialVersionUID has changed. If we were to attempt to read a version 1 Person file with a version 2 of the code, we would get

```
java.io.InvalidClassException: App$Person; local class incompatible: stream classdesc serialVersionUID = 1, local class serialVersionUID = 2
```

If you attempt to serialize an object that doesn't implement the Serializable interface, or has fields that don't implement Serializable, an java.io.NotSerializableException exception will be thrown when attempting to serialize it.

Finally, as mentioned above, the transient keyword is used to indicate fields that should not be serialized. If we declare last name transient, as
```
public transient String lastname
```
then the output of the program will be
Hello, World!
Got this from the file: Joe null Thu May 20 06:29:07 PDT 2021
```

The lastName field wasn't serialized to the file.
