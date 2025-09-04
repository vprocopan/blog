# YAML Maps vs Lists: A Quick Guide

YAML uses two fundamental data structures: **maps** (key/value pairs) and **lists** (ordered sequences). Understanding the difference is essential for writing clear, structured configuration files.

---

## 📌 1. Maps (Key → Value Pairs)

Maps are like a **dictionary in Python** or an **object in JSON**.

* Keys must be **unique**.
* Values can be **strings, numbers, booleans, lists, or other maps**.
* Order is usually not important (YAML preserves order, but semantically it doesn’t matter).

**Example (YAML):**

```yaml
person:
  name: Alice
  age: 30
  city: Paris
```

**Equivalent (JSON):**

```json
{ "person": { "name": "Alice", "age": 30, "city": "Paris" } }
```

---

## 📌 2. Lists (Ordered Sequences)

Lists are like a **list in Python** or an **array in JSON**.

* Defined using a leading `-` (dash).
* **Order matters.**
* Items can be strings, numbers, maps, or even other lists.

**Example (YAML):**

```yaml
fruits:
  - apple
  - banana
  - cherry
```

**Equivalent (JSON):**

```json
{ "fruits": ["apple", "banana", "cherry"] }
```

---

## 📌 3. Combining Maps and Lists

YAML allows you to nest maps inside lists and vice versa.

**Example (YAML):**

```yaml
users:
  - name: Alice
    role: admin
  - name: Bob
    role: user
```

Here:

* `users` is a **list**.
* Each list item is a **map** with `name` and `role` keys.

---

## ✅ Quick Comparison

| Feature    | Map (Key/Value)            | List (Ordered Items) |
| ---------- | -------------------------- | -------------------- |
| Structure  | Key → Value pairs          | Sequence of items    |
| Uniqueness | Keys must be unique        | Items can repeat     |
| Order      | Not semantically important | Order matters        |
| Access by  | Key name                   | Index number         |

---

👉 **In short:** Use maps when you need **labeled data** (like config options), and lists when you need an **ordered collection** (like a sequence of items).

---
