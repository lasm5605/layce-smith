---
layout: page
title: projects
permalink: /projects/
description: Selected work in front-end development, data, and UX.
nav: true
nav_order: 3
display_categories: [work, fun]
horizontal: false
---

# To Do Ta Da: A React Task Dashboard

**[Live demo](https://leafy-capybara-0d6161.netlify.app/)** · **[Source code](https://github.com/lasm5605/ReactTaskDashboard)**

**Role:** Designer and developer (solo project)
**Timeline:** 10 weeks
**Stack:** React, Vite, Material UI, dnd-kit, CSS, localStorage, Netlify

To Do Ta Da is a single-page task manager for planning a day and checking things off. I took it from wireframe to deployed app in ten weeks, as part of moving from UX research toward data analytics and front-end development.

<img width="2000" height="1405" alt="The finished To Do Ta Da dashboard, showing the To Do list, due dates, and recently completed tasks" src="https://github.com/user-attachments/assets/bfef1725-bf47-4381-94bd-6886dba7f4f1" />

---

## Why I built it

Tools like Tableau and Power BI are great for standard reporting, but they limit how far you can customize an interface. I wanted to learn to build interactive dashboards from the ground up, and to carry a product all the way from design through working code. My background is in researching and designing interfaces. This project was my chance to build one myself.

## Key features

- **Add, edit, and delete tasks**, with optional due dates and overdue warnings
- **Drag-and-drop prioritization**: the top of the list is the highest priority, and new tasks go to the bottom until moved
- **Collapsible subtasks** for breaking a big task into steps (e.g. a grocery list under "go grocery shopping")
- **To Do / Ta Da views**: checking off a task moves it to the Ta Da view with a completion timestamp, and unchecking it returns it to its original position
- **Recently completed sidebar** showing the five latest finished tasks, newest first
- **Time-of-day greeting** with the current date and time
- **Automatic saving**: tasks persist between sessions with no account or backend

---

## Process

### 1. Planning and scope

I started with a 10-week plan covering setup, core task features, persistence, filtering, visual design, testing, and deployment. I flagged four risks early and planned for each:

- **Scope creep:** build the essential features first, and add extras only if time allowed
- **Data loss:** save tasks to the browser's localStorage
- **Complex state:** follow React best practices, keeping as little state as possible and deriving values from it
- **Bugs:** build in small steps and test often

### 2. Wireframing

Before writing interface code, I sketched the layout I wanted to build.

<img width="949" height="713" alt="Hand-drawn wireframe of the task dashboard layout" src="https://github.com/user-attachments/assets/35418a2a-5b4a-412b-8674-d06ad5436840" />

### 3. First prototype with Material UI

I set up the project with Node.js and Vite, whose live reloading made debugging much faster. Then I built a first interface using Material UI's list, checkbox, text field, and container components. It got the look close to my wireframe quickly.

<img width="1914" height="958" alt="Early prototype built with Material UI components" src="https://github.com/user-attachments/assets/952706c0-0ba1-4e5c-b471-30b3e0f69468" />

### 4. Rethinking the architecture

The prototype had a basic flaw: each component held its own data, so any re-render wiped the task list. I stepped back from visual design and planned how data should move through the app.

<img width="641" height="686" alt="Diagram of component hierarchy and state flow in the app" src="https://github.com/user-attachments/assets/329f6f88-0119-4d9c-95b3-9457a3879412" />

In React, state that several components share lives in their nearest common parent, and data flows down as props. In my app:

- **`App`** owns the shared state (the task list and the selected filter) and derives the visible tasks from them. It also holds the add, edit, and delete functions and passes them down.
- **`FilterTabs`** displays the To Do and Ta Da tabs and reports which one is clicked.
- **`TaskList`** renders the visible tasks.
- **`TaskForm`** gets only the add function and manages the text being typed.
- **`TaskItem`** tracks only whether it's being edited.

I rebuilt the components to follow this plan, so every feature after it had a clear place for its data.

### 5. Core functionality

With state in place, I implemented adding, editing, deleting, and completing tasks, plus switching between the two views.

<img width="565" height="481" alt="Functional version of the app with To Do and Ta Da tabs" src="https://github.com/user-attachments/assets/81c58d12-1ccb-46f6-80cf-264e39bcdc3c" />

### 6. Saving data between sessions

React's `useState` keeps data across re-renders but not page refreshes. I wrote a custom hook that saves the task list and the active filter to localStorage and loads them back on page load. I also rebuilt the visual design with plain HTML in the components and a single stylesheet.

<img width="1505" height="776" alt="Browser developer tools showing saved task and filter data in localStorage" src="https://github.com/user-attachments/assets/e250dbe6-9db1-4eba-8b87-9111f397fcc8" />

### 7. Prioritization, due dates, and subtasks

- **Prioritization:** I used dnd-kit, a well-maintained drag-and-drop library for React. Each task has a drag handle. Because only one view shows at a time, my reorder function rearranges just the visible tasks and slots them back into the full list, so hidden tasks keep their order.
- **Due dates:** the app uses the browser's built-in date and time picker, opened from a calendar icon that matches the app's style. The due date is stored as another property of the task.
- **Subtasks:** these reuse most of the task item logic as a second, collapsible level under any task.

I had planned a "tasks remaining" counter, but dropped it in favor of subtasks, which I found more useful in my own daily lists.

### 8. Deployment

Because the app is static and stores data in the browser, it can run on any static host. I deployed it on Netlify straight from the GitHub repository. Deployment took far less time than I'd budgeted, so I used the extra time on subtasks, the time-of-day greeting, and typography and spacing.

---

## What I learned

- **Plan state before styling.** My first prototype looked right but couldn't hold onto data. Mapping data flow first made every later feature easier to add.
- **Use libraries, but understand them.** Material UI sped up prototyping, but I got more control by writing my own HTML and CSS once I knew what the app needed.
- **Cut features with a reason.** Dropping the counter for subtasks came from using the app myself, much like a usability finding.
- **UX research skills carry over to engineering.** Wireframing, scoping, and iterating on my own feedback shaped the build as much as the code did.

## Next steps

- A progress ring showing how much of the day's list is done
- Selectable color schemes and themes
- User accounts and a backend so lists sync across devices
