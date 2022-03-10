---
title: How to interpret the Qbiz Hacker Rank Results
description: 
published: true
date: 2022-03-10T23:31:21.681Z
tags: interview
editor: markdown
dateCreated: 2022-03-10T23:31:21.681Z
---

# How to interpret the Qbiz Hacker Rank results

As a part of the Qbiz technical interview process, we ask candidates to do a 1 hour hacker rank quiz.

One thing we ask interviewers to do is to review the results of the candidate's Hacker Rank quiz before doing a technical coding interview.

Hopefully, this page will help interviewers with that step in the process.

### The Hacker Rank Quiz
##### Structure
The hacker rank quiz that we give candidates is a mix of SQL and python questions.  The current version of the quiz (as of 2022-03-10) includes 10 questions, 5 for each area.

##### SQL Questions
The SQL questions provide a common schema and table descriptions for a Lyft-like ride sharing application.  The questions require an increasingly complex level of SQL statements ranging from simple WHERE clauses to complicated JOINs, window functions and CTEs.

The instructions provided may be daunting for a candidate at first but the overview information presented in the first question is repeated in each subsequent question, so it's not as complicated as it seems.

##### Python Questions
The python questions are a more diverse set of content

### The Results PDF
The unfortunately long results PDF that HackerRank produces is available from within FreshTeams.  It is listed as an "Interview" and you can open the PDF by clicking on the "View Report" link.
![hackerrankinfreshteams.png](/hackerrankinfreshteams.png){.align-center}
The top of the PDF contains a summary of the results, there are four important pieces of information contained at the top.
1. A link to the results on the HackerRank website
1. An overall score, which can give a rough sense of how the candidate did
1. The amount of time they took on the test
1. The email submitted to HackerRank (redacted in image below)
![hackerrankpdf-topsummary.png](/hackerrankpdf-topsummary.png)

Below the top area is a summary of the questions with an indicator of how long they spent on that question and a red/yellow/green/grey indicator if they got the question wrong, partially right, right, or didn't respond to the question.
![hackerrank-questionsummary.png](/hackerrank-questionsummary.png)

Individual questions are reproduced in the PDF in their entirety, which leads to the unfortunate length issue.  You'll want to look for the "Candidate Answer" box to see what the candidate actually did.
![hackerrank-candidateanswer.png](/hackerrank-candidateanswer.png)

### The Results on the HackerRank website
Viewing the results on the HackerRank website provides some additional context including a time line of what the candidate worked on when.  The most useful aspect of the website review is the ability to see the differences in results that the candidate produced versus what was expected.
Although not the best diff (see the screenshot below for a correct answer which shows as different because of ordering issues) it can help you to understand where a candidate may have gone off track in their answer. 
![hackerrankwebsite-question.png](/hackerrankwebsite-question.png)

### Things to look for
#### Time
> We expect candidates to run out of time doing the quiz, this is intentional.  HackerRank recommends that we give 90 minutes, we give 60 because we want to see how they manage their time.
{.is-info}

#### Coding Style 
- Is their code readable?
- Can you follow their logic in coming up with an answer?
- Do they use good aliases for tables in SQL or just go with "A", "B"?

#### Comments
- Candidates can and do sometimes add comments in their responses, especially if they are running out of time to communicate their intent.  If they're on the right track, but maybe don't remember an exact syntax for something, take that into account.

#### Logical Flaws
When the candidate has a wrong answer 
- Is it obvious why their answer is wrong?  
- Is it an misunderstanding of the requirements or an implementation issue?

> Reviewing the HackerRank quiz can help you focus the questions you ask during the interview itself.  If they did well on the SQL, but not on the python, you may to spend more time probing their python skills or vice versa.
{.is-success}
