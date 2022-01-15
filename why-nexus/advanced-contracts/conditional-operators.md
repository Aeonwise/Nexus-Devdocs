# Conditional Operators

Find below the various conditional operators.

| Core validation types |
| --------------------- |
| //RESERVED to 0x7f    |
| UINT8\_T = 0x70,      |
| UINT16\_T = 0x71,     |
| UINT32\_T = 0x72,     |
| UINT64\_T = 0x73,     |
| UINT256\_T = 0x74,    |
| UINT512\_T = 0x75,    |
| UINT1024\_T = 0x76,   |
| STRING = 0x77,        |
| BYTES = 0x78,         |

| Core validation operations |
| -------------------------- |
| //RESERVED to 0x8f         |
| EQUALS = 0x80,             |
| LESSTHAN = 0x81,           |
| GREATERTHAN = 0x82,        |
| NOTEQUALS = 0x83,          |
| CONTAINS = 0x84,           |
| LESSEQUALS = 0x85,         |
| GREATEREQUALS = 0x86,      |
|                            |
| //RESERVED to 0x9f         |
| ADD = 0x90,                |
| SUB = 0x91,                |
| DIV = 0x92,                |
| MUL = 0x93,                |
| MOD = 0x94,                |
| INC = 0x95,                |
| DEC = 0x96,                |
| EXP = 0x97,                |
| SUBDATA = 0x98,            |
| CAT = 0x99,                |
|                            |
| //RESERVED to 0x2f         |
| AND = 0xa0,                |
| OR = 0xa1,                 |
| GROUP = 0xa2,              |
| UNGROUP = 0xa3,            |
|                            |



| Register Layer State Values |
| --------------------------- |
| CREATED = 0xb0,             |
| MODIFIED = 0xb1,            |
| OWNER = 0xb2,               |
| TYPE = 0xb3,                |
| STATE = 0xb4,               |
|                             |
| //object registers          |
| VALUE = 0xb5                |



| Caller Values (The conditional script caller) |
| --------------------------------------------- |
| GENESIS = 0xc0,                               |
| TIMESTAMP = 0xc1,                             |
| OPERATIONS = 0xc2,                            |
| CONDITIONS = 0xc3,                            |



| Register Pre State Values |
| ------------------------- |
| CREATED = 0xc4,           |
| MODIFIED = 0xc5,          |
| OWNER = 0xc6,             |
| TYPE = 0xc7,              |
| STATE = 0xc8,             |
|                           |
| //object registers        |
| VALUE = 0xc9              |



| GENESIS = 0xca,    |
| ------------------ |
| TIMESTAMP = 0xcb,  |
| OPERATIONS = 0xcc, |
| CONDITIONS = 0xcd, |



| HEIGHT = 0xd0,   |
| ---------------- |
| SUPPLY = 0xd1,   |
| TIMESTAMP = 0xd2 |



| SK256 = 0xe0, |
| ------------- |
| SK512 = 0xe1  |
