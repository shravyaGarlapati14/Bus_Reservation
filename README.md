# Bus Reservation System 🚌

A web-based bus ticket booking app with user signup/login, search & booking, booking history, and basic admin views.  
Built with **Java Servlets/JSP**, **MySQL**, and **Maven**, and runs locally via **Jetty** (no separate Tomcat install required).

> **Note:** This repository is set up for local development with a `.env` file for database configuration and Java 17 compilation.

---

## Features

- **Users:** Registration, login, password reset, booking history  
- **Bookings:** Search buses, create/view tickets  
- **Admin:** View all tickets, basic user oversight  
- **UX:** Responsive JSP pages with simple styling

---

## Tech Stack

- **Backend:** Java 17, Servlets/JSP (Servlet API 4.0)
- **Build/Run:** Maven, Jetty Maven Plugin
- **Database:** MySQL 8.x (JDBC)
- **Libs:** JSTL, iText (PDF)

---

## Prerequisites

- **Java 17 (JDK)**
- **Maven 3.8+**
- **MySQL 8.x**

Verify:
```bash
java -version
mvn -version
mysql --version
Database Setup
Create the DB and load seed data:

mysql -u root -p -e "CREATE DATABASE bus_reservation CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
mysql -u root -p bus_reservation < bus_reservation.sql
# optional check
mysql -u root -p -e "USE bus_reservation; SHOW TABLES;"
Configuration (Environment Variables)
Create a .env file in the project root:

DB_URL=jdbc:mysql://localhost:3306/bus_reservation?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
DB_USER=root
DB_PASS=your_mysql_password
.env is git-ignored. The app reads DB_URL, DB_USER, DB_PASS via environment variables or JVM properties.
If not provided, sensible defaults are used (localhost DB, user root, empty password).

PowerShell (local only) – load .env for the current terminal session:

Get-Content .env | ForEach-Object {
  if ($_ -match '^\s*#' -or $_ -notmatch '=') { return }
  $name,$value = $_ -split '=',2
  [System.Environment]::SetEnvironmentVariable($name.Trim(), $value.Trim(), 'Process')
}
(Alternatively, pass them as JVM properties when running Maven):

mvn -DDB_URL="jdbc:mysql://localhost:3306/bus_reservation?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC" \
    -DDB_USER=root -DDB_PASS="your_mysql_password" jetty:run
Run Locally (Jetty)
mvn clean package -DskipTests
mvn jetty:run
Open: http://localhost:8080/Bus_Reservation

If port 8080 is busy:

mvn -Djetty.http.port=8081 jetty:run
# then open http://localhost:8081/Bus_Reservation
Build a WAR (optional, for Tomcat 9+)
mvn clean package
Deploy target/Bus_Ticketing_System.war to your Tomcat webapps/ directory and start Tomcat.

Troubleshooting
Communications link failure / cannot connect to MySQL

Ensure MySQL service is running (Windows services.msc → MySQL80)

Verify DB_URL, DB_USER, DB_PASS

Use the provided flags in DB_URL: useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC

Access denied for user

Credentials incorrect; test via:

mysql -u root -p
ClassNotFoundException: com.mysql.cj.jdbc.Driver

Confirm mysql-connector-java is in pom.xml and rerun mvn clean package.

Port already in use (8080)

Run Jetty on a different port:

bash
Copy code
mvn -Djetty.http.port=8081 jetty:run
Duplicate JDBC jars warnings

Remove any ojdbc*.jar from src/main/webapp/WEB-INF/lib/ and rely on Maven-managed dependencies.

Security Notes
Do not commit secrets. .env is ignored by default.

For production hardening, add password hashing (e.g., BCrypt) and a JDBC connection pool.

Roadmap / Nice-to-haves
Password hashing & input validation

DAO/Service refactor + HikariCP pooling

Better error pages and logging

Role-based access control for admin pages

Docker Compose (app + MySQL)

