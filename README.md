This Express server uses a simple REST API to handle CRUD (Create, Read, Update, Delete) operations for blog posts. Instead of an external database, this application uses an in-memory array `posts` to store blog data.

#### 1. **Import Modules and Initialize App**
```javascript
import express from "express";
import bodyParser from "body-parser";

const app = express();
const port = process.env.PORT || 4000;
```
- **express**: Creates the server and handles routing.
- **body-parser**: Parses JSON and URL-encoded data from the request bodies.

Here, the server runs on `PORT` set in the environment or defaults to `4000`.

#### 2. **Sample Posts Data**
```javascript
let posts = [
  {
    id: 1,
    title: "The Evolution of High-Performance Engines",
    content: "Over the years, car engines have evolved...",
    author: "Dominic Toretto",
    date: "2024-08-01T10:00:00Z",
  },
  // Additional posts...
];
```
This array serves as a mock database to store blog posts.

#### 3. **Middleware Setup**
```javascript
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
```
- **body-parser**: Parses request bodies in JSON and URL-encoded formats.

#### 4. **Route Definitions**

##### Fetch All Posts
```javascript
app.get("/posts", (req, res) => {
  res.json(posts);
});
```
**GET** `/posts`: Returns all posts in JSON format.

##### Fetch Post by ID
```javascript
app.get("/posts/:id", (req, res) => {
  const postid = parseInt(req.params.id);
  const postbyid = posts.find((post) => post.id === postid);
  res.json(postbyid);
});
```
**GET** `/posts/:id`: Finds and returns a post matching the `id` parameter.

##### Create a New Post
```javascript
app.post("/posts", (req, res) => {
  const newpostid = posts.length + 1;
  const newpost = {
    id: newpostid,
    title: req.body.title,
    content: req.body.content,
    author: req.body.author,
    date: new Date(),
  };
  posts.push(newpost);
  res.status(201).json(newpost);
});
```
**POST** `/posts`: Creates a new post with data from the request body, assigns an ID, and returns the created post with a `201` status.

##### Update a Post
```javascript
app.patch("/posts/:id", (req, res) => {
  const postid = parseInt(req.params.id);
  const updatepost = posts.find((post) => post.id === postid);
  if (!updatepost) return res.status(404).json({ message: "Invalid post id" });

  if (req.body.title) updatepost.title = req.body.title;
  if (req.body.content) updatepost.content = req.body.content;
  if (req.body.author) updatepost.author = req.body.author;
  res.json(updatepost);
});
```
**PATCH** `/posts/:id`: Finds a post by `id` and updates any fields present in the request body. If the post doesn’t exist, it returns a `404` error.

##### Delete a Post
```javascript
app.delete("/posts/:id", (req, res) => {
  const delid = parseInt(req.params.id);
  const index = posts.findIndex((post) => post.id === delid);
  if (index === -1) return res.status(404).json({ message: "Post not available" });

  posts.splice(index, 1);
  res.json({ message: "Post deleted" });
});
```
**DELETE** `/posts/:id`: Finds a post by `id`, removes it from the array if it exists, and returns a success message. If not, returns a `404` error.

#### 5. **Server Start**
```javascript
app.listen(port, () => {
  console.log(`API is running at http://localhost:${port}`);
});
```
The server listens on the specified port and logs the URL on startup.

### Documentation for GitHub

#### Project Structure
```
app.js              # Main application file
package.json        # Project dependencies and scripts
```

#### Setup Instructions

1. **Clone the Repository**
   ```bash
   git clone https://github.com/iamrahul-l/car-blog-REST-API.git
   cd blog-api-server
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Run the Server**
   ```bash
   npm start
   ```
   By default, the server runs at `http://localhost:4000`.

#### Environment Variables

- **PORT**: (Optional) Port for the server. Defaults to `4000` if not set.

#### Routes Documentation

| Route             | Method | Description                     |
|-------------------|--------|---------------------------------|
| `/posts`          | GET    | Retrieve all posts.             |
| `/posts/:id`      | GET    | Retrieve a post by ID.          |
| `/posts`          | POST   | Create a new post.              |
| `/posts/:id`      | PATCH  | Update an existing post by ID.  |
| `/posts/:id`      | DELETE | Delete a post by ID.            |

#### Sample Request and Response

**Create a New Post**

- **Request**: `POST /posts`
  ```json
  {
    "title": "The Rise of Autonomous Racing",
    "content": "Autonomous cars are now entering the racing scene...",
    "author": "Jane Doe"
  }
  ```
- **Response**:
  ```json
  {
    "id": 4,
    "title": "The Rise of Autonomous Racing",
    "content": "Autonomous cars are now entering the racing scene...",
    "author": "Jane Doe",
    "date": "2024-11-08T10:00:00.000Z"
  }
  ```

**Update a Post**

- **Request**: `PATCH /posts/1`
  ```json
  {
    "content": "Updated content about high-performance engines"
  }
  ```
- **Response**:
  ```json
  {
    "id": 1,
    "title": "The Evolution of High-Performance Engines",
    "content": "Updated content about high-performance engines",
    "author": "Dominic Toretto",
    "date": "2024-08-01T10:00:00Z"
  }
  ```

#### Error Handling

- **404 Not Found**: Returned if a post with a specified ID does not exist.
- **400 Bad Request**: If required fields are missing in a POST request.

### Example GitHub README

```markdown
# Blog API Server

This Express server provides a REST API to manage blog posts in-memory. It supports CRUD operations to create, read, update, and delete blog posts.

## Installation

Clone the repository:
```bash
git clone https://github.com/yourusername/blog-api-server.git
cd blog-api-server
```

Install dependencies:
```bash
npm install
```

## Usage

To start the server:
```bash
npm start
```
