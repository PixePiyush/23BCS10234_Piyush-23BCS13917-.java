# 23BCS10234_Piyush-23BCS13917-.java


/* ----------------------------------------------------
 * LOGIN FORM  (login.html)
 * Place in: webapp/login.html
 * ---------------------------------------------------- */
<!DOCTYPE html>
<html>
<head><title>User Login</title></head>
<body>
<h2>PBLJ User Login</h2>
<form action="LoginServlet" method="post">
    Username: <input type="text" name="username" required><br><br>
    Password: <input type="password" name="password" required><br><br>
    <input type="submit" value="Login">
</form>
</body>
</html>


/* ----------------------------------------------------
 * DATABASE CONNECTION (DBConnection.java)
 * Place in: src/DBConnection.java
 * ---------------------------------------------------- */
import java.sql.*;
public class DBConnection {
    public static Connection getConnection() throws Exception {
        Class.forName("com.mysql.cj.jdbc.Driver");
        return DriverManager.getConnection(
            "jdbc:mysql://localhost:3306/pbljdb",
            "root",
            "yourpassword"
        );
    }
}


/* ----------------------------------------------------
 * LOGIN SERVLET (LoginServlet.java)
 * Place in: src/LoginServlet.java
 * ---------------------------------------------------- */
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.sql.*;

public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {

        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        String user = request.getParameter("username");
        String pass = request.getParameter("password");

        try {
            Connection con = DBConnection.getConnection();
            PreparedStatement ps = con.prepareStatement(
                "SELECT * FROM users WHERE username=? AND password=?"
            );
            ps.setString(1, user);
            ps.setString(2, pass);
            ResultSet rs = ps.executeQuery();

            if (rs.next()) {
                out.println("<h3>Login Success! Welcome " + user + "</h3>");
            } else {
                out.println("<h3>Login Failed! Incorrect Credentials</h3>");
            }

        } catch (Exception e) {
            out.println("Error: " + e);
        }
    }
}


/* ----------------------------------------------------
 * WEB.XML CONFIGURATION
 * Place in: WEB-INF/web.xml
 * ---------------------------------------------------- */
<web-app>
    <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>LoginServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/LoginServlet</url-pattern>
    </servlet-mapping>
</web-app>


/* ----------------------------------------------------
 * DATABASE SQL COMMANDS (MySQL)
 * Run in phpMyAdmin / MySQL CLI
 * ---------------------------------------------------- */
CREATE DATABASE pbljdb;
USE pbljdb;

CREATE TABLE users(
  id INT PRIMARY KEY AUTO_INCREMENT,
  username VARCHAR(50),
  password VARCHAR(50)
);

INSERT INTO users(username, password)
VALUES ('admin', 'admin123');
