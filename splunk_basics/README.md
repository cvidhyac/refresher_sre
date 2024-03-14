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