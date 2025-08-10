In YAML, maps and lists are two fundamental data structures — basically key/value pairs vs ordered sequences.

⸻

📌 1. Map = Key → Value pairs (like a dictionary in Python, object in JSON)
	•	Keys are unique
	•	Values can be strings, numbers, booleans, lists, or even other maps
	•	Order usually doesn’t matter (though YAML preserves it, semantics treat it as unordered)

Example:

person:
  name: Alice
  age: 30
  city: Paris

Equivalent in JSON:

{ "person": { "name": "Alice", "age": 30, "city": "Paris" } }


⸻

📌 2. List = Ordered sequence of items (like an array in JSON, list in Python)
	•	Items are defined with a - dash
	•	Order matters
	•	Items can be strings, numbers, maps, or nested lists

Example:

fruits:
  - apple
  - banana
  - cherry

Equivalent in JSON:

{ "fruits": ["apple", "banana", "cherry"] }


⸻

📌 3. Combined Example

YAML lets you nest maps inside lists and vice versa:

users:
  - name: Alice
    role: admin
  - name: Bob
    role: user

Here:
	•	users is a list
	•	Each list item is a map with name and role keys

⸻

✅ Quick Difference Table

Feature	Map	List
Structure	Key → Value pairs	Ordered items
Uniqueness	Keys must be unique	Items can repeat
Order	Not semantically important	Important
Access by	Key name	Index number


⸻
