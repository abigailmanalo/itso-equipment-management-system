# ITSO Equipment Management System

A web-based system for managing, borrowing, and reserving IT equipment, built for the **Institute of Technology Services Office (ITSO)** at **FEU Institute of Technology**.

Students and associates can request equipment online. ITSO personnel can track inventory, process requests and returns, and export reports.

> Developed for academic fulfillment at FEU Institute of Technology, **BSITWMA Program**.

---

## Table of Contents

- [Features](#features)
- [User Roles](#user-roles)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Database](#database)
- [Getting Started](#getting-started)
- [Routes Overview](#routes-overview)
- [Development Team](#development-team)
- [License](#license)

---

## Features

### Accounts and Authentication
- Register and log in with an ID number and password
- Email verification on registration
- Forgot password and reset password through an emailed link that expires
- Profile page: upload or remove a profile picture, change password
- Role-based redirects and menus after login

### Equipment Management
- Add, view, edit, deactivate, and reactivate equipment
- Track **total quantity** and **available count** for each item
- **Equipment bundles**: accessories are linked to a main item (for example, a laptop includes a charger). Borrowing or returning the main item updates the accessories' stock automatically.
- Active and unusable equipment lists

### Borrowing
- Students and associates submit borrow requests (equipment, room, date, time)
- ITSO personnel mark requests as **received** and **returned**
- Stock is checked so inactive or unavailable equipment can't be borrowed
- Borrowing history

### Reservations (Associates)
- Reserve equipment for a future date and time
- Cancel or reschedule a reservation
- ITSO personnel mark reservations as received and returned

### Email Notifications
Sent over SMTP for:
- Account verification and password reset
- Borrow confirmation and return confirmation
- Reservation confirmation, return, cancellation, and reschedule

### Reports
- Active equipment report
- Unusable equipment report
- Borrowing history report
- Each report can be **exported as CSV**

### User Management (ITSO Personnel)
- Add, view, edit, and delete user accounts
- Assign roles

---

## User Roles

| Role | Access |
|---|---|
| **ITSO Personnel** | Dashboard, user management, equipment management, reservations, borrowed and returned items, reports |
| **Associate** | Dashboard, view equipment, borrow equipment, **reserve** equipment |
| **Student** | Dashboard, view equipment, borrow equipment |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | PHP 8.1+, [CodeIgniter 4](https://codeigniter.com) (MVC) |
| Database | MySQL / MariaDB (MySQLi driver) |
| Frontend | HTML, CSS, JavaScript, Bootstrap 5.3, Bootstrap Icons |
| Email | CodeIgniter Email library over SMTP (Gmail) |
| Local server | XAMPP (Apache + MySQL) |

---

## Project Structure

```
PixelCore/
├── app/
│   ├── Config/          # App, database, email, routes, and validation config
│   ├── Controllers/     # Auth, Dashboard, Users, Equipment, Borrow, Reserve, Reports
│   ├── Models/          # UserModel, EquipmentModel, EquipmentBundleModel, BorrowModel, ReserveModel
│   ├── Services/        # EmailService (all email notifications)
│   └── Views/           # Pages grouped by module (borrow/, reserve/, equipments/, users/, include/)
├── public/              # Front controller, CSS, JS, images, uploaded profile pictures
├── system/              # CodeIgniter 4 framework core
├── writable/            # Logs, cache, sessions (not committed)
├── itso.sql             # Database schema and starter data
└── .env                 # Local environment settings (not committed)
```

---

## Database

The schema is in [`itso.sql`](itso.sql) and creates these tables:

| Table | Purpose |
|---|---|
| `tblusers` | Accounts, roles, verification and password-reset tokens |
| `tblequipment` | Equipment items, quantity, available count, status |
| `tblequipment_bundles` | Links main equipment to its accessories |
| `tblborrows` | Borrow requests and their status |
| `tblreservations` | Reservations and their status |

The script includes starter equipment and bundle data. It does **not** include any user accounts.

---

## Getting Started

### Requirements
- [XAMPP](https://www.apachefriends.org/) (or any Apache + PHP 8.1+ + MySQL setup)
- PHP extensions: `intl`, `mbstring`, `mysqli`
- A Gmail account with an [App Password](https://support.google.com/accounts/answer/185833), for sending emails

### 1. Clone the repository
Clone it into XAMPP's `htdocs` folder under the name `PixelCore`:

```bash
cd C:/xampp/htdocs
git clone https://github.com/abigailmanalo/itso-equipment-management-system.git PixelCore
```

> If you use a different folder name, update `$baseURL` in `app/Config/App.php`. The default is `http://localhost/PixelCore/`.

### 2. Set up the database
1. Start **Apache** and **MySQL** in the XAMPP Control Panel.
2. Open [phpMyAdmin](http://localhost/phpmyadmin) and create a database named **`itso`**.
3. Select the `itso` database, go to **Import**, and import `itso.sql`.

The default connection settings (`localhost`, user `root`, no password) are in `app/Config/Database.php`.

### 3. Configure the environment
Create a file named `.env` in the project root:

```ini
CI_ENVIRONMENT = development

email.SMTPUser = your-email@gmail.com
email.SMTPPass = your-gmail-app-password
```

> Never commit `.env`. It's already listed in `.gitignore`.

### 4. Run the app
Open **http://localhost/PixelCore/** in your browser.

### 5. Create your first account
Go to **/register** and sign up, then sign in at **/login** with your ID number and password.

- The **first account** registered on a fresh install becomes **ITSO Personnel** (admin).
- Every sign-up after that is a **Student**.
- ITSO Personnel can change a user's role (Student, Associate, or ITSO Personnel) from **Users → Edit**.

---

## Routes Overview

| Module | Main routes |
|---|---|
| Auth | `/login`, `/register`, `/logout`, `/forgot-password`, `/reset-password/{token}` |
| Profile | `/profile`, `/profile/change-password` |
| Dashboard | `/dashboard`, `/equipments`, `/reservations`, `/borrowed`, `/returned` |
| Users | `/users`, `/users/add`, `/users/view/{id}`, `/users/edit/{id}` |
| Equipment | `/equipment`, `/equipment/add`, `/equipment/view/{id}`, `/equipment/edit/{id}` |
| Borrow | `/borrow`, `/borrow/received/{id}`, `/borrow/returned/{id}` |
| Reserve | `/reserve`, `/reserve/cancel/{id}`, `/reserve/reschedule/{id}` |
| Reports | `/reports`, `/active-equipment`, `/unusable-equipment`, `/borrowing-history` |

The full list is in [`app/Config/Routes.php`](app/Config/Routes.php).

---

## Development Team

**Team PixelCore**, BSITWMA, FEU Institute of Technology

| Name | Role |
|---|---|
| [Abigail Manalo](https://github.com/abigailmanalo) | Back-end Developer |
| Hazel Mito| Front-end Developer |
| Ria Cruz | Leader |

---

## License

This project was created for academic purposes at FEU Institute of Technology.

It is built on the [CodeIgniter 4](https://codeigniter.com) framework, which is licensed under the MIT License. See [`LICENSE`](LICENSE).
