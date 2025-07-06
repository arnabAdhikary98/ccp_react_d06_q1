# Simple Blog App with Vite + React + React Router

## 📦 Setup

```bash
npm create vite@latest my-blog-app -- --template react
cd my-blog-app
npm install
npm install react-router-dom
npm run dev
```

---

## 📁 Folder Structure

```
src/
├── components/
│   └── Navbar.jsx
├── pages/
│   ├── Home.jsx
│   ├── PostDetails.jsx
│   └── About.jsx
├── App.jsx
├── main.jsx
```

---

## 🗺️ App.jsx

```jsx
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Navbar from './components/Navbar';
import Home from './pages/Home';
import About from './pages/About';
import PostDetails from './pages/PostDetails';

function App() {
  return (
    <Router>
      <Navbar />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/post/:id" element={<PostDetails />} />
      </Routes>
    </Router>
  );
}

export default App;
```

---

## 🧭 Navbar.jsx

```jsx
import { Link } from 'react-router-dom';

const Navbar = () => {
  return (
    <nav style={{ padding: '10px', background: '#f0f0f0' }}>
      <Link to="/" style={{ marginRight: '10px' }}>Home</Link>
      <Link to="/about">About</Link>
    </nav>
  );
};

export default Navbar;
```

---

## 🏠 Home.jsx

```jsx
import { useEffect, useState } from 'react';
import { Link } from 'react-router-dom';

const Home = () => {
  const [posts, setPosts] = useState([]);
  const [search, setSearch] = useState('');

  useEffect(() => {
    fetch('https://dummyjson.com/posts')
      .then(res => res.json())
      .then(data => setPosts(data.posts))
      .catch(err => console.error(err));
  }, []);

  const filteredPosts = posts.filter(post =>
    post.title.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div style={{ padding: '20px' }}>
      <h1>Blog Posts</h1>
      <input
        type="text"
        placeholder="Search by title..."
        value={search}
        onChange={e => setSearch(e.target.value)}
        style={{ marginBottom: '20px', padding: '5px' }}
      />
      <ul>
        {filteredPosts.map(post => (
          <li key={post.id}>
            <Link to={`/post/${post.id}`} style={{ textDecoration: 'none' }}>
              {post.title}
            </Link>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Home;
```

---

## 📝 PostDetails.jsx

```jsx
import { useParams } from 'react-router-dom';
import { useEffect, useState } from 'react';

const PostDetails = () => {
  const { id } = useParams();
  const [post, setPost] = useState(null);

  useEffect(() => {
    fetch(`https://dummyjson.com/posts/${id}`)
      .then(res => res.json())
      .then(data => setPost(data))
      .catch(err => console.error(err));
  }, [id]);

  if (!post) return <p>Loading...</p>;

  return (
    <div style={{ padding: '20px' }}>
      <h2>{post.title}</h2>
      <p>{post.body}</p>
      <div>
        <strong>Tags:</strong>
        <ul>
          {post.tags.map(tag => (
            <li key={tag}>{tag}</li>
          ))}
        </ul>
      </div>
    </div>
  );
};

export default PostDetails;
```

---

## ℹ️ About.jsx

```jsx
const About = () => {
  return (
    <div style={{ padding: '20px' }}>
      <h1>About This Blog</h1>
      <p>This is a simple blog app built with React, Vite, and react-router-dom. It displays mock blog posts from an API.</p>
      <p>Author: Your Name</p>
    </div>
  );
};

export default About;
```

---

## 🏁 main.jsx

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

## ✅ Done!

### Features

* Home page with list of posts and search
* Post details page with full content and tags
* Static About page
* Navbar navigation
