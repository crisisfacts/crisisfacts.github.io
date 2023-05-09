---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# 2023 TREC CrisisFACTS Track


{::options parse_block_html="true" /}
<div class='mailing-list-banner'>

## Announcements
{: .mailing-list-title}
- __2023-05-10__: _CrisisFACTS task guidelines have been released!_ You can check out the guidelines on this page, ask questions on <a href='https://groups.google.com/g/trec-is'>our mailing list</a>, or join the `#crisis-facts-2023` channel on the <a href="https://trectalk.slack.com/">TREC Slack channel</a>.
- __2023-03-10__: _CrisisFACTS 2022 overview paper accepted to ISCRAM 2023!_ The overview paper for CrisisFACTS 2022 has been accepted for [ISCRAM 2023](https://www.unomaha.edu/college-of-information-science-and-technology/iscram2023/index.php). A pre-print of this paper is available [here](/assets/pdf/crisisfacts2022.iscram2023.pdf)
- __2023-01-06__: _CrisisFACTS 2022 Facts and automatic evaluation are available!_ You can find the gold-standard facts from CrisisFACTS 2022 <a href="https://github.com/crisisfacts/utilities/blob/main/03-Evaluation/01-AutoEvaluation/CrisisFACTs-2022.facts.json">here</a> and the 2022 automatic evaluation in <a href="https://github.com/crisisfacts/utilities/tree/main/03-Evaluation/01-AutoEvaluation">GitHub</a>.

{: .mailing-list-announcement-new}

</div>


<div class='register-banner'>

Participation in CrisisFACTS and access to the track's datasets (i.e., data streams and queries) are free but require __registration with TREC__. Your registration can be submitted [__here__](https://ir.nist.gov/trecsubmit.open/application.html).  
{: .register-banner-items}  

</div>

{::options parse_block_html="false" /}


Tracking developments in topics and events has been studied at TREC and other venues for several decades (e.g., from [DARPA’s early Topic-Detection and Tracking](https://ciir.cs.umass.edu/tdt) initiative to the more recent [Temporal Summarization](https://trec.nist.gov/pubs/trec24/papers/Overview-TS.pdf) and [Real-Time Summarization](http://trecrts.github.io) TREC tracks). Today's high-velocity, multi-stream information ecosystem, however, leads to missed critical information or new developments, especially during crises. While modern search engines are adept at providing users with search results relevant to an event, they are ill-suited to multi-stream fact-finding and summarization needs. The CrisisFACTS track aims to foster research that closes these gaps. 

CrisisFACTS is making available multi-stream datasets from several disasters, covering __Twitter__, __Reddit__, __Facebook__, and __online news sources__ gathered from the [NELA Toolkit](https://github.com/BenjaminDHorne/The-NELA-Toolkit). We supplement these datasets with queries defining the information needs of disaster-response stakeholders (extracted from [FEMA ICS 209 forms](/assets/pdf/ics209.pdf)). Participants’ systems should integrate these streams into daily lists of facts, which we can aggregate into summaries for disaster response personnel. 

-------

#### Jump to:

- [Overview](#overview)
- [2023 Task Overview](#tasks)
- [Datasets](#datasets)
- [Event List](#events)
- [Submissions](#submissions)
- [Evaluation](#evaluation)
- [Important Dates](#timeline)
- [Organizers](#organizers)


-------

## Overview

This track's core information need is: 

    What critical new developments have occurred *today* 
    that I need to know about?

Many pieces of information posted during a disaster are not essential for responders or disaster-response managers. To make these needs explicit, we have made a list of general and disaster-specific queries/"user profiles", available [here](/assets/json/CrisisFACTs-2022-v3.queries.json). These queries capture a responder might consider important, such as the following:

- Emerging/approaching threats
- Changes to affected areas
- Changes to damage assessments
- Critical needs, such as food, water or medicine
- Damage to key infrastructure, evacuations, or emerging threats
- Progress made and accomplishments by responders
- Statistics on casualties or numbers missing
- Risks from hazardous materials
- Restriction on or availability of resources
- Weather concerns

Responders typically want to receive a summary of this information at regular intervals during an emergency event. Stakeholders current fulfill these information needs via manual summarization, e.g., by filling daily incident reports such as the [FEMA ICS 209 forms](/assets/pdf/ics209.pdf).

-------

## 2023 Tasks -- Fact Extraction for Downstream Summarization {#tasks}

As in 2022, the main 2023 task focuses on fact extraction, where systems consume a multi-stream dataset for a given disaster, broken into disaster-day pairs. From this stream, the system should produce a minimally redundant list of atomic facts, with importance scores denoting how critical the fact is for responders. CrisisFACTS organizers will aggregate these facts into daily summaries for these disasters, along the following lines:

![Example](/assets/img/Example.png)
![ConOps](/assets/img/CrisisFACTSDiagram.png)
<p style="text-align:center">Fig 1. ConOps/High-Level System Overview</p>

### System Input

Input to participant systems include:

- __Event Definitions__: We provide content for multiple crises, including wildfires, hurricanes and flood events. The following is an example event definition:

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
<p style="text-align:center">Fig 2. Example Event Definition for the 2017 Lilac Fire</p>

- __User Profiles__: The itemised list of general and event-type-specific queries representing a responder’s information needs. These queries provide a method for filtering disaster-related content to only ICS-related facts. We also include TREC-IS category mappings for each query. Example queries include:

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
<p style="text-align:center">Fig 3. Example Query Definition</p>

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
<p style="text-align:center">Fig 4. Example Summary Requests</p>

- __Content Streams__: A set of content snippets from online streams. Each snippet is approximately a sentence in length and may not be relevant to the event--i.e., __content streams are noisy__. Each snippet has an associated event ID, stream identifier (CrisisFACTS-*Event ID*-*Stream Name*-*Stream ID*-*Snippet*), source, timestamp and a piece of text. Your summary should be built from these items.

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
<p style="text-align:center">Fig 5. Three Event Snippets for Event CrisisFACTS-001</p>

### System Output {#output}

Your system should produce one summary for each *event-day* request using the content provided for that *event-day* and posted between the *event-day*  starting and ending timestamps. 

This task differs from traditional summarization in that you should not simply produce a block of text of a set length. Instead, this track’s daily "summaries" contain sets of facts describing the target disaster’s evolution. Your summaries should contain ‘facts’ that match one or more of the queries outlined in __User Profiles__. 

For evaluation, CrisisFACTS organizers will use the top-k "most important" facts from a given *event-day* pair as the summary for that event-day.

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
{
    "requestID": "CrisisFACTS-001-r3",
    "factText": "Increased threat of wind damage in the San Diego area.",
    "unixTimestamp":1512604876,
    "importance": 0.71,
    "sources": [
    "CrisisFACTS-001-Twitter-14023-0"
    ],
    "streamID": null,
    "informationNeeds": ["CrisisFACTS-General-q015"]
}
...
```

Fig 6. Example System Output with Abstractive Facts. The `streamID` field is empty as this fact may not appear in the dataset verbatim. It is, however, supported by one Twitter message.
{: style="text-align:center"}

#### Example Extractive Output

```json
{
    "requestID": "CrisisFACTS-001-r3",
    "factText": "Big increase in the wind plus drop in humidity tonight into Thursday for San Diego County #SanDiegoWx https://t.co/1pVOZAhsJH",
    "unixTimestamp":1512604876,
    "importance": 0.71,
    "sources": [
    "CrisisFACTS-001-Twitter-14023-0"
    ],
    "streamID": "CrisisFACTS-001-Twitter-14023-0",
    "informationNeeds": ["CrisisFACTS-General-q015"]
}
...
```
Fig 7. Example System Output with Extractive Facts. The `streamID` field is populated with the Twitter document from which this text was taken.
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

### 2022 Training Events

The eight events from 2022 are listed below. Gold-standard fact-lists from these events are available <a href="https://github.com/crisisfacts/utilities/blob/main/03-Evaluation/01-AutoEvaluation/CrisisFACTs-2022.facts.json">here</a>.

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


### 2023 New Events

| eventID         | Title                         | Type      |  Tweets | Reddit | News  | Facebook |
| --------------- | ----------------------------- | --------- | ------:| ------:| -----:| --------:|
| CrisisFACTS-009 | Beirut Explosion, 2020        | Accident  |  94,892  | 3,257   | 1,163  | 368,866     |
| CrisisFACTS-010 | Houston Explosion, 2020       | Accident  |  58,370  | 5,704    | 2,175  | 6,281     |
| CrisisFACTS-011 | Rutherford TN Floods, 2020    | Floods    |  11,019  | 475    | 268  | 9,116     |
| CrisisFACTS-012 | TN Derecho, 2020              | Storm/Flood|  49,247  | 1,496 | 15,425 | 13,521   |
| CrisisFACTS-013 | Edenville Dam Fail, 2020      | Accident  |  16,527  | 2,339   | 961  | 8,358     |
| CrisisFACTS-014 | Hurricane Dorian, 2019        | Hurricane |  86,915  | 91,173   | 7,507  | 370,644     |
| CrisisFACTS-015 | Kincade Wildfire, 2019        | Wildfire  |  91,548  | 10,174  | 339  | 35,011     |
| CrisisFACTS-016 | Easter Tornado Outbreak, 2020 | Tornadoes | 91,812  | 5,070  | 750 | 34,343    |
| CrisisFACTS-017 | Tornado Outbreak, 2020 Apr    | Tornadoes | 99,575  | 1,233  | 217 | 19,878    |
| CrisisFACTS-018 | Tornado Outbreak, 2020 March  | Tornadoes | 95,221  | 16,911  | 641 | 87,242    |




## Submissions

Runs will be submitted through the NIST submission system at [trec.nist.gov](http://trec.nist.gov). Runs that do not pass validation will be rejected outright. Submitted runs will be asked to specify the following:

- Whether they are (a) manual or automatic, 
- Whether they use (b) extant TREC-IS labels for Twitter data, and 
- Whether the summaries built are (c) abstractive or extractive.

### Automatic Runs

Each run submission must indicate whether the run is manual or automatic. An automatic run is any run that receives no human intervention once the system is started and provided with the task inputs. We expect most CrisisFACTS runs to be automatic. 

### Manual Runs

Results on manual runs will be specifically identified when results are reported. A manual run is any run in which a person manually changes, summarises, or re-ranks queries, the system, or the system's lists of facts. Simple bug fixes that address only format handling do not result in manual runs, but the changes should be described.

### Submission Format

The submission format for CrisisFACTS is a newline-delimited `JSON` file, where each entry in the submitted file contains the fields outlined in [System Output](#output) section above. Each submission file corresponds to a single submitted run (i.e., all event-day pairs for all events), with the submission’s runtag included in the filename. 

Example submissions are available in [Output Examples](#output-examples).


-------

## Evaluation

As in 2022, participant runs will be evaluated on two approaches. In both approaches, participant systems’ lists of facts will be truncated to a private k value based on NIST assessors’ results.

### Metric Set 1 – ROUGE-based Summarization Against Daily Summaries

- Top facts for each event-day pair will be aggregated into a "summary" of that day’s developments as a single document. This summary will be compared against extant summaries via ROUGE-x, [BERTScore](https://arxiv.org/abs/1904.09675), and similar.
- Gold-standard summaries include:
    - NIST-based assessments of these events and related summaries
    - Summaries extracted from the ICS209 reports from FEMA, when available


### Metric Set 2 – Individual Fact-Matching Between Runs and Manual Fact Lists

- First, NIST assessors will construct a set of facts for each event-day pair from a pooled set of facts from all participant runs.
- After constructing this fact list, assessors will match output from participant runs to each of these facts to assess __comprehensiveness__ and __redundancy ratio__, which act as proxies for recall and precision respectively.
- For a given event-day pair, we therefore have:
    - A set of “gold standard” facts $S$ produced by NIST assessors
    - A set of submitted facts $f \in F$ from a participant run
    - A fact-matching function $M(F, S)$ that measures the overlap between submitted facts $F$ and the gold-standard fact set $S$. 
    - Using this fact-matching function, we define __comprehensiveness__ (a recall-oriented metric):
        - $C(F, S) = \frac{M(F, S)}{\|S\|}$
    - We likewise define __redundancy__ as the number of submitted facts that match a gold-standard fact (i.e., precision):
        - $R(F, S) = \frac{M(F, S)}{\|F\|}$
    - For each disaster event, we calculate the average __comprehensiveness__ and __redundancy__ scores over all days in the event where $S$ contains facts.


-------

## Timeline

| Milestone | Date |
|-----------------------|---------------|
| Guidelines released | 10 May 2023 |
| Submissions Due | 1 September 2023 |
| NIST-Assessor Evaluation | 5-22 September 2023| 
| Scores returned to participants | 29 September 2022| 
| TREC Notebook Drafts Due | 7 November 2023 (Tentative)| 
| TREC Conference | 15-17 November 2023 (Tentative)| 


## Organizers


![image-left](/assets/img/buntain.jpg){: .align-left .avatar} 

[Cody Buntain](http://cody.bunta.in) <br/> [@cbuntain](http://twitter.com/codybuntain) <br/> he/him <br/> College of Information Studies, University of Maryland, College Park.
{: .align-left}

![image-left](/assets/img/horne.jpeg){: .align-left .avatar} 

[Benjamin Horne](https://benjamindhorne.github.io) <br/> [@benjamindhorne](https://kolektiva.social/@benjamindhorne) <br/> he/him <br/> University of Tennessee–Knoxville.
{: .align-left}

![image-left](/assets/img/hughes.jpg){: .align-left .avatar} 

[Amanda Hughes](http://amandaleehughes.com) <br/> [@PIOResearcher](http://twitter.com/PIOResearcher) <br/> she/her <br/> Brigham Young University.
{: .align-left}

![image-left](/assets/img/imran.jpg){: .align-left .avatar} 

[Muhammad Imran](https://mimran.me) <br/> [@mimran15](http://twitter.com/mimran15) <br/> he/him <br/> Qatar Computing Research Institute.
{: .align-left}

![image-left](/assets/img/mccreadie.jpg){: .align-left .avatar} 

[Richard McCreadie](http://dcs.gla.ac.uk/~richardm/Home/) <br/> [@richardm_](http://twitter.com/richardm_) <br/> he/him <br/> School of Computing Science, University of Glasgow.
{: .align-left}

![image-left](/assets/img/purohit.jpg){: .align-left .avatar} 

[Hemant Purohit](https://mason.gmu.edu/~hpurohit/) <br/> [@hemant_pt](http://twitter.com/hemant_pt) <br/> he/him <br/> George Mason University.
{: .align-left}
