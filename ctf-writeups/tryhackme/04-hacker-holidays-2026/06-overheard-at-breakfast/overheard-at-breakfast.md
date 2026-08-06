# Overheard At Breakfast

## Objective

This is the sixth challenge from [TryHackMe](https://tryhackme.com)'s **Hacker Holidays 2026**, focusing on **OSINT, Social Media, and Hashing**.

The objectives were to:

- Analyze the provided conversation for identifying details.
- Extract the relevant clues.
- Locate the hidden account.
- Retrieve the flag.

## Investigation Steps

I began by downloading and extracting the provided task files on my virtual machine. The challenge included an image of a chat conversation containing several clues.

While reading the conversation, I noticed a reference to a free service that allows users to create profiles and link their social media accounts, with the only hint being that its name starts with the letter **"G"**. The conversation also revealed the following email address:

```text
lambobytelotushotel@gmail.com
```

After researching the clue, I determined that the service was **Gravatar**.

Gravatar identifies user profiles by the **MD5 hash** of an email address rather than the email itself. To generate the required hash, I used the following command:

```bash
echo -n "lambobytelotushotel@gmail.com" | md5sum
```

This produced the following MD5 hash:

```text
d4a5fc5d3128890778667e24617d7cc0
```

Using this hash, I located the corresponding Gravatar profile. The profile contained another clue: a Base64-encoded string.

I copied the encoded value into **CyberChef** and applied the **From Base64** operation, which decoded the string and revealed the flag.

## Final Thoughts

This challenge demonstrated how seemingly insignificant pieces of publicly available information can be combined to uncover additional data through **Open-Source Intelligence (OSINT)** techniques. A single email address, together with knowledge of how Gravatar generates profile identifiers, was enough to locate the associated profile.

The challenge also highlighted the importance of understanding common encoding and hashing methods. While the email address itself was not directly searchable on Gravatar, generating its MD5 hash and decoding the final Base64-encoded clue ultimately led to the flag.
