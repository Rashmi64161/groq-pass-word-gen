# 🔐 AI-Powered Password Generator

A Python script that uses **Groq's LLM API** (powered by LLaMA 3.3 70B) to generate strong, randomized passwords of any desired length.

---

## 📋 Overview

Instead of using traditional algorithmic password generation, this script leverages a large language model via the Groq API to produce secure passwords on demand. The generated password is then stored in a Python dictionary (vault) for easy access.

---

## 🧰 Dependencies

- [`groq`](https://pypi.org/project/groq/) — Groq Python SDK for interacting with the API

Install it with:

```bash
pip install groq
```

---

## 🔍 Code Breakdown

### 1. Client Initialization

```python
from groq import Groq
client = Groq(api_key="")
```

Imports the Groq library and initializes the API client. The `api_key` should be filled in with your actual Groq API key.

---

### 2. `generate_password(length=16)` Function

```python
def generate_password(length=16):
```

The core function. Accepts a `length` parameter (default: 16) and returns a generated password string.

**Validation:**
```python
if length < 8:
    raise ValueError("Password length should be at least 8 characters for security reasons.")
```
Enforces a minimum length of 8 characters. Raises a `ValueError` if the input is too short.

**API Call:**
```python
chat_completion = client.chat.completions.create(
    model="llama-3.3-70b-versatile",
    messages=[...]
)
```
Sends a chat request to Groq using the **LLaMA 3.3 70B** model with two messages:
- A **system prompt** that instructs the model to act as a password generator and return only the password.
- A **user prompt** specifying the desired password length.

**Response Extraction:**
```python
password = chat_completion.choices[0].message.content.strip()
return password
```
Extracts the plain text password from the API response and trims any surrounding whitespace.

---

### 3. User Input & Execution

```python
password_length = int(input("Enter desired password length (minimum 8): "))
new_password = generate_password(password_length)
print(f"Generated Password: {new_password}")
```

Prompts the user to enter a desired password length, calls the generator function, and prints the result.

Error handling covers:
- `ValueError` — for invalid lengths (less than 8)
- General `Exception` — for unexpected errors like API failures or network issues

---

### 4. Password Vault (Dictionary Storage)

```python
vault = {
    "generated_password": new_password
}
print(f"Dictionary Storage: {vault}")
```

Stores the generated password in a simple Python dictionary called `vault`. This acts as a basic in-memory store and can be extended to save passwords to a file or database.

---

## 🚀 How to Run

1. Add your Groq API key to the `Groq(api_key="...")` line.
2. Run the script:

```bash
python password_generator.py
```

3. Enter your desired password length when prompted (minimum 8).

**Example output:**

```
Enter desired password length (minimum 8): 20
Generated Password: kR7$mXp2@Lq9#Zt4!Wn8
Dictionary Storage: {'generated_password': 'kR7$mXp2@Lq9#Zt4!Wn8'}
```

---

## 📄 License

MIT License — free to use and modify.
