![image](https://user-images.githubusercontent.com/103275138/178811787-e65a0c26-21f5-4013-bdcd-8c533cb4964d.png)

# "Hostess Pie"

`hostess.py` is a script that will automate the subdomain discovery for a semi-blind external assessment. Then, it will take what it discovers and check to see if the discovered subdomains are within the client-provided scope. Great! Start hacking, as the results are clearly laid out for you.

If the subdomains are not located in the client-defined scope, the entries will be placed in a CSV file that you can forward to your client. Ask them for approval to include entries from `additional-subdomains.csv` to the assessment scope.

## How to run:

**Ensure that you have a `fullscope.txt` and `domains.txt` file present in the same directory:**
  - `fullscope.txt`: the full list of IP addresses, CIDR ranges, and hostnames provided by the client.
  - `domains.txt`: all of the relevant client-controlled domain names you would like to enumerate.

```
🐙 hostess ➤ cat domains.txt
nathanburchfield.com
burm.at
burmat.co

🐙 hostess ➤ cat fullscope.txt
www.burm.at
burmat.local
www.burmat.co
8.8.8.8
nathanburchfield.com
1.1.1.2-16
```

**Execute the script:**
```
🐙 hostess ➤ python3 hostess.py
[>] Welcome to v1.0 of Hostess Pie. This is subdomain enumeration tool named after the portable and superior type of pies.
[>] Executing amass..
[>] Executing assetfinder...
[>] Executing getallurls...
[>] The 'subdomains.txt' file has been compiled
[>] Resolving the discovered results within 'subdomains.txt'...
[>] IP address resolution completed
[>] Generating inscope- or additional-subdomains.csv..
[>] The inscope- and additional-subdomains CSV files have been created!
```

## The files you care about

- `inscope-subdomains.csv`: hosts ready to be hacked. 
- `additional-subdomains.csv`: other discovered assets you should send to your client for review/approval to add as in-scope for hacking.

## What is happening in this script

1. The script will "clean" the `fullscope.txt` file and create a new list named `hostess-scope.txt` for use by the script by:
   - Converting CIDR and IP address ranges to individual IP addresses;
   - Converting provided hostnames (contained within `fullscope.txt`) to IP addresses to add to the new list; and
   - Append the in-scope domains/subdomains from `fullscope.txt` to the new list.
2. Execute amass, getallurls, assetfinder, and others. Parse the results. Append the results to `subdomains.txt`.
3. Resolve the IP addresses for all entries in `subdomains.txt` discovered to generate `subdomains.csv`.
4. Compare all entries of `subdomains.csv` to the scope provided by the client:
  - Determine if the resolved IP address within `subdomains.csv` exists in `hostess-scope.txt`. 
  - Determine if the resolved hostname exists in `hostess-scope.txt`.
5. Split these results into two different files: `inscope-subdomains.csv`, and `additional-subdomains.csv`.
   - 'inscope-subdomains.csv' includes all IP addresses and subdomains that are considered inscope, based on `fullscope.txt` file.
   - 'additional-subdomains.csv' should send to your client for review/approval to add as in-scope for hacking.
