// Full ready-to-upload PiCShare frontend code // React + Tailwind CSS | Mobile Web Style | Connects to https://picshare-cywc.onrender.com

// --- File: package.json --- { "name": "picshare-frontend", "version": "1.0.0", "private": true, "dependencies": { "axios": "^1.6.0", "react": "^18.2.0", "react-dom": "^18.2.0", "react-router-dom": "^6.18.0", "react-scripts": "5.0.1" }, "scripts": { "start": "react-scripts start", "build": "react-scripts build" } }

// --- File: tailwind.config.js --- module.exports = { content: ["./src/**/*.{js,jsx}"], theme: { extend: {}, }, plugins: [], }

// --- File: postcss.config.js --- module.exports = { plugins: { tailwindcss: {}, autoprefixer: {}, }, }

// --- File: public/index.html ---

<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>PiCShare</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>// --- File: src/index.css --- @tailwind base; @tailwind components; @tailwind utilities;

body { font-family: sans-serif; background: #f3f4f6; }

// --- File: src/index.js --- import React from 'react'; import ReactDOM from 'react-dom/client'; import './index.css'; import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root')); root.render(<App />);

// --- File: src/api/axios.js --- import axios from 'axios'; export default axios.create({ baseURL: 'https://picshare-cywc.onrender.com', });

// --- File: src/App.js --- import React, { useState, useEffect } from "react"; import axios from "./api/axios"; import "./index.css";

function App() { const [page, setPage] = useState("login"); const [user, setUser] = useState(null); const [form, setForm] = useState({}); const [file, setFile] = useState(null); const [posts, setPosts] = useState([]);

const handleInput = (e) => { setForm({ ...form, [e.target.name]: e.target.value }); };

const handleLogin = async () => { try { const res = await axios.post("/api/auth/login", form); setUser(res.data.user); setPage("feed"); } catch (e) { alert("Login failed"); } };

const handleRegister = async () => { try { const data = new FormData(); Object.entries(form).forEach(([k, v]) => data.append(k, v)); if (file) data.append("profilePic", file); await axios.post("/api/auth/register", data); alert("Registered, now login"); setPage("login"); } catch (e) { alert("Registration failed"); } };

const handleUpload = async () => { try { const data = new FormData(); data.append("photo", file); data.append("caption", form.caption); data.append("userId", user._id); await axios.post("/api/posts", data); alert("Photo posted"); setForm({}); setFile(null); fetchPosts(); setPage("feed"); } catch (e) { alert("Upload failed"); } };

const fetchPosts = async () => { try { const res = await axios.get("/api/posts"); setPosts(res.data.reverse()); } catch (e) { console.log("Fetch failed"); } };

useEffect(() => { if (page === "feed") fetchPosts(); }, [page]);

const BottomNav = () => ( <div className="fixed bottom-0 w-full flex justify-around bg-white p-2 shadow"> <button onClick={() => setPage("feed")}>🏠</button> <button onClick={() => setPage("upload")}>➕</button> <button onClick={() => setPage("profile")}>👤</button> </div> );

if (!user) { return ( <div className="min-h-screen flex flex-col items-center justify-center bg-gray-100 p-4"> <h1 className="text-2xl mb-4 font-bold">PiCShare</h1> <input name="username" placeholder="Username" className="mb-2 p-2 w-full" onChange={handleInput} /> <input name="password" type="password" placeholder="Password" className="mb-2 p-2 w-full" onChange={handleInput} /> {page === "register" && ( <> <input name="email" placeholder="Email" className="mb-2 p-2 w-full" onChange={handleInput} /> <input type="file" onChange={(e) => setFile(e.target.files[0])} className="mb-2 w-full" /> </> )} <button className="bg-blue-500 text-white px-4 py-2 rounded" onClick={page === "login" ? handleLogin : handleRegister}> {page === "login" ? "Login" : "Register"} </button> <button className="mt-2 underline" onClick={() => setPage(page === "login" ? "register" : "login")}> {page === "login" ? "Create account" : "Already have an account? Login"} </button> </div> ); }

return ( <div className="pb-16"> {page === "feed" && ( <div> <h2 className="text-center text-xl font-bold p-4">Feed</h2> {posts.map((p) => ( <div key={p._id} className="bg-white m-2 p-4 rounded shadow"> <div className="flex items-center mb-2"> <img src={p.user?.profilePic} className="w-8 h-8 rounded-full mr-2" /> <span>{p.user?.username}</span> </div> <img src={p.photo} alt="" className="w-full rounded mb-2" /> <p>{p.caption}</p> </div> ))} </div> )}

{page === "upload" && (
    <div className="p-4">
      <h2 className="text-xl font-bold mb-4">Upload Post</h2>
      <input type="file" onChange={(e) => setFile(e.target.files[0])} className="mb-4" />
      <input name="caption" placeholder="Caption..." className="w-full mb-4 p-2 border" onChange={handleInput} />
      <button onClick={handleUpload} className="bg-blue-600 text-white px-4 py-2 rounded">Post</button>
    </div>
  )}

  {page === "profile" && (
    <div className="p-4">
      <h2 className="text-xl font-bold mb-4">{user.username}</h2>
      <img src={user.profilePic} className="w-24 h-24 rounded-full mb-4" />
      <button onClick={() => setUser(null)} className="text-red-600 underline">Logout</button>
    </div>
  )}

  <BottomNav />
</div>

); }

export default App;

