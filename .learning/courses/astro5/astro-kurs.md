# Astro 5 dla Java Developera - Od Zera do Pierwszej Aplikacji

Cześć! Widzę, że masz solidne doświadczenie z Java/Spring. To **ogromna zaleta** - wiele konceptów z backendu przełoży się na Astro. Pokażę Ci Astro przez analogie do tego, co już znasz.

---

## 🎯 Czym jest Astro? (Analogia dla Java Dev)

| Koncepcja | Spring Boot | Astro 5 |
|-----------|-------------|---------|
| **Framework** | Spring MVC | Astro (static site generator) |
| **Szablony** | Thymeleaf/JSP | `.astro` components |
| **Routing** | `@GetMapping("/path")` | Plik `src/pages/path.astro` |
| **Component** | `@Component` class | `.astro` file |
| **Build** | Maven/Gradle → JAR | NPM → static HTML/CSS/JS |
| **Runtime** | JVM (server-side) | Build time + minimal JS |
| **Dependency Injection** | `@Autowired` | `import` i props |

**Kluczowa różnica:** 
- **Spring:** Server-side rendering (każde żądanie → serwer generuje HTML)
- **Astro:** Build-time rendering (raz build → statyczne pliki HTML)

---

## 🚀 Quick Start: Pierwsze 15 minut

### Krok 1: Setup środowiska (5 min)

```bash
# Sprawdź Node.js (jak sprawdzasz Java version)
node --version          # Potrzebujesz v18+ (jak JDK 21)
npm --version           # Package manager (jak Maven)

# Jeśli nie masz Node.js:
# Ubuntu/Debian: sudo apt install nodejs npm
# Mac: brew install node
# Windows: https://nodejs.org/
```

### Krok 2: Utwórz projekt Astro (3 min)

```bash
# Podobne do: spring init --dependencies=web myapp
npm create astro@latest my-first-astro-app

# Astro zapyta o:
# 1. Template: wybierz "Empty" (jak Spring Boot starter bez dependencies)
# 2. Install dependencies? → Yes
# 3. TypeScript? → No (na razie, potem dodasz)
# 4. Git? → Yes
```

### Krok 3: Uruchom dev server (2 min)

```bash
cd my-first-astro-app

# Jak: ./gradlew bootRun
npm run dev

# Otwórz: http://localhost:4321
```

**Gratulacje! 🎉** Masz działającą aplikację Astro!

---

## 📁 Struktura projektu (porównanie ze Spring Boot)

```
my-first-astro-app/
│
├── src/                        ← Jak src/main/
│   ├── pages/                  ← ROUTING (jak @Controller)
│   │   └── index.astro        ← / endpoint (jak @GetMapping("/"))
│   │
│   ├── components/             ← Komponenty wielokrotnego użytku
│   │   └── Header.astro       ← Jak @Component class
│   │
│   ├── layouts/                ← Szablony bazowe
│   │   └── Layout.astro       ← Jak base.html w Thymeleaf
│   │
│   └── content/                ← Dane (jak resources/)
│       └── blog/               ← Markdown content
│
├── public/                     ← Static files (jak src/main/resources/static)
│   ├── favicon.svg
│   └── images/
│
├── astro.config.mjs            ← Konfiguracja (jak application.yml)
├── package.json                ← Dependencies (jak pom.xml/build.gradle)
├── tsconfig.json               ← TypeScript config (opcjonalny)
└── README.md
```

---

## 🧩 Podstawowy koncept: Astro Component

### Analogia: Spring Controller + Thymeleaf

**Spring Boot:**
```java
@Controller
public class HomeController {
    
    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("title", "Welcome");
        model.addAttribute("items", Arrays.asList("A", "B", "C"));
        return "home";  // → templates/home.html
    }
}
```

```html
<!-- templates/home.html -->
<!DOCTYPE html>
<html>
<head>
    <title th:text="${title}">Default</title>
</head>
<body>
    <h1 th:text="${title}">Welcome</h1>
    <ul>
        <li th:each="item : ${items}" th:text="${item}"></li>
    </ul>
</body>
</html>
```

**Astro (to samo w jednym pliku!):**

```astro
---
// src/pages/index.astro
// Część "backendowa" (działa podczas buildu, jak controller)
const title = "Welcome";
const items = ["A", "B", "C"];

// Możesz też fetch z API:
// const response = await fetch('https://api.example.com/data');
// const data = await response.json();
---

<!-- Część "frontendowa" (template, jak Thymeleaf) -->
<!DOCTYPE html>
<html>
<head>
    <title>{title}</title>
</head>
<body>
    <h1>{title}</h1>
    <ul>
        {items.map(item => (
            <li>{item}</li>
        ))}
    </ul>
</body>
</html>
```

**Kluczowe różnice:**
- `---` separator = "code fence" (backend logic)
- `{}` zamiast `${}` dla zmiennych
- `.map()` zamiast `th:each`
- Wszystko w jednym pliku `.astro`

---

## 🛣️ Routing: File-based (jak Spring MVC, ale prostsze)

### Spring Boot routing:
```java
@Controller
public class MyController {
    @GetMapping("/")              → http://localhost:8080/
    @GetMapping("/about")         → http://localhost:8080/about
    @GetMapping("/blog/{id}")     → http://localhost:8080/blog/123
}
```

### Astro routing (automatyczny!):

```
src/pages/
├── index.astro              → http://localhost:4321/
├── about.astro              → http://localhost:4321/about
├── blog/
│   ├── index.astro          → http://localhost:4321/blog
│   └── [id].astro           → http://localhost:4321/blog/123
└── api/
    └── users.json.ts        → http://localhost:4321/api/users.json
```

**Przykład: Dynamic route**

```astro
---
// src/pages/blog/[id].astro
// Jak @PathVariable w Spring

// Pobierz ID z URL
const { id } = Astro.params;

// Fetch danych (jak @Service)
const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
const post = await response.json();
---

<html>
<head>
    <title>{post.title}</title>
</head>
<body>
    <h1>{post.title}</h1>
    <p>{post.body}</p>
    <a href="/blog">← Back to blog</a>
</body>
</html>
```

---

## 🎓 Seria ćwiczeń - Rosnąca trudność

### **Ćwiczenie 1: Hello World** (10 min)

**Cel:** Zrozumieć podstawową strukturę `.astro` file

```astro
---
// src/pages/hello.astro
const name = "Java Developer";
const currentYear = new Date().getFullYear();
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello Astro</title>
    <style>
        body {
            font-family: system-ui;
            max-width: 800px;
            margin: 2rem auto;
            padding: 0 1rem;
        }
        .card {
            background: #f0f0f0;
            padding: 2rem;
            border-radius: 8px;
        }
    </style>
</head>
<body>
    <div class="card">
        <h1>Hello, {name}!</h1>
        <p>Welcome to Astro 5</p>
        <p>Current year: {currentYear}</p>
    </div>
</body>
</html>
```

**Zadania:**
1. Utwórz ten plik
2. Otwórz http://localhost:4321/hello
3. Zmień wartość `name` na swoje imię
4. Dodaj więcej zmiennych (np. `favoriteLanguage`)

---

### **Ćwiczenie 2: Reusable Component** (15 min)

**Cel:** Tworzyć komponenty (jak @Component w Spring)

**Krok 1:** Utwórz komponent

```astro
---
// src/components/Card.astro
// Props = jak parametry metody w Java

interface Props {
    title: string;
    description: string;
    link?: string;  // Optional (jak Optional<String> w Java)
}

const { title, description, link } = Astro.props;
---

<div class="card">
    <h2>{title}</h2>
    <p>{description}</p>
    {link && (
        <a href={link}>Learn more →</a>
    )}
</div>

<style>
    .card {
        background: white;
        border: 1px solid #ddd;
        border-radius: 8px;
        padding: 1.5rem;
        margin-bottom: 1rem;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    
    h2 {
        margin-top: 0;
        color: #333;
    }
    
    a {
        color: #0066cc;
        text-decoration: none;
    }
    
    a:hover {
        text-decoration: underline;
    }
</style>
```

**Krok 2:** Użyj komponentu

```astro
---
// src/pages/components-demo.astro
import Card from '../components/Card.astro';

const technologies = [
    {
        title: "Java",
        description: "15 years of experience",
        link: "https://java.com"
    },
    {
        title: "Spring Boot",
        description: "Building enterprise applications",
        link: "https://spring.io"
    },
    {
        title: "Astro",
        description: "Learning new frontend framework",
        link: "https://astro.build"
    }
];
---

<!DOCTYPE html>
<html>
<head>
    <title>My Tech Stack</title>
</head>
<body>
    <h1>My Technology Stack</h1>
    
    <!-- Jak iterowanie w Thymeleaf -->
    {technologies.map(tech => (
        <Card 
            title={tech.title}
            description={tech.description}
            link={tech.link}
        />
    ))}
</body>
</html>
```

**Analogia do Spring:**
```java
// To jest jak:
@Component
public class Card {
    private String title;
    private String description;
    private String link;
    
    // getters, setters, constructor
}

// I używanie:
technologies.forEach(tech -> {
    Card card = new Card(tech.getTitle(), tech.getDescription());
    // render card
});
```

---

### **Ćwiczenie 3: Layout (Master Page)** (20 min)

**Cel:** DRY principle - nie powtarzaj HTML

**Analogia:** Jak `base.html` w Thymeleaf z `th:fragment`

```astro
---
// src/layouts/Layout.astro
// Base layout dla wszystkich stron

interface Props {
    title: string;
    description?: string;
}

const { title, description = "My Astro Site" } = Astro.props;
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content={description}>
    <title>{title}</title>
    
    <style is:global>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: system-ui, -apple-system, sans-serif;
            line-height: 1.6;
            color: #333;
            background: #f5f5f5;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
        }
        
        header {
            background: #2c3e50;
            color: white;
            padding: 1rem 0;
            margin-bottom: 2rem;
        }
        
        nav {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 2rem;
            display: flex;
            gap: 2rem;
        }
        
        nav a {
            color: white;
            text-decoration: none;
        }
        
        nav a:hover {
            text-decoration: underline;
        }
        
        footer {
            background: #2c3e50;
            color: white;
            text-align: center;
            padding: 2rem 0;
            margin-top: 4rem;
        }
    </style>
</head>
<body>
    <header>
        <nav>
            <a href="/">Home</a>
            <a href="/about">About</a>
            <a href="/blog">Blog</a>
            <a href="/contact">Contact</a>
        </nav>
    </header>
    
    <main class="container">
        <!-- slot = miejsce na content (jak th:insert w Thymeleaf) -->
        <slot />
    </main>
    
    <footer>
        <p>&copy; {new Date().getFullYear()} My Astro Site</p>
    </footer>
</body>
</html>
```

**Użycie layoutu:**

```astro
---
// src/pages/index.astro
import Layout from '../layouts/Layout.astro';
import Card from '../components/Card.astro';
---

<Layout title="Home Page" description="Welcome to my site">
    <h1>Welcome to My Site</h1>
    
    <Card 
        title="Built with Astro"
        description="Fast, modern web framework"
    />
</Layout>
```

**Analogia Spring/Thymeleaf:**
```html
<!-- base.html -->
<html th:fragment="layout(title, content)">
  <head><title th:text="${title}"></title></head>
  <body>
    <nav>...</nav>
    <main th:insert="${content}"></main>
    <footer>...</footer>
  </body>
</html>

<!-- page.html -->
<html th:replace="~{base :: layout('Home', ~{::content})}">
  <div th:fragment="content">
    <h1>Page content</h1>
  </div>
</html>
```

---

### **Ćwiczenie 4: Fetch Data (REST API)** (25 min)

**Cel:** Pobierać dane z API (jak RestTemplate/WebClient w Spring)

```astro
---
// src/pages/users.astro
import Layout from '../layouts/Layout.astro';

// Fetch data podczas buildu (jak @Service call)
const response = await fetch('https://jsonplaceholder.typicode.com/users');
const users = await response.json();

// Możesz też używać try-catch jak w Java:
// try {
//     const response = await fetch('...');
//     if (!response.ok) {
//         throw new Error(`HTTP error! status: ${response.status}`);
//     }
//     const users = await response.json();
// } catch (error) {
//     console.error('Error fetching users:', error);
//     const users = [];
// }
---

<Layout title="Users List">
    <h1>Users from API</h1>
    
    <div class="users-grid">
        {users.map(user => (
            <div class="user-card">
                <h2>{user.name}</h2>
                <p><strong>Email:</strong> {user.email}</p>
                <p><strong>Company:</strong> {user.company.name}</p>
                <a href={`/users/${user.id}`}>View Profile →</a>
            </div>
        ))}
    </div>
</Layout>

<style>
    .users-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
        gap: 1.5rem;
        margin-top: 2rem;
    }
    
    .user-card {
        background: white;
        padding: 1.5rem;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    
    .user-card h2 {
        margin-bottom: 1rem;
        color: #2c3e50;
    }
    
    .user-card p {
        margin: 0.5rem 0;
    }
    
    .user-card a {
        display: inline-block;
        margin-top: 1rem;
        color: #0066cc;
        text-decoration: none;
    }
</style>
```

**Dynamic user profile:**

```astro
---
// src/pages/users/[id].astro
import Layout from '../../layouts/Layout.astro';

const { id } = Astro.params;

// Fetch single user
const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`);
const user = await response.json();

// Fetch user's posts
const postsResponse = await fetch(`https://jsonplaceholder.typicode.com/users/${id}/posts`);
const posts = await postsResponse.json();
---

<Layout title={user.name}>
    <a href="/users">← Back to Users</a>
    
    <div class="profile">
        <h1>{user.name}</h1>
        <p class="username">@{user.username}</p>
        
        <div class="info">
            <p><strong>Email:</strong> {user.email}</p>
            <p><strong>Phone:</strong> {user.phone}</p>
            <p><strong>Website:</strong> {user.website}</p>
            <p><strong>Company:</strong> {user.company.name}</p>
        </div>
        
        <h2>Posts by {user.name}</h2>
        <div class="posts">
            {posts.map(post => (
                <article class="post">
                    <h3>{post.title}</h3>
                    <p>{post.body}</p>
                </article>
            ))}
        </div>
    </div>
</Layout>

<style>
    .profile {
        max-width: 800px;
    }
    
    .username {
        color: #666;
        font-size: 1.2rem;
        margin-bottom: 2rem;
    }
    
    .info {
        background: #f9f9f9;
        padding: 1.5rem;
        border-radius: 8px;
        margin: 2rem 0;
    }
    
    .info p {
        margin: 0.5rem 0;
    }
    
    .posts {
        margin-top: 2rem;
    }
    
    .post {
        background: white;
        padding: 1.5rem;
        border-radius: 8px;
        margin-bottom: 1.5rem;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    
    .post h3 {
        margin-bottom: 1rem;
        color: #2c3e50;
    }
</style>
```

---

### **Ćwiczenie 5: Interaktywność (Client-side JavaScript)** (30 min)

**Cel:** Dodać JavaScript dla interakcji (jak Thymeleaf + jQuery, ale modern)

```astro
---
// src/components/Counter.astro
// Islands Architecture: Ten komponent będzie interaktywny na kliencie
---

<div class="counter">
    <h2>Counter Component</h2>
    <p>Count: <span id="count">0</span></p>
    <button id="increment">Increment</button>
    <button id="decrement">Decrement</button>
    <button id="reset">Reset</button>
</div>

<style>
    .counter {
        background: white;
        padding: 2rem;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        text-align: center;
    }
    
    button {
        background: #0066cc;
        color: white;
        border: none;
        padding: 0.5rem 1rem;
        margin: 0.5rem;
        border-radius: 4px;
        cursor: pointer;
        font-size: 1rem;
    }
    
    button:hover {
        background: #0052a3;
    }
    
    #count {
        font-size: 2rem;
        font-weight: bold;
        color: #2c3e50;
    }
</style>

<script>
    // Client-side JavaScript (działa w przeglądarce)
    // Podobne do jQuery event handlers
    
    let count = 0;
    const countElement = document.getElementById('count');
    const incrementBtn = document.getElementById('increment');
    const decrementBtn = document.getElementById('decrement');
    const resetBtn = document.getElementById('reset');
    
    function updateCount() {
        if (countElement) {
            countElement.textContent = count.toString();
        }
    }
    
    incrementBtn?.addEventListener('click', () => {
        count++;
        updateCount();
    });
    
    decrementBtn?.addEventListener('click', () => {
        count--;
        updateCount();
    });
    
    resetBtn?.addEventListener('click', () => {
        count = 0;
        updateCount();
    });
</script>
```

**Użycie:**

```astro
---
// src/pages/interactive.astro
import Layout from '../layouts/Layout.astro';
import Counter from '../components/Counter.astro';
---

<Layout title="Interactive Demo">
    <h1>Client-side Interactivity</h1>
    <p>This component runs JavaScript in the browser.</p>
    
    <Counter />
</Layout>
```

---

## 🏗️ Projekt demonstracyjny: Blog Application

**Cel:** Zbudować kompletną aplikację (jak Spring Boot + Thymeleaf blog)

### Struktura projektu:

```
src/
├── layouts/
│   └── Layout.astro
├── components/
│   ├── Header.astro
│   ├── Footer.astro
│   └── BlogCard.astro
├── pages/
│   ├── index.astro
│   ├── about.astro
│   ├── blog/
│   │   ├── index.astro
│   │   └── [slug].astro
│   └── api/
│       └── posts.json.ts
└── content/
    └── blog/
        ├── post-1.md
        └── post-2.md
```

### 1. Content Collections (nowa feature w Astro 5)

```typescript
// src/content/config.ts
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
    type: 'content',
    schema: z.object({
        title: z.string(),
        description: z.string(),
        pubDate: z.date(),
        author: z.string(),
        tags: z.array(z.string()).optional(),
    }),
});

export const collections = { blog };
```

### 2. Markdown content

```markdown
---
title: "Getting Started with Astro"
description: "Learn how to build fast websites with Astro"
pubDate: 2024-01-15
author: "Java Developer"
tags: ["astro", "web", "tutorial"]
---

# Getting Started with Astro

This is my first blog post using Astro. It's similar to writing documentation
in Markdown, which Java developers often use for README files.

## Why Astro?

- Fast build times
- Zero JavaScript by default
- Great developer experience
- Easy for backend developers to learn

## Conclusion

Astro is a great choice for content-focused websites!
```

### 3. Blog list page

```astro
---
// src/pages/blog/index.astro
import { getCollection } from 'astro:content';
import Layout from '../../layouts/Layout.astro';
import BlogCard from '../../components/BlogCard.astro';

// Pobierz wszystkie posty (jak JPA repository.findAll())
const posts = await getCollection('blog');

// Sortuj po dacie (jak Stream API w Java)
const sortedPosts = posts.sort((a, b) => 
    b.data.pubDate.valueOf() - a.data.pubDate.valueOf()
);
---

<Layout title="Blog">
    <h1>Blog Posts</h1>
    <p>Total posts: {sortedPosts.length}</p>
    
    <div class="posts-grid">
        {sortedPosts.map(post => (
            <BlogCard 
                title={post.data.title}
                description={post.data.description}
                pubDate={post.data.pubDate}
                slug={post.slug}
                tags={post.data.tags}
            />
        ))}
    </div>
</Layout>

<style>
    .posts-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
        gap: 2rem;
        margin-top: 2rem;
    }
</style>
```

### 4. Single blog post

```astro
---
// src/pages/blog/[slug].astro
import { getCollection } from 'astro:content';
import Layout from '../../layouts/Layout.astro';

// Generuj statyczne ścieżki (jak Spring MVC routing)
export async function getStaticPaths() {
    const posts = await getCollection('blog');
    return posts.map(post => ({
        params: { slug: post.slug },
        props: { post },
    }));
}

const { post } = Astro.props;
const { Content } = await post.render();

// Format date (jak SimpleDateFormat w Java)
const formattedDate = post.data.pubDate.toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
});
---

<Layout title={post.data.title} description={post.data.description}>
    <article>
        <a href="/blog">← Back to Blog</a>
        
        <header>
            <h1>{post.data.title}</h1>
            <div class="meta">
                <span>By {post.data.author}</span>
                <span>{formattedDate}</span>
            </div>
            {post.data.tags && (
                <div class="tags">
                    {post.data.tags.map(tag => (
                        <span class="tag">{tag}</span>
                    ))}
                </div>
            )}
        </header>
        
        <div class="content">
            <Content />
        </div>
    </article>
</Layout>

<style>
    article {
        max-width: 800px;
        margin: 0 auto;
    }
    
    header {
        margin-bottom: 3rem;
    }
    
    h1 {
        font-size: 2.5rem;
        margin-bottom: 1rem;
        color: #2c3e50;
    }
    
    .meta {
        color: #666;
        display: flex;
        gap: 1rem;
        margin-bottom: 1rem;
    }
    
    .tags {
        display: flex;
        gap: 0.5rem;
        flex-wrap: wrap;
    }
    
    .tag {
        background: #e0e0e0;
        padding: 0.25rem 0.75rem;
        border-radius: 4px;
        font-size: 0.875rem;
    }
    
    .content {
        font-size: 1.125rem;
        line-height: 1.8;
    }
    
    .content :global(h2) {
        margin-top: 2rem;
        margin-bottom: 1rem;
        color: #2c3e50;
    }
    
    .content :global(pre) {
        background: #f5f5f5;
        padding: 1rem;
        border-radius: 4px;
        overflow-x: auto;
    }
    
    .content :global(code) {
        font-family: 'Courier New', monospace;
    }
</style>
```

---

## 🔧 Dodatkowe funkcje Astro 5

### 1. API Routes (jak Spring REST Controller)

```typescript
// src/pages/api/posts.json.ts
import { getCollection } from 'astro:content';
import type { APIRoute } from 'astro';

// Jak @GetMapping("/api/posts")
export const GET: APIRoute = async () => {
    const posts = await getCollection('blog');
    
    const simplifiedPosts = posts.map(post => ({
        title: post.data.title,
        slug: post.slug,
        pubDate: post.data.pubDate.toISOString(),
    }));
    
    return new Response(JSON.stringify(simplifiedPosts), {
        status: 200,
        headers: {
            'Content-Type': 'application/json'
        }
    });
};
```

### 2. Environment Variables (jak application.properties)

```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';

export default defineConfig({
    site: 'https://mysite.com',
    // Inne konfiguracje
});
```

```astro
---
// Dostęp do env variables
const apiKey = import.meta.env.PUBLIC_API_KEY;  // Publiczne
const secret = import.meta.env.SECRET_KEY;       // Prywatne (tylko w build)
---
```

### 3. Image Optimization

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/photo.jpg';
---

<!-- Automatyczna optymalizacja obrazów -->
<Image 
    src={myImage} 
    alt="Description"
    width={800}
    height={600}
    loading="lazy"
/>
```

---

## 📚 Następne kroki (4-tygodniowy plan)

### Tydzień 1: Podstawy
- [ ] Setup projektu i explore strukturę
- [ ] Zrób wszystkie ćwiczenia 1-3
- [ ] Stwórz 3-5 prostych stron
- [ ] Dodaj navigation i layout

### Tydzień 2: Komponenty i Style
- [ ] Stwórz 5+ reusable components
- [ ] Naucz się CSS-in-JS lub Tailwind
- [ ] Zbuduj responsive navigation
- [ ] Dodaj dark mode

### Tydzień 3: Content i Data
- [ ] Skonfiguruj Content Collections
- [ ] Napisz 5 postów na bloga w Markdown
- [ ] Fetch data z external API
- [ ] Dodaj search functionality

### Tydzień 4: Deploy i Optymalizacja
- [ ] Deploy na Netlify/Vercel
- [ ] Dodaj SEO meta tags
- [ ] Optymalizuj images
- [ ] Dodaj analytics

---

## 🎯 Kluczowe różnice: Spring Boot vs Astro

| Aspekt | Spring Boot | Astro |
|--------|-------------|-------|
| **Rendering** | Server-side (każde request) | Build-time (raz) |
| **Runtime** | JVM na serwerze | Static HTML + minimal JS |
| **Routing** | Annotations (@GetMapping) | File-based (automatic) |
| **Templates** | Thymeleaf/JSP | .astro components |
| **State** | Session, DB | Static lub client-side |
| **Scalability** | Horizontal scaling | CDN edge deployment |
| **Cost** | Server + JVM resources | Hosting static files (cheap!) |
| **Use Case** | Dynamic apps, APIs | Content sites, blogs, docs |

---

## 💡 Kiedy używać Astro vs Spring Boot?

### Użyj Astro dla:
- ✅ Marketing websites
- ✅ Blogs i documentation
- ✅ Portfolio sites
- ✅ Landing pages
- ✅ Content-focused sites
- ✅ Sites z rzadko zmieniającą się treścią

### Użyj Spring Boot dla:
- ✅ E-commerce z real-time inventory
- ✅ Social media platforms
- ✅ Banking applications
- ✅ Admin dashboards z live data
- ✅ APIs dla mobile apps
- ✅ Applications z complex business logic

### Użyj obu razem! 🤝
```
Astro (frontend)  →  REST API  →  Spring Boot (backend)
     Static HTML      JSON         Database + Logic
```

---

## 🆘 Najczęstsze problemy

### Problem 1: "Module not found"
```bash
# Rozwiązanie: Install dependencies
npm install
```

### Problem 2: Port już zajęty
```bash
# Zmień port w astro.config.mjs
export default defineConfig({
    server: { port: 3000 }
});
```

### Problem 3: CSS nie działa
```astro
<!-- Użyj <style> tag INSIDE .astro file -->
<style>
    /* CSS działa tylko w tym komponencie (scoped) */
</style>

<!-- LUB globalnie -->
<style is:global>
    /* CSS działa globalnie */
</style>
```

---

## 🚀 Build i Deploy

```bash
# Development
npm run dev              # Jak ./gradlew bootRun

# Build (production)
npm run build            # Jak ./gradlew build
# → Tworzy dist/ folder ze static files

# Preview production build
npm run preview

# Deploy do Netlify (jednym klikiem!)
# 1. Push do GitHub
# 2. Import w Netlify
# 3. Build command: npm run build
# 4. Publish directory: dist
```

---

## 📖 Resursy do nauki

1. **Official Docs:** https://docs.astro.build
2. **Tutorial:** https://docs.astro.build/en/tutorial/0-introduction/
3. **Examples:** https://github.com/withastro/astro/tree/main/examples
4. **Discord Community:** https://astro.build/chat
5. **Video Course:** https://egghead.io/courses/build-a-blog-with-astro

---

Gotowy na pierwszy projekt? Zacznij od bloga (ćwiczenie demonstracyjne powyżej) i stopniowo dodawaj features. Masz pytania? Pytaj! 🚀

