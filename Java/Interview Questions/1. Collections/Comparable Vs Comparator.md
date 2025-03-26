The main difference between `Comparator` and `Comparable` lies in how they are used to define the sorting behavior for objects in Java. Here's a detailed comparison:

| **Aspect**         | **Comparable**                                                                                                                                                | **Comparator**                                                                                                                                           |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Definition**     | A**Comparable** interface defines the natural ordering of objects within the class itself.                                                                    | A**Comparator** interface defines custom ordering externally to the class.                                                                               |
| **Package**        | `java.lang`                                                                                                                                                       | `java.util`                                                                                                                                                  |
| **Method**         | `compareTo(Object o)`                                                                                                                                             | `compare(Object o1, Object o2)`                                                                                                                              |
| **How It Works**   | - A class implements `Comparable` and overrides the `compareTo`method to define its own sorting logic.``- Sorting is tied to the natural property of the class. | - A separate `Comparator` class is created to implement the `compare` method.``- Multiple comparators can be created to define different sorting criteria. |
| **Example of Use** | - Use when the sorting logic is intrinsic to the class.``- Example:`String` and `Integer` classes implement `Comparable`.                                     | - Use for external or custom sorting logic.``- Example: Sorting people by name, age, or salary externally.                                                     |
| **Flexibility**    | Sorting logic is fixed and directly embedded into the class.                                                                                                        | More flexible, as you can have multiple comparators for different sorting criteria.                                                                            |
| **Usage**          | Directly on objects using `Collections.sort()`or `Arrays.sort()`.                                                                                               | Pass the comparator to `Collections.sort()`or `Arrays.sort()`as an argument.                                                                               |

---

Here are example snippets for both `Comparable` and `Comparator`:

### **Comparable Example**

A `Student` class implementing the `Comparable` interface to define natural ordering based on **roll numbers**.

```java
import java.util.ArrayList;
import java.util.Collections;

class Student implements Comparable<Student> {
    int rollNo;
    String name;

    public Student(int rollNo, String name) {
        this.rollNo = rollNo;
        this.name = name;
    }

    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.rollNo, other.rollNo); // Natural ordering by rollNo
    }

    @Override
    public String toString() {
        return "Student{rollNo=" + rollNo + ", name='" + name + "'}";
    }
}

public class ComparableExample {
    public static void main(String[] args) {
        ArrayList<Student> students = new ArrayList<>();
        students.add(new Student(3, "Alice"));
        students.add(new Student(1, "Bob"));
        students.add(new Student(2, "Charlie"));

        Collections.sort(students); // Sorting based on compareTo
        System.out.println(students); // Output: [Student{rollNo=1, name='Bob'}, ...]
    }
}
```

### **Comparator Example**

A `Student` class with custom sorting logic using the `Comparator` interface to sort students by **name**.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

class Student {
    int rollNo;
    String name;

    public Student(int rollNo, String name) {
        this.rollNo = rollNo;
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{rollNo=" + rollNo + ", name='" + name + "'}";
    }
}

public class ComparatorExample {
    public static void main(String[] args) {
        ArrayList<Student> students = new ArrayList<>();
        students.add(new Student(3, "Alice"));
        students.add(new Student(1, "Bob"));
        students.add(new Student(2, "Charlie"));

        // Define a custom Comparator to sort by name
        Comparator<Student> nameComparator = (s1, s2) -> s1.name.compareTo(s2.name);

        Collections.sort(students, nameComparator); // Sorting based on custom Comparator
        System.out.println(students); // Output: [Student{rollNo=3, name='Alice'}, ...]
    }
}
```

### Key Difference in Usage:

- **Comparable:** Sorting logic is tied to the class (`compareTo` method is within `Student`).
- **Comparator:** Sorting logic is external, enabling different criteria for sorting (e.g., by `rollNo`, `name`, or anything else).

---
