# 🎉 React Events — Event Creation and Management Application

<p align="left">
  <img alt="c-sharp" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/csharp/csharp.png" />
  <img alt="dotnet" width="26px" src="https://raw.githubusercontent.com/github/explore/93d8a67084f94b2a444e510199a6e7622e5b09a3/topics/dotnet/dotnet.png" />
  <img alt="React" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/react/react.png" />
  <img alt="TypeScript" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/typescript/typescript.png" />
  <img alt="Node.js" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/nodejs/nodejs.png" />
  <img alt="PostgreSQL" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/postgresql/postgresql.png" />
  <img alt="docker" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/docker/docker.png" />
</p>

A full-stack, social-media-style **event creation and management platform**. Users can create events, RSVP/attend events, follow other users, chat live during events, and upload profile/event photos — all built on a **.NET Web API** backend (Clean Architecture + CQRS) and a **React + TypeScript** frontend.

> 🔗 **Live Demo:** [react-events-app.fly.dev](https://react-events-app.fly.dev/)

---

## 📖 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Architecture Overview](#-architecture-overview)
- [Prerequisites](#-prerequisites)
- [Getting Started (Step-by-Step)](#-getting-started-step-by-step)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Backend Setup (.NET API)](#2-backend-setup-net-api)
  - [3. Database Setup (PostgreSQL)](#3-database-setup-postgresql)
  - [4. Frontend Setup (React Client)](#4-frontend-setup-react-client)
  - [5. Running the Application](#5-running-the-application)
- [Environment Variables & Configuration](#-environment-variables--configuration)
- [Running with Docker](#-running-with-docker)
- [API Overview](#-api-overview)
- [Deployment](#-deployment)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## ✨ Features

- 📅 **Create, edit, and manage events** with details, dates, categories, and locations
- 🙋 **Attend / cancel attendance** for events, with host-only management permissions
- 👥 **Follow/unfollow other users** and view their profiles, photos, and activity
- 💬 **Live chat** on event pages powered by **SignalR**
- 🖼️ **Photo uploads** (profile & events) via **Cloudinary** integration, with in-browser cropping
- 🔐 **Authentication & Authorization** using ASP.NET Core Identity + JWT tokens
- 📜 **Infinite scroll, paging, and filtering** for browsing events
- 🏗️ **Clean Architecture** with the **CQRS + Mediator** pattern (via MediatR)
- 🐳 **Dockerized** and deployable to **Fly.io**
- 🎨 Polished UI built with **Semantic UI React**

---

## 🛠 Tech Stack

**Backend**
- ASP.NET Core Web API (C#)
- Entity Framework Core 7 (Npgsql provider for PostgreSQL)
- MediatR (CQRS pattern)
- AutoMapper
- FluentValidation
- ASP.NET Core Identity + JWT Bearer Authentication
- SignalR (real-time chat)
- Cloudinary (photo storage/CDN)
- Swagger / Swashbuckle (API docs)

**Frontend**
- React 18 + TypeScript
- MobX & MobX React Lite (state management)
- React Router v5
- Axios
- Formik + Yup (forms & validation)
- Semantic UI React
- React Cropper, React Dropzone, React Datepicker, React Infinite Scroller
- @microsoft/signalr (chat client)

**DevOps**
- Docker
- Fly.io (hosting/deployment)

---

## 🗂 Project Structure

The solution follows a **Clean Architecture** layout — each layer has a single, clear responsibility, and dependencies point inward (API → Application → Domain).

```
Event-Creation-and-Management-Application/
│
├── API/                          # Presentation layer — ASP.NET Core Web API
│   ├── Controllers/              # REST endpoints (Events, Account, Profiles, Photos, Follow...)
│   ├── DTOs/                     # Data Transfer Objects
│   ├── Extensions/                # Service/Identity/CORS configuration extensions
│   ├── Middleware/                # Global exception handling middleware
│   ├── Properties/                # launchSettings.json
│   ├── Services/                  # Supporting API services
│   ├── SignalR/                   # Real-time chat hub
│   ├── wwwroot/                   # Compiled React build output (served by API)
│   ├── Program.cs                 # App entry point, DB migration & seeding
│   ├── Startup.cs                 # Middleware pipeline & service registration
│   └── appsettings.Development.json
│
├── Application/                  # Business logic layer — CQRS Commands/Queries + Handlers
│   ├── Comments/                  # Comment features (SignalR chat)
│   ├── Core/                      # Shared helpers (Result wrapper, mapping profiles, paging)
│   ├── Events/                    # Event CRUD, attendance logic
│   ├── Followers/                 # Follow/unfollow logic
│   ├── Interfaces/                 # Abstractions (IUserAccessor, IPhotoAccessor, etc.)
│   └── Photos/                    # Photo upload/delete logic
│
├── Domain/                       # Core entities (no dependencies on other layers)
│   ├── AppUser.cs
│   ├── Comment.cs
│   ├── Event.cs
│   ├── EventAttendee.cs
│   ├── Photo.cs
│   └── UserFollowing.cs
│
├── Infrastructure/                # External services implementation
│   ├── Photos/                    # Cloudinary settings & accessor implementation
│   └── Security/                  # JWT/user access helpers, authorization requirements
│
├── Persistence/                   # Data access layer (EF Core)
│   ├── Migrations/                # EF Core database migrations
│   ├── DataContext.cs             # EF Core DbContext
│   └── Seed.cs                    # Sample/demo data seeding
│
├── client-app/                    # React + TypeScript frontend
│   ├── public/                    # Static assets
│   └── src/
│       ├── app/
│       │   ├── api/                # Axios agent & API calls
│       │   ├── common/             # Shared components/utilities
│       │   ├── layout/             # App shell, navbar, layout components
│       │   ├── models/             # TypeScript interfaces/types
│       │   └── stores/             # MobX stores (state management)
│       └── features/
│           ├── errors/              # Error pages (404, server error)
│           ├── events/              # Event list, details, form, filters
│           ├── home/                # Landing/home page
│           ├── profiles/            # User profile pages & components
│           └── users/               # Login & registration forms
│
├── Dockerfile                     # Multi-stage build for API + client
├── fly.toml                       # Fly.io deployment configuration
├── react-events-app.sln           # Visual Studio solution file
└── README.md
```

---

## 🏛 Architecture Overview

This project implements **Clean Architecture** combined with the **CQRS (Command Query Responsibility Segregation)** pattern using **MediatR**:

- **Domain** — plain entity classes with zero external dependencies.
- **Application** — contains all business logic as MediatR *Commands* (writes) and *Queries* (reads), one folder per feature (Events, Photos, Followers, Comments).
- **Infrastructure** — implements interfaces defined in `Application` for external concerns (photo storage, current-user access).
- **Persistence** — EF Core `DataContext`, migrations, and database seeding.
- **API** — thin controllers that simply dispatch MediatR requests and return results; hosts SignalR hubs and serves the compiled React app from `wwwroot`.

This separation keeps business rules independent of frameworks, databases, and UI — making the app easier to test, extend, and maintain.

---

## ✅ Prerequisites

Make sure the following are installed before you start:

| Tool | Version | Notes |
|---|---|---|
| [.NET SDK](https://dotnet.microsoft.com/download) | 8.0.x | Required to build/run the API |
| [Node.js](https://nodejs.org/) | 16.x or later | Required to run the React client |
| [PostgreSQL](https://www.postgresql.org/download/) | 13+ | Local database, or use a hosted instance |
| [Docker](https://www.docker.com/) *(optional)* | Latest | For containerized setup |
| [Cloudinary account](https://cloudinary.com/) *(optional)* | Free tier | Needed for photo upload feature |
| Git | Latest | To clone the repository |

---

## 🚀 Getting Started (Step-by-Step)

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/Event-Creation-and-Management-Application.git
cd Event-Creation-and-Management-Application
```

### 2. Backend Setup (.NET API)

Restore all solution dependencies:

```bash
dotnet restore react-events-app.sln
```

Build the solution to confirm everything compiles:

```bash
dotnet build react-events-app.sln
```

### 3. Database Setup (PostgreSQL)

1. Create a local PostgreSQL database (matching the connection string, or your own):

   ```sql
   CREATE DATABASE "react-events-app";
   CREATE USER admin WITH PASSWORD 'secret';
   GRANT ALL PRIVILEGES ON DATABASE "react-events-app" TO admin;
   ```

2. Update the connection string in `API/appsettings.Development.json` if your credentials differ:

   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Server=localhost; Port=5432; User Id=admin; Password=secret; Database=react-events-app"
     },
     "TokenKey": "super secret key"
   }
   ```

3. Migrations run **automatically** on startup (`Program.cs` calls `context.Database.MigrateAsync()` and seeds demo data), so no manual `dotnet ef database update` is required — simply starting the API is enough.

   *(Optional, if you want to run migrations manually):*

   ```bash
   dotnet tool install --global dotnet-ef
   cd Persistence
   dotnet ef database update --startup-project ../API
   ```

### 4. Frontend Setup (React Client)

```bash
cd client-app
npm install
```

### 5. Running the Application

**Run the API** (from the project root, in one terminal):

```bash
cd API
dotnet watch run
```

The API will start (by default) at `https://localhost:5001` and `http://localhost:5000`, with Swagger docs available at `/swagger`.

**Run the React client** (in a second terminal):

```bash
cd client-app
npm start
```

The client will start at `http://localhost:3000` and proxy API requests to the backend.

Open **http://localhost:3000** in your browser — you're up and running! 🎉

> 💡 **Demo login:** if you keep the default seed data (`Seed.cs`), sample users are created automatically — check `Persistence/Seed.cs` for the seeded usernames/passwords.

---

## ⚙️ Environment Variables & Configuration

The backend reads configuration from `appsettings.json` / `appsettings.Development.json` and environment variables. Key settings:

| Key | Description | Example |
|---|---|---|
| `ConnectionStrings:DefaultConnection` | PostgreSQL connection string | `Server=localhost;Port=5432;User Id=admin;Password=secret;Database=react-events-app` |
| `TokenKey` | Secret key used to sign JWT tokens | `super secret key` (⚠️ change this in production) |
| `Cloudinary:CloudName` | Cloudinary cloud name | `your-cloud-name` |
| `Cloudinary:ApiKey` | Cloudinary API key | `123456789012345` |
| `Cloudinary:ApiSecret` | Cloudinary API secret | `your-api-secret` |

Add the Cloudinary section to your `appsettings.Development.json` (not committed by default) to enable photo uploads:

```json
{
  "Cloudinary": {
    "CloudName": "your-cloud-name",
    "ApiKey": "your-api-key",
    "ApiSecret": "your-api-secret"
  }
}
```

> ⚠️ **Security note:** Never commit real secrets (`TokenKey`, Cloudinary credentials, DB passwords) to source control. Use environment variables, user-secrets, or a secrets manager in production.

---

## 🐳 Running with Docker

A multi-stage `Dockerfile` is included, which builds the .NET solution and serves the API (with the React build copied into `API/wwwroot`).

Build the client first so it's included in `wwwroot`:

```bash
cd client-app
npm run build
cd ..
```

Then build and run the Docker image:

```bash
docker build -t event-management-app .
docker run -p 8080:8080 event-management-app
```

The app will be available at **http://localhost:8080**.

---

## 📡 API Overview

The API exposes RESTful endpoints (documented via Swagger at `/swagger` in development):

| Controller | Responsibility |
|---|---|
| `AccountController` | Register, login, current user info |
| `EventsController` | CRUD for events, attendance management |
| `ProfilesController` | User profile data, photos, following list |
| `FollowController` | Follow/unfollow other users |
| `PhotosController` | Upload/delete/set-main photos |
| `BuggyController` | Error-handling test endpoints |

Real-time features (event comments/chat) are served over a **SignalR hub** rather than REST.

---

## ☁️ Deployment

This project is configured for deployment on **Fly.io** via `fly.toml` and the included `Dockerfile`.

```bash
fly launch     # first-time setup
fly deploy     # deploy updates
```

Environment secrets (Cloudinary keys, token key, DB connection) should be set using:

```bash
fly secrets set TokenKey="your-production-secret"
fly secrets set Cloudinary__ApiSecret="your-cloudinary-secret"
```

---

## 🐛 Troubleshooting

- **Database connection errors** — double check PostgreSQL is running and the connection string credentials match your local setup.
- **CORS errors in the browser** — ensure the API's CORS policy allows `http://localhost:3000` (already configured for local development).
- **Photo upload fails** — confirm valid `Cloudinary` credentials are set in `appsettings.Development.json`.
- **SignalR/chat not connecting** — verify both API and client are running and that the JWT token is valid (chat connections are authenticated).
- **Port conflicts** — change the ports in `API/Properties/launchSettings.json` or `client-app/package.json` proxy config if `5000`/`5001`/`3000` are already in use.

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

---

## 📄 License

This project is available for personal and educational use.

---

<p align="center">Made with ❤️ by AzizUllah using .NET 8, React, and TypeScript</p>
