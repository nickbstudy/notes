#Data types:

#### All data values must be typed inside quotation marks except number types.

#### Text types:
- `CHAR(5)` - 5 is length - fixed byte consumption - max 255
- `VARCHAR(5)` - 5 is length, only uses bytes required - max 65k
- `ENUM` - short for enumerate, can filter e.g. `ENUM('M','F')`

#### Number types
Each can be signed or unsigned - signed allows negative, so lesser top range, unsigned is double that but doesn't allow negative.  Signed is the default assigned.

Max listed as signed below:
- `TINYINT` - 1 byte,  max 127
- `SMALLINT` - 2 byte,  max 32k
- `MEDIUMINT` - 3 byte,  max 8.4m
- `TINYINT` - 4 byte,  max 2.1b
- `BIGINT` - 8 byte,  max 9qu

#### Fixed and Floating-Point types


In the number 10.523 its "precision" is 5 (total digits), and scale is 3 (digits to the right of the decimal)

**Fixed point** data represents exact values. `DECIMAL (5, 3)` could only be something like 10.523 - if less precision SQL addes 0s, otherwise will round to correct scale.  If only one digit is specified it will be the precision. `NUMERIC(p, s)` is also an option, and is more strict  
`FLOAT (5, 3)` is another option, it won't throw a warning if rounding whereas decimal will. Maximum digits is 23  
`DOUBLE` acts as float but can store up to 53

#### Date

- `DATE` will store a date in the format of `YYYY-MM-DD`
- `DATETIME` as named, `YYYY-MM-DD HH:MM:SS[.fraction]` up to .999999 of a second
- `TIMESTAMP` Range is 1 Jan 1970 to 19 Jan 2038 - UTC.  Stores seconds passed since start.  Advantages are comparing time values easily, and using UTC removes timezones.

#### BLOB

Refers to a file of binary data - involves saving files in a record