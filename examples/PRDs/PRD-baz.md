# PRD-baz.md — Task Prioritization

## Title  
Add Priority Levels to Tasks

## Overview  
Users want to distinguish between high-priority and low-priority tasks. Add priority levels (High, Medium, Low) that are selectable on task creation and visible in the list.

## Requirements  
1. **Task Creation**  
   - Add a dropdown selector (High/Medium/Low) next to the new task input.  
   - Default = Medium.  

2. **Task Display**  
   - Show priority level next to each task.  
   - Use color coding: High = red, Medium = yellow, Low = green.  

3. **Sorting/Filtering**  
   - Add a filter control in the footer (“Show high priority only”).  

4. **Persistence**  
   - Save priority level in local storage with the task.  

5. **Testing**  
   - Unit tests for priority assignment and filtering logic.  
   - Browser test: create one High, one Low, confirm filter works.  
