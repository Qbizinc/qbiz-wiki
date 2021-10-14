---
title: Variant Fields
description: A writeup on my findings about using VARIANT type fields within Snowflake
published: true
date: 2021-10-14T23:18:00.699Z
tags: snowflake
editor: markdown
dateCreated: 2021-10-14T23:17:22.852Z
---

# Variant Fields
I recently had to do a comparison between using a JSON block stored in either a VARCHAR or a VARIANT field within Snowflake.  

| VARIANT | STRING/VARCHAR |
|---------|----------------|
| Snowflake specific implementation | Standard ANSI SQL type |
| Sorts JSON contents alphabetically | Maintains JSON ordering |
| JSON contents referenceable in SQL via colon notation | JSON contents referenceable in SQL via parse_json function and colon notation | 


* There does not seem to be any functionality lost with either approach
* There may be internal optimizations that go in within Snowflake for the VARIANT fields which would not be achieved by using multiple calls to parse_json (i.e. it's parsed only once to store as a VARIANT), but we don't have enough data present for that to be apparent.
* Changing the table definition and then moving the data between a VARIANT and a VARCHAR field is possible and does not seem to be a costly operation, so if we "guess wrong", we can change our minds later with only a minor manual operation as impact.
* Writing from the spark dataframes to the VARIANT field works the same as when writing to the VARCHAR field.

## Data Transfer
This was done within Snowflake UI directly, note that just selecting the alert_payload, doesn't work, it needs to be wrapped in a parse_json block.  

 
{.is-info}
> Selecting from a VARCHAR field and writing to a VARIANT field is done with the `parse_json` function


```
INSERT INTO table_with_variant( ID, PAYLOAD )
      SELECT ID, parse_json(PAYLOAD) 
      FROM table_with_varchar
```

## Differences
A couple of things jump out when the data is examined in the VARIANT field.
```
SELECT ID, 
       PAYLOAD:"City" as inner_city,
       PAYLOAD 
FROM   table_with_variant tv
WHERE  ID = 'Value'
   AND PAYLOAD:"State" = 'CA'
```

* The contents of the Payload can be referenced using a colon (:) notation
* If the JSON header contains spaces it can be accommodated with double quotes
* The colon notation works in both the select and the WHERE clauses
* The PAYLOAD field when clicked on brings up a neatened and alphabetized json structure
	
To contrast those statements, I tried to do the same things against the  table where the PAYLOAD is defined as a STRING, and it turns out that everything I could do with the VARIANT column, I can still do by calling parse_json in place of the variant field.

```
SELECT ID, 
       parse_json(PAYLOAD):"City" as inner_city, 
       PAYLOAD, 
       parse_json(PAYLOAD) as PAYLOAD_PARSED 
FROM   table_with_varchar tv
WHERE  ID = 'Value'
   AND parse_json(PAYLOAD):"State" = 'CA'
```


* The contents of the payload are still accessible in the SQL query by putting the "parse_json" function call in line.
* The PAYLOAD from the STRING column is not alphabetized and does not display as "neat" JSON
* The colon notation with the parse_json call inline works in both SELECT and WHERE clauses.

> Referencing into the JSON is case sensitive, i.e. "Gender" != "gender", so that's something to be aware of.
{.is-warning}



