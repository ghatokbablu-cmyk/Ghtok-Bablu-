# Ghtok-Bablu-
Ghtok Bablu @gmail.com
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/RegisterBioData")
public class RegisterBioData extends HttpServlet {

    // Database configurations
    private static final String DB_URL = "jdbc:mysql://localhost:3306/GhatakBabluDB";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "password";

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        response.setContentType("text/html;charset=UTF-8");
        request.setCharacterEncoding("UTF-8");

        String type = request.getParameter("type");
        String fullName = request.getParameter("fullName");
        int age = Integer.parseInt(request.getParameter("age"));
        String profession = request.getParameter("profession");
        String district = request.getParameter("district");
        String phone = request.getParameter("phone");

        PrintWriter out = response.getWriter();

        try {
            // MySQL Driver Load করা
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);

            // SQL Query দিয়ে ডেটা সেভ করা
            String sql = "INSERT INTO biodata (type, full_name, age, profession, district, phone) VALUES (?, ?, ?, ?, ?, ?)";
            PreparedStatement pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, type);
            pstmt.setString(2, fullName);
            pstmt.setInt(3, age);
            pstmt.setString(4, profession);
            pstmt.setString(5, district);
            pstmt.setString(6, phone);

            int result = pstmt.executeUpdate();

            out.println("<html><body style='text-align:center; font-family:sans-serif; padding-top:50px; background-color:#fce4ec;'>");
            if (result > 0) {
                out.println("<h2 style='color:#2e7d32;'>অভিনন্দন! 'ঘটক বাবলু' ওয়েবসাইটে আপনার বায়োডাটা সফলভাবে জমা হয়েছে।</h2>");
                out.println("<br><a href='index.html' style='padding:10px 20px; background:#c2185b; color:white; text-decoration:none; border-radius:5px;'>হোম পেজে ফিরে যান</a>");
            } else {
                out.println("<h2 style='color:#c62828;'>দুঃখিত, তথ্য জমা দেওয়া সম্ভব হয়নি। আবার চেষ্টা করুন।</h2>");
            }
            out.println("</body></html>");

            pstmt.close();
            conn.close();
        } catch (Exception e) {
            e.printStackTrace();
            out.println("<h3 style='color:red;'>Error: " + e.getMessage() + "</h3>");
        }
    }
}
