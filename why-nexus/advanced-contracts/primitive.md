# Primitive



| //used for pattern matching                              |
| -------------------------------------------------------- |
| WILDCARD = 0x00,                                         |
|                                                          |
| //register operations                                    |
| WRITE = 0x01,                                            |
| CREATE = 0x02,                                           |
| AUTHORIZE = 0x03, //to be determined how this will work  |
| TRANSFER = 0x04,                                         |
| CLAIM = 0x05,                                            |
| APPEND = 0x06,                                           |
|                                                          |
| //financial operations                                   |
| DEBIT = 0x10,                                            |
| CREDIT = 0x11,                                           |
| COINBASE = 0x12,                                         |
| GENESIS = 0x13, //for proof of stake                     |
| TRUST = 0x14,                                            |
| FEE = 0x17, //to pay fees to network                     |
|                                                          |
| //consensus operations                                   |
| ACK = 0x30, //a vote to credit trust towards a proposal  |
| NACK = 0x31, //a vote to withdrawl trust from a proposal |
|                                                          |
| //conditional operations                                 |
| VALIDATE = 0x40,                                         |
| CONDITION = 0x41,                                        |
|                                                          |
| //legacy operations                                      |
| LEGACY = 0x50,                                           |
| MIGRATE = 0x51,                                          |
|                                                          |
| //0x31 = 0x6f RESERVED                                   |
|                                                          |
| //final reserved ENUM                                    |
| RESERVED = 0xff                                          |

|                       |
| --------------------- |
| //RESERVED to 0x8f    |
| EQUALS = 0x80,        |
| LESSTHAN = 0x81,      |
| GREATERTHAN = 0x82,   |
| NOTEQUALS = 0x83,     |
| CONTAINS = 0x84,      |
| LESSEQUALS = 0x85,    |
| GREATEREQUALS = 0x86, |
|                       |
| //RESERVED to 0x9f    |
| ADD = 0x90,           |
| SUB = 0x91,           |
| DIV = 0x92,           |
| MUL = 0x93,           |
| MOD = 0x94,           |
| INC = 0x95,           |
| DEC = 0x96,           |
| EXP = 0x97,           |
|                       |
| SUBDATA = 0x98,       |
| CAT = 0x99,           |
|                       |
| //RESERVED to 0x2f    |
| AND = 0xa0,           |
| OR = 0xa1,            |
| GROUP = 0xa2,         |
| UNGROUP = 0xa3,       |
| };                    |
|                       |

\
