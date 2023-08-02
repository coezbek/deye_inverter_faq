# deye_inverter_faq
My own list of stuff I discovered about my Deye Solar Inverter

## Retrieving data from the inverter

- Q: How can I get the daily production of the inverter from the CLI?
- A: Use https://github.com/s10l/deye-logger-at-cmd to send a ModBUS read command to the register 003C (Daily Production) and convert the resulting value from Hex to decimal:

```
./main -t 192.168.178.61:48899 -xmb 003C0001 2>&1 | awk -F '+ok=' '/+ok/ { payload = substr($2, 7, 4); printf "Daily Total: %.1f kWh\n", (strtonum("0x" payload) * 0.1); }'
```

The structure of the Modbus response is explained well here: https://github.com/s10l/deye-logger-at-cmd#sending-modbus-read-command

The available Modbus registers of the Deye inverter can be looked up here https://github.com/jedie/inverter-connect/ in the example screenshot.

## Firmware

- Q: How does the Deye inverter receive firmware updates?
- A:

## Security

- Q: How long can the administrator password be?
- A: 15 characters. Any other characters are truncated

- Q: I lost the admin password to the Deye's web front end, what can I do?
- A: Run the AT command 'AT+WEBU' using https://github.com/s10l/deye-logger-at-cmd:

```
./main -t <ip>:48899 -xat AT+WEBU
```

- Q: Wait. Doesn't this mean there is no security?
- A: Yep, the Deye solar inverter doesn't have any security from the WIFI network it is on.


