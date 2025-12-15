# CPP Module 07

This repository contains my implementation of **C++ Module 07**, developed as part of the 42 School curriculum. This module introduces C++ templates, one of the most powerful features of the language, enabling generic programming and code reuse through function templates and class templates.

**"C++ templates"**

## Table of Contents

- [Overview](#overview)
- [General Rules](#general-rules)
- [Exercises](#exercises)
  - [Exercise 00: Start with a few functions](#exercise-00-start-with-a-few-functions)
  - [Exercise 01: Iter](#exercise-01-iter)
  - [Exercise 02: Array](#exercise-02-array)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Submission and Peer Evaluation](#submission-and-peer-evaluation)
- [Acknowledgments](#acknowledgments)
- [Disclaimer](#disclaimer)
- [License](#license)

## Overview

C++ is a general-purpose programming language created by **Bjarne Stroustrup** as an extension of the C programming language, or "C with Classes". This module introduces **templates**, which allow writing generic code that works with any data type, promoting code reusability and type safety.

Key topics covered in this module:
- **Function templates** - generic functions that work with multiple types
- **Class templates** - generic classes parameterized by type
- **Template instantiation** - how the compiler generates concrete functions/classes from templates
- **Template specialization** - providing specific implementations for certain types
- **Type deduction** - automatic type inference by the compiler
- **Generic programming** - writing code that works with any type meeting certain requirements

All exercises must be written in **C++98** and compiled using the appropriate flags.

**Important:** Template definitions must be placed in header files (`.hpp` or `.tpp`), as they need to be visible at compile time for instantiation.

## General Rules

- **Compilation:**
  - Compile with `c++ -Wall -Wextra -Werror`
  - Code must still compile with `-std=c++98`

- **Formatting & Naming:**
  - Exercises are stored in directories `ex00`, `ex01`, `ex02`, …  
  - Class names in `UpperCamelCase` (e.g., `Array`)  
  - Source files: `ClassName.cpp`  
  - Header files: `ClassName.hpp` or `ClassName.h`
  - Template implementation files: `ClassName.tpp` (optional)

- **Allowed / Forbidden:**
  - Allowed: most of the C++ standard library
  - Forbidden: external libraries, C++11 or newer, Boost libraries
  - Forbidden functions: `*printf()`, `*alloc()`, `free()`
  - Forbidden keywords: `using namespace <ns_name>`, `friend` (unless explicitly stated)
  - STL containers and algorithms are only allowed starting from Module 08

- **Design Requirements:**
  - All classes must follow the Orthodox Canonical Form (unless stated otherwise)
  - Avoid memory leaks when using `new` / `delete`
  - Each header must be self-contained and protected by include guards
  - Function implementations in headers are allowed ONLY for templates

## Exercises

### Exercise 00: Start with a few functions

**Directory:** `ex00/`  
**Files to turn in:** `Makefile`, `main.cpp`, `whatever.{h,hpp}`

Implement three function templates that work with any type.

**Function templates to implement:**

**1. swap**
```cpp
template<typename T>
void swap(T& a, T& b);
```
- Swaps the values of two given parameters
- Returns nothing

**2. min**
```cpp
template<typename T>
T min(T const & a, T const & b);
```
- Compares two values and returns the smallest
- If equal, returns the second one

**3. max**
```cpp
template<typename T>
T max(T const & a, T const & b);
```
- Compares two values and returns the greatest
- If equal, returns the second one

**Requirements:**
- Functions work with any type
- Both arguments must have the same type
- Type must support all comparison operators
- Templates must be defined in header files

**Example usage:**
```cpp
int a = 2;
int b = 3;
::swap(a, b);
std::cout << "a = " << a << ", b = " << b << std::endl;
std::cout << "min(a, b) = " << ::min(a, b) << std::endl;
std::cout << "max(a, b) = " << ::max(a, b) << std::endl;

std::string c = "chaine1";
std::string d = "chaine2";
::swap(c, d);
std::cout << "c = " << c << ", d = " << d << std::endl;
```

**Expected output:**
```
a = 3, b = 2
min(a, b) = 2
max(a, b) = 3
c = chaine2, d = chaine1
min(c, d) = chaine1
max(c, d) = chaine2
```

**Key Learning:** Basic function templates, type deduction, template instantiation.

### Exercise 01: Iter

**Directory:** `ex01/`  
**Files to turn in:** `Makefile`, `main.cpp`, `iter.{h,hpp}`

Implement a function template that applies a function to every element of an array.

**Function template to implement:**

```cpp
template<typename T, typename F>
void iter(T* array, size_t length, F func);
```

**Parameters:**
- `array` - address of an array
- `length` - length of the array
- `func` - function to be called on every element

**Requirements:**
- Works with any type of array
- Third parameter can be an instantiated function template
- Function passed as third parameter may take argument by:
  - `const T&` for read-only operations
  - `T&` for operations that modify the element

**Important consideration:** Think carefully about how to support both const and non-const elements in your iter function. You may need to provide multiple template versions or use template specialization.

**Example usage:**
```cpp
template<typename T>
void print(T const & x) {
    std::cout << x << std::endl;
}

int main() {
    int array[] = {1, 2, 3, 4, 5};
    iter(array, 5, print<int>);
}
```

**Key Learning:** Function pointers and templates, template parameters for functions, const correctness with templates.

### Exercise 02: Array

**Directory:** `ex02/`  
**Files to turn in:** `Makefile`, `main.cpp`, `Array.{h,hpp}` and optional `Array.tpp`

Develop a class template Array that contains elements of type T.

**Class template:**
```cpp
template<typename T>
class Array;
```

**Constructors:**
- **Default constructor:** Creates an empty array
- **Parameterized constructor:** `Array(unsigned int n)` - creates array of n elements initialized by default
- **Copy constructor:** Deep copy of the array
- **Assignment operator:** Deep copy assignment

**Important:** After copying, modifying either the original or the copy must NOT affect the other array.

**Member functions:**

**Subscript operator:**
```cpp
T& operator[](unsigned int index);
const T& operator[](unsigned int index) const;
```
- Access elements with `[]` operator
- Throws `std::exception` if index is out of bounds

**Size function:**
```cpp
unsigned int size() const;
```
- Returns number of elements in the array
- Does not modify the current instance

**Memory management requirements:**
- MUST use `new[]` to allocate memory
- Preventive allocation (allocating in advance) is forbidden
- Must never access non-allocated memory
- Proper cleanup in destructor

**Tip:** Try compiling `int * a = new int();` and display `*a` to understand default initialization.

**Example usage:**
```cpp
Array<int> numbers(5);
numbers[0] = 42;
numbers[1] = 21;

Array<int> copy = numbers;  // Deep copy
copy[0] = 100;  // Does not affect numbers[0]

try {
    numbers[10] = 0;  // Throws exception
} catch (std::exception& e) {
    std::cout << "Out of bounds!" << std::endl;
}
```

**Key Learning:** Class templates, template member functions, deep copy with templates, exception handling in templates, RAII principles.

## Project Structure

```
CPP07/
├── ex00/
│   ├── Makefile
│   ├── main.cpp
│   └── whatever.hpp
├── ex01/
│   ├── Makefile
│   ├── main.cpp
│   └── iter.hpp
├── ex02/
│   ├── Makefile
│   ├── main.cpp
│   ├── Array.hpp
│   └── Array.tpp (optional)
└── README.md
```

## Installation

1. **Clone the Repository:**

   ```sh
   git clone https://github.com/marco-ht/CPP07.git
   cd CPP07
   ```

2. **Build the Desired Exercise:**

   ```sh
   cd ex00 && make
   ```

## Usage

Each exercise produces its own executable. Navigate to the exercise directory and run:

**Exercise 00:**
```sh
./templates
```
Demonstrates swap, min, and max templates with different types.

**Exercise 01:**
```sh
./iter
```
Demonstrates applying functions to array elements.

**Exercise 02:**
```sh
./array
```
Demonstrates the templated Array class with various operations.

## Submission and Peer Evaluation

- Submit your project through the designated Git repository.
- Only the files within your Git repository will be evaluated during defense.
- Double-check that all file names and structures match the subject requirements.
- Ensure your Makefile compiles without relinking and includes all required rules.
- Pay special attention to:
  - Template definitions in headers
  - Proper const correctness
  - Deep copy implementation
  - Exception handling
  - Memory management (no leaks)

## Acknowledgments

- Thanks to the 42 community, mentors, and fellow students for their guidance and support.
- Special thanks to resources on C++ templates and generic programming.

## Disclaimer

**IMPORTANT**:
This project was developed solely as part of the educational curriculum at 42 School. The code in this repository is published only for demonstration purposes to showcase my programming and C++ development skills.

**Academic Integrity Notice**:
It is strictly prohibited to copy or present this code as your own work in any academic submissions at 42 School. Any misuse or uncredited use of this project will be considered a violation of 42 School's academic policies.

## License

This repository is licensed under the Creative Commons - NonCommercial-NoDerivatives (CC BY-NC-ND 4.0) license. You are free to share this repository as long as you:

- Provide appropriate credit.
- Do not use it for commercial purposes.
- Do not modify, transform, or build upon the material.

For further details, please refer to the CC BY-NC-ND 4.0 license documentation.

Developed with dedication and in strict adherence to the high standards of 42 School.
