# Cookie syncing

This audience sync would take place using a cookie syncing process whereby the GlobalWebIndex cookie is dropped/read on a panellist (via the panel websites) then immediately calling the partner tag with the GlobalWebIndex identifier ‘gwid’ and/or 'mid' embedded into the request. This allows Partner to store the GWI identifier alongside the Partner one in a match table. This match table is built and stored by the partner. This sync should occur on an ongoing basis to constantly refresh the match table.

Partner can then request data for each matched member and connect this back to their own cookie data via the match table.

# RLD files format

Respondent level data can be delivered in form of json and csv file. Data delivery can be delivered to sftp, s3 or gcs server. 

## CSV file format

Every attribute of the respondent (the answer to the question) is provided as a set of data points. Each of them has either 0 (negative answer) or 1 (positive answer) as a value, for example for gender question (“Which of the following best describes your gender?”) following 3 data points are being delivered:

- q2_1 - Male
- q2_2 - Female
- q2_3 - Other	   

There are a few possible examples provided in the table below.

| Respondent id | q2_1 | q2_2 | q2_3 |
|---------------|------|------|------|
| resp_id_1     | 1    | 0    | 0    |
| resp_id_2     | 0    | 1    | 0    |
| resp_id_3     | 0    | 0    | 1    |
| resp_id_4     |      |      |      |

The empty value in the particular column means that the question was not exposed to the respondent. 

Some of the questions contain 2 levels of answers which allow respondents to provide more details to the answer that they have selected for example for the question: “On average, how often would you say you do the following things?”. The first level (datapoint) of the answer can be:

- r3_4 Donate to charity
- r3_3 Drink alcohol
- r3_1 Drive a car
- r3_9 Eat fast food

Once the second level (suffix) defines how regular respondent do the selected thing:
 
- [4] Regulars
- [3] Semi-regulars
- [2] Occasionals
- [1] Non-Engagers

Such questions are structured in the export data in the following way: questioncode_datapointcode_suffix_code. There are a few examples provided in the table below.

| Respondent id | r3_3_1 | r3_3_2 | r3_3_3 | r3_3_4 | r3_1_1 | r3_1_2 | r3_1_3 | r3_1_4 |
|---------------|--------|--------|--------|--------|--------|--------|--------|--------|
| resp_id_1     | 1      | 0      | 0      | 0      | 0      | 1      | 0      | 0      |
| resp_id_2     | 0      | 0      | 1      | 0      | 1      | 0      | 0      | 0      |
| resp_id_3     | 0      | 1      | 0      | 0      | 0      | 0      | 0      | 1      |
| resp_id_4     | 0      | 0      | 0      | 1      | 1      | 1      | 0      | 0      |

The files can be split based on:
- respondents location (country)
- number of datapoints

## Json file format

The Json file format contains the information about the postive answers only and looks as follow:

```
{"userid": "resp_id_1", "segments": ["q13newau_1", "q2_2", "q4_6", "q6_3", "q8_4", "s2_61"]}
{"userid": "resp_id_2", "segments": ["q13newus_10", "q2_1", "q4_5", "q6_5", "q8_1", "s2_1"]}
{"userid": "resp_id_3", "segments": ["q13newus_9", "q2_1", "q4_6", "q6_3", "q8_1", "s2_1"]}
```

# Labels

GWI holds the following information on each attribute
- Question text - The text asked the respondent to generate the data points (e.g. “Which of the following best describes your gender?”)
- Question label - A short description of the question (e.g. “Gender”)
- Data point text - The text of the data point selected (e.g. Male)
- Option text (Optional) - For questions that are asked in a grid we will have a text which represents the option(s) the respondent chose for each data point. e.g. 
- The datapoints and the optiosn are represented with a single column in the data. E.g. q4_1_5 (Question Code_Data point code_ Option code)

Labels can be accessed using an API or delivered in a separate file.
