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

### Text Between Two Delimeters and remove the delimeters

##### Test Values

```
'ModelServerValidationMessage': None,
'RedirectUrl': '',
'PreviousUrl': '/',
```

#### Regex
Regex: `/(?<=\:).*(?=\,$)\gm`
Replace string: `''test`

#### Result

```
'ModelServerValidationMessage': 'test',
'RedirectUrl': 'test',
'PreviousUrl': 'test',
```

### Text Between Two Delimeters including the Delimeters

##### Test Values

```
'ModelServerValidationMessage': None,
'RedirectUrl': '',
'PreviousUrl': '/',
```

#### Regex
Regex: `/:[^:]+(?=,*$)\gm`
Replace string: `,`

#### Result

```
'ModelServerValidationMessage',
'RedirectUrl',
'PreviousUrl',
