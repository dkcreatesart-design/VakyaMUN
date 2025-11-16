// VākyaMUN — Full Next.js Website Project
// Upload these files exactly into your GitHub repo for Vercel deployment

// ================================
// package.json
// ================================
{
  "name": "vakyamun",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "14.0.0",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "framer-motion": "10.16.4"
  }
}

// ================================
// next.config.js
// ================================
/** @type {import('next').NextConfig} */
const nextConfig = {};
module.exports = nextConfig;

// ================================
// app/layout.js
// ================================
import './globals.css';
export const metadata = {
  title: 'VākyaMUN',
  description: 'Official website of VākyaMUN',
};
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body className="bg-slate-50 text-slate-800">{children}</body>
    </html>
  );
}

// ================================
// app/page.js
// ================================
import HomePage from "../src/HomePage";
export default function Page() {
  return <HomePage />;
}

// ================================
// app/globals.css
// ================================
@tailwind base;
@tailwind components;
@tailwind utilities;

// ================================
// tailwind.config.js
// ================================
module.exports = {
  content: ["./app/**/*.{js,jsx}", "./src/**/*.{js,jsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};

// ================================
// src/HomePage.js
// ================================
"use client";
import React, { useState } from "react";
import { motion } from "framer-motion";
import Committees from "./components/Committees";
import Registration from "./components/Registration";
import About from "./components/About";
import Contact from "./components/Contact";
import Nav from "./components/Nav";
import Hero from "./components/Hero";

export default function HomePage() {
  const [active, setActive] = useState("home");
  const [registrations, setRegistrations] = useState([]);

  function handleSubmit(reg) {
    const timestamp = Date.now();
    setRegistrations([...registrations, { ...reg, id: timestamp }]);
    alert("Registration received! You will get a confirmation email.");
  }

  return (
    <>
      <Nav active={active} setActive={setActive} />
      {active === "home" && (
        <>
          <Hero openRegister={() => setActive("register")} />
          <Committees />
          <About />
          <Contact />
        </>
      )}
      {active === "committees" && <Committees />}
      {active === "about" && <About />}
      {active === "contact" && <Contact />}
      {active === "register" && <Registration onSubmit={handleSubmit} />}
    </>
  );
}

// ================================
// src/components/Nav.js
// ================================
export default function Nav({ active, setActive }) {
  const tabs = [
    ["home", "Home"],
    ["committees", "Committees"],
    ["register", "Register"],
    ["about", "About"],
    ["contact", "Contact"],
  ];
  return (
    <nav className="max-w-6xl mx-auto p-4 flex justify-between items-center">
      <div className="text-2xl font-bold tracking-tight">VākyaMUN</div>
      <div className="space-x-3">
        {tabs.map(([key, label]) => (
          <button
            key={key}
            className={`px-3 py-2 rounded-md text-sm ${active === key
              ? "bg-slate-900 text-white"
              : "hover:bg-slate-200"}`}
            onClick={() => setActive(key)}
          >
            {label}
          </button>
        ))}
      </div>
    </nav>
  );
}

// ================================
// src/components/Hero.js
// ================================
"use client";
import { motion } from "framer-motion";

export default function Hero({ openRegister }) {
  return (
    <section className="text-center py-20 bg-white">
      <motion.h1
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        className="text-5xl font-extrabold"
      >
        Welcome to VākyaMUN
      </motion.h1>
      <p className="mt-4 text-lg max-w-2xl mx-auto">
        Where Dialogue Creates Diplomacy.
      </p>
      <button
        onClick={openRegister}
        className="mt-6 px-6 py-3 bg-slate-900 text-white rounded-lg"
      >
        Register Now
      </button>
    </section>
  );
}

// ================================
// src/components/Committees.js
// ================================
export default function Committees() {
  return (
    <section className="max-w-4xl mx-auto p-6">
      <h2 className="text-3xl font-bold mb-4">Committees</h2>
      <ul className="space-y-3 text-lg">
        <li>🟦 UNGA – Global Security & Peace</li>
        <li>🟩 UNHRC – Human Rights Challenges</li>
        <li>🟪 ECOSOC – Sustainable Development</li>
        <li>🟥 AIPPM – Indian Political Affairs</li>
        <li>🟧 UNSC – Crisis Committee</li>
        <li>🟨 Lok Sabha – National Legislative Debate</li>
      </ul>
    </section>
  );
}

// ================================
// src/components/About.js
// ================================
export default function About() {
  return (
    <section className="max-w-4xl mx-auto p-6">
      <h2 className="text-3xl font-bold mb-4">About VākyaMUN</h2>
      <p>
        VākyaMUN is a platform where young diplomats engage in meaningful
        dialogue, debate global issues, and craft sustainable solutions.
      </p>
    </section>
  );
}

// ================================
// src/components/Contact.js
// ================================
export default function Contact() {
  return (
    <section className="max-w-4xl mx-auto p-6">
      <h2 className="text-3xl font-bold mb-4">Contact Us</h2>
      <p>Email: vakyamun@gmail.com</p>
      <p>Instagram: @vakyamun</p>
    </section>
  );
}

// ================================
// src/components/Registration.js
// ================================
"use client";
import { useState } from "react";

export default function Registration({ onSubmit }) {
  const [form, setForm] = useState({ name: "", email: "", committee: "" });

  function handleChange(e) {
    setForm({ ...form, [e.target.name]: e.target.value });
  }

  function submit(e) {
    e.preventDefault();
    onSubmit(form);
  }

  return (
    <section className="max-w-md mx-auto p-6">
      <h2 className="text-3xl font-bold mb-4">Register</h2>
      <form onSubmit={submit} className="space-y-4">
        <input
          name="name"
          placeholder="Full Name"
          className="w-full p-3 border rounded"
          onChange={handleChange}
        />
        <input
          name="email"
          placeholder="Email"
          className="w-full p-3 border rounded"
          onChange={handleChange}
        />
        <select
          name="committee"
          className="w-full p-3 border rounded"
          onChange={handleChange}
        >
          <option>Select Committee</option>
          <option>UNGA</option>
          <option>UNHRC</option>
          <option>ECOSOC</option>
          <option>AIPPM</option>
          <option>UNSC</option>
          <option>Lok Sabha</option>
        </select>
        <button className="w-full p-3 bg-slate-900 text-white rounded">
          Submit
        </button>
      </form>
    </section>
  );
}
