## Useful GDB Commands


#### 1. Breakpoint
```bash
(GDB) b [function_name]
(GDB) b [file_name]:[function_name]
(GDB) b [line_num]
(GDB) b 10 if var == 0
```

#### 2. Breakpoint Information
```bash
(GDB) info b
```

#### 3. Remove Breakpoints
```bash
(GDB) cl [function_name]
(GDB) cl # clear all breakpoints
(GDB) disable b 1 3   # disable breakpoint number 1,3
(GDB) enable b 1 3   # enable breakpoint number 1,3
```

#### 4. Processing
```bash
(GDB) r # run program
(GDB) s # run next line. If containing the function, move inside the function and run single line
(GDB) n # run next line. It doesn't step into functions
(GDB) c # continue. Run the program until next breakpoint
(GDB) u # Goes up a level in the stack

```

#### 5. Monitoring the variables
```bash
(GDB) info locals # print current stack's local variables
(GDB) info variables # print all of the global variables
(GDB) info registers # print all of the registers
(GDB) p [variable_name]
```

#### 6. Stack
```bash
(GDB) bt # print stack frame
```
