# Room 404 Write-Up

## Objective

This is the second challenge from [TryHackMe](https://tryhackme.com)'s **Hacker Holidays 2026**, focusing on web security.

The objective was to locate the hidden flag within the Byte Lotus website.

## Investigation Steps

I first navigated to the Byte Lotus website and inspected the page source to check for any hidden comments or embedded information. However, I did not find anything useful.

Since the source code did not reveal the flag, I proceeded with directory enumeration using **Gobuster**.

```bash
gobuster dir -u http://machineip:8080 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

The initial scan did not reveal the hidden content, so I performed another scan using the **SecLists** common wordlist.

```bash
gobuster dir -u http://machineip:8080 -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

This scan revealed an exposed **`.git`** directory, indicating that the Git repository was publicly accessible.

To retrieve the repository, I used **git-dumper**.

```bash
git-dumper http://10.114.137.65:8080/.git/ repo
```

After downloading the repository, I examined its contents. The flag was located inside the **README.md** file.

## Final Thoughts

This challenge demonstrated the importance of checking for exposed Git repositories during web application assessments. An accessible **`.git`** directory can allow an attacker to reconstruct the entire repository, potentially exposing sensitive information such as source code, credentials, configuration files, or hidden flags.

It also highlighted the value of directory enumeration. When inspecting the source code does not reveal any useful information, tools such as **Gobuster** can uncover hidden files and directories that may lead to further findings.
