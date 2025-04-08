<a id="readme-top"></a>
<p align="left"><a href="../README.md">Back to main readme</a></p>

## 📦 TypeScript / Node.js Project Structure & Best Practices

TypeScript brings strong typing and better tooling to JavaScript projects. Here's how to structure and scale a clean backend using TypeScript with Node.js.

---

### Table of content

- [📦 TypeScript / Node.js Project Structure \& Best Practices](#-typescript--nodejs-project-structure--best-practices)
  - [Table of content](#table-of-content)
  - [✅ 1. Best Practices](#-1-best-practices)
  - [📛 2. Naming Conventions](#-2-naming-conventions)
  - [📁 3. Folder Structure](#-3-folder-structure)
  - [🧪 4. Testing (Optional)](#-4-testing-optional)
  - [📚 5. Recommended Tools](#-5-recommended-tools)
- [(back to top)](#back-to-top)
  - [⚙️ 6. tsconfig.json Tips](#️-6-tsconfigjson-tips)

---

### ✅ 1. Best Practices

- Use TypeScript strictly: enable strict mode in `tsconfig.json`.
- Use `interfaces` or `types` to define data contracts.
- Split logic into layers: controller, service, model, etc.
- Avoid using `any` – prefer proper typing or `unknown`.
- Use aliases (with `paths` in `tsconfig`) to avoid deep relative imports.
- Use a linter (`eslint`) with TypeScript support.
- Use async/await and centralized error handling.
- Always compile with `tsc` before deploying – don’t run `.ts` files directly in production.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### 📛 2. Naming Conventions

| Type              | Convention      | Example                     |
|-------------------|-----------------|-----------------------------|
| Files & Folders   | `kebab-case`    | `user-service.ts`           |
| Variables         | `camelCase`     | `userId`, `totalPrice`      |
| Classes           | `PascalCase`    | `UserController`            |
| Interfaces        | `PascalCase` prefixed with `I` | `IUser`, `IService` |
| Enums             | `PascalCase`    | `UserRole`                  |
| Constants         | `UPPER_SNAKE`   | `DEFAULT_TIMEOUT`           |
| Functions         | `camelCase`     | `getUserById()`             |

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### 📁 3. Folder Structure

A clean, scalable structure for TypeScript projects:

```bash
project-root/
├── src/
│   ├── controllers/         # Route handlers
│   │   └── user.controller.ts
│   │
│   ├── routes/              # Route definitions
│   │   └── user.routes.ts
│   │
│   ├── services/            # Business logic
│   │   └── user.service.ts
│   │
│   ├── models/              # Data models & interfaces
│   │   └── user.model.ts
│   │
│   ├── middlewares/         # Express middlewares
│   │   └── auth.middleware.ts
│   │
│   ├── utils/               # Utility functions
│   │   └── logger.ts
│   │
│   ├── config/              # Configuration files
│   │   └── db.config.ts
│   │
│   ├── types/               # Global/shared TypeScript types
│   │   └── index.d.ts
│   │
│   └── app.ts               # Express app setup
│
├── tests/                   # Unit & integration tests
│   └── user.test.ts
│
├── .env
├── .gitignore
├── package.json
├── tsconfig.json
├── README.md
└── server.ts                # Entry point
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### 🧪 4. Testing (Optional)
```bash
tests/
├── unit/
│   └── user.service.test.ts
├── integration/
│   └── auth.flow.test.ts
```
✅ Use ts-jest or vitest for type-aware testing.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

--- 

### 📚 5. Recommended Tools
- Compiler: typescript
  - Linting: eslint, @typescript-eslint
  - Testing: jest, ts-jest, vitest
  - Validation: zod, class-validator
  - Logging: winston, pino
  - Environment: dotenv
  - Dev Utils: ts-node, nodemon, tsx

<p align="right">(<a href="#readme-top">back to top</a>)</p>
--- 

### ⚙️ 6. tsconfig.json Tips
Make sure you have a solid tsconfig.json for a production-ready setup:
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true,
    "resolveJsonModule": true,
    "baseUrl": "src",
    "paths": {
      "@controllers/*": ["controllers/*"],
      "@services/*": ["services/*"]
    }
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```
💡 Use baseUrl + paths to clean up imports like ../../../.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

Following this structure will help you scale, maintain, and onboard new devs quickly in any TypeScript Node.js backend project. 🚀

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---
