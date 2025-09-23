# PRD-bar.md — Dark Mode Toggle

## Title  
Enable Dark Mode Toggle

## Overview  
Users increasingly expect apps to support dark mode. Add a dark/light mode toggle to the TODO app that persists between sessions.

## Requirements  
1. **UI Toggle**  
   - Add a toggle switch in the app header.  

2. **Theme Styles**  
   - Light mode = current default.  
   - Dark mode = dark background (#121212), light text (#f0f0f0), muted button styles.  

3. **Persistence**  
   - Remember user’s choice in local storage.  

4. **Testing**  
   - Unit tests to ensure theme choice persists across reloads.  
   - Browser test: toggle dark mode, reload page, confirm theme persists.  
