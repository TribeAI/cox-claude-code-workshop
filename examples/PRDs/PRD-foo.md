# PRD-foo.md — Due Dates & Overdue Filter

## Title  
Add Due Dates to Tasks with Overdue Filtering

## Overview  
Users want to manage not just *what* they need to do, but also *when* they need to do it. The TODO app should allow users to assign a due date to each task and provide a way to filter tasks that are overdue.

## Requirements  
1. **Task Creation**  
   - Add a date picker to the “Add Task” input form.  
   - Default value = today’s date.  

2. **Task Display**  
   - Show due date next to the task text in the task list.  
   - Overdue tasks should appear in red text.  

3. **Filter Control**  
   - Add a filter toggle (“Show overdue only”) in the footer.  

4. **Persistence**  
   - Ensure due dates are saved in local storage along with the task.  

5. **Testing**  
   - Unit tests for date assignment and filtering logic.  
   - Browser test: create a task with yesterday’s date and confirm it shows in red + filter works.  
