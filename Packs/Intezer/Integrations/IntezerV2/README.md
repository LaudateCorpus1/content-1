Use the Intezer v2 integration to detect and analyze malware, based on code reuse.

## Configure Intezer v2 on Cortex XSOAR

1. Navigate to **Settings** > **Integrations** > **Servers & Services**.
2. Search for Intezer v2.
3. Click **Add instance** to create and configure a new integration instance.

    | **Parameter** | **Description** | **Required** |
    | --- | --- | --- |
    | API Key |  | True |
    | Intezer Analyze Base URL | The API address to intezer Analyze - i.e. https://analyze.intezer.com/api/ | False |
    | Use system proxy settings |  | False |
    | Trust any certificate (not secure) |  | False |

4. Click **Test** to validate the URLs, token, and connection.

## Commands
You can execute these commands from the Cortex XSOAR CLI, as part of an automation, or in a playbook.
After you successfully execute a command, a DBot message appears in the War Room with the command details.

### intezer-analyze-by-hash
***
Checks file reputation of the given hash, supports SHA256, SHA1 and MD5


#### Base Command

`intezer-analyze-by-hash`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| file_hash | Hash of the file to query. Supports SHA256, MD5 and SHA1. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| Intezer.Analysis.ID | string | Intezer analysis id | 
| Intezer.Analysis.Status | string | status of the analysis | 
| Intezer.Analysis.Type | string | type of the analysis | 


#### Command Example
``` 
!intezer-analyze-by-hash file_hash="8cbf90aeab2c93b2819fcfd6262b2cdb"
```

#### Human Readable Output
```
Analysis created successfully: 59e2f081-45f3-4822-bf45-407670dcb4d7
```


### intezer-get-latest-report
***
Checks file reputation of the given hash, supports SHA256, SHA1 and MD5 by looking at the latest available report


#### Base Command

`intezer-get-latest-report`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| file_hash | Hash of the file to query. Supports SHA256, MD5 and SHA1. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| File.SHA256 | string | Hash SHA256 | 
| File.Malicious.Vendor | string | For malicious files, the vendor that made the decision | 
| DBotScore.Indicator | string | The indicator we tested | 
| DBotScore.Type | string | The type of the indicator | 
| DBotScore.Vendor | string | Vendor used to calculate the score | 
| DBotScore.Score | number | The actual score | 
| File.Metadata | Unknown | Metadata returned from Intezer analysis \(analysis id, analysis url, family, family type, sha256, verdict, sub_verdict\). Metedata will be retuned only for supported files. | 
| File.ExistsInIntezer | Boolean | Does the file exists on intezer genome database | 


#### Command Example
```
intezer-get-latest-report file_hash="8cbf90aeab2c93b2819fcfd6262b2cdb"
```

#### Human Readable Output
```
Intezer File analysis result
----
SHA256: bf293bda73c5b4c1ec66561ad20d7e2bc6692d051282d35ce8b7b7020c753467
Verdict: malicious (known_malicious)
Family: WannaCry


Analysis Report
---
analysis_id	006c54ba-3159-43a0-98a0-1c5032145f47
analysis_time	Tue, 29 Jun 2021 13:40:01 GMT
analysis_url	https://analyze.intezer.com/analyses/006c54ba-3159-43a0-98a0-1c5032145f47
family_id	0b13c0d4-7779-4c06-98fa-4d33ca98f8a9
family_name	WannaCry
is_private	false
sha256          bf293bda73c5b4c1ec66561ad20d7e2bc6692d051282d35ce8b7b7020c753467
sub_verdict	known_malicious
verdict	        malicious
```


### intezer-analyze-by-file
***
Checks file reputation for uploaded file (up to 150MB)


#### Base Command

`intezer-analyze-by-file`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| file_entry_id | The file entry id to upload. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| Intezer.Analysis.ID | string | Intezer analysis id | 
| Intezer.Analysis.Status | string | status of the analysis | 
| Intezer.Analysis.Type | string | type of the analysis | 


#### Command Example
``` 
intezer-analyze-by-file file_entry_id=1188@6
```

#### Human Readable Output
```
Analysis created successfully: 675515a1-62e9-4d55-880c-fd46a7963a56
```


### intezer-get-analysis-result
***
Check the analysis status and get analysis result, support file and endpoint analysis


#### Base Command

`intezer-get-analysis-result`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| analysis_id | The analysis ID we want to get results for. | Optional | 
| analysis_type | The type of the analysis. Possible values are: File, Endpoint. Default is File. | Optional | 
| indicator_name | indicator to classify. | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| File.SHA256 | string | Hash SHA256 | 
| File.Malicious.Vendor | string | For malicious files, the vendor that made the decision | 
| DBotScore.Indicator | string | The indicator we tested | 
| DBotScore.Type | string | The type of the indicator | 
| DBotScore.Vendor | string | Vendor used to calculate the score | 
| DBotScore.Score | number | The actual score | 
| File.Metadata | Unknown | Metadata returned from Intezer analysis \(analysis id, analysis url, family, family type, sha256, verdict, sub_verdict\). Metedata will be retuned only for supported files. | 
| Endpoint.Metadata | Unknown | Metadata returned from Intezer analysis \(endpoint analysis id, endpoint analysis url, families,  verdict, host_name\) | 
| File.ExistsInIntezer | Boolean | Does the file exists on intezer genome database | 


#### Command Example
``` 
intezer-get-analysis-result analysis_id="9e3acdc3-b7ea-412b-88ae-7103eebc9398"
```

#### Human Readable Output
```
Intezer File analysis result
----
SHA256: bf293bda73c5b4c1ec66561ad20d7e2bc6692d051282d35ce8b7b7020c753467
Verdict: malicious (known_malicious)
Family: WannaCry


Analysis Report
---
analysis_id	006c54ba-3159-43a0-98a0-1c5032145f47
analysis_time	Tue, 29 Jun 2021 13:40:01 GMT
analysis_url	https://analyze.intezer.com/analyses/006c54ba-3159-43a0-98a0-1c5032145f47
family_id	0b13c0d4-7779-4c06-98fa-4d33ca98f8a9
family_name	WannaCry
is_private	false
sha256          bf293bda73c5b4c1ec66561ad20d7e2bc6692d051282d35ce8b7b7020c753467
sub_verdict	known_malicious
verdict	        malicious
```



### intezer-get-sub-analyses
***
Get a list of the analysis sub analyses


#### Base Command

`intezer-get-sub-analyses`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| analysis_id | The analysis ID we want to get the sub analyses for. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| Intezer.Analysis.ID | string | Intezer analysis id | 
| Intezer.Analysis.SubAnalyses | Unknown | List of all sub analyses of the give analysis | 


#### Command Example
```
intezer-get-sub-analyses analysis_id=006c54ba-3159-43a0-98a0-1c5032145f47
```

#### Human Readable Output
```
Sub Analyses -
[
...
List of analyses ids
...
]
```


### intezer-get-family-info
***
Get family information from Intezer Analyze


#### Base Command

`intezer-get-family-info`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| family_id | The Family ID. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| Intezer.Family.ID | string | Family id in intezer genome database | 
| Intezer.Family.Name | string | Family name | 
| Intezer.Family.Type | string | Family Type | 


#### Command Example
``` 
intezer-get-family-info family_id=006c54ba-3159-43a0-98a0-1c5032145f47
```

#### Human Readable Output
```
Family Info
---

FamilyId    006c54ba-3159-43a0-98a0-1c5032145f47
FamilyName  Some Family Name
FamilyType  Malware
```


### intezer-get-analysis-code-reuse
***
Get All code reuse report for an analysis or sub analysis
To get the code reuse results of a sub analysis you also must specify the "parent analysis",

For example - If you ran the command `intezer-get-sub-analyses analysis_id=123` 
and got the sub analysis `456`, you need to specify both in the command

#### Base Command

`intezer-get-analysis-code-reuse`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| analysis_id | The analysis ID (parent analysis in case we're trying to get sub abalysis) we want to get the code reuse for. | Required | 
| sub_analysis_id | The Sub Analysis we want to get the code reuse for. | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| Intezer.Analysis.ID | string | The composed analysis ID | 
| Intezer.Analysis.CodeReuse | Unknown | General Code Reuse of the analysis | 
| Intezer.Analysis.CodeReuseFamilies | Unknown | List of the families appearing in the code reuse | 
| Intezer.Analysis.ComposedAnalysisID | string | The Composed analysis id | 


#### Command Example
``` 
# Get the code reuse of an analysis
intezer-get-analysis-code-reuse analysis_id=<Root analysis>

# Get the root analysis sub analyses
intezer-get-sub-analyses analysis_id=<Root analysis>

# Use one of the results to get the sub analysis code reuse
intezer-get-analysis-code-reuse analysis_id=<Root analysis> sub_analysis_id=<Sub Analysis Id>
```

#### Human Readable Output
This will show information about the analysis code reuse and families

```
Code Reuse
---
common_gene_count   544
gene_count          543
gene_type           native_windows
unique_gene_count   0

Families:
---

WannaCry
family_id	        0b13c0d4-7779-4c06-98fa-4d33ca98f8a9
family_name	        WannaCry
family_type	        malware
reused_gene_count	362

Lazarus
family_id	        7ae9c0f1-5e81-4ed1-928d-d966a1b1525c
family_name	        Lazarus
family_type	        malware
reused_gene_count	33

... More Families if available
```


### intezer-get-analysis-metadata
***
Get metadata for an analysis or sub analysis
To get the metadata of a sub analysis you also must specify the "parent analysis",

For example - If you ran the command `intezer-get-sub-analyses analysis_id=123` 
and got the sub analysis `456`, you need to specify both in the command


#### Base Command

`intezer-get-analysis-metadata`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| analysis_id | The analysis ID we want to get the metadata for. | Required | 
| sub_analysis_id | The Sub Analysis we want to get the metadata for. | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| Intezer.Analysis.ID | string | The composed analysis ID | 
| Intezer.Analysis.Metadata | Unknown | The Analysis metadata | 
| Intezer.Analysis.ComposedAnalysisID | string | The Composed analysis id | 


#### Command Example
``` 
# Get the code reuse of an analysis
intezer-get-analysis-metadata analysis_id=<Root analysis>

# Get the root analysis sub analyses
intezer-get-sub-analyses analysis_id=<Root analysis>

# Use one of the results to get the sub analysis code reuse
intezer-get-analysis-metadata analysis_id=<Root analysis> sub_analysis_id=<Sub Analysis Id>
```

#### Human Readable Output
```
Analysis Metadata
---

architecture	        i386
company	                Microsoft Corporation
compilation_timestamp	2009:07:13 23:19:35+00:00
file_type	        pe
md5	                264b6a2d214586ed9b213d29b2262046
original_filename	LODCTR.EXE
product	                Microsoft® Windows® Operating System
product_version	        6.1.7600.16385 ^^^
sha1	                0e838068e42a7bfbd7d40a4743fae3cf733f7ac1
sha256	                bf293bda73c5b4c1ec66561ad20d7e2bc6692d051282d35ce8b7b7020c7534^^^67
size_in_bytes	        245760
ssdeep	                3072:Rmrhd5J1eigWcR+uiUg6p4FLlG4tlL8z+mmCeHFZjoHEo3m:REd5vIZiZhLlG4AimmCo
```

