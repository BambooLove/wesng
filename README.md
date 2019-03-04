# Windows Exploit Suggester - Next Generation (WES-NG)
WES-NG is a tool based on the output of Windows' `systeminfo` utility which provides the list of vulnerabilities the OS is vulnerable to, including any exploits for these vulnerabilities. Every Windows OS between Windows XP and Windows 10, including their Windows Server counterparts, is supported.

## Usage
1. Obtain the latest database of vulnerabilities by executing the command `wes.py --update`.
2. Use Windows' built-in `systeminfo.exe` tool to obtain the system information of the local system, or from a remote system using `systeminfo.exe /S MyRemoteHost`, and redirect this to a file: `systeminfo > systeminfo.txt`
3. Execute WES-NG with the systeminfo.txt output file as the parameter: `wes.py systeminfo.txt`. WES-NG then uses the database to determine which patches are applicable to the system and to which vulnerabilities are currently exposed, including exploits if available.

## Demo
![Gif animation showing usage of Windows Exploit Suggester - Next Generation](https://raw.githubusercontent.com/bitsadmin/wesng/master/demo.gif)

## Collector
This GitHub repository regularly updates the database of vulnerabilities, so running `wes.py` with the `--update` parameter gets the latest version.
If manual generation of the .csv file with hotfix information is required, use the scripts from the [/collector](collector) folder to compile the database. Read the comments at the top of each script and execute them in the order as they are listed below. Executing these scripts will produce CVEs.csv.
The WES-NG collector pulls information from various sources:
- Microsoft Security Bulletin Data: KBs for older systems [1]
- MSRC: The Microsoft Security Update API of the Microsoft Security Response Center (MSRC): Standard source of information for modern Microsoft Updates [2]
- NIST National Vulnerability Database (NVD): Complement vulnerabilities with Exploit-DB links [3]
These are combined into a single .csv file which is compressed and hosted in this GitHub repository.

## Rationale
I developed WES-NG because while [GDSSecurity's Windows-Exploit-Suggester](https://github.com/GDSSecurity/Windows-Exploit-Suggester/) worked excellently for operating systems in the Windows XP and Windows Vista era, GDSSecurity's Windows-Exploit-Suggester does not work for operating systems like Windows 10 and vulnerabilities published in recent years. This is because Microsoft replaced the Microsoft Security Bulletin Data Excel file [1] on which GDSSecurity's Windows-Exploit-Suggester is fully dependent, by the MSRC API [2]. The Microsoft Security Bulletin Data Excel file has not been updated since Q1 2017, so later operating systems and vulnerabilities cannot be detected. Thanks [@gdssecurity](https://twitter.com/gdssecurity), for this great tool which has served many of us for so many years!

## Improvements
- Add support for [NoPowerShell](https://github.com/bitsadmin/nopowershell/)'s `Get-SystemInfo` cmdlet output
- Add support for `wmic qfe` output together with support for parameters to manually specify the operating system
- Add support for alternative output formats of `systeminfo` (csv, table)
- More testing on the returned false positive vulnerabilities


[1] https://www.microsoft.com/download/details.aspx?id=36982

[2] https://portal.msrc.microsoft.com/en-us/developer

[3] https://nvd.nist.gov/vuln/data-feeds


**Authored by Arris Huijgen ([@bitsadmin](https://twitter.com/bitsadmin/) - https://github.com/bitsadmin/)**
