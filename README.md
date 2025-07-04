# 🚀 Lunarwave Backend - A Disboard-like Server Listing API

Welcome to the **Lunarwave Backend**!  
This project provides a robust API for building a Discord server listing website, similar to Disboard. It handles:

- User authentication via Discord  
- Server submission and management  
- Reviews & bumping  
- Admin functionalities  
- Giveaways  
- AI-powered description generation via Gemini  

This backend is open-source, allowing developers to build custom frontend experiences on top of it.

---

## ✨ Features

### 🔐 Authentication
- Discord OAuth2 login via Passport.js
- Secure session management

### 🧑 User Profiles
- Custom backgrounds, banners, tags, links
- Public profile viewing and reactions (👍 / 👎)

### 📌 Server Submission & Listing
- Server submission with admin approval
- Sorting: Sponsored, Bumped, Online count
- Review system (1–5 stars)
- Detailed server pages with custom elements

### 🛠️ Server Management (Owner)
- Update server info, invite links, NSFW flag
- Edit custom pages
- Delete your submitted servers

### 🔼 Bumping System
- Bump cooldown enforced
- Moves server to top of listing

### ⭐ Reviews & Ratings
- Users can rate & review any listed server
- View server’s average rating

### 🛡️ Admin Panel
- Approve/Reject/Delete servers
- Ban/unban users & servers
- Sponsor/verify servers
- Admin permissions and super admin role
- Send messages and announcements

### 🎉 Giveaways
- Host public giveaways with optional password protection
- Auto end system and winner selection
- Admin + host permissions for editing/deleting

### 🧠 AI Description Generator
- Uses Gemini API to generate server descriptions based on prompts

### 🔁 Background Jobs
- Server member count updater
- Sponsored expiry checker
- Giveaway auto-ender

### 💾 Local JSON Database
- File-based persistent storage
- Zero external database required

---

## 🛠️ Technologies Used

- **Node.js** – Backend runtime  
- **Express.js** – HTTP server  
- **Passport.js** – Authentication middleware  
- **Passport-Discord** – OAuth2 strategy  
- **express-session** – Session storage  
- **cors** – CORS handling  
- **node-fetch** – Lightweight HTTP client  
- **fs** – JSON file handling  
- **Gemini API** – For AI text generation  

---

## 🚀 Getting Started

### 📦 Prerequisites

- Node.js (LTS recommended)  
- npm (comes with Node)  
- Git  

---

### ⚙️ Configuration

#### 1. Create a Discord Application

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications)  
2. Click **"New Application"** → Name it (e.g. *Lunarwave*)  
3. Under **OAuth2 → General**:
   - Add `http://localhost:3000/callback` as a redirect
   - Copy **Client ID** and **Client Secret**  
4. Under **Bot**:
   - Add Bot
   - Enable: `Presence Intent`, `Server Members Intent`, `Message Content Intent`
   - Copy your **Bot Token**

#### 2. Generate a Session Secret
Use [randomkeygen.com](https://randomkeygen.com) to generate a secure string.

#### 3. (Optional) Get a Gemini API Key
Visit [Google AI Studio](https://makersuite.google.com/app) to generate your key.

---

### 🔧 Edit Configuration

Inside `api.js`, edit the `config` object:

```js
const config = {
  clientID: 'YOUR_DISCORD_CLIENT_ID',
  clientSecret: 'YOUR_DISCORD_CLIENT_SECRET',
  callbackURL: 'http://localhost:3000/callback',
  botToken: 'YOUR_DISCORD_BOT_TOKEN',
  sessionSecret: 'YOUR_GENERATED_SESSION_SECRET',
  geminiApiKey: 'YOUR_GEMINI_API_KEY_OPTIONAL'
};

const SUPER_ADMIN_ID = 'YOUR_DISCORD_USER_ID'; // Enable Developer Mode > Right-click Profile > Copy ID
```

---

## 🗂️ JSON Database Files

These are auto-generated in the `./db` folder on first run:

- `profiles.json`  
- `admins.json`  
- `servers.json`  
- `pending.json`  
- `reports.json`  
- `chat.json` *(unused)*  
- `lastBumps.json`  
- `reviews.json`  
- `serverPages.json`  
- `sponsoredServers.json`  
- `verifiedServers.json`  
- `bannedServers.json`  
- `bannedUsers.json`  
- `inboxMessages.json`  
- `announcements.json`  
- `giveaways.json`  

---

## 🏁 Running the Server

Start the backend:

```bash
node api.js
```

Console output:

```
Initializing database files...
[FILE_IO] File not found: ./db/profiles.json. Initializing...
...
🚀 Lunarwave backend is live on port 3000
🔗 Access at http://localhost:3000
```

---

## 🌐 API Endpoints

### 🔑 Authentication
- `GET /login` – Start Discord login
- `GET /callback` – OAuth2 callback
- `GET /logout` – Logout

### 👤 User & Guild Info
- `GET /api/user` – Logged-in user data & status  
- `GET /api/user/guilds` – Guilds user manages  
- `GET /api/bot/:id` – Bot’s guild info  

### 👥 Profiles
- `GET /api/profiles` – All public profiles  
- `GET /api/profile/:userId` – Specific user profile  
- `POST /api/profiles/add` – Create/update profile  
- `DELETE /api/profiles/delete` – Delete profile  
- `POST /api/profiles/react/:userId` – React to profile  

### 📜 Server Management
- `GET /api/servers` – All approved servers  
- `GET /api/myservers` – User's submitted servers  
- `POST /api/submit` – Submit a new server  
- `PUT /api/server/:id` – Update server info  
- `DELETE /api/myserver/delete/:id` – Delete user’s server  

### ⭐ Reviews & Bumps
- `GET /api/server/:id/reviews` – Server reviews  
- `POST /api/server/:id/review` – Submit a review  
- `GET /api/server/:id/myreview` – Get user’s review  
- `POST /api/bump/:id` – Bump server  
- `GET /api/reports` – All reports (Admin only)  

### 🛡️ Admin Panel
- `GET /api/admins`  
- `POST /api/admin/add` / `remove`  
- `GET /api/pending`  
- `POST /api/approve/:id`  
- `DELETE /api/admin/pending/delete/:id`  
- `DELETE /api/admin/server/delete/:id`  
- `POST /api/admin/sponsor/:id` / `verify/:id`  
- `DELETE /api/admin/sponsor/:id` / `verify/:id`  
- `POST /api/admin/ban/server/:id` / `user/:id`  
- `DELETE /api/admin/unban/server/:id` / `user/:id`  

### 📥 Inbox & Announcements
- `POST /api/admin/message` – DM a user  
- `POST /api/admin/announce` – Create announcement  
- `GET /api/inbox/messages` / `announcements`  
- `POST /api/inbox/read-message/:messageId`  
- `POST /api/inbox/read-announcement/:announcementId`  
- `GET /api/inbox/unread-count`  

### 🎁 Giveaways
- `GET /api/giveaways` / `giveaway/:id`  
- `POST /api/giveaways/create`  
- `POST /api/giveaway/:id/enter`  
- `POST /api/giveaway/:id/end`  
- `PUT /api/giveaway/:id/edit`  
- `DELETE /api/giveaway/:id/delete`  

### 🖼️ Server Pages & AI
- `GET /api/server-page/:id`  
- `POST /api/server-page/:id/save` / `reset`  
- `POST /api/generate-description`  

### 🧾 Static Routes
- `/` → `public/index.html`  
- `/profile/:userId` → `public/profile-view.html`  
- `/server/:id` → `public/defaultpage.html`  
- All others → `public/404.html`  

---

## 🖼️ Frontend Integration

This project includes **backend only**.  
You can build your frontend using **React**, **Vue**, or even **vanilla HTML/CSS/JS** that consumes this API.  
Simple frontend templates may be provided in the future.

---

## 🤝 Contributing

Pull requests, issues, and ideas are welcome!

```bash
1. Fork the repo  
2. Create a new branch: git checkout -b feature/your-feature  
3. Commit changes: git commit -m "Added awesome feature"  
4. Push branch: git push origin feature/your-feature  
5. Open a Pull Request
```

---

## 📄 License

Licensed under the **MIT License**.  
Feel free to use, modify, and share freely.

---

## 🙏 Acknowledgements

- Inspired by [Disboard](https://disboard.org)  
