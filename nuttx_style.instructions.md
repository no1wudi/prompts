# Coding Style Guide

## General Conventions
- **File Extensions**: `.h` for C headers, `.c` for C source
- **Line Endings**: Unix-style (`\n`), no trailing whitespace
- **Line Width**: 78 columns maximum
- **Indentation**: 2 spaces per level, no tabs in C files
```c
if (condition)
  {
    /* Indented by 2 spaces */

    dosomething();
  }
```
- **Spacing**: Single blank line between logical sections
- **Block Comments**: Special format for grouping sections
```c
/****************************************************************************
 * Included Files
 ****************************************************************************/
```

## File Organization
- **File Header**: Include relative path, optional description, and Apache 2.0 license
```c
/****************************************************************************
 * examples/example/example.c
 *
 * Licensed to the Apache Software Foundation (ASF) under one or more
 * contributor license agreements.  See the NOTICE file distributed with
 * this work for additional information regarding copyright ownership.  The
 * ASF licenses this file to you under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance with the
 * License.  You may obtain a copy of the License at
 *
 *   http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
 * WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
 * License for the specific language governing permissions and limitations
 * under the License.
 *
 ****************************************************************************/
```
- **Standard Sections**:
  - Included Files
  - Pre-processor Definitions
  - Private/Public Types
  - Private Function Prototypes
  - Private/Public Data
  - Private/Public Functions
- **Header File Idempotence**: Use include guards
```c
#ifndef __INCLUDE_NUTTX_EXAMPLE_H
#define __INCLUDE_NUTTX_EXAMPLE_H

/* Header content */

#endif /* __INCLUDE_NUTTX_EXAMPLE_H */
```

## Comments
- **Grammar**: Standard English with proper punctuation
- **Single Line**: `/* Comment with period. */`
- **Multi-line**:
```c
/* First line of comment.
 * Subsequent lines start with asterisk.
 * Final line ends with period.
 */
```
- **Right-hand Comments**: Discouraged but if used should be aligned
```c
int  x;      /* Variable x. */
int  y;      /* Variable y. */
long value;  /* Long value. */
```
- **Blank Lines After Comments**: Always add a blank line after any comment line or comment block
```c
/*
 * This is a comment block, blank line is required after it.
 * This is the second line of the comment block.
 */

int some_variable;

/* Single line comment */

if (condition)
  {
    /* Comment inside a block. */

    dosomething();
  }
```

## Braces
- **Always on Separate Lines**: Braces appear alone on their lines
```c
if (condition)
  {
    dosomething();
  }
else
  {
    dosomethingelse();
  }
```
- **Indentation**:
  - Statements indented +2 spaces from the containing block
  - Opening brace at same indentation as control statement
- **Blank Line After Closing Braces**:
  - Insert blank line after closing braces, except for if-else constructs
```c
/* With blank line after block */

while (condition)
  {
    dosomething();
  }

nextstatement();

/* Exception for if-else, no blank line needed */

if (condition)
  {
    dosomething();
  }
else
  {
    dosomethingelse();
  }

/* For nested if-else, maintain structure */

if (condition1)
  {
    if (condition2)
      {
        dosomething();
      }
    else
      {
        dosomethingelse();
      }
  }
else
  {
    doanotheraction();
  }

nextstatement();
```
- **Exception**: Structure/enum definitions align braces with the type name
```c
struct example_s
{
  int field1;  /* Field 1. */
  int field2;  /* Field 2. */
};
```

## Variables & Types
- **One Definition Per Line**: Never combine declarations
```c
/* Incorrect */
int a, b, c;

/* Correct */
int a;
int b;
int c;
```
- **Global Variables**: Prefix with `g_`, use all lowercase
```c
static int g_count;
int g_module_value;
```
- **Parameters/Local Variables**: Short, descriptive lowercase names
```c
void process_data(int size, FAR uint8_t *buffer)
{
  int i;
  int result = 0;

  /* Function body */
}
```
- **Type Definitions**: End with `_t` suffix
```c
typedef uint32_t example_value_t;
```
- **Structures**: End with `_s` suffix
```c
struct device_s
{
  int id;           /* Device ID. */
  bool initialized; /* Initialization flag. */
};
```
- **Unions**: End with `_u` suffix
```c
union register_u
{
  uint32_t w;      /* Word access. */
  uint16_t h[2];   /* Half-word access. */
  uint8_t  b[4];   /* Byte access. */
};
```
- **Enumerations**: End with `_e` suffix and use uppercase values
```c
enum state_e
{
  STATE_INIT = 0,   /* Initial state. */
  STATE_RUNNING,    /* Running state. */
  STATE_STOPPED     /* Stopped state. */
};
```

## Functions
- **Naming**: All lowercase, module prefix, descriptive verb-object format
```c
int module_process_data(int size, FAR uint8_t *buffer);
bool module_is_ready(void);
```
- **Headers**: Specific block comment format with Name, Description, Input Parameters, etc.
```c
/****************************************************************************
 * Name: module_process_data
 *
 * Description:
 *   Process the data buffer with module-specific algorithm.
 *
 * Input Parameters:
 *   size   - Size of the buffer in bytes
 *   buffer - Pointer to data buffer
 *
 * Returned Value:
 *   Number of bytes processed or negative errno on failure
 *
 ****************************************************************************/
```
- **Body**: Local variables first, statements after, return without parentheses
```c
int module_process_data(int size, FAR uint8_t *buffer)
{
  int i;
  int result = 0;

  if (buffer == NULL)
    {
      return -EINVAL;
    }

  for (i = 0; i < size; i++)
    {
      result += buffer[i];
    }

  return result;
}
```
- **Parameters**: `const` encouraged for read-only parameters
```c
int module_checksum(int size, FAR const uint8_t *buffer);
```
- **Return Values**: Errors as negative errno values
```c
if (condition_failed)
  {
    return -EINVAL;  /* Invalid argument */
  }
```

## Statements
- **One Per Line**: No multiple statements on a single line
```c
/* Incorrect */

if (x < 0) { x = 0; y = 0; }

/* Correct */

if (x < 0)
  {
    x = 0;
    y = 0;
  }
```
- **Spaces**: Space before/after binary operators, no space with unary operators
```c
a = b + c;      /* Space around binary operators */
a++;            /* No space with unary operators */
if (a == b)     /* Space after if */
```
- **Control Statements**: Always use braces even for single statements
```c
if (x < 0)
  {
    x = 0;
  }
```
- **goto**: Limited to error handling only, labels at column 1
```c
int function(void)
{
  int ret = OK;

  /* Do something */

  if (error_condition)
    {
      ret = -ENOMEM;
      goto errout;
    }

  return ret;

errout:
  /* Clean up */

  return ret;
}
```

## C++ Guidelines
- Use CamelCase for names
```cpp
class CMyClass
{
public:
  void doSomething();

private:
  int myVariable;
};
```
- `.cxx` for source files, `.hxx` for headers
- Variables/methods start with lowercase
```cpp
int myVariable;
void doSomething();
```
- Classes/namespaces start with uppercase
```cpp
namespace MyNamespace
{
  class CMyClass
  {
    // Class definition
  };
}
```

## Pointer Qualifiers
- Use `FAR` for pointers to data in stack/heap/bss/data in common code (e.g., libc, libpthread, device drivers, middleware, etc.)
```c
FAR uint8_t *buffer;
```
- Use `CODE` for function pointers in common code
```c
CODE void (*callback)(void);
```
- Neither `FAR` nor `CODE` is necessary in platform-specific code such as low-level architecture support code for ARM, RISC-V, Xtensa etc.
```c
/* In arch-specific code */
uint8_t *buffer;
void (*callback)(void);
```

## Structure Comment Block
Structure declarations should be preceded with a descriptive comment block that explains the purpose of the structure:

```c
/****************************************************************************
 * Name: example_device_s
 *
 * Description:
 *   This structure defines the interface to an example device driver.
 *   It contains all the information needed to identify and control
 *   the device.
 *
 ****************************************************************************/

struct example_device_s
{
  int         id;       /* Unique device identifier. */
  FAR void   *priv;     /* Private driver data. */
  bool        enabled;  /* Device enabled state. */
  uint8_t     options;  /* Configuration options. */
  FAR char   *name;     /* Device name string. */
};
```

## Avoid
- C++ style comments (`//`) in C code
- Tabs in C files
- Multiple assignments in a single statement
```c
/* Incorrect */

a = b = c = 0;

/* Correct */

a = 0;
b = 0;
c = 0;
```
- Unnecessary parentheses
```c
/* Incorrect */

return (result);

/* Correct */

return result;
```
- Doxygen tags
- Magic numbers (use named constants)
```c
/* Incorrect */

if (status == 5)

/* Correct */

#define STATUS_COMPLETE 5

/* ... */

if (status == STATUS_COMPLETE)
```
