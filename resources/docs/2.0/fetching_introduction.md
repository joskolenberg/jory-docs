# Fetching Data

---

- [Introduction](#introduction)

<a name="introduction"></a>
## Introduction
Once you've got your Jory Resources set up the data can be fetched by sending a Jory Query to the Jory Endpoints.

Jory Queries are JSON strings containing your query data. All keys have a full and minified version.

| Full key        | Minified           | Description  |
| ------------- | ------------- | ----- |
| fields      | fld | Define which fields you want to be returned (SELECT) |
| filter      | flt      | Apply a filter (WHERE) |
| sorts | srt     | Apply sorting (ORDER BY) |
| relations | rlt     | Define which relations you want to be returned |
| offset | ofs     | Apply an offset |
| limit | lmt     | Limit |

<br>
Select clauses are more complex and therefor have their own "sub" keys.

| Full key        | Minified           | Description  |
| ------------- | ------------- | ----- |
| field      | f | Define on which field you want to filter |
| operator      | o      | Define what operator you want to use in the filter. E.g. "=", ">" |
| data | d     | Define by which value you want to filter |
| group_and | and     | Create a grouped ```AND``` clause |
| group_or | or     | Create a grouped ```OR``` clause |

> {info} The full and minified keys can always be used both and can even be mixed within a single Jory Query. For clarity and consistency all documentation only uses the full keys. 
