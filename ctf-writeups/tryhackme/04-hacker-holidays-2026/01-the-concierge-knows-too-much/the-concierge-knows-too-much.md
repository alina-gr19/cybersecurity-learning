# The Guestbook

## Objective

This is the 13th challenge from [TryHackMe](https://tryhackme.com)'s **Hacker Holidays 2026**, focusing on **AI security**.

VERA is the AI assistant of the Byte Lotus. The objective of this challenge was to gain VERA's trust and persuade her to reveal the flag.

## Investigation Steps

This challenge was similar to the first AI challenge and focused on **indirect prompt injection**.

After some trial and error, I was able to successfully manipulate VERA and retrieve the flag.

![Flag](image.png)

## Final Thoughts

This challenge demonstrated how carefully crafted instructions can influence the behaviour of an AI assistant and potentially cause it to reveal information that it was intended to keep protected.

It highlighted an important security concern when developing AI-powered applications. AI assistants should not blindly follow user-provided instructions, especially when those instructions could conflict with their intended rules or cause sensitive information to be disclosed.
