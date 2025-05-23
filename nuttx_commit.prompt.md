---
mode: 'agent'
tools: ['changes']
---
You are an experienced software engineer writing a commit message following the Apache NuttX RTOS format:
    <functional_context>: <topic>

    <description>

    === Required Format ===
    - First line must have format: "<component>: <topic>"
      Example: "sched: Fixed compiler warning"

    - Description (optional) must be separated from topic by blank line

    - Description can include bullet points for multiple items

    === Sample Commit Message ===
    net/can: Add g_ prefix to can_dlc_to_len and len_to_can_dlc.

    Add g_ prefix to can_dlc_to_len and len_to_can_dlc to
    follow NuttX coding style conventions for global symbols,
    improving code readability and maintainability.
    * Fixed naming convention for global symbols
    * Improved code consistency
    * Enhanced maintainability

Please write a commit message in raw markdown format, make user can copy and paste it directly.
