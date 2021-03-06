# eMMC Commands

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| | | | | |

# Basic commands (class 0 and class 1)

## CMD0

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| GO_IDLE_STATE | bc | [31:0] 00000000 | None | Resets the Device to idle state |
| GO_PRE_IDLE_STATE | bc | [31:0] F0F0F0F0 | None | Resets the Device to pre-idle state |
| BOOT_INITIATION | - | [31:0]FFFFFFFA | None | Initiate alternative boot operation |

## CMD1

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SEND_OP_COND | bcr | [31:0] OCR without busy | R3 | Asks Device, in idle state, to send its [Operating Conditions Register](./emmc_device_registers.html#ocr-register) contents in the response on the CMD line. |

## CMD2

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| ALL_SEND_CID | bcr | [31:0] stuff bits | R2 | Asks Device to send its CID number on the CMD line |

## CMD3

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SET_RELATIVE_ADDR | ac | [31:16] RCA, [15:0] stuff bits | R1 | Assigns relative address to the Device |

## CMD4

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SET_DSR | bc | [31:16] DSR, [15:0] stuff bits | - | Programs the DSR of the Device |

## CMD5

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SLEEP_AWAKE | ac | [31:16] RCA, [15]Sleep/Awake, [14:0] stuff bits | R1b | Toggles the Device between Sleep state and Standby state. |

## CMD6

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SWITCH | ac | [31:26] Set to 0, [25:24] Access, [23:16] Index, [15:8] Value, [7:3] Set to 0, [2:0] Cmd Set | R1b  | Switches the mode of operation of the selected Device or modifies the EXT_CSD registers. |

## CMD7

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| SELECT/DESELECT_CARD | ac | [31:16] RCA, [15:0] stuff bits | R1/R1b  | Command toggles a device between the standby and transfer states or between the programming and disconnect states. |

## CMD8

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SEND_EXT_CSD | adtc | [31:0] stuff bits | R1 | Device sends its EXT_CSD register as a block of data. |

## CMD9

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| SEND_CSD | ac | [31:16] RCA, [15:0] stuff bits | R2 | Addressed Device sends its Device-pecific data (CSD) on the CMD line. |

## CMD10

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| SEND_CID | ac | [31:16] RCA, [15:0] stuff bits | R2 | Addressed Device sends its Device identification (CID) on CMD the line. |

## CMD12

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| STOP_TRANSMISSION | ac | [31:16] RCA, [15:1] stuff bits, [0] HPI | R1/R1b | Forces the Device to stop transmission. If HPI flag is set the device shall interrupt its internal operations in a well-defined timing. |

## CMD13

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| SEND_STATUS | ac | [31:16] RCA, [15] SQS, [14:1] stuff bits, [0] HPI | R1 | In case SQS bit = 0: Addressed Device sends its status register. If HPI flag is set the device shall interrupt its internal operations in a well-defined timing. In case SQS bit = 1: indicate that this is a QSR query. In response device shall send the QSR (Queue Status Register). In this case HPI must be set to ‘0’ |

## CMD14

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| BUSTEST_R | adtc | [31:0] stuff bits | R1 | A host reads the reversed bus testing data pattern from a Device. |

## CMD15

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| GO_INACTIVE_STATE | ac | [31:16] RCA, [15:0] stuff bits | - | Sets the Device to inactive state |

## CMD19

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| BUSTEST_W | adtc | [31:0] stuff bits | R1 | A host sends the bus test data pattern to a Device |

> NOTE 1 - CMD7, R1 while selecting from Stand-By State to Transfer State; R1b while selecting from Disconnected State to Programming State.  
> NOTE 2 - CMD12, RCA in CMD12 is used only if HPI bit is set. The argument does not imply any RCA check on the device side.  
> NOTE 3 - CMD12, R1 for read cases and R1b for write cases.

# Block-oriented read commands (class 2)

## CMD16

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SET_BLOCKLEN | ac | [31:0] block length | R1 | Sets the block length (in bytes) for all following block commands (read and write). Default block length is specified in the CSD |

## CMD17

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| READ_SINGLE_BLOCK | adtc | [31:0] data address | R1 | Reads a block of the size selected by the SET_BLOCKLEN command |

## CMD18

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| READ_MULTIPLE_BLOCK | adtc | [31:0] data address | R1 | Continuously transfers data blocks from Device to host until interrupted by a stop command, or the requested number of data blocks is transmitted If sent as part of a packed read command, the argument shall contain the first read data address in the pack (address of first individual read command inside the pack).  |

## CMD21

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SEND_TUNING_BLOCK | adtc | [31:0] stuff bits | R1 | 128 clocks of tuning pattern (64 byte in 4bit mode or 128 byte in 8 bit mode) is sent for HS200 optimal sampling point detection. |

> NOTE 1 - CMD17 & CMD18, Data address for media =<2 GB is a 32 bit byte address and data address for media > 2GB is a 32 bit sector (512 B) address.  
> NOTE 2 - CMD17, The transferred data must not cross a physical block boundary, unless READ_BLK_MISALIGN is set in the CSD register.

# Block-oriented write commands (class 4)

## CMD23 (default)

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SET_BLOCK_COUNT | ac | [31] Reliable Write Request, [30] ‘0’ non- packed, [29] tag request,[28:25] context ID, [24]: forced programming, [23:16] set to 0, [15:0] number of blocks | R1 | Defines the number of blocks (read/write) and the reliable writer parameter (write) for a block read or write command.|

## CMD23 (packed)

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| SET_BLOCK_COUNT | ac | [31] set to 0, [30] ‘1’ packed, [29:16] set to 0, [15:0] number of blocks | R1 | Defines the number of blocks (read/write) for the following packed write command or for the header of the following packed read command.|

## CMD24

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| WRITE_BLOCK | adtc | [31:0] data address | R1 | Writes a block of the size selected by the
SET_BLOCKLEN command. |

## CMD25

| Abbr. | Type  | Argument | Resp | Description |
| -- | --  | -- | -- | -- |
| WRITE_MULTIPLE_BLOCK | adtc | [31:0] data address | R1 | Continuously writes blocks of data until a STOP_TRANSMISSION follows or the requested number of block received. If sent as a packed command (either packed write, or the header of packed read) the argument shall contain
the first read/write data address in the pack (address of first individual command inside the pack). |

## CMD26

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| PROGRAM_CID | adtc | [31:0] stuff bits | R1 | Programming of the Device identification register. This command shall be issued only once. The Device contains hardware to prevent this operation after the first programming. Normally this command is reserved for the manufacturer.|

## CMD27

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| PROGRAM_CSD | adtc | [31:0] stuff bits | R1 | Programming of the programmable bits of the CSD. |

## CMD49

| Abbr. | Type | Argument | Resp | Description |
| -- | -- | -- | -- | -- |
| SET_TIME | adtc | [31:0] stuff bits | R1 | Sets the real time clock according to the RTC information in the 512 B data block. |

> NOTE 1 - CMD24 & CMD25, Data address for media =<2 GB is a 32 bit byte address and data address for media > 2GB is a 32 bit sector (512 B) address.  
> NOTE 2 - CMD24, The transferred data must not cross a physical block boundary, unless READ_BLK_MISALIGN is set in the CSD register.

# Block-oriented write protection commands (class 6)

# Erase commands (class 5)

# I/O mode commands (class 9)

# Lock Device commands (class 7)

# Application-specific commands (class 8)

# Security Protocols (class 10)

# Command Queue (Class 11)




































































































































