---
title: Beginners Guide to Qbiz Technical Interviews
description: 
published: true
date: 2022-05-09T23:09:20.300Z
tags: interview
editor: markdown
dateCreated: 2022-02-16T16:13:52.095Z
---


# The beginners guide to giving a Qbiz Technical Interview


## Overview

This document is meant to help ensure some standardization in how we conduct technical interviews for Qbiz.


## The Qbiz Interview Process

1. Resumes are sourced
2. Initial screen of candidate by a recruiter
3. HackerRank quiz  - see [hacker-rank-results](/technical-interviewing/hacker-rank-results) for more on the HackerRank quiz
4. **Initial Technical Screen**
5. "Final" Interviews

This document is going to focus on the Initial Technical Screen.

## L3/L4 vs. L5/L6 Candidates

Keep in mind that the "Initial Technical Screen" is the opportunity we have to assess hands-on coding live. In your feedback, please provide a sense of SQL proficiency and Python proficiency at a particular level. This isn't hard-and-fast, as consultative skills factor in more, but these data points go into the rest of the process with the candidate.

**The "Initial Technical Screen" is more than simply a pass/fail for mutual fit at Qbiz (though it includes that). It also establishes a sense of level/seniority for hands-on coding.**

### L3/L4 Candidates

We are converging on a standard approach for L3/L4 candidates:

* SQL to a level that includes:
  * Set theory (JOINs including cardinality and NULL handling)
  * Aggregation (multiple levels of aggregation in a single resultset; not window functions)
  * Awareness of potential data quality issues and how to handle them
    * Checking assumptions about data as part of the interview
    * Poking at the data as part of the assumption checking process
* Python to a level that includes:
  * Ability to put together orchestration (DAGs w/ tasks)
  * Conversation and discovery
    * Ideally, "operational awareness" - what could go wrong as scheduled jobs and tasks run live
    
At this level, the questions are:

* SQL: [Vordo](/technical-interviewing/question-bank/vordo)
* Python3: [Orchestration](/technical-interviewing/question-bank/orchestration)

For less senior interviewers, the final round interview should be conducted by a more senior interviewer as a feedback loop for internal training and development purposes.

### L5/L6 Candidates

We are currently refining the approach with more senior interviewers.

## Structure of the Interview

* Brief introductions (~5 min)
    * Give a brief intro of yourself first, it will set the expectations of what you're asking the candidate to do.
* {Optional} Review the Hacker Rank results with the candidate (~5 min)
* Python evaluation (~15-20 min)
* SQL evaluation (~20-25 min)
* Provide the candidate an opportunity to ask you questions (~5-10 min)
    * They have to want to come to Qbiz if we make them an offer
* Wrap up (~5 min)
    * This should be the same whether you think you will recommend the candidate or not.
    * "The next step is for you to provide your feedback on this interview to Emily (or other recruiter) and they will be in touch about next steps"
    * "Thank you for taking the time to talk with me today"


## Things you should do before the Interview

* Review the candidates resume
    * Look at their background, see how much experience they have
* Review the hacker rank quiz responses
    * You should be able to get a sense of their coding style and may find some areas that you'll want to probe during the interview
* Set up a CodingView Interview and name it appropriately
    * Recommended: {Candidate Name} {Date} i.e. "John Doe 2022-02-09"
* Select the questions you want to ask for both python and SQL
    * Pick out 2-3 python questions, and 2-3 SQL questions, you'll only have an hour for the interview, but you'll want to have some extras in case they fly through the easier questions.
    * See [question-bank](/technical-interviewing/question-bank)


## Things you should do after the interview

* Hit the "End Interview" button in CodingView
    * You can end it during the wrap up of the interview, but if you don't make sure to do it once the interview is over
* Fill out the feedback form in FreshTeams
    * As much as possible write some context around the ratings, giving someone three stars on a section doesn't convey much information, but giving them three stars and saying "Candidate struggled with window functions, but did well with the complicated joins" 

## Things to keep in mind
* It's OK to be nervous about giving an interview, but keep in mind the candidate is nervous too
* Our goal is to assess the candidate's skills, not to torture or trick the candidate with "gotcha" questions
