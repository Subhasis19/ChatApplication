# Realtime Chat Application

A lightweight realtime chat application built with PHP, MySQL (MariaDB), and vanilla JavaScript. It provides user registration/login, profile images, and real-time messaging using AJAX calls.

**Status:** Ready for local development (Windows / XAMPP tested)

**Quick links:**
- Code entry: `index.php` (signup) and `login.php`
- Main app: `chat.php`
- Database schema: `ChatApp/chatapp.sql`

**Table of contents**
- Overview
- Features
- Tech stack
- Prerequisites
- Installation (Windows / XAMPP)
- Database setup
- Configuration
- Running the app
- Project layout (key files)
- Security & hardening notes
- Troubleshooting
- Contributing
- License & Contact

## Overview

This project is a minimal realtime chat application that demonstrates a full-stack PHP + MySQL workflow with frontend JavaScript for asynchronous interactions. It is intended as a starter project or a teaching example.

## Features

- User registration (with profile image upload)
- User login and session-based authentication
- User list and search
- One-to-one realtime chat using periodic AJAX polling
- Message persistence in MySQL (messages and users tables)

## Tech stack

- Frontend: HTML, CSS, JavaScript (vanilla) and Font Awesome for icons
- Backend: PHP (original codebase targets PHP 7.3+)
- Database: MySQL / MariaDB
- Tested environment: XAMPP on Windows (Apache + MySQL)

## Prerequisites

- Windows with XAMPP installed (or any environment with Apache + PHP + MySQL)
- PHP 7.3 or newer recommended
- MySQL / MariaDB
- A modern browser (Chrome, Firefox, Edge)

## Installation (Windows / XAMPP)

1. Clone or download this repository and place the `ChatApp` folder inside your XAMPP `htdocs` directory. Example path:

   `C:\xampp\htdocs\ChatApp`

2. Start Apache and MySQL from the XAMPP Control Panel.

3. Import the database schema:

   - Open `http://localhost/phpmyadmin`
   - Create a new database named `chatapp` (collation `utf8mb4_general_ci` works)
   - Select the new database, then use the **Import** tab to upload and import `ChatApp/chatapp.sql`

4. Ensure the file upload directory is writable by the web server:

   - The project saves uploaded profile images to `ChatApp/php/images`.
   - On Windows with XAMPP this typically works out-of-the-box. On Linux adjust permissions (e.g., `chown -R www-data:www-data php/images` and `chmod -R 755 php/images`).

5. Configure database connection if needed (default uses root / no password):

   - Open `ChatApp/php/config.php` and update the following variables if your setup differs:

     ```php
     $hostname = 'localhost';
     $username = 'root';
     $password = '';
     $dbname   = 'chatapp';
     ```

6. Open the application in your browser:

   `http://localhost/ChatApp/`

   - Use the Signup page to register a new user, then log in and start chatting.

## Database setup (schema)

- The schema is provided in `ChatApp/chatapp.sql` and creates two tables:
  - `users` — user profiles and authentication data
  - `messages` — persisted chat messages

Import this file into your `chatapp` database using phpMyAdmin or the `mysql` CLI:

```powershell
# Example using mysql CLI (from XAMPP's MySQL/bin folder):
mysql -u root -p chatapp < "C:\xampp\htdocs\ChatApp\chatapp.sql"
```

## Configuration

- Database connection is in `ChatApp/php/config.php`.
- Uploaded images are stored under `ChatApp/php/images/` and referenced in markup as `php/images/<image>`.
- AJAX endpoints used by the frontend live in `ChatApp/php/`:
  - `signup.php`, `login.php`, `logout.php`
  - `get-chat.php`, `insert-chat.php`
  - `users.php`, `search.php`

## Running & Usage

1. Start XAMPP services (Apache + MySQL).
2. Navigate to `http://localhost/ChatApp/`.
3. Register a user (upload an avatar image) and then log in.
4. From the users list, open a chat with someone and send messages.

Note: The application uses AJAX polling (via `javascript/chat.js`) to refresh messages.

## Project layout (key files)

- `index.php` — Signup page
- `login.php` — Login page
- `chat.php` — Conversation UI
- `users.php` — People / user list
- `php/config.php` — Database connection settings
- `php/signup.php`, `php/login.php` — Authentication endpoints
- `php/get-chat.php`, `php/insert-chat.php` — Messaging endpoints
- `chatapp.sql` — Database schema
- `javascript/` — Client-side scripts (signup, login, chat, users)
- `style.css` — Styling for pages

## Security & Hardening Notes

This project is a learning/demo application. Before using in production, consider the following improvements:

- Do not use the `root` database account for the app — create a dedicated MySQL user with limited privileges.
- Replace `md5` password hashing with PHP's `password_hash()` and `password_verify()` (bcrypt/Argon2).
- Validate and sanitize all inputs on the server-side (current code uses `mysqli_real_escape_string`, but more checks are recommended).
- Add CSRF protection to forms and AJAX endpoints.
- Restrict upload types and scan/validate images more robustly.
- Use HTTPS in any real deployment.

