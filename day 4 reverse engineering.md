
# Assembly Code ♫ ♫ ♫ ♫ ♫
* There are 16 General purpose registers
  - %rax - The first return register
  - %rbp - The base pointer that keeps track if the base of the stack
  - %rsp - The stack pointer that points to the top of the stack
### Common Terms
* HEAP - Memory that can be allocated and deallocated
* STACK - A contiguous section of memory used for passing arguments
* GENERAL REGISTER - A multipurpose register that can be used by either programmer or user to store data or a memory location address
* CONTROL REGISTER - A processor register that changes or controls the behavior of a CPU
* FLAGS REGISTER - Contains the current state of the processor
### Memory Offset
* Instruction Pointer - holds address for next instruction to be executed
  - RIP (64-Bit)
  - EIP (Lower 32 Bits)
  - IP (Lower 16 Bits)
### Common Instruction Pointers
* MOV  - move source to destination
* PUSH - push source onto stack
* POP  - Pop top of stack to destination
* INC  - Increment source by 1
* DEC  - Decrement source by 1
* ADD  - Add source to destination
* SUB  - Subtract source from destination
* CMP  - Compare 2 values by subtracting them and setting the %RFLAGS register. ZeroFlag set means they are the same.
* JMP  - Jump to specified location
* JLE  - Jump if less than or equal
* JE   - Jump if equal
### Reverse Engineering Workflow
* Static - use file, strings, cat the file
* Behavioral - Seeing how the program works while its running
* Dissassembly - Debugger
* Document findings - You document findings
# Assembly Code Demo
#### Example 1
```
main:
   mov rax, 16     # Moving into rax the value of 16
   push rax        # Pushing the value in rax (16) onto the stack. RSP is pushed up by 8 bytes (64 bits)
   jmp mem2        # Jump to mem2 function
   
mem1:
   mov rax, 0      # Moving into rax the value of 0 (error free)
   ret             # Return
   
mem2:
  pop r8           # Popping value (16) off of stack and storing in r8. RSP falls by 8 bytes
  cmp rax, r8      # Comparing the rax value (16) the value of r8 (16) they are equal to each pther, the zero flag is set
  je mem1          # Previous comparison ends with zero flag set; jump to mem1; "je" - jump if equal

```
#### Example 2
```
main:
  mov rcx, 25      # Moving into rcx the value of 25
  mov rbx, 62      # Moving into rbx the value of 62
  jmp mem1         # Jump to mem1

mem1:
  sub rbx, 40      # Subtract from the value in rbx  (62) 40. Result is rbx=22
  mov rsi, rbx     # Moving into rsi the value of (22)
  cmp rcx, rsi     # Compare to rcx (25) the value of rsi (22). sign flag is set, overflow flag is not set
                        >if rsi is greater than rcx it crashes 
  jmple mem2       # Zero flag is not set. RSI < RCX. Jump to mem2; "jmple" - jump if less than or equal to

mem2:
  mov rax, 0       # Moving into rax the value of 0 (error free)
  ret              # Return value in the first return register
```

.\downloads\SYSINTERNALS\strings.exe .\downloads\demo1_new.exe  (uses sysinternals suite against exe file) | select -first 1 (see some info)

## analysis method
```
1. run strings against the malware and analyze the garbage 
2. import file to ghidra then double click, say yes to analyze it
3. search string "enter key"
4. double click function on the right
5. go to success and work backwards
