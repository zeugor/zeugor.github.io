---
layout: post
title: "Book Notes: Scrum and XP from the Trenches, Henrik Kniberg"
tags: [resource, book, agile, scrum]
date: 2021-05-24
slug: book-scrum-and-xp-from-the-trenches
description: "Notes after reading 'Scrum and XP from the Trenches'"
---

## 1. Intro
- Scrum is not a methodology, it is a framework, and it is not going to tell you exactly what to do.

## 2. How we do product backlogs
-  The product backlog is a prioritized list of stories, described using the customer's terminology.
- Stories has the fields: id, name, importance and
  - Story points: corresponds roughly to "ideal man-days". Absolute estimation is not important (i.e. a 2-point story should take 2 days), but the relative estimation is (i.e. a 2-point story should take half as much work as a 4-point story).
  - How to demo: high level description of how this story will be demonstrated at the sprint demo.

### How we keep the product backlog at a business level
- ask the product owner a series of "but why" questions until we find the underlying goal.

## 3. How we prepare for spring planning

- make sure the product backlog is in shipshape before the spring planning meeting.
- the product owner should understand each story. He does not need to know exactly what needs to be implemented, but he should understand why the story is there.
- other people than the product owner may add stories to the backlog, but they may not assign an importance level, that is the product owner's sole right. They may not add time estimates either, that is the team's sole right.

## 4. How we do sprint planning
The purpose of the spring planning meeting is to give the team enough information to be able to work in undisturbed peace for a few weeks, and to give the product owner enough confidence to let them do so.

Spring planning output:
- a sprint goal.
- list of team members and their commitment levels.
- spring backlog.
- a defined sprint demo date.
- a defined time and place for the daily scrum

### Why the product owner has to attend
- each story contains 3 variables: scope, estimate and importance.
- scope and importance are set by the product owner.
- estimate is set by the team.
- normally the product owner starts summarizing his goal for the sprint, and the most important stories.
- then the team goes through and time-estimates each story. Starting with the most important one.
- the team would formulate scope questions.
- the team time-estimation would not be what the product owner expected, then he will change the importance and/or scope. Then the team would need to re-estimate again.

### Why quality is not negotiable
- external quality: what is perceived by the users.
- internal quality: what usually is not visible to the user, but have a profound effect on the maintainability of the system.
- high internal quality does not imply high external quality, but low internal quality implies low external quality.
- external quality is part of the scope. The product owner may decide to release a clumsy user interface, and clean up in a future version later.
- internal quality is not up to discussion. It is the team responsibility to maintain the system quality under all circumstances and this is simply not negotiable.

### Sprint planning meeting agenda
- 30min. Product owner goes through the spring goal and summarized the product backlog.
- 1h30: team time-estimates and breaks down items. Product owner updates importance ratings as necessary. Items are clarified. "How to demo" is filled for the high importance items.

### Defining the sprint length
- short sprint are good: allow the company to be agile, change direction often, short feedback cycle = more frequent deliveries = more frequent customer feedback == less time spent running in the wrong direction = learn and improve faster...
- long sprints are good: the team get momentum.

### Defining the sprint goal
- it should be in business terms, not technical terms.
- it should answer the question "why are we doing this sprint?"
- let the whole company knows what the company is doing and why.

### Deciding which stories to include in the sprint
- the team decide how may stories to include in the sprint. Not the product owner or anybody else.

### How can product owner affect which stories make it to the sprint?
- if product owner is disappointed because a story is not included in the sprint. He can:
  - reprioritize up this story, bumping out another story.
  - reduce the scope of the story, until the team believe that the story will be into the sprint.
  - split the story between really important part and not so. And included in the sprint only the important part.

### How does the team decide which stories to include in the sprint?
- techniques:
  - gut feel.
  - velocity calculation.
- velocity: "amount of work done" measure, based on the initial weight estimations.
- almost completed story don't count as partial point for the velocity, because Scrum is all about getting stuff completely shippable done. The value of stuff half-done is zero (may in fact be negative).

How to estimate velocity?
- look at the team's history.
- resource calculation:
(available man-days) x (focus factor) = (estimated velocity)
(focus factor) = (actual velocity) / (available man-days)
actual velocity = sum of the initial estimates of all stories that were completed last sprint.
Better if we calculate a averaged sprint focus-factor.


### Why we use index cards
- sprint planning focus on dealing with stories in the product backlog, estimate them, reprioritize them, clarifying them breaking them down...
- the story breakdown is usually quite volatile. They are frequently change and refined during the sprint. The product owner does not to be involved at this level of detail anyway.

## Definition of done
- team and product owner should agree the definition of done.

### Keep the product owner at bay
The product owner should be near enough so that the team can wander over and ask him something, and so that he can wander over to the taskboard. But he should not be seated with the team. Because chances are he will not be able to stop himself from meddling in details.

## 9. How we do sprint demos

### Why we insist that all sprints end with a demo
- the team gets credits for their accomplishment. They feel good.
- Other people learn what your team is doing.
- get feedback from stakeholders.
- forces the team to actually finish stuff and release it.

Checklist for sprint demos
- present clearly the sprint goal.
- don't spend much time preparing the demo, just demonstrate actual working code.
- keep high pace, making it at fast-paced rather than beautiful.
- keep business-oriented level, leave out the technical details.

## 10. How we do sprint retrospective
- without retrospectives you will find that the team keeps making the same mistake over and over again.

## 13. How we combine Scrum with XP

### Pair programming
- improve the code
- shift pairs frequently
- spread the knowledge within the group
- code review is an OK alternative

### TDD
- TDD is hard
- TDD has a profoundly positive effect on system design.

### Incremental design
- keep design simple from start and continuously improving it, rather than trying to get it all right from the start and then freezing it.

### Collective code ownership
- pair programming helps.
- teams with collective code ownership have proven to be very robust. Not risk of a key person gets sick.

## 15. How we handle multiple Scrum teams
- the product owner should not allocate and reassign people. The product owner is the domain expert who tells the team in which direction they should run.

### Rearrange teams between sprint - or not?
- a key aspect of the Scrum is "team gel". When a team gets to work together over multiple sprints they will usually become very tight. They will learn to achieve group flow, and reach incredible productivity.
- only rearrange teams if it is a long term change.

### Offshoring
- separated team member
  - good communication between the two locations
  - intense knowledge sharing
  - later the off-shored team may end as a "separated team"

## Recommended reading
- Managing the design factory - a product developer's toolkit
- Implementing lean software development
- Agile estimating and planning
- Joel of software
- Lean - software development - an agile toolkit
- Balancing agility and discipline
- Agile software development with scrum
- The mythical man-month
- Agile project management with scrum
- Extreme programming explained
- Peopleware - Productive Projects and Teams
