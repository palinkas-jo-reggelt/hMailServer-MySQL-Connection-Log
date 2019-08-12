# hMailServer-MySQL-Connection-Log

Searchable Event Log for hMailServer 


# Change Log

- v.0.06 Fixed bug that messed up REGEX searches with "+" in the URL
- v.0.05 Added REGEX search on HELO column - See notes below for usage
- v.0.04 Fixed a typo
- v.0.03 Minor changes to display of results statement above table
- v.0.02 Added auto-populate drop down boxes to search "reason", "port" and "event"; removed "stringport" as superfluous; added hmsCLExpire.ps1 to auto expire entries via scheduled task; added OnClientLogon event to EventHandlers.vbs
- v.0.01 Initial Commit


# Prerequisites: 
* MySQL
* Apache/IIS with PHP
* RvdH's OnHELO custom build (to obtain sender EHLO): http://hmailserver.com/forum/viewtopic.php?p=213193#p213193


# Instructions:

1) Create MySQL table in existing hmailserver database:

```
CREATE TABLE `hm_accrej` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `timestamp` datetime NOT NULL,
  `port` int(3) NOT NULL,
  `event` varchar(20) NOT NULL,
  `accrej` varchar(20) NOT NULL,
  `reason` varchar(20) NOT NULL,
  `ipaddress` varchar(15) NOT NULL,
  `country` varchar(30) DEFAULT NULL,
  `helo` varchar(192) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

2) Download VbsJson.vbs and place in program files\hMailServer\Events folder.
https://github.com/eklam/VbsJson

3) Modify EventHandlers.vbs

4) Place index.php in a web accessible folder.

5) Place hmsCLExpire.ps1 somewhere and create scheduled task to run once a day
```Powershell -ExecutionPolicy Bypass -File C:\scripts\hmailserver\hmsCLExpire.ps1```
!! Task must run with Administrator privileges in order to access MySQL !!


# REGEX Search Instructions

All regex queries must be made in the "Search Term" textbox prefaced with `REGEX:` (uppercase letters required). 

All regex queries with escape backslashes MUST be double escaped. For example, `\.` needs to be entered as `\\.` or the query will throw an error. 

Example query: 

`REGEX:^\\[(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\\.){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\\]$`
