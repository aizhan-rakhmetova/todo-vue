# Vue.js Todo List Application

A simple Todo List application built with Vue.js 3 and the Composition API. This application uses JSONPlaceholder as a mock API for backend operations.

## Features

As per the technical specification, this application includes the following core features:

- Create new todo items
- View a list of all todo items
- Mark todo items as complete/incomplete
- Edit existing todo items
- Delete todo items
- Filter todo items by status (All/Active/Completed)
- Clear all completed todo items
- Display count of remaining active items

## Project Structure

```
todo-app/
├── public/
├── src/
│   ├── assets/
│   │   └── main.css
│   ├── components/
│   │   ├── TodoHeader.vue
│   │   ├── TodoList.vue
│   │   ├── TodoItem.vue
│   │   └── TodoFooter.vue
│   ├── App.vue
│   └── main.js
├── index.html
├── package.json
├── vite.config.js
└── README.md
```

## API Integration

This application uses [JSONPlaceholder](https://jsonplaceholder.typicode.com/) as a mock API for backend operations. The following endpoints are used:

- GET /todos - Fetch todos
- POST /todos - Create a new todo
- PUT /todos/:id - Update a todo
- DELETE /todos/:id - Delete a todo

## Setup and Installation

1. Clone the repository
2. Install dependencies:
   ```
   npm install
   ```
3. Run the development server:
   ```
   npm run dev
   ```
4. Build for production:
   ```
   npm run build
   ```