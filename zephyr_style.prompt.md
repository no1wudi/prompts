# Zephyr Coding Style Guidelines

Follow these style guidelines for all contributions to the Zephyr project.

## General Guidelines
- Line length: 100 columns or fewer (except for long URLs in documentation)
  ```c
  /* Good: Line under 100 columns */
  int calculate_checksum(const uint8_t *data, size_t len);

  /* Bad: Line exceeds 100 columns */
  int very_long_function_name_that_unnecessarily_exceeds_the_line_length_limit(const uint8_t *very_long_parameter_name, size_t length_of_data);
  ```

- Apply style to new or modified code only; you're not expected to fix existing code style
- Follow the style of nearby code when guidelines permit multiple approaches
- Avoid using non-ASCII symbols in code unless it significantly improves clarity
  ```c
  /* Good */
  #define PI 3.14159

  /* Bad */
  #define π 3.14159
  ```

- Avoid emojis in all cases
- Use proper capitalization of nouns in comments (e.g., UART not uart, CMake not cmake)
  ```c
  /* Good: Proper capitalization */
  /* Initialize UART for communication */

  /* Bad: Improper capitalization */
  /* initialize uart for communication */
  ```

## C Code Style
- Use snake_case for code and variables
  ```c
  /* Good */
  int calculate_checksum(const uint8_t *data, size_t len);
  uint16_t packet_length;

  /* Bad */
  int CalculateChecksum(const uint8_t *data, size_t len);
  uint16_t packetLength;
  ```

- Add braces to every `if`, `else`, `do`, `while`, `for`, and `switch` body, even for single-line blocks
  ```c
  /* Good */
  if (condition) {
      do_something();
  }

  /* Bad */
  if (condition)
      do_something();
  ```

- Use spaces instead of tabs to align comments after declarations
  ```c
  /* Good */
  int foo;      /* Comment about foo */
  long buffer;  /* Comment about buffer */

  /* Bad (using tabs) */
  int foo;→→→/* Comment about foo */
  long buffer;→/* Comment about buffer */
  ```

- Use C89-style comments `/*  */` (C99-style `//` is not allowed)
  ```c
  /* Good: C89-style comment */

  // Bad: C99-style comment
  ```

- Use `/**  */` for doxygen comments that need to appear in documentation
  ```c
  /**
   * @brief Calculate checksum over a data buffer
   *
   * @param data Pointer to data buffer
   * @param len Length of data in bytes
   * @return checksum value
   */
  int calculate_checksum(const uint8_t *data, size_t len);
  ```

- Avoid binary literals (constants starting with `0b`)
  ```c
  /* Good */
  #define FLAG_ACTIVE    0x01
  #define FLAG_READY     0x02

  /* Bad */
  #define FLAG_ACTIVE    0b00000001
  #define FLAG_READY     0b00000010
  ```

## CMake Style
- Indentation: 2 spaces (no tabs)
  ```cmake
  if(ENABLE_FEATURE)
    add_subdirectory(feature)
    set(FEATURE_ENABLED TRUE)
  endif()
  ```

- No space before opening brackets: `if(...)` not `if (...)`
  ```cmake
  # Good
  if(ENABLE_TESTS)
    add_subdirectory(tests)
  endif()

  # Bad
  if (ENABLE_TESTS)
    add_subdirectory(tests)
  endif ()
  ```

- Lowercase CMake commands: `add_library()` not `ADD_LIBRARY()`
  ```cmake
  # Good
  add_library(mylib STATIC source.c)

  # Bad
  ADD_LIBRARY(mylib STATIC source.c)
  ```

- Break file arguments across multiple lines for readability
  ```cmake
  # Good
  target_sources(app PRIVATE
    src/main.c
    src/helper.c
    src/module.c
  )

  # Bad
  target_sources(app PRIVATE src/main.c src/helper.c src/module.c)
  ```

- Naming conventions:
  - UPPERCASE for cache variables or variables shared across files
    ```cmake
    set(ZEPHYR_BASE "${CMAKE_CURRENT_SOURCE_DIR}")
    option(CONFIG_NETWORKING "Enable networking stack" ON)
    ```
  - lowercase/snake_case for local variables
    ```cmake
    set(source_dir "${CMAKE_CURRENT_SOURCE_DIR}/src")
    set(binary_dir "${CMAKE_CURRENT_BINARY_DIR}/bin")
    ```
  - Use consistent prefixes to avoid name conflicts
    ```cmake
    set(APP_VERSION_MAJOR 1)
    set(APP_VERSION_MINOR 0)
    ```

- Quote strings and variables but not boolean values
  ```cmake
  # Good
  set(source_dir "${CMAKE_CURRENT_SOURCE_DIR}/src")
  option(ENABLE_TESTS "Enable tests" ON)

  # Bad
  set(source_dir ${CMAKE_CURRENT_SOURCE_DIR}/src)
  option(ENABLE_TESTS "Enable tests" "ON")
  ```

- Use CMake variables instead of hardcoding paths
  ```cmake
  # Good
  set(output_dir "${CMAKE_BINARY_DIR}/output")

  # Bad
  set(output_dir "/path/to/build/output")
  ```

- Use comments to document complex logic
  ```cmake
  # Check if compiler supports specific flag
  include(CheckCCompilerFlag)
  check_c_compiler_flag("-Wextra" HAS_WEXTRA)
  if(HAS_WEXTRA)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wextra")
  endif()
  ```

## Devicetree Style
- Indent with tabs
  ```dts
  / {
  →	node {
  →→		property = "value";
  →	};
  };
  ```

- Follow the Devicetree specification conventions and rules
- Follow Linux kernel Devicetree rules when applicable
- Use dashes (`-`) as word separators for node and property names
  ```dts
  /* Good */
  flash-controller {
      erase-block-size = <4096>;
  };

  /* Bad */
  flash_controller {
      erase_block_size = <4096>;
  };
  ```

- Use underscores (`_`) as word separators in node labels
  ```dts
  /* Good */
  uart0: uart_controller@40002000 {
      compatible = "vendor,uart";
  };

  /* Bad */
  uart0: uart-controller@40002000 {
      compatible = "vendor,uart";
  };
  ```

- Leave a single space on each side of the equal sign (`=`) in property definitions
  ```dts
  /* Good */
  clock-frequency = <100000000>;

  /* Bad */
  clock-frequency=<100000000>;
  ```

- Don't insert empty lines before a dedenting `};`
  ```dts
  /* Good */
  spi {
      compatible = "vendor,spi";
      status = "okay";
  };

  /* Bad */
  spi {
      compatible = "vendor,spi";
      status = "okay";

  };
  ```

- Use a single empty line to separate nodes at the same hierarchy level
  ```dts
  uart0: uart@40002000 {
      compatible = "vendor,uart";
      status = "okay";
  };

  spi0: spi@40003000 {
      compatible = "vendor,spi";
      status = "okay";
  };
  ```

- Optionally separate related property groups with one empty line for readability
  ```dts
  flash0: flash@8000000 {
      compatible = "vendor,flash";
      reg = <0x08000000 0x100000>;

      partitions {
          compatible = "fixed-partitions";
          #address-cells = <1>;
          #size-cells = <1>;

          boot_partition: partition@0 {
              label = "bootloader";
              reg = <0x0 0x10000>;
          };
      };
  };
  ```

## Kconfig Style
- Line length: 100 columns or fewer
- Indent with tabs, except for `help` entry text (one tab plus two spaces)
  ```kconfig
  config EXAMPLE_FEATURE
  →	bool "Example feature"
  →	help
  →	  This is help text for the example feature.
  →	  Note the indentation with one tab plus two spaces.
  ```

- Leave a single empty line between option declarations
  ```kconfig
  config FEATURE_A
  	bool "Feature A"
  	default y
  	help
  	  Enable feature A

  config FEATURE_B
  	bool "Feature B"
  	default n
  	help
  	  Enable feature B
  ```

- Format comments as `# Comment` (with a space) not `#Comment`
  ```kconfig
  # Good: This is a comment with a space after #
  config EXAMPLE
  	bool "Example"

  #Bad: This comment has no space after the hash
  config EXAMPLE2
  	bool "Example 2"
  ```

- Insert an empty line before/after each top-level `if` and `endif` statement
  ```kconfig
  config BASIC_FEATURE
  	bool "Basic feature"

  if NETWORKING

  config NET_FEATURE
  	bool "Networking feature"
  	depends on NETWORKING

  endif # NETWORKING

  config ANOTHER_FEATURE
  	bool "Another feature"
  ```

- Use `select` statements carefully
  ```kconfig
  # Only use select when one option logically implies another
  config USB_CDC_ACM
  	bool "USB CDC ACM class driver"
  	select SERIAL
  	help
  	  USB CDC ACM implementation requires serial subsystem
  ```
