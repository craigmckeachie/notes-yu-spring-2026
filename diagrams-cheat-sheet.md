This markdown cheat sheet is designed to be shared directly with your students. It covers everything from basic setup to the nuances of "gluing" lines properly.

---

# 📋 Diagrams.net Class Diagram Cheat Sheet

## 1. Setup & Tools

1. Go to **[app.diagrams.net](https://app.diagrams.net)**.
2. Click **+ More Shapes** (bottom left).
3. Check the box for **UML** and click **Apply**.
4. Use the **Search shapes** box and type "Class" to find the standard 3-row box.

---

## 2. The Class Anatomy

Each class should use the **3-row rectangle**:

* **Top:** Class Name (Bold, TitleCase).
* **Middle:** Attributes (Fields/Variables).
* **Bottom:** Methods (Functions).

**Visibility Prefix:**

* `+` **Public**: Accessible everywhere.
* `-` **Private**: Accessible only inside this class.
* `#` **Protected**: Accessible by this class and its children.

---

## 3. Connecting Classes (The Right Way)

To ensure lines stay attached when you move your classes, you must **"Glue"** them.

### How to "Glue" a Line:

1. **Start:** Hover over the center of your "Source" class. Click and drag the **blue arrow** that appears.
2. **Finish:** Drag the line to the "Target" class. Wait for the target class to be outlined in a **blue or green highlight** before letting go.
3. **Test:** Move one of the classes. If the line stretches to follow it, it is successfully glued.

### Styling the Relationship:

Click on a line, then use the **Style Panel** (on the right) to change the line ends.

| Relationship | Line Style | End Arrow Style | Meaning |
| --- | --- | --- | --- |
| **Inheritance** | Solid | **Large Empty Triangle** | "Is a" (Dog is an Animal). |
| **Realization** | **Dashed** | **Large Empty Triangle** | Implementing an Interface. |
| **Association** | Solid | **Open V-Arrow** | General "uses" relationship. |
| **Composition** | Solid | **Filled Diamond** | "Part of" (Room cannot exist without House). |
| **Aggregation** | Solid | **Empty Diamond** | "Has a" (Library has Books). |

---

## 4. Multiplicity & Labels

Multiplicity defines how many instances of one class are linked to another (e.g., one student has many courses).

1. **Double-click** the line near the end where you want the label.
2. Type the value:
* `1` : Exactly one.
* `0..1` : Zero or one.
* `*` or `0..*` : Many.
* `1..*` : One or many.


3. **Drag** the text box slightly to the side of the line for better readability.

---

## 5. Pro-Tips for Clean Diagrams

* **Straighten Lines:** If a line has too many bends, right-click it and select **Line Waypoints > Clear Waypoints**.
* **Avoid Overlaps:** Use the **Arrange > Layout > Tree** menu to automatically organize messy diagrams.
* **Fast Copy:** Select a class and hold **Ctrl (or Cmd)** while dragging to clone it instantly.
* **Exporting:** When finished, go to **File > Export as > PNG**. Ensure **"Crop"** is checked so your image doesn't have a giant white border.