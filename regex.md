# Regex Cheatsheet

Use the Regexr to test: 

https://regexr.com/   
https://github.com/gskinner/regexr/

## Find and Replace

### Double Quote a List of Values

#### Test Values
```
Value 1
Value 2
Value 3
Value 4
Value 5
```

#### Regex
Regex: `/^([A-Za-z0-9 ]+)$\gm` or `/^([A-Za-z0-9 ]+)$`  
Replace string: `"$1",`

#### Result
```
"Value 1",
"Value 2",
"Value 3",
"Value 4",
"Value 5",
```
