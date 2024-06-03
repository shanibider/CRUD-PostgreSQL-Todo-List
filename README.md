# 📝 PostgreSQL To-Do List 
[![postgresql](https://img.shields.io/badge/postgresql-★★★★★-yellow)](https://vitejs.dev/)
[![pgadmin](https://img.shields.io/badge/pgadmin-★★★★★-blue)](https://vitejs.dev/)
[![tableplus](https://img.shields.io/badge/tableplus-★★★★★-pink)](https://vitejs.dev/)
[![Javascript](https://img.shields.io/badge/JavaScript-★★★★★-orange)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

This project is a simple ToDoList application built with `Express.js, Node.js, PostgreSQL, and EJS`. <img height=30px src="https://skillicons.dev/icons?i=postgresql"> <img height=30px src="https://skillicons.dev/icons?i=express"> <img height=30px src="https://skillicons.dev/icons?i=nodejs"> <br>
It allows users to manage their tasks by adding, editing, and deleting items from their to-do list.

## 🚀 Getting Started

### Prerequisites



Before you begin, ensure you have the following installed:

- [x] `PostgreSQL` <img height=25px src="https://skillicons.dev/icons?i=postgresql">
- [x] `pgAdmin`, management tool for PostgreSQL (for local development) <img width=20px src="https://github.com/shanibider/CRUD-PostgreSQL-Todo-List/assets/72359805/c9665630-0e52-48a9-a828-c985d701097e">
- [x] `TablePlus`, Database Management Tool (for production) <img width=30px src="https://github.com/shanibider/CRUD-PostgreSQL-Todo-List/assets/72359805/b5276c92-011a-4364-82fd-808a9c089b1b">
- [x] `Render`, Cloud hosting service. (for deploying the project) <img width=auto height="25px" src="https://github.com/shanibider/CRUD-PostgreSQL-Todo-List/assets/72359805/8d1e4703-bede-41d5-8984-736ab425ec97">
- [x] `Node.js` <img height=25px src="https://skillicons.dev/icons?i=nodejs">
- [x] `npm`<img height=25px src="https://skillicons.dev/icons?i=npm">

### Local Development Setup 🛠️

1. **Clone the repository:**

   ```bash
   git clone https://github.com/your-username/todo-list-project.git
   cd todo-list-project
   ```

2. **Install dependencies:**

   ```bash
   npm install
   ```

3. **Set up PostgreSQL:**

   - Open pgAdmin and create a new database named `permalist`.
   - Update the database configuration in your project's environment variables file (`.env`):

     ```env
     DB_HOST=localhost
     DB_USER=your-username
     DB_PASSWORD=your-password
     DB_DATABASE=permalist
     DB_PORT=5432
     ```

4. **Run the application:**

   ```bash
   npm start
   ```
Your server is now running on: [http://localhost:3000](http://localhost:3000)

---
### Production Setup 🛠️

1. **Database Setup on Render:**

   - [x] Create a new `PostgreSQL database` on Render.
   - [x] Note down the database URL provided by Render.

2. **Connect using `TablePlus`:**

   - [x] Open TablePlus and create a new connection using the Render database URL.

3. **Deploy the application on Render:**

   - [x] Push your code to a repository (e.g., GitHub).
   - [x] Connect your Render service to the repository.
   - [x] Set the environment variables in Render:

     ```env
     DB_HOST=your-render-database-host
     DB_USER=your-render-database-username
     DB_PASSWORD=your-render-database-password
     DB_DATABASE=your-render-database-name
     DB_PORT=5432
     ```

   - [x] Deploy the service.

## 🛠️ Technologies Used
[![My Skills](https://skillicons.dev/icons?i=postgresql,express,nodejs,js,html,css)](https://skillicons.dev)

- [ ] **Backend:**
  - Node.js
  - Express.js
- [ ] **Frontend:**
  - javascript
  - EJS templating
- [ ] **Database:**
  - PostgreSQL
  - pgAdmin (for local development)
  - TablePlus (for production)
- [ ] **Deployment:**
  - Render, cloud hosting web service, and PostgreSQL DB

## 📂 Project Structure

```plaintext
├── views
│   ├── footer.ejs
│   ├── header.ejs
│   ├── index.ejs
├── index.js
├── .env
├── queries.sql
├── package.json
├── README.md
```
---

## 📋 API Endpoints
These endpoints provide **full CRUD (Create, Read, Update, Delete)** functionality for managing tasks in the To-Do List application.<br>
These code examples demonstrate how to interact with these endpoints using the `fetch` API.<br>

- [x] **GET** `/tasks` - Retrieve all tasks and render the main page
- [x] **POST** `/tasks` - Create a new task
- [x] **PUT** `/tasks/:id` - Update a task
- [x] **DELETE** `/tasks/:id` - Delete a task


### GET / 📋
Retrieve all items from the to-do list and render the index page.

- **URL:** `/`
- **Method:** `GET`
- **Response:**
  - **200 OK**: Renders the `index.ejs` page with the list of items.

```javascript
app.get("/", async (req, res) => {
  try {
    // Query the database to get all items, ordered by their ID in ascending order
    const result = await db.query("SELECT * FROM items ORDER BY id ASC");
    
    // Store the result rows in the items array
    items = result.rows;

    // Render the index.ejs template with the current date and list of items
    res.render("index.ejs", {
      listTitle: currentDate() + " To-Do List",
      listItems: items,
    });
  } catch (err) {
    // Log any errors that occur

```javascript
    console.log(err);
  }
});
```

### POST /add ➕
Add a new item to the to-do list.

- **URL:** `/add`
- **Method:** `POST`
- **Request Body:**
  - `newItem` (string): The title of the new to-do item.
- **Response:**
  - **302 Found**: Redirects to `/` after adding the item.

```javascript
app.post("/add", async (req, res) => {
  // Get the new item title from the request body
  const item = req.body.newItem;
  
  try {
    // Insert the new item into the database
    await db.query("INSERT INTO items (title) VALUES ($1)", [item]);
    
    // Redirect to the homepage to show the updated list
    res.redirect("/");
  } catch (err) {
    // Log any errors that occur
    console.log(err);
  }
});
```

### POST /edit ✏️
Edit an existing item in the to-do list.

- **URL:** `/edit`
- **Method:** `POST`
- **Request Body:**
  - `updatedItemTitle` (string): The updated title of the to-do item.
  - `updatedItemId` (number): The ID of the to-do item to be updated.
- **Response:**
  - **302 Found**: Redirects to `/` after editing the item.

```javascript
app.post("/edit", async (req, res) => {
  // Get the updated item title and ID from the request body
  const item = req.body.updatedItemTitle;
  const id = req.body.updatedItemId;

  try {
    // Update the item in the database
    await db.query("UPDATE items SET title = ($1) WHERE id = $2", [item, id]);
    
    // Redirect to the homepage to show the updated list
    res.redirect("/");
  } catch (err) {
    // Log any errors that occur
    console.log(err);
  }
});
```

### POST /delete ❌
Delete an item from the to-do list.

- **URL:** `/delete`
- **Method:** `POST`
- **Request Body:**
  - `deleteItemId` (number): The ID of the to-do item to be deleted.
- **Response:**
  - **302 Found**: Redirects to `/` after deleting the item.

```javascript
app.post("/delete", async (req, res) => {
  // Get the ID of the item to be deleted from the request body
  const id = req.body.deleteItemId;
  
  try {
    // Delete the item from the database
    await db.query("DELETE FROM items WHERE id = $1", [id]);
    
    // Redirect to the homepage to show the updated list
    res.redirect("/");
  } catch (err) {
    // Log any errors that occur
    console.log(err);
  }
});
```

---


## Screenshots 🖼️
![todo2](https://github.com/shanibider/CRUD-PostgreSQL-Todo-List/assets/72359805/df422a92-7def-4969-bd69-62f5be3c5060)

### pgAdmin Tool (for local development):
![pgAdmin-todo](https://github.com/shanibider/CRUD-PostgreSQL-Todo-List/assets/72359805/070794ce-8d83-4629-8bf3-5d24217c7a40)


### Render (for deploying the project + hosted PostgreSQL DB) + TablePlus (DB for production):
![Production db (tableplus) + deployment in render](https://github.com/shanibider/CRUD-PostgreSQL-Todo-List/assets/72359805/8e8f1be6-a8bc-4c59-8648-4b8d2c8381c7)

### Tableplus - local and production Workspace
![tableplus - local and production](https://github.com/shanibider/CRUD-PostgreSQL-Todo-List/assets/72359805/599a9b7a-f66e-4d8c-9fa9-837b41de4af5)


## 📫 Connect with me 😊
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/shani-bider/)
[![portfolio](https://img.shields.io/badge/my_portfolio-000?style=for-the-badge&logo=ko-fi&logoColor=white)](https://shanibider.onrender.com/)
[![gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:shanibider@gmail.com)

<footer>
<p style="float:left; width: 20%;">
Copyright © Shani Bider, 2024
</p>
</footer>
