---
course: PMIT1027
week: 7
source: "PMIT1027 - W7D1 WBS.pptx, PMIT1027 - W7D2 WBS and Excel.pptx"
tags: [PMIT1027, wbs, excel, planning]
---
# Work Breakdown Structure
Back to [[PMIT1027]] · Prev: [[Project Scope]] · Next: [[Gantt Charts and Critical Path]]

* Making a pizza from scratch → listing every step = the idea behind a WBS: **break a big goal into smaller, manageable tasks**
* Even huge projects (the Pattullo Bridge replacement) decompose into deliverables: demolition, design, fabrication, substructure, superstructure, roadways, utilities, testing, environmental, closeout

## What is a WBS?
* Breaks a project into smaller, manageable **work packages**; each level adds detail
* A **visual map** of the whole project
* Goal: **organize work → assign responsibility → estimate effort → measure progress**

## Why use one
* See the whole project at a glance
* More accurate **costing and scheduling**
* Clear **ownership**
* Prevents missed deliverables / duplicate work
* Foundation for the **Gantt chart** and resource plan

## From scope to structure
* **Scope** defines *what* is delivered; the **WBS** defines *how* the work is organized
* Each scope deliverable becomes a **branch**; decompose into tasks and subtasks
* **Full coverage** - every scope item appears somewhere (the 100% rule)

## Levels
| Level | Content |
| --- | --- |
| 0 | Project title |
| 1 | Major deliverables or phases |
| 2 | Work packages (tasks per deliverable) |
| 3 | Detailed activities/steps |
* Lower levels add detail **without changing scope**

## Layouts
* **Hierarchical tree** diagram
* **Tabular** - table with extra columns (Excel/MS Project)
* **Indented list:**
```
1.0 Build a Fence Project
  1.1 Planning
    1.1.1 Check property lines
    1.1.2 Get required permits
  1.2 Materials
    1.2.1 Buy wood and concrete
    1.2.2 Gather tools
  1.3 Construction
    1.3.1 Set posts
    1.3.2 Attach rails and boards
    1.3.3 Build gate
  1.4 Finishing
    1.4.1 Paint or stain fence
    1.4.2 Clean up site
```

## Resources and time
* **Assign people at the lowest task level** (higher levels = summary/oversight responsibility)
* Estimate **person-hours** per task; sum = total effort
* Durations, milestones and dependencies feed the Gantt chart

| Level | Task | Assign? | Why |
| --- | --- | --- | --- |
| 1 | 1.0 Build a Fence | Probably not | Too broad |
| 2 | 1.3 Construction | Not ideal | Still a grouping |
| 3 | 1.3.1 Set posts | **Yes** | Measurable, one owner |

## Building it
1. Start with the project goal
2. Review the charter + scope deliverables
3. Break each deliverable into tasks and subtasks
4. Review for completeness
* It's normal to discover deliverables need refining/combining/adding - fine as long as **objectives stay the same**

## Quality checklist
- [ ] Includes **all** work needed for the scope
- [ ] Every task specific and measurable
- [ ] No overlaps or gaps
- [ ] Every lowest-level task can be assigned and tracked
- [ ] Makes sense visually and logically

## WBS in Excel
* Accessible, focuses on **structure not scheduling**, flexible, transfers to MS Project later
* Columns: **WBS ID, Task Name, Owner, Duration, Notes**
* Show hierarchy with **Increase Indent**, **bold summary rows**, shading or row grouping
* Logical numbering (1.0, 1.1, 1.1.1); borders/shading for readability
* ⚠️ For the WBS assessment use **Excel only** - MS Project comes with scheduling

## Activity
* 3-level WBS for a **Thanksgiving dinner for 25** - deliverables: food prep, decorations, invitations, entertainment, cleanup; assign each task to a family member

## Resources
* [WBS (Atlassian)](https://www.atlassian.com/work-management/project-management/work-breakdown-structure) · [Create a WBS (Microsoft)](https://learn.microsoft.com/en-us/dynamics365/project-operations/project-management/create-wbs) · [WBS guide](https://itpmschool.com/work-breakdown-structure-guide/) · [What is a WBS? (video)](https://www.youtube.com/watch?v=wEWhnodF6ig)
* *Excel_WBS_Example.xlsx*, *Westly Reg Excel WBS Deck.xlsx* (Brightspace)
* Assessment: [[Work Breakdown Structure Assessment]] (10%)
