You are an experienced software engineer writing a pull request description for Apache NuttX RTOS:

=== Required Format ===
Summary:
- Why change is necessary (fix, update, new feature)
- What functional part of code is being changed
- How change exactly works
- Related issue/PR references if applicable

Impact:
- New feature added? YES/NO (describe if yes)
- Impact on user? YES/NO (describe if yes)
- Impact on build? YES/NO (describe if yes)
- Impact on hardware? YES/NO (describe if yes)
- Impact on documentation? YES/NO (describe if yes)
- Impact on security? YES/NO (describe if yes)
- Impact on compatibility? YES/NO (describe if yes)

=== Sample PR Description ===
Summary:
* Add support for new RISC-V SiFive board
* Implements basic GPIO and UART drivers
* Adds board configuration and documentation
* Related to issue #1234

Impact:
* New feature added?: YES - Adds new board support
* Impact on user?: NO
* Impact on build?: YES - New board configs added to build system
* Impact on hardware?: YES - New RISC-V board support
* Impact on documentation?: YES - Added board documentation
* Impact on security?: NO
* Impact on compatibility?: NO""",
"summary": """You are an experienced software engineer. Create a one-line summary of the following code changes.
The summary should:
- Follow the format "<module/area>: <Summary of changes>"
- Be concise (maximum 80 characters)
- Clearly state what was changed
- Use present tense (e.g., "Add feature" not "Added feature")
- Be specific about the component and action
- Always include the module/area prefix

=== Sample Summaries ===
"drivers/uart: Add UART driver support for STM32H7 series"
"libc: Fix buffer overflow in strcpy implementation"
"ci: Update CI pipeline to use Python 3.10"
"mm/heap: Optimize memory allocation for small blocks"
