---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# 2022 TREC CrisisFACTS Track


{::options parse_block_html="true" /}
<div class='mailing-list-banner'>

## Announcements
{: .mailing-list-title}

- __2022-09-23__: The [TREC submission page](https://ir.nist.gov/trecsubmit/crisis.html) for CrisisFACTS is now open!
- __2022-07-18__: Baseline system, based on PyTerrier, and scripts for verifying run files are now available on the [CrisisFACTS Utilities GitHub repo](https://github.com/crisisfacts/utilities).
- __2022-06-24__: We have updated the CrisisFACTS website.
- __2022-06-18__: We have released utilities for downloading the track data via `ir_datasets`, available on [GitHub](https://github.com/crisisfacts/utilities).
- __2022-06-17__: _CrisisFACTS task guidelines have been released!_ You can check out the <a href='/assets/pdf/guidelines.v1.0.pdf'>guidelines here</a>, ask questions on <a href='https://groups.google.com/g/trec-is'>our mailing list</a>, or join the `#crisis-facts-2022` channel on the <a href="https://trectalk.slack.com/">TREC Slack channel</a>.
{: .mailing-list-announcement-new}

</div>


<div class='register-banner'>

Participation in CrisisFACTS and access to the track's datasets (i.e., data streams and queries) are free but require __registration with TREC__. Your registration can be submitted [__here__](https://ir.nist.gov/trecsubmit.open/application.html).  
{: .register-banner-items}  

</div>

{::options parse_block_html="false" /}


Tracking developments in topics and events has been studied at TREC and other venues for several decades (e.g., from [DARPA’s early Topic-Detection and Tracking](https://ciir.cs.umass.edu/tdt) initiative to the more recent [Temporal Summarization](https://trec.nist.gov/pubs/trec24/papers/Overview-TS.pdf) and [Real-Time Summarization](http://trecrts.github.io) TREC tracks). Today's high-velocity, multi-stream information ecosystem, however, leads to missed critical information or new developments, especially during crises. While modern search engines are adept at providing users with search results relevant to an event, they are ill-suited to multi-stream fact-finding and summarization needs. The CrisisFACTS track aims to foster research that closes these gaps. 

CrisisFACTS is making available multi-stream datasets from several disasters, covering __Twitter__, __Reddit__, __Facebook__, and __online news sources__. We supplement these datasets with queries defining the information needs of disaster-response stakeholders (extracted from [FEMA ICS209 forms](/assets/pdf/ics209.pdf)). Participants’ systems should integrate these streams into temporally ordered lists of important facts, which we can aggregate into summaries for disaster response personnel. 

-------

#### Jump to:

- [Overview](#overview)
- [2022 Task Overview](#tasks)
- [Datasets](#datasets)
- [Event List](#events)
- [Submissions](#submissions)
- [Evaluation](#evaluation)
- [Important Dates](#timeline)
- [Organizers](#organizers)


-------

## Overview

This track's core information need is: 

    What critical new developments have occurred 
    that I need to know about?

Many pieces of information posted during a disaster are not essential for responders or disaster-response managers. To make these needs explicit, we have made a list of general and disaster-specific queries/"user profiles", available [here](/assets/json/CrisisFACTs-2022-v3.queries.json). These queries capture a responder might consider important, such as the following:

- Approaching threats to life, property, infrastructure, response operations, etc.
- Changes to affected areas
- Changes to damage assessments
- Critical-resource needs, such as food, water or medicine
- Damage to key infrastructure, evacuations, or emerging threats
- Incident command transitions (setup or transfer of command and control capabilities)
- Progress made and accomplishments by responders
- Reports and statistics regarding civilians and responders, such as casualties, those in temporary shelters, needing immunizations, or those missing
- Risks from hazardous materials, such as chemicals, fuels, infectious agents or radiation
- Restriction to the use or availability of resources
- Weather concerns, e.g. high wind, temperatures, humidity, floods, or watches/warnings

Emergency response staff typically want to receive a summary of this information at particular points during the emergency. Such a summary might be generated at the start of a new shift so the next group of team members can be informed about new developments. Alternatively, local government or media agencies might request updates on the emergency. 

Currently, these information needs are fulfilled via manual summarization, e.g. by filling an incident report such as the [FEMA ICS209 forms](/assets/pdf/ics209.pdf).

-------

## 2022 Task -- Fact Extraction for Downstream Summarization {#tasks}

The 2022 track will have a single fact-extraction task, where systems consume a multi-stream dataset for a given disaster, broken into disaster-day pairs. From this stream, the system should produce a minimally redundant list of atomic facts, with importance scores denoting how critical the fact is for responders. CrisisFACTS organizers will aggregate these facts into daily summaries for these disasters, along the following lines:

![Example](/assets/img/Example.png)
![ConOps](/assets/img/HighLevel.png)
<p style="text-align:center">Fig 0. ConOps/High-Level System Overview</p>

### System Input

Input to participant systems include:

- __Event Definitions__: We provide content for a set of eight crises, including wildfires, hurricanes and flood events. The following is an example event definition:

```json
{
    "eventID": "CrisisFACTS-001",
    "trecisId": "TRECIS-CTIT-H-092",
    "dataset": "2017_12_07_lilac_wildfire.2017",
    "title": "Lilac Wildfire 2017",
    "type": "Wildfire",
    "url": "https://en.wikipedia.org/wiki/Lilac Fire",
    "description": "The Lilac Fire was a fire that burned in northern San Diego County, California, United States, and the second-costliest one one of multiple of multiple wildfires that erupted in Southern California in December 2017."
}
```
<p style="text-align:center">Fig 1. Example Event Definition for the 2017 Lilac Fire</p>

- __User Profiles__: The itemised list of general and queries representing the user’s information needs. The union of these queries should be used to create a directed summary containing only matching information. An example of such queries is as follows:

```json
[{
  "queryID": "CrisisFACTS-General-q001",
  "indicativeTerms": "airport closed",
  "query": "Have airports closed",
  "trecisCategoryMapping": "Report-Factoid"
},
{
  "queryID": "CrisisFACTS-General-q002",
  "indicativeTerms": "rail closed",
  "query": "Have railways closed",
  "trecisCategoryMapping": "Report-Factoid"
},
{
  "queryID": "CrisisFACTS-General-q003",
  "indicativeTerms": "water supply",
  "query": "Have water supplies been contaminated",
  "trecisCategoryMapping": "Report-EmergingThreats"
},
...,
{
  "queryID": "CrisisFACTS-Wildfire-q001",
  "indicativeTerms": "acres size",
  "query": "What area has the wildfire burned",
  "trecisCategoryMapping": "Report-Factoid"
},
{
  "queryID": "CrisisFACTS-Wildfire-q002",
  "indicativeTerms": "wind speed",
  "query": "Where are wind speeds expected to be high",
  "trecisCategoryMapping": "Report-Weather"
},
...
]
```
<p style="text-align:center">Fig 2. Example Query Definition</p>

- __Summary Requests__: Each request lists the event ID, date to summarise, and the start and end timestamps bounding the requested summary. A multi-day disaster will have multiple such summary requests:

```json
[{
  "eventID": "CrisisFACTS-001",
  "requestID": "CrisisFACTS-001-r3",
  "dateString": "2017-12-07",
  "startUnixTimestamp": 1512604800,
  "endUnixTimestamp": 1512691199
},
...,
{
  "eventID": "CrisisFACTS-001",
  "requestID": "CrisisFACTS-001-r4",
  "dateString": "2017-12-08",
  "startUnixTimestamp": 1512691200,
  "endUnixTimestamp": 1512777599
}]
```
<p style="text-align:center">Fig 3. Example Summary Requests</p>

- __Content Streams__: A set of content streams from online sources, containing itemised text snippets (e.g., sentences) related to the event (though a specific snippet may not be a fact relevant to the disaster’s user profile). Each item has an event, item identifier, source, timestamp and a piece of text. These items contain the content that your system should use to produce your summary, and the union of these snippets should reproduce the original content stream from Twitter, Reddit, etc.

```json
[{
  "event": "CrisisFACTS-001",
  "streamID": "CrisisFACTS-001-Twitter-14023-0",
  "unixTimestamp": 1512604876,
  "text": "Big increase in the wind plus drop in humidity tonight into Thursday for San Diego County #SanDiegoWX https://t.co/1pV0ZAhsJH",
  "sourceType": "Twitter"
},
{
  "event": "CrisisFACTS-001",
  "streamID": "CrisisFACTS-001-Twitter-27052-0",
  "unixTimestamp": 1512604977,
  "text": "Prayers go out to you all! From surviving 2 massive wild fires in San Diego and California in general we have all c… https://t.co/B5Y7KLY0uS",
  "sourceType": "Twitter"
},
{
  "event": "CrisisFACTS-001",
  "streamID": "CrisisFACTS-001-Twitter-43328-0",
  "unixTimestamp": 1512691164,
  "text": "If you're in the San Diego area (or north of it), you should probably turn on tweet notifs from @CALFIRESANDIEGO fo… https://t.co/hNjEuEfKaB",
  "sourceType": "Twitter"
}]
```
<p style="text-align:center">Fig 4. Three Event Snippets for Event CrisisFACTS-001</p>

### System Output {#output}

Your system should produce one summary for each request using only the content provided for that event and only between the starting and ending timestamps. 

This task differs from traditional summarization in that you should not simply produce a block of text of a set length. Instead, this track’s “summaries” are comprised of the facts describing the target disaster’s evolution. As such, your summaries consist of an itemised list of ‘facts’ that match one or more of the user information needs. 

We will use these top-k "most important" facts from a given event-day pair as the summary for that event's day.

Each fact should contain the following:

#### Required:

- __RequestID__: The identifier of the summary request.
- __Fact Text__: A short single-sentence statement that conveys an important, atomic and self-contained piece of information about the event.
- __Timestamp__: A UNIX epoch timestamp indicating the earliest point in time that your system detected the fact.
- __Importance__: A numerical score between 0 and 1 that indicates how important your system thinks it is for this fact to be included in the final summary. Evaluation will use this value to rank your facts, taking the top-k as the summary.
- __Sources__: A list of StreamID identifiers that your system believes support the fact (what you used as input to produce the fact text).

#### Optional:

- __StreamID__: If your system is extractive (i.e., your facts are direct copies of text found in the input streams), include the `StreamID` of your fact’s original item.
- __InformationNeeds__: If your system directly uses the queries/information-needs to decide what to include in your summary, and these can act as an explanation for what your system included an item, then list the queries that were core to this item’s selection.

### Output Examples {#output-examples}

#### Example Abstractive Output

Examples of system output are as follows:

```json
[{
    "requestID": "CrisisFACTS-001-r3",
    "factText": "Increased threat of wind damage in the San Diego area.",
    "unixTimestamp":1512604876,
    "importance": 0.71,
    "sources": [
    "CrisisFACTS-001-Twitter-14023-0"
    ],
    "streamID": null,
    "informationNeeds": ["CrisisFACTS-General-q015"]
}, 
...
]
```

Fig 5. Example System Output with Abstractive Facts. The `streamID` field is empty as this fact may not appear in the dataset verbatim. It is, however, supported by one Twitter message.
{: style="text-align:center"}

#### Example Extractive Output

```json
[{
    "requestID": "CrisisFACTS-001-r3",
    "factText": "Big increase in the wind plus drop in humidity tonight into Thursday for San Diego County #SanDiegoWx https://t.co/1pVOZAhsJH",
    "unixTimestamp":1512604876,
    "importance": 0.71,
    "sources": [
    "CrisisFACTS-001-Twitter-14023-0"
    ],
    "streamID": "CrisisFACTS-001-Twitter-14023-0",
    "informationNeeds": ["CrisisFACTS-General-q015"]
}, 
...
]
```
Fig 6. Example System Output with Extractive Facts. The `streamID` field is populated with the Twitter document from which this text was taken.
{: style="text-align:center"}

### Other Output Details

Participant systems may produce as many facts as they wish for a specific summary request. However, to handle variable summary length, each fact may not contain more than 200 characters. 

For days after the first, your system should avoid returning information that has been reported in previous summaries for the same event. Furthermore, evaluation will be performed at a predetermined number of facts (not revealed in advance). To truncate your list of facts, we will rank them by importance score and  cut at a specific rank k – which will vary across event-day pairs. 

We recommend that you return at least 100 facts per summary request.   

-------  

## Datasets and Sources {#datasets}

### Disaster Data Streams

For each day during an event, the following content is available:

- __Twitter Stream__: We are re-using tweets collected as part of the TREC Incident Streams track (http://trecis.org). These tweets were crawled by keyword, should be relevant to the event (though some noise likely exists), but are not necessarily good candidates for inclusion into a summary of what is happening.
- __Reddit Stream__: We have collected relevant Reddit threads to each event, where we include both the original submission and subsequent comments within those threads. These streams are built from relevant top-level posts with all comments associated with the post, so some comments may not be informative.
- __Facebook Stream__: We provide Facebook/Meta posts from public pages that are relevant to each event using [CrowdTangle](http://crowdtangle.com). While we cannot share the text content of these posts directly, we have included the post and page IDs of this content within the stream. This data can then be re-collected manually.
- __News Stream__: Traditional news agencies are often a good source of information during an emergency, and we have included a small number of news articles collected during each event as well.

### Accessing the Data

CrisisFACTS has transitioned to the ir_datasets infrastructure for making data available to the community. We provide a GitHub repository with Jupyter notebooks and a Collab notebook to accelerate participants’ access to this data:

- [GitHub Repository](https://github.com/crisisfacts/utilities )
    - [Jupyter Notebook](https://github.com/crisisfacts/utilities/blob/main/00-Data/00-CrisisFACTS.Downloader.ipynb)
- [Google Collab Notebook](https://colab.research.google.com/github/crisisfacts/utilities/blob/main/00-Data/00-CrisisFACTS.Downloader.ipynb)


-------

## Disaster Events {#events}

We provide these data streams to participants to use as the source of content for inclusion into their summaries. The statistics for each of the eight 2022 events are listed below:

| eventID         | Title                     | Type      |  Tweets | Reddit | News  | Facebook |
| --------------- | ------------------------- | --------- | ------:| ------:| -----:| --------:|
| CrisisFACTS-001 | Lilac Wildfire 2017       | Wildfire  |  41,346  | 1,738   | 2,494  | 5,437     |
| CrisisFACTS-002 | Cranston Wildfire 2018    | Wildfire  |  22,974  | 231    | 1,967  | 5,386     |
| CrisisFACTS-003 | Holy Wildfire 2018        | Wildfire  |  23,528  | 459    | 1,495  | 7,016     |
| CrisisFACTS-004 | Hurricane Florence 2018   | Hurricane |  41,187  | 120,776 | 18,323 | 196,281   |
| CrisisFACTS-005 | Maryland Flood 2018       | Flood     |  33,584  | 2,006   | 2,008  | 4,148     |
| CrisisFACTS-006 | Saddleridge Wildfire 2019 | Wildfire  |  31,969  | 244    | 2,267  | 3,869     |
| CrisisFACTS-007 | Hurricane Laura 2020      | Hurricane |  36,120  | 10,035  | 6,406  | 9,048     |
| CrisisFACTS-008 | Hurricane Sally 2020      | Hurricane  | 40,695  | 11,825  | 15,112 | 48,492    |




## Submissions

Runs will be submitted through the NIST submission system at [trec.nist.gov](http://trec.nist.gov). Runs that do not pass validation will be rejected outright. Submitted runs will be asked to specify the following:

- Whether they are (a) manual or automatic, 
- Whether they use (b) extant TREC-IS labels for Twitter data, and 
- Whether the summaries built are (c) abstractive or extractive.

### Automatic Runs

Each run submission must indicate whether the run is manual or automatic. An automatic run is any run that receives no human intervention once the system is started and provided with the task inputs. We expect most CrisisFACTS runs to be automatic. 

### Manual Runs

Results on manual runs will be specifically identified when results are reported. A manual run is any run in which a person manually changes, summarises, or re-ranks queries, the system, or the system’s lists of facts. Simple bug fixes that address only format handling do not result in manual runs, but the changes should be described.

### Submission Format

The submission format for CrisisFACTS is a newline-delimited `JSON` file, where each entry in the submitted file contains the fields outlined in [System Output](#output) section above. Each submission file corresponds to a single submitted run (i.e., all event-day pairs for all events), with the submission’s runtag included in the filename. 

Example submissions are available in [Output Examples](#output-examples).


-------

## Evaluation

Submitted systems will be evaluated using two sets of approaches for this first year. In both approaches, participant systems’ lists of facts will be truncated to a private k value based on NIST assessors’ assessments.

### Metric Set 1 – Standard Summarization Metrics Against Extant Summaries

- First, the top-k facts for each system’s event-day pair will be aggregated into a “summary” of that day’s new developments. Then, this summary will be compared against extant summaries using standard summarization metrics (e.g., Rouge-1, Rouge-2, and/or Rouge-L).
- These extant summaries may include:
    - NIST-based assessments of these events and related summaries
    - Summaries extracted from the ICS209 reports from FEMA, which are available for each disaster event
    - Wikipedia articles summarising the events


### Metric Set 2 – Matching Facts Between Participant System and Manual Assessments

- For this metric set, we first manually construct a map for matching a system’s submitted facts for a given event-day pair to the NIST-assessor-developed set of facts for that event-day.
- For a given event-day pair, we therefore have:
    - A set of “gold standard” facts $S$ produced by NIST assessors
    - A set of submitted facts $n \in N$ from a system 
    - A fact-matching function $M(N, S)$ that measures the overlap between submitted facts and the gold-standard fact set. This fact-matching function comes in two forms:
        - Graded matching:  $\sum_{s \in S}MaxSim(s,N)$
            - $MaxSim(x, Y)$ measures the maximum similarity in the range $[0, 1]$ between fact $x$ and each fact in $y \in Y$.
        - Binary matching: $\sum_{s \in S} BinaryMatch(s, N)$
            - $BinaryMatch(s, N)$ returns 1 iff fact $s$ is in set of facts $N$
    - Using this fact-matching function, we define __comprehensiveness__ (essentially a recall-oriented metric):
        - $C(N, S) = \frac{M(N, S)}{\|S\|}$
    - We likewise define a __verbosity__ metric as the number of submitted facts that match a gold-standard fact (i.e., precision):
        - $V(N, S) = \frac{M(S, N)}{\|N\|}$
    - For each disaster event, we calculate the average __comprehensiveness__ and __verbosity__ scores over all days in the event where $S$ contains facts.


-------

## Timeline

| Milestone | Date |
|-----------------------|---------------|
| Guidelines released | 17 June 2022 |
| Submissions Due | 26 September 2022 |
| NIST-Assessor Evaluation | 29 Sept - 27 Oct 2022| 
| Scores returned to participants | 28 October 2022| 
| TREC Notebook Drafts Due | 7 November 2022 (Tentative)| 
| TREC Conference | 14 November 2022| 


## Organizers


![image-left](/assets/img/buntain.jpg){: .align-left .avatar} 

[Cody Buntain](http://cody.bunta.in) <br/> [@cbuntain](http://twitter.com/codybuntain) <br/> he/him <br/> College of Information Studies, University of Maryland, College Park.
{: .align-left}

![image-left](/assets/img/mccreadie.jpg){: .align-left .avatar} 

[Richard McCreadie](http://dcs.gla.ac.uk/~richardm/Home/) <br/> [@richardm_](http://twitter.com/richardm_) <br/> he/him <br/> School of Computing Science, University of Glasgow.
{: .align-left}
