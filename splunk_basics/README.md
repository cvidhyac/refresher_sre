# Splunk Basics

## Splunk Terminology

See - [Splunk Lexicon](https://docs.splunk.com/Splexicon)

## Terminologies for daily use

- Event
- Search
- Report
- Dashboard
- SPL

Additionally, understand:

- SourceType
- Index
- Field Extractions
- Lookup Tables

### Event

- (Interpreted from splunk lexicon definition) A single piece of data in splunk software - record in a log file
- Each event **HAS-A**:
    - timestamp
    - host
    - source
    - sourceType
- Events are JSON/XML, and similar events can be grouped to an `EventType`

### Search

- Primary way users navigate in splunk
- Search to - retrieve events, run statistical commands to calculate metrics and generate reports,
  chart trends, predicting future patterns etc.,

### Report
- A saved search == Report

### Calculating Statistics

**Key Commands**
- `stats`
- `timechart`
- `chart`
- `top`

1. stats

- Has a variety of functions and uses
- Typically accompanied by a visualization in the form of a table
- Many functions

#### Stats usages

* Display integer Count by field : 
  ```shell
    index=myIndex host=myHost "ERROR" | stats count 
  ```
  
* Display events by field occurrence: 
  ```shell 
   index=myIndex host=myHost "ERROR" | stats count by breadcrumb_id, span_id
  ```
  
* Status by fast, normal and slow transactions:
  - ```shell
    index=myIndex host=myHost | stats
    count(eval(responseTime < 1000)) AS "FAST"
    count(eval(responseTime > 1000 AND responseTime < 2000)) AS "NORMAL"
    count(eval(responseTime > 2000) AS "SLOW"
    ```
    
* Print a table finding the count of errors for each status code across referer

```shell
index=myIndex host=myHost earliest==5m (statusCode=4* OR statusCode=5*) | stats
count AS "Total number of Errors", values(referer) by statusCode
```

* Print count of unique values
```shell
index=myIndex host=myHost | stats estdc(cliIP) by statusCode
index=myIndex host=myHost | stats distinct_count(cliIP) by statusCode
```

* List values of a field with duplicates
```shell
index=myIndex host=myHost | stats list(cliIp) by statusCode
```

* list values of a field without duplicates(unique values)
```shell
index=myIndex host=myHost | stats values(cliIP) by statusCode
```

#### top AND rare usages

**Tip** `showPerc=f` hides default percentage displayed, `f`=false

Find top 5 status codes in the last 30 seconds

```shell
index=myIndex sourceType=mySourceType host=myHost earliest=-30s | top 5 statusCode showPerc=f
```

Find least 5 status code occurrences in last 3 minutes

```shell
index=myIndex host=myHost sourceType=mySourceType earliest=-3m | rare 5 statusCode showPerc=f
```