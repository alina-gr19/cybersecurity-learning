# Packed Light

## Objective

This is the fourth challenge from [TryHackMe](https://tryhackme.com)'s **Hacker Holidays 2026**, focusing on **Forensics**.

The objectives were to:

- Analyze the provided network capture for a covert communication channel.
- Identify where the exfiltrated data was hidden and reconstruct it.
- Decode the recovered data to obtain the flag.

## Investigation Steps

I began by downloading and extracting the provided task files on my virtual machine. The challenge included a **PCAPNG** capture file, which I analyzed using **Wireshark**.

I first filtered the HTTP traffic and noticed a **GET** request for a Python script. After examining the script, I determined that it was a **keylogger**. Each captured keystroke was XOR-encrypted using a hardcoded key, Base64-encoded, and then transmitted to the command-and-control (C2) server inside the `hotel_sess_state` HTTP cookie.

To locate all transmitted keystrokes, I filtered for requests containing the cookie:

```text
http.cookie contains "hotel_sess_state"
```

This returned **30 HTTP requests**, each containing an encoded keystroke.

To extract the cookie values, I used **TShark**:

```bash
tshark -r traffic.pcapng \
-Y "http.request && tcp.port == 8080" \
-T fields \
-e http.cookie | sed 's/^hotel_sess_state=//'
```

The extracted values were:

```text
HA==
AA==
BQ==
Mw==
Hg==
ew==
Og==
fA==
Fw==
eQ==
Ow==
Fw==
Pw==
fA==
PA==
Kw==
IA==
eQ==
Jg==
Lw==
Fw==
eA==
Pg==
LQ==
Gg==
Fw==
MQ==
eA==
PQ==
NQ==
```

The final step was to decode the recovered data. I imported the extracted values into **CyberChef**, where I first applied **From Base64** and then **XOR** using the hardcoded key recovered from the Python script. This successfully reconstructed the original keystrokes and revealed the flag.

## Final Thoughts

This challenge demonstrated how attackers can hide exfiltrated data within seemingly legitimate network traffic. Instead of transmitting data directly, the keylogger concealed each encrypted keystroke inside an HTTP cookie, making the traffic appear less suspicious at first glance.

The exercise highlighted the importance of examining both network traffic and downloaded payloads during forensic investigations. By correlating the packet capture with the malicious script, it was possible to understand the exfiltration method, reconstruct the transmitted data, and recover the flag.
