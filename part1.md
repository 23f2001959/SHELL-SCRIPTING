# Week 7 – Bash Shell Scripts Explanation

---

# 1. Using `bc` Calculator (Floating-Point Arithmetic)

### Code

```bash
#!/bin/bash
a=2.5
b=3.2
c=4

d=$(bc -l << EOF
scale=5
($a+$b)^$c
EOF
)

echo $d
```

### Output

```
1057.4401
```

### Explanation

* Bash **cannot handle floating-point arithmetic directly**.
* `bc` (Basic Calculator) is used for **decimal and scientific calculations**.
* `-l` loads the **math library**.
* `<<EOF` creates a **heredoc**, allowing multi-line input to `bc`.
* `scale=5` sets the **precision to 5 decimal places**.
* `$()` captures the output of `bc`.

### Key Points

* `bc` is used for **floating-point operations**
* `scale` controls decimal precision
* `$()` stores command output in a variable
* `<<EOF` is called **heredoc input**

---

# 2. Looping Through `$PATH` Using `IFS`

### Code

```bash
#!/bin/bash
i=0
IFS=:

for n in $PATH
do
    echo $i $n
    (( i++ ))
done
```

### Output

```
0 /usr/local/sbin
1 /usr/local/bin
2 /usr/sbin
3 /usr/bin
4 /sbin
5 /bin
```

### Explanation

* `$PATH` contains directories separated by **colon (`:`)**.
* `IFS` stands for **Internal Field Separator**.
* Setting `IFS=:` tells Bash to split `$PATH` at `:`.
* Each directory is processed in the loop.
* `((i++))` increments the counter.

### Key Points

* `IFS` controls **how strings are split**
* `$PATH` stores executable directories
* `(( ))` performs arithmetic operations

---

# 3. C-Style `for` Loop in Bash

### Syntax

```bash
for (( initialization; condition; increment ))
do
    commands
done
```

### Explanation

This loop works like **C / Java loops**.

Example flow:

```
initialization → condition → execute → increment → repeat
```

### Key Points

* Uses **double parentheses**
* Supports **multiple variables**
* Similar to loops in **C, C++, Java**

---

# 4. Printing Squares Using `for` Loop

### Code

```bash
#!/bin/bash
begin=1
finish=10

for (( a=$begin; a<$finish; a++ ))
do
    b=$(( a**2 ))
    echo $b
done
```

### Output

```
1
4
9
16
25
36
49
64
81
```

### Explanation

* Loop runs from **1 to 9**.
* `a**2` calculates square of `a`.
* `$(( ))` performs arithmetic expansion.

### Key Points

* `**` is **power operator**
* `$(( ))` evaluates arithmetic expressions

---

# 5. Multiple Variables in `for` Loop

### Code

```bash
#!/bin/bash
begin1=1
begin2=20
finish=20

for (( a=$begin1, b=$begin2; a<$finish; a++, b-- ))
do
    c=$(( a**2 ))
    d=$(( b**2 ))
    echo $c $d
done
```

### Output (first few lines)

```
1 400
4 361
9 324
16 289
...
```

### Explanation

* Two loop variables:

  * `a` increases
  * `b` decreases
* Each iteration prints their squares.

### Key Points

* Multiple variables separated by **comma**
* `a++` increments
* `b--` decrements

---

# 6. `case` Statement (Pattern Matching)

### Code

```bash
#!/bin/bash

echo "what is your favorite image processor?"
read pname

case $pname in
    [gG]imp | inkscape)
        echo "good choice"
        ;;
    [aA]dobe*)
        echo "that costs a lot and needs cloud"
        ;;
    imagej)
        echo "measuring things on the image?"
        ;;
    *)
        echo "I did not know $pname could do image stuff"
        ;;
esac
```

### Output Example

```
what is your favorite image processor?
Gimp
good choice
```

### Explanation

* `case` is used for **pattern matching**.
* `[gG]imp` matches `gimp` or `Gimp`.
* `|` means **OR** between patterns.
* `*` is a wildcard.

### Key Points

* Ends with **`esac`**
* `;;` terminates each block
* Cleaner than long `if-elif` chains

---

# 7. Checking Number of Arguments (`$#`)

### Code

```bash
#!/bin/bash

if [ $# -gt 2 ]
then
    echo arguments given and more than sufficient $#
elif [ $# -gt 1 ]
then
    echo arguments given and sufficient $#
elif [ $# -gt 0 ]
then
    echo arguments given but not sufficient $#
else
    echo arguments are needed $#
fi
```

### Explanation

`$#` stores **number of command-line arguments**.

Example:

```
./script.sh a b c
```

Output:

```
arguments given and more than sufficient 3
```

### Key Points

* `$#` → number of arguments
* `-gt` → greater than
* `-lt` → less than

---

# 8. File Check and Write Loop

### Code

```bash
#!/bin/bash

filename=largefile.txt

if [ -e $filename ]
then
    echo file $filename exists
    exit 1
fi

i=1
while [ $i -lt 10 ]
do
    echo $i $((i+1))
    ((i++))
done > $filename

echo file $filename written
ls -l $filename
```

### Explanation

* `-e` checks if file exists.
* If it exists → script exits.
* `done > file` redirects **entire loop output** to file.

### Key Points

* `-e` → file exists test
* `>` → redirect output
* `exit 1` stops script

---

# 9. `break` in Loop

### Code

```bash
#!/bin/bash
n=10
i=0

while [ $i -lt $n ]
do
    echo $i
    ((i++))

    if [ $i -eq 5 ]
    then
        echo "5 is bad news"
        break
    fi
done
```

### Output

```
0
1
2
3
4
5 is bad news
```

### Explanation

* Loop normally runs 0-9.
* When `i=5`, `break` stops loop immediately.

### Key Points

* `break` exits loop
* Works inside `for`, `while`, `until`

---

# 10. `break 2` (Nested Loop Exit)

### Code

```bash
#!/bin/bash
n=10
i=0

while [ $i -lt $n ]
do
    echo $i
    j=0

    while [ $j -le $i ]
    do
        printf "$j "
        ((j++))

        if [ $i -eq 7 ]
        then
            break 2
        fi
    done

    ((i++))
done
```

### Explanation

* Two nested loops.
* When `i=7`, `break 2` exits **both loops**.

### Key Points

* `break` → exits 1 loop
* `break 2` → exits 2 loops

---

# 11. `continue` in Loop

### Code

```bash
#!/bin/bash
n=9
i=1

while [ $i -lt $n ]
do
    printf "\n loop $i:"
    j=0
    ((i++))

    while [ $j -le $i ]
    do
        ((j++))

        if [ $j -gt 3 ] && [ $j -lt 6 ]
        then
            continue
        fi

        printf "$j "
    done
done
```

### Explanation

* Numbers **4 and 5 are skipped**.
* `continue` jumps to **next iteration**.

### Key Points

* `continue` skips current iteration
* `&&` means logical AND

---

# 12. `shift` Command (Argument Processing)

### Code

```bash
#!/bin/bash

echo Number of arguments:
echo $#

i=1
while [ -n "$1" ]
do
    echo argument $i is $1
    shift
    ((i++))
done

echo Number of arguments now:
echo $#
```

### Output

```
$ ./script.sh a b c

Number of arguments:
3
argument 1 is a
argument 2 is b
argument 3 is c
Number of arguments now:
0
```

### Explanation

* `shift` removes the **first argument**.
* Remaining arguments shift left.

Example:

```
before shift: $1=a $2=b $3=c
after shift : $1=b $2=c
```

### Key Points

* `shift` processes **multiple arguments**
* `$1` becomes `$2`, `$2` becomes `$3`

---

# Final Summary (Most Important Bash Concepts)

| Concept    | Purpose                     |
| ---------- | --------------------------- |
| `$#`       | Number of arguments         |
| `$1 $2`    | Positional parameters       |
| `shift`    | Move arguments left         |
| `IFS`      | Field separator             |
| `case`     | Pattern matching            |
| `break`    | Exit loop                   |
| `continue` | Skip iteration              |
| `$(( ))`   | Arithmetic operations       |
| `bc`       | Floating-point calculations |
| `>`        | Redirect output to file     |

---
## Script 1 — `prg15.sh`: Using `shift` to walk through arguments

### Code

```bash
#!/bin/bash
i=1
while [ -n "$1" ]
do
    echo "Argument $i = $1"
    i=$((i+1))
    shift
done
echo "Remaining arguments = $#"
```

### Explanation

This script loops over all command-line arguments using the `shift` command.

* `$1` always refers to the **current first argument**
* `shift` removes the current first argument
* `$2` becomes `$1`, `$3` becomes `$2`, and so on

The condition:

```bash
while [ -n "$1" ]
```

checks whether `$1` is a non-empty string, so the loop continues until all arguments are consumed.

`$#` stores the count of remaining arguments:

* initially → total arguments given
* finally → becomes 0 after all shifts

### Key Points

* `shift` is the standard way to iterate positional parameters
* Without `shift`, `$1` would remain unchanged
* `-n` checks for non-empty string
* `(( i++ ))` or `$((i+1))` increments counter

---

## Script 2 — `prg16.sh`: PID and `exec bash`

### Code

```bash
#!/bin/bash
echo "PID before exec = $$"
exec bash
echo "This line will not execute"
```

### Explanation

`$$` is a special variable that stores the PID (Process ID) of the current shell.

`exec bash` replaces the current shell process with a new bash shell.

Important:

* no new child process is created
* current process is overwritten

Therefore code after `exec` never runs.

### Key Points

* `$$` gives current shell PID
* `exec` replaces process image
* lines after `exec` are dead code

---

## Script 3 — `execex.sh`: `exec` replaces the shell

### Code

```bash
#!/bin/bash
echo "Before exec"
exec ls
echo "After exec"
```

### Explanation

This script demonstrates process replacement clearly.

Execution flow:

1. "Before exec" prints
2. `exec ls` replaces current shell with `ls`
3. directory listing appears
4. "After exec" never executes

### Key Points

* `exec` consumes current shell process
* following commands become unreachable

---

## Script 4 — `prg17.sh`: `eval` to execute a dynamic command

### Code

```bash
#!/bin/bash
cmd=date
fmt=+%d-%B-%Y
eval $cmd $fmt
```

### Explanation

`eval` performs a second round of expansion and executes the result as a command.

Here:

* `$cmd` → `date`
* `$fmt` → `+%d-%B-%Y`

So effectively this becomes:

```bash
date +%d-%B-%Y
```

Another example:

```bash
a="echo Hello"
eval $a
```

This executes:

```bash
echo Hello
```

### Key Points

* `eval` helps build commands dynamically
* performs two-pass evaluation
* risky if user input is passed directly ⚠️

---

## Script 5 — `prg18.sh`: Functions with arguments

### Code

```bash
#!/bin/bash

usage()
{
    echo "Usage: $1 arg1 arg2"
}

swap()
{
    echo "$2 $1"
}

if [ $# -lt 2 ]
then
    usage $0
    exit 1
fi

swap $1 $2
```

### Explanation

Two functions are defined:

### `usage()`

Prints help message using `$1`

### `swap()`

Prints two arguments in reverse order

The script first checks:

```bash
if [ $# -lt 2 ]
```

If fewer than 2 arguments are given:

* usage message shown
* script exits

Otherwise:

```bash
swap $1 $2
```

is called.

### Key Points

* Inside function, `$1` and `$2` refer to function arguments
* `$0` is script name
* `exit 1` means abnormal termination

---

## Script 6 — `getoptex.sh`: Parsing flags using `getopts`

### Code

```bash
#!/bin/bash

while getopts "n:a:" opt
do
    case $opt in
        n) echo "Name = $OPTARG" ;;
        a) echo "Age = $OPTARG" ;;
        ?) echo "Invalid option" ;;
    esac
done
```

### Explanation

`getopts` parses short command-line options.

Option string:

```bash
"n:a:"
```

means:

* `-n` requires argument
* `-a` requires argument

If input is:

```bash
./script.sh -n Rahul -a 21
```

Then:

* `$OPTARG` stores Rahul for `-n`
* `$OPTARG` stores 21 for `-a`

### Key Points

* colon means argument required
* `$OPTARG` stores value
* `getopts` handles short options only

---

## Script 7 — `select` with `case` and `break`

### Code

```bash
#!/bin/bash

select i in apple mango grape
do
    case $i in
        apple) echo "Apple selected" ;;
        mango) echo "Mango selected" ;;
        grape) echo "Grape selected"
               break ;;
    esac
done

echo "Last selected = $i"
```

### Explanation

`select` automatically creates a numbered menu.

Example menu:

```bash
1) apple
2) mango
3) grape
```

User enters a number.

Selected item is stored in `$i`.

`case` checks selected value.

When grape is selected:

```bash
break
```

stops the loop.

### Key Points

* `select` creates interactive menu
* loops continuously until `break`
* `$i` stores selected text

---

## Overall Concepts Covered

These scripts explain important Bash concepts:

✅ `shift` → argument processing
✅ `exec` → process replacement
✅ `eval` → dynamic execution
✅ functions
✅ `getopts` → option parsing
✅ `select` → menu handling

---
