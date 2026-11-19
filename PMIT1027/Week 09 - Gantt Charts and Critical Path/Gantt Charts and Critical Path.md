---
cssclasses: [pmit1027]
course: PMIT1027
week: 9
source: "Gantt charts using MS Project.pptx, Gantt Charts and Critical Path.pptx"
tags: [PMIT1027, gantt, critical-path, ms-project, scheduling]
---
# Gantt Charts and Critical Path
Back to [[PMIT1027]] · Prev: [[Work Breakdown Structure]] · Next: [[Project Planning Phase and RACI]]

## Gantt charts
* Developed by **Henry L. Gantt** in the early 1900s; tasks on a **horizontal timeline**
* Shows **what** needs doing, **when**, and **by whom**

| Feature | Meaning |
| --- | --- |
| Tasks | Activities from the WBS |
| Durations | Estimated time per task |
| Dependencies | Which tasks must finish before others start |
| Milestones | Markers for important goals (**zero duration**) |
| Progress | Bars fill by **% complete** |

* Benefits: **visibility** (status meetings), **accountability** (owners + deadlines), **coordination** (overlaps), **stakeholder communication**
* Limitations: cluttered on huge projects; **doesn't show resource overload**; must be maintained by a skilled PM

## MS Project
* Specialized scheduling/tracking; **auto-adjusts linked tasks** when one moves; live resource allocation; integrates with Excel/Outlook
* Microsoft is shifting toward **Planner**, but the course uses **MS Project** (free via NAIT entitlements)

### Building a schedule (camping trip demo)
1. **Blank Project** → *Project → Project Information* → set **Start date**, *Schedule from Project Start Date* → save `.mpp`
2. Enter tasks + durations

| Task | Duration |
| --- | --- |
| Buy food and supplies | 2 days |
| Pack food and supplies | 1 day |
| Load car | 0.5 days |
| Drive to campsite | 1 day |
| Set up campsite | 1 day |
| *Arrive at campsite* | **0 days → milestone** ◆ |

3. **Task mode:** *Auto Scheduled* (blue bar - recalculates from dependencies) vs *Manually Scheduled* (pushpin - dates don't move)
4. **Milestones:** duration 0, or *Task Information → Advanced → Mark task as milestone*
5. **Dependencies:** set **Predecessors** (e.g. Buy must finish before Pack)
6. **Resources:** assign people/costs/materials
7. **Track:** add the **% Complete** column → bars fill in

## Dependency types
| Type | Meaning |
| --- | --- |
| **FS** Finish-to-Start (default) | B starts after A finishes |
| SS Start-to-Start | B starts when A starts |
| FF Finish-to-Finish | B finishes when A finishes |
| SF Start-to-Finish | B finishes when A starts (rare) |

## Critical path
* The **longest sequence of dependent tasks** - it sets the **minimum project duration**
* Delay any critical task → **the whole project finishes later**
* Non-critical tasks have **float/slack** (flexibility)
* **Concurrent (parallel) tasks** use time but don't lengthen the project - e.g. while the pie bakes, whip cream and set the table

| Term | Meaning |
| --- | --- |
| **Free slack** | Delay possible without affecting the **next** task |
| **Total slack** | Delay possible without affecting the **project finish** |
| Critical task | **Total slack = 0** |

* Why it matters: shows which tasks control the finish date, warns of delays, helps balance workload, clarifies float

### In MS Project
* Critical tasks = **red**, non-critical = **blue**; calculated automatically once dependencies exist
* *File → Options → Advanced* → "Tasks are critical if slack is less than or equal to **0 days**"
* *View → Tables → Schedule* → shows **Free Slack** and **Total Slack**
* *View → Filter → Critical* (only critical tasks), or *Gantt Chart Format → ☑ Critical Tasks* (highlight)

## Worked example - pickleball tournament
| Task | Duration | Predecessor |
| --- | --- | --- |
| A Book gym | 2 d | - |
| B Recruit players | 3 d | A |
| C Create brackets | 1 d | B |
| D Arrange equipment | 2 d | B (parallel with C) |
| E Print schedules/rules | 1 d | C |
| F Tournament setup | 0.5 d | D, E |
| G Host tournament | 1 d | F |

* Paths: A-B-C-E-F-G = 2+3+1+1+0.5+1 = **8.5 d** · A-B-D-F-G = 2+3+2+0.5+1 = **8.5 d**
* Both chains are critical here (equipment has **zero float**) - if recruitment, brackets, schedules *or* equipment slip, the tournament is late

## Practice
* Mini Gantt for a **lakeside cabin in Sylvan Lake**; **bake sale** critical path (Brightspace)

## Videos
* [MS Project in 20 min](https://www.youtube.com/watch?v=gsh4Cj1KAGY) · [MS Project for beginners](https://www.youtube.com/watch?v=QAHpNf2IJ_8) · [Full MS Project course](https://www.youtube.com/watch?v=R9pH_7lxxj8)
* Assessment: [[Project Schedule]] (10%)
