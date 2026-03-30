# 1) `case` statement program

## ✅ Proper code

```bash
#!/bin/bash

echo "What is your favorite image processor?"
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

---

## ✅ Sample Output

```bash
What is your favorite image processor?
gimp
good choice
```

Another:

```bash
What is your favorite image processor?
adobe photoshop
that costs a lot and needs cloud
```

---

## ✅ Explanation

* `read pname` → user input store karta hai.
* `case $pname in` → input ke basis pe matching hoti hai.
* `[gG]imp` → gimp ya Gimp dono chalega.
* `adobe*` → adobe se start hone wale sab words match honge.
* `*` → default case.

---

## ✅ Key Points

⭐ `case` multiple conditions ke liye if-else se cleaner hota hai
⭐ `;;` each case close karta hai
⭐ `*` = default case

---

# 2) Argument counting program

## ✅ Proper code

```bash
#!/bin/bash

if [ $# -gt 2 ]
then
    echo "arguments given and more than sufficient $#"

elif [ $# -gt 1 ]
then
    echo "arguments given and sufficient $#"

elif [ $# -gt 0 ]
then
    echo "arguments given but not sufficient $#"

else
    echo "arguments are needed $#"
fi
```

---

## ✅ Sample Output

### Run:

```bash
./file.sh a b c
```

Output:

```bash
arguments given and more than sufficient 3
```

### Run:

```bash
./file.sh a
```

Output:

```bash
arguments given but not sufficient 1
```

---

## ✅ Explanation

* `$#` = total command line arguments count
* `-gt` = greater than
* if/elif conditions count check karti hain

---

## ✅ Key Points

⭐ `$#` very important shell variable
⭐ `if` conditions order matters
⭐ first true condition execute hoti hai

---

# 3) `bc` calculation program

## ✅ Proper code

```bash
#!/bin/bash

a=2.5
b=3.2
c=4

d=$(bc -l << EOF
scale=5
($a+$b)*$c
EOF
)

echo $d
```

---

## ✅ Output

```bash
22.80000
```

---

## ✅ Explanation

* `bc` = calculator utility
* `-l` = math library enable
* `scale=5` = 5 decimal places
* `<< EOF ... EOF` = multiline input to bc

Calculation:
[
(2.5+3.2)\times4 = 22.8
]

---

## ✅ Key Points

⭐ Bash directly decimal calculate nahi karta
⭐ decimal ke liye `bc` use hota hai
⭐ here-document (`<< EOF`) multiline input deta hai

---

# 4) PATH splitting program

## ✅ Proper code

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

---

## ✅ Output

Example:

```bash
0 /usr/local/sbin
1 /usr/local/bin
2 /usr/sbin
3 /usr/bin
4 /sbin
5 /bin
6 /usr/games
7 /usr/local/games
8 /snap/bin
```

---

## ✅ Explanation

* `$PATH` contains directories separated by `:`
* `IFS=:` changes separator to colon
* `for n in $PATH` loops each path
* `((i++))` increments counter

---

## ✅ Key Points

⭐ `IFS` = Internal Field Separator
⭐ PATH normally colon-separated hota hai
⭐ loop automatically split karta hai

---
