# RDC8445S Memory Map

Controller memory read from an RDC8445S (card ID `0x90109010`) with
`GET_SETTING`, cross-checked against the values the controller's machine
configuration reports. Every address from `0x0000` to `0x0F7F` replies, so
only addresses whose meaning could be confirmed are listed here.

## Method

1. Read every address with batched `GET_SETTING` commands (read-only; no
   `SET_SETTING` is ever sent). On TCP, 16 queries per write return one ACK
   followed by 16 replies; the full sweep takes about 35 seconds.
2. Compare each raw 35-bit value with the configured value of the matching
   machine setting to find the unit scaling.
3. Mark the mnemonic `# Verified RDC8445S` in `protocols/ruida/ruida_protocol.py`
   only where the existing name matches the confirmed meaning.

## Units

| Quantity | Stored as |
| --- | --- |
| Length, position, travel | µm |
| Velocity | µm/s |
| Acceleration | µm/s² |
| Step length | 10⁻⁶ µm (pm) |
| Frequency, power %, ratio % | unscaled |
| Pre-ignition % | tenths of a percent |

## Settings

| Address | Mnemonic | Meaning | Unit | RDC8445S raw | Raw → unit | Verified |
| --- | --- | --- | --- | ---: | --- | --- |
| `0x0005` | `MEM_G0_VELOCITY` | Idle speed | mm/s | 150000 | ÷ 1000 | yes |
| `0x000B` | `MEM_ENG_FACULA` | Facula Size (50 - 99%) |  | 800 | ÷ 10 | yes |
| `0x000C` | `MEM_HOME_VELOCITY` | Homing Speed | mm/s | 80000 | ÷ 1000 | yes |
| `0x000E` | `MEM_ENG_VERT_VELOCITY` | Line shift speed | mm/s | 100000 | ÷ 1000 | yes |
| `0x0011` | `MEM_LASER_PWM_FREQUENCY_1` | Laser 1 frequency | Hz | 20000 | × 1 | yes |
| `0x0012` | `MEM_LASER_MIN_POWER_1` | Laser 1 minimum power | % | 1 | × 1 | yes |
| `0x0013` | `MEM_LASER_MAX_POWER_1` | Laser 1 maximum power | % | 99 | × 1 | yes |
| `0x0017` | `MEM_LASER_PWM_FREQUENCY_2` | Laser 2 frequency | Hz | 20000 | × 1 | yes |
| `0x0018` | `MEM_LASER_MIN_POWER_2` | Laser 2 minimum power | % | 1 | × 1 | yes |
| `0x0019` | `MEM_LASER_MAX_POWER_2` | Laser 2 maximum power | % | 99 | × 1 | yes |
| `0x001A` | `MEM_LASER_STANDBY_FREQUENCY_1` | Laser 1 pre-ignition frequency | Hz | 5000 | × 1 | yes |
| `0x001B` | `MEM_LASER_STANDBY_PULSE_1` | Laser 1 pre-ignition percent | % | 5 | ÷ 10 | yes |
| `0x001C` | `MEM_LASER_STANDBY_FREQUENCY_2` | Laser 2 pre-ignition frequency | Hz | 5000 | × 1 | yes |
| `0x001D` | `MEM_LASER_STANDBY_PULSE_2` | Laser 2 pre-ignition percent | % | 5 | ÷ 10 | yes |
| `0x0021` | `MEM_AXIS_PRECISION_1` | Step length | µm | 15624730 | ÷ 10⁶ | yes |
| `0x0023` | `MEM_AXIS_MAX_VELOCITY_1` | Max speed | mm/s | 500000 | ÷ 1000 | yes |
| `0x0024` | `MEM_AXIS_START_VELOCITY_1` | Jumpoff speed | mm/s | 15000 | ÷ 1000 | yes |
| `0x0025` | `MEM_AXIS_MAX_ACC_1` | Max acceleration (mm/s^2) |  | 5000000 | ÷ 1000 | yes |
| `0x0026` | `MEM_BED_SIZE_X` | Max travel | mm | 1300000 | ÷ 1000 | yes |
| `0x0027` | `MEM_AXIS_BTN_START_VEL_1` | Keypad jumpoff speed | mm/s | 15000 | ÷ 1000 | yes |
| `0x0028` | `MEM_AXIS_BTN_ACC_1` | Keypad acceleration (mm/s^2) |  | 3000000 | ÷ 1000 | yes |
| `0x0029` | `MEM_AXIS_ESTP_ACC_1` | E-Stop acceleration (mm/s^2) |  | 8000000 | ÷ 1000 | yes |
| `0x002A` | `MEM_AXIS_HOME_OFFSET_1` | Home offset | mm | 0 | unconfirmed (0) | yes |
| `0x002B` | `MEM_AXIS_BACKLASH_1` | X Axis Backlash | mm | 0 | unconfirmed (0) | yes |
| `0x0031` | `MEM_AXIS_PRECISION_2` | Step length | µm | 15607650 | ÷ 10⁶ | yes |
| `0x0033` | `MEM_AXIS_MAX_VELOCITY_2` | Max speed | mm/s | 500000 | ÷ 1000 | yes |
| `0x0034` | `MEM_AXIS_START_VELOCITY_2` | Jumpoff speed | mm/s | 15000 | ÷ 1000 | yes |
| `0x0035` | `MEM_AXIS_MAX_ACC_2` | Max acceleration (mm/s^2) |  | 3000000 | ÷ 1000 | yes |
| `0x0036` | `MEM_BED_SIZE_Y` | Max travel | mm | 900000 | ÷ 1000 | yes |
| `0x0037` | `MEM_AXIS_BTN_START_VEL_2` | Keypad jumpoff speed | mm/s | 15000 | ÷ 1000 | yes |
| `0x0038` | `MEM_AXIS_BTN_ACC_2` | Keypad acceleration (mm/s^2) |  | 1500000 | ÷ 1000 | yes |
| `0x0039` | `MEM_AXIS_ESTP_ACC_2` | E-Stop acceleration (mm/s^2) |  | 5000000 | ÷ 1000 | yes |
| `0x003A` | `MEM_AXIS_HOME_OFFSET_2` | Home offset | mm | 0 | unconfirmed (0) | yes |
| `0x003B` | `MEM_AXIS_BACKLASH_2` | Y Axis Backlash | mm | 0 | unconfirmed (0) | yes |
| `0x0041` | `MEM_AXIS_PRECISION_3` | Step length | µm | 8000000 | ÷ 10⁶ | yes |
| `0x0043` | `MEM_AXIS_MAX_VELOCITY_3` | Max speed | mm/s | 90000 | ÷ 1000 | yes |
| `0x0044` | `MEM_AXIS_START_VELOCITY_3` | Jumpoff speed | mm/s | 1000 | ÷ 1000 | yes |
| `0x0045` | `MEM_AXIS_MAX_ACC_3` | Max acceleration (mm/s^2) |  | 3000000 | ÷ 1000 | yes |
| `0x0046` | `MEM_AXIS_RANGE_3` | Max travel | mm | 10000000 | ÷ 1000 | yes |
| `0x0047` | `MEM_AXIS_BTN_START_VEL_3` | Keypad jumpoff speed | mm/s | 1000 | ÷ 1000 | yes |
| `0x0048` | `MEM_AXIS_BTN_ACC_3` | Keypad acceleration (mm/s^2) |  | 1500000 | ÷ 1000 | yes |
| `0x0049` | `MEM_AXIS_ESTP_ACC_3` | E-Stop acceleration (mm/s^2) |  | 5000000 | ÷ 1000 | yes |
| `0x004A` | `MEM_AXIS_HOME_OFFSET_3` | Home offset | mm | 0 | unconfirmed (0) | yes |
| `0x0051` | `MEM_AXIS_PRECISION_4` | Step length | µm | 6337498 | ÷ 10⁶ | yes |
| `0x0053` | `MEM_AXIS_MAX_VELOCITY_4` | Max speed | mm/s | 300000 | ÷ 1000 | yes |
| `0x0054` | `MEM_AXIS_START_VELOCITY_4` | Jumpoff speed | mm/s | 15000 | ÷ 1000 | yes |
| `0x0055` | `MEM_AXIS_MAX_ACC_4` | Max acceleration (mm/s^2) |  | 5000000 | ÷ 1000 | yes |
| `0x0056` | `MEM_AXIS_RANGE_4` | Max travel | mm | 600000 | ÷ 1000 | yes |
| `0x0057` | `MEM_AXIS_BTN_START_VEL_4` | Keypad jumpoff speed | mm/s | 15000 | ÷ 1000 | yes |
| `0x0058` | `MEM_AXIS_BTN_ACC_4` | Keypad acceleration (mm/s^2) |  | 4000000 | ÷ 1000 | yes |
| `0x0059` | `MEM_AXIS_ESTP_ACC_4` | E-Stop acceleration (mm/s^2) |  | 10000000 | ÷ 1000 | yes |
| `0x005A` | `MEM_AXIS_HOME_OFFSET_4` | Home offset | mm | 0 | unconfirmed (0) | yes |
| `0x0200` | `MEM_SYSTEM_SETTINGS` | Return Position |  | 32768 |  | no: name does not describe the value (return position) |
| `0x0201` | `MEM_TURN_VELOCITY` | Start speed | mm/s | 3000 | ÷ 1000 | no: existing name differs from meaning |
| `0x0202` | `MEM_SYN_ACC` | Max acceleration (mm/s^2) |  | 500000 | ÷ 1000 | no: existing name differs from meaning |
| `0x0203` | `MEM_G0_DELAY` | Idle delay | ms | 0 | unconfirmed (0) | yes |
| `0x0209` | `MEM_TURN_ACC` | Min acceleration (mm/s^2) |  | 50000 | ÷ 1000 | no: existing name differs from meaning |
| `0x020A` | `MEM_G0_ACC` | Idle acceleration (mm/s^2) |  | 1500000 | ÷ 1000 | yes |
| `0x020E` | `MEM_FOCUS_DEPTH` | Focus Distance |  | 44000 | ÷ 1000 | yes |
| `0x0215` | `MEM_X_DOCKING_POSITION` | X Axis docking position | mm | 0 | unconfirmed (0) | yes |
| `0x0216` | `MEM_Y_DOCKING_POSITION` | Y Axis docking position | mm | 0 | unconfirmed (0) | yes |
| `0x021A` | `MEM_ACC_RATIO` | Accel factor % (0 to 200) |  | 10 | × 1 | yes |
| `0x021B` | `MEM_TURN_RATIO` | Speed factor % (0 to 200) |  | 10 | × 1 | no: existing name differs from meaning |
| `0x021C` | `MEM_ACC_G0_RATIO` | G0 accel factor % (0 to 200) |  | 30 | × 1 | yes |
| `0x021F` | `MEM_ROTATE_PULSE` | Pulses per rotation |  | 10000000 | ÷ 1000 | yes |
| `0x0221` | `MEM_ROTATE_D` | Diameter |  | 100000 | ÷ 1000 | yes |
| `0x0224` | `MEM_X_MINIMUM_ENG_VELOCITY` | X start speed | mm/s | 15000 | ÷ 1000 | yes |
| `0x0225` | `MEM_X_ENG_ACC` | X acceleration (mm/s^2) |  | 5000000 | ÷ 1000 | yes |
| `0x022D` | `MEM_U_WORK_VELOCITY` | Z Axis docking position | mm | 0 | unconfirmed (0) | no: existing name differs on this model |
| `0x0231` | `MEM_MANUAL_FAST_SPEED` | Wireless panel speed fast | mm/s | 200000 | ÷ 1000 | yes |
| `0x0232` | `MEM_MANUAL_SLOW_SPEED` | Wireless panel speed slow | mm/s | 50000 | ÷ 1000 | yes |
| `0x0233` | `MEM_RESET_DELAY` | Reset delay | ms | 0 | unconfirmed (0) | yes |
| `0x0234` | `MEM_Y_MINIMUM_ENG_VELOCITY` | Y start speed | mm/s | 15000 | ÷ 1000 | yes |
| `0x0235` | `MEM_Y_ENG_ACC` | Y acceleration (mm/s^2) |  | 3000000 | ÷ 1000 | yes |
| `0x0237` | `MEM_ENG_ACC_RATIO` | Engraving factor % (0 to 100) |  | 80 | × 1 | yes |
| `0x0238` | `MEM_STATUS_ON_DELAY` | Status on delay | ms | 0 | unconfirmed (0) | yes |
| `0x0240` | `MEM_Z_HOME_VELOCITY_ALT` | Z Home Speed | mm/s | 70000 | ÷ 1000 | yes |
| `0x0241` | `MEM_Z_WORK_VELOCITY_ALT` | Z Work Speed | mm/s | 70000 | ÷ 1000 | yes |
| `0x0242` | `MEM_U_HOME_VELOCITY_ALT` | U Home Speed | mm/s | 70000 | ÷ 1000 | yes |
| `0x0243` | `MEM_U_WORK_VELOCITY_ALT` | U Work Speed | mm/s | 50000 | ÷ 1000 | yes |
| `0x0351` | `MEM_STATUS_OFF_DELAY` | Status off delay | ms | 0 | unconfirmed (0) | yes |
| `0x0352` | `MEM_FINISH_DELAY` | Finish delay | ms | 0 | unconfirmed (0) | yes |

## Flag registers

| Address | Mnemonic | Bits |
| --- | --- | --- |
| `0x0004` | `MEM_IO_ENABLE` | `0x0001` door protect, `0x0002` air assist output, `0x0020`/`0x0040` water protect laser 1/2, `0x0200`/`0x0400` laser 1/2 output signal high |
| `0x0010` | `MEM_SYSTEM_CONTROL_MODE` | `0x0003` tube type, `0x0400` engraving mode, `0x2000`/`0x4000` laser 1/2 enabled, `0x8000` multi-tube |
| `0x0020`, `0x0030`, `0x0040`, `0x0050` | `MEM_AXIS_CONTROL_PARA_1`-`4` | `0x0200` direction polarity, `0x0400` limiter polarity, `0x0800` PWM rising edge, `0x1000` invert keypad direction, `0x4000` limit trigger, `0x8000` enable homing; `0x2000` is set on X, Y and Z but its meaning is unknown |
| `0x0226` | `MEM_USER_PARA_1` | `0x0001` rotary enabled, `0x0004` panel speed shift |
| `0x023D` | `MEM_AIR_PROTECT_CONFIG` | `0x0001` air protect enabled |
| `0x030F` | `MEM_FOCUS_CONFIG` | `0x0001` focus enabled, `0x0008` Z return to docking, `0x0600` air assist mode |

RDC8445S values: `0x0004` = `0x0002`, `0x0010` = `0xE000`, axis control =
`0xE000` / `0xE200` / `0xA000` / `0x1200` (X / Y / Z / U), `0x030F` = `0x8201`.
The Z axis has homing enabled but no limit trigger: it is referenced by the
focus probe (`FOCUS_Z`), and `HOME_Z` drives the table into its travel limit.

## Differences from the shared memory table

`MT` is shared by all controller models. On the RDC8445S:

- `0x0228`-`0x022A` (`MEM_Z_HOME_VELOCITY`, `MEM_Z_WORK_VELOCITY`,
  `MEM_Z_G0_VELOCITY`) read an invalid value (`4063821824`). The Z/U home and
  work velocities are at `0x0240`-`0x0243`, added as `MEM_*_VELOCITY_ALT`.
- `0x022D` holds the Z docking position rather than `MEM_U_WORK_VELOCITY`.

Existing names are kept so scripts for other models keep working. A
per-model overlay of `MT` keyed by card ID would let models with a different
layout use their own names without affecting others.
