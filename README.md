# CounselFlow

### Smart College Counseling & Appointment Management System

CounselFlow is a Django-based web application designed to streamline counseling appointment management in colleges and educational institutions.

The system allows students to book counseling sessions, counselors to manage appointments and maintain counseling records, and principals to monitor counselors through reports, analytics, and intelligent insights.

---

## ✨ Features

### 🎓 Student Portal

* Student login
* View available counseling sessions
* Book counseling appointments
* Select department and academic year
* Normal appointment booking
* Emergency appointment booking
* Automatic emergency appointment prioritization
* Automatic rescheduling of affected appointments
* Email notification when an appointment is rescheduled
* View booking information
* Student session history

### 👨‍🏫 Counselor Portal

* Secure counselor authentication
* Counselor dashboard
* Create counseling slots
* Automatically generate appointment sessions
* View upcoming bookings
* View complete counseling history
* Mark appointments as attended
* Add counseling remarks
* Search students by name
* View a student's complete counseling history
* Access previous session remarks and appointment information

### 👑 Principal Portal

* Secure principal authentication
* Principal dashboard
* Add counselors
* Remove counselors
* View counselor performance reports
* View bookings handled by each counselor
* Advanced analytics
* Filter analytics by counselor
* Attendance analytics
* Emergency booking analytics
* Booking statistics
* Smart insights about counseling activity

---

## 🚨 Emergency Booking System

CounselFlow includes an intelligent emergency appointment system.

When a student requests an emergency appointment:

1. The system searches for the nearest available counseling session.
2. If the session is free, the emergency appointment is assigned immediately.
3. If the session contains another emergency appointment, it is not displaced.
4. If the session contains a normal appointment, the system attempts to reschedule that appointment.
5. The system first searches for a later available session.
6. If no suitable session is available on the same day, it searches future days.
7. The affected student receives an email notification containing the new appointment details.
8. The emergency appointment receives the higher-priority slot.

This allows emergency appointments to be handled without unnecessarily displacing other emergency appointments.

---

## 📧 Email Notifications

When an existing appointment is rescheduled because of an emergency booking, CounselFlow automatically sends an email to the affected student.

The notification includes:

* Previous appointment date
* Previous appointment time
* New appointment date
* New appointment time
* Reason for rescheduling

---

## 📊 Analytics & Smart Insights

The principal portal provides an analytics layer for monitoring counseling activity.

Administrators can analyze:

* Total appointments
* Completed appointments
* Pending appointments
* Emergency appointments
* Regular appointments
* Counselor-wise booking statistics
* Counselor attendance statistics
* Appointment trends
* Counselor workload
* Counseling activity patterns

The Smart Insights section transforms raw appointment data into useful observations that can help administrators identify workload patterns and counseling trends.

---

## 🗂️ Counseling Records

Each student has a persistent profile containing their counseling information.

Counselors can search for students by name and access:

* Student information
* Previous appointments
* Appointment dates
* Appointment times
* Department
* Academic year
* Attendance status
* Counselor remarks
* Previous counseling sessions

This provides counselors with historical context before conducting a new counseling session.

---

## 🔐 Role-Based Access

CounselFlow uses role-based authentication to separate access between different users.

| Role      | Access                                        |
| --------- | --------------------------------------------- |
| Student   | Booking & personal appointment information    |
| Counselor | Slots, bookings, counseling records & remarks |
| Principal | Counselor management, reports & analytics     |

Unauthorized users cannot access protected counselor or principal functionality.

---

## 🛠️ Technology Stack

### Backend

* Python
* Django
* Django ORM
* SQLite

### Frontend

* HTML5
* CSS3
* JavaScript
* Bootstrap 5
* Chart.js

### Authentication

* Django Authentication System
* Role-based access using Django Groups
* Session-based student authentication

### Communication

* SMTP
* Email notifications for appointment rescheduling

---

## 🏗️ System Architecture

```text
                    ┌─────────────────────┐
                    │      Students       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   CounselFlow Web   │
                    │      Interface      │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
        Student Portal   Counselor Portal   Principal Portal
              │                │                │
              └────────────────┼────────────────┘
                               ▼
                    ┌─────────────────────┐
                    │    Django Backend   │
                    └──────────┬──────────┘
                               │
             ┌─────────────────┼──────────────────┐
             │                 │                  │
             ▼                 ▼                  ▼
        Booking Engine    Student Records    Analytics
             │
             ▼
       Emergency Scheduling
             │
             ▼
      Email Notification
```

---

## 📁 Project Structure

```text
counselflow/
│
├── booking/
│   ├── migrations/
│   ├── templates/
│   │   └── booking/
│   ├── admin.py
│   ├── forms.py
│   ├── models.py
│   ├── urls.py
│   └── views.py
│
├── college_counsel/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── manage.py
├── db.sqlite3
└── README.md
```

---

## 🚀 Installation

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/counselflow.git
cd counselflow
```

### 2. Create a virtual environment

```bash
python3 -m venv venv
```

### 3. Activate the virtual environment

Linux/macOS:

```bash
source venv/bin/activate
```

Windows:

```bash
venv\Scripts\activate
```

### 4. Install dependencies

```bash
pip install django
```

If the project contains a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

### 5. Apply migrations

```bash
python3 manage.py migrate
```

### 6. Create an administrator

```bash
python3 manage.py createsuperuser
```

Follow the prompts to create the administrator account.

### 7. Start the development server

```bash
python3 manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/
```

---

## ⚙️ Initial Configuration

Before using the system, configure the required Django groups:

```text
Principal
Counselor
```

Counselor accounts should belong to the `Counselor` group, while administrative accounts should belong to the `Principal` group.

Email configuration should also be added to `settings.py` for appointment rescheduling notifications.

Example:

```python
EMAIL_BACKEND = "django.core.mail.backends.smtp.EmailBackend"

EMAIL_HOST = "smtp.gmail.com"
EMAIL_PORT = 587
EMAIL_USE_TLS = True

EMAIL_HOST_USER = "your-email@gmail.com"
EMAIL_HOST_PASSWORD = "your-app-password"

DEFAULT_FROM_EMAIL = EMAIL_HOST_USER
```

For Gmail, use a Google **App Password** rather than your normal Gmail password.

---

## 🔄 Appointment Workflow

```text
Student
   │
   ▼
Login
   │
   ▼
View Available Sessions
   │
   ├───────────────┐
   │               │
 Normal         Emergency
   │               │
   ▼               ▼
Book Slot     Find Nearest Slot
                   │
            ┌──────┴──────┐
            │             │
           Free         Occupied
            │             │
            ▼             ▼
        Assign       Check Booking
                          │
                 ┌────────┴────────┐
                 │                 │
             Emergency           Normal
                 │                 │
                 ▼                 ▼
               Skip            Reschedule
                                   │
                                   ▼
                             Send Email
                                   │
                                   ▼
                           Assign Emergency
```

---

## 🔮 Future Improvements

Potential future improvements include:

* SMS notifications
* Calendar integration
* Counselor availability management
* Student self-service appointment cancellation
* Appointment reminders
* PDF counseling reports
* Export analytics to Excel/CSV
* PostgreSQL production database
* REST API
* Mobile application
* AI-assisted counseling insights
* Automated appointment recommendations
* Multi-college support

---

## 🔒 Security Considerations

For production deployment:

* Use environment variables for secrets
* Never commit email passwords
* Use PostgreSQL instead of SQLite
* Configure HTTPS
* Set `DEBUG = False`
* Configure `ALLOWED_HOSTS`
* Use secure cookies
* Configure CSRF protection
* Store sensitive configuration outside the repository

---

## 🎯 Project Objective

The goal of CounselFlow is to replace fragmented manual counseling appointment management with a centralized digital system.

Instead of maintaining separate records, appointment schedules, and counseling histories, CounselFlow brings them together into a single platform with:

**Booking + Scheduling + Emergency Management + Student Records + Counselor Management + Analytics**

---

## 👨‍💻 Author

**Stijin Earnest Abraham**

Developed as an academic software project focused on applying Django, database management, authentication, scheduling logic, and analytics to a real-world educational workflow.

---

## 📄 License

This project is intended primarily for educational and academic purposes.

You may modify and extend the project for learning and development.
