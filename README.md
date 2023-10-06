import javax.swing.*;
import javax.swing.filechooser.FileNameExtensionFilter;
import javax.swing.text.BadLocationException;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.*;
import com.itextpdf.text.Document;
import com.itextpdf.text.DocumentException;
import com.itextpdf.text.Paragraph;
import com.itextpdf.text.pdf.PdfPTable;
import com.itextpdf.text.pdf.PdfWriter;

import com.toedter.calendar.JDateChooser;
import java.sql.*;

public class Resume {
    
    JFrame jf;
    // static JPanel p1;
    JTextField fnamef, mnamef, imgpath, lnamef, phonef, emailf, dob, addr;
    private JButton img;
    BufferedWriter bw;
    JComboBox<String> s1, s2, s3, s4;
    JProgressBar jp;
    // private JButton gnerate;
    JDateChooser dateChooser;
    JComboBox<String> exp;
    JComboBox<String> qly;
    static String fname;
    static String mname;
    static String lname;
    static String date;
    static long phone;
    static String email;
    static String add;
    static String sk1;
    static String sk3;
    static String sk4;
    static String qlyf;
    static String expr;
    static String sk2;
    String color = "dark";

    Resume() {
        
        jf = new JFrame("Resume_Builder");
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        jf.setBounds(500, 50, 400, 600);

        JPanel inputPanel = new JPanel(new GridLayout(18, 2));

        inputPanel.add(new JLabel("First Name:"));
        fnamef = new JTextField();
        inputPanel.add(fnamef);
        inputPanel.add(new JLabel("Middle Name:"));
        mnamef = new JTextField();
        inputPanel.add(mnamef);
        inputPanel.add(new JLabel("Last Name:"));
        lnamef = new JTextField();
        inputPanel.add(lnamef);

        // here we use jdatechooser jar file for selecting date using calender
        inputPanel.add(new JLabel("Date-Of-Birth: (dd-mm-yyyy)"));
        dateChooser = new JDateChooser();
        inputPanel.add(dateChooser);

        inputPanel.add(new JLabel("Phone: (10 digits)"));
        phonef = new JTextField();
        inputPanel.add(phonef);

        inputPanel.add(new JLabel("Email:"));
        emailf = new JTextField();
        inputPanel.add(emailf);

        inputPanel.add(new JLabel("Address:"));
        addr = new JTextField();
        inputPanel.add(addr);

        inputPanel.add(new JLabel("Enter skills"));
        inputPanel.add(new JLabel("(Minimum 3 Skills Required)"));

        String skills[] = { "-", "Java", "Python", "C++", "JavaScript", "SQL", "HTML/CSS", "Data Analysis",
                "Problem Solving", "Other" };

        inputPanel.add(new JLabel("Skill 1:"));
        s1 = new JComboBox<String>(skills);
        inputPanel.add(s1);
        inputPanel.add(new JLabel("Skill 2:"));
        s2 = new JComboBox<String>(skills);
        inputPanel.add(s2);
        inputPanel.add(new JLabel("Skill 3:"));
        s3 = new JComboBox<String>(skills);
        inputPanel.add(s3);
        inputPanel.add(new JLabel("Skill 4:"));
        s4 = new JComboBox<String>(skills);
        inputPanel.add(s4);

        inputPanel.add(new JLabel("Education Qualification::"));
        String[] qlData = { "High School", "Bachelor's Degree", "Master's Degree", "Ph.D." };
        qly = new JComboBox<String>(qlData);
        inputPanel.add(qly);

        inputPanel.add(new JLabel("Experience Status:"));
        String[] expData = { "Fresher", "1-3 years", "3-5 years", "5-10 years", "> 10 years" };
        exp = new JComboBox<>(expData);
        inputPanel.add(exp);

        // JButton username = new JButton("Username");
        // JTextField usertextfield = new JTextField();
        // username.addActionListener(new ActionListener() {

        // public void actionPerformed(ActionEvent e) {
        // try {
        // if (fnamef.getText().equals(null)) {
        // JOptionPane.showMessageDialog(null, "Fill all details first");
        // // if (phonef.getText().length() != 10) {
        // // JOptionPane.showMessageDialog(null, "Phone Number must be 10 digits");
        // // return;
        // // }
        // return;
        // } else {
        // String uname = fnamef.getText() + "_" + phonef.getText(0, 5);
        // usertextfield.setText(uname);
        // }
        // } catch (Exception ee) {
        // System.out.println(ee.getMessage());
        // }
        // }
        // });

        // inputPanel.add(username);
        // inputPanel.add(usertextfield);
        img = new JButton("Select Image(jpg/jpeg/png)");
        imgpath = new JTextField();

        img.addActionListener(new ActionListener() {

            public void actionPerformed(ActionEvent e) {

                JFileChooser imgFile = new JFileChooser();
                FileNameExtensionFilter filter = new FileNameExtensionFilter(".*IMAGE", "jpg", "png", "jpeg");
                imgFile.addChoosableFileFilter(filter);
                int rs = imgFile.showSaveDialog(null);
                if (rs == JFileChooser.APPROVE_OPTION) {
                    File selected = imgFile.getSelectedFile();
                    img.setText(selected.getName());
                    imgpath.setText(selected.getAbsolutePath());
                    // imgSelection.setIcon(resize(imgpath.getText()));
                }
            }
        });
        inputPanel.add(img);
        inputPanel.add(imgpath);
        // JOptionPane.showConfirmDialog(null, "Want to create Resume ?", "Resume
        // Builder", JOP)

        JButton generButton = new JButton("GENERATE");
        generButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                try {
                    fname = fnamef.getText();
                    mname = mnamef.getText();
                    lname = lnamef.getText();
                    String temp[] = dateChooser.getDate().toString().trim().split(" ");

                    date = temp[2] + "/" + temp[1] + "/" + temp[5] + ", " + temp[0];
                    if (phonef.getText().length() != 10) {
                        JOptionPane.showMessageDialog(null, "phonenumber must be 10 digits", "Invalid phone no",
                                JOptionPane.INFORMATION_MESSAGE);
                        return;
                    }
                    phone = Long.parseLong(phonef.getText());

                    email = emailf.getText();

                    add = addr.getText();

                    sk1 = (String) s1.getSelectedItem();
                    sk2 = (String) s2.getSelectedItem();
                    sk3 = (String) s3.getSelectedItem();
                    sk4 = (String) s4.getSelectedItem();

                    if (sk1.equals(sk2) || sk1.equals(sk3) || sk1.equals(sk4)) {
                        JOptionPane.showMessageDialog(null, "Invalid Case: Same skills are not valid case");
                        return;
                    }
                    if (sk2.equals(sk1) || sk2.equals(sk3) || sk2.equals(sk4)) {
                        JOptionPane.showMessageDialog(null, "Invalid Case: Same skills are not valid case");
                        return;
                    }
                    if (sk3.equals(sk2) || sk3.equals(sk1) || sk3.equals(sk4)) {
                        JOptionPane.showMessageDialog(null, "Invalid Case: Same skills are not valid case");
                        return;
                    }
                    if (sk4.equals(sk2) || sk4.equals(sk3) || sk4.equals(sk1)) {
                        JOptionPane.showMessageDialog(null, "Invalid Case: Same skills are not valid case");
                        return;
                    }

                    qlyf = (String) qly.getSelectedItem();

                    expr = (String) exp.getSelectedItem();
                    if (fname != null && lname != null && date != null ) {

                        // addToSQLDatabase();// set dta to sql database

                        // addToPGDatabase();// set to postgre

                        // writeIntoTXT();// text file generating

                        // writeTOPDF();// pdf file
                    } else {
                        JOptionPane.showMessageDialog(null, "Fill all fields", "Attention..!!",
                                JOptionPane.INFORMATION_MESSAGE);
                        return;
                    }
                    JOptionPane.showMessageDialog(null, "Resume Generated..!!");
                } catch (Exception ex) {
                    JOptionPane.showMessageDialog(null, "Fill all details first");
                }
            }
        });
        inputPanel.add(generButton);

        JButton clear = new JButton("CLEAR");
        clear.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                fnamef.setText("");
                mnamef.setText("");
                imgpath.setText("");
                lnamef.setText("");
                phonef.setText("");
                emailf.setText("");
                dateChooser.setDate(null);
                s1.setSelectedItem("-");
                s2.setSelectedItem("-");
                s3.setSelectedItem("-");
                s4.setSelectedItem("-");
                qly.setSelectedItem("High School");
                exp.setSelectedItem("Fresher");
                imgpath.setText("");
                // s1.set
                addr.setText("");

            }
        });
        // button for change color of input panel
        JButton themebutton = new JButton("Dark Mode");
        themebutton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                if (color == "dark") {
                    themebutton.setText("Light Mode");
                    inputPanel.setBackground(Color.GRAY);
                    
                    color = "white";
                } else if (color == "white") {
                    themebutton.setText("Dark Mode");
                    inputPanel.setBackground(Color.WHITE);
                    
                    
                    color = "dark";
                }
            }
        });

        inputPanel.add(clear);
        inputPanel.add(themebutton);

        // inputPanel.add(clear);
        // inputPanel.add(generButton);
        // inputPanel.add(WhiteTheme);
        // button for change color of input panel
        JButton adminbutton = new JButton("Admin Contact");
        adminbutton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                ImageIcon img=new ImageIcon("D://SHIVAM.jpg");
                JFrame imageframe=new JFrame("Admin Contact:");
                imageframe.setBounds(400, 100, 400, 600);
                JLabel label=new JLabel(img);
                imageframe.add(label);
                imageframe.pack();
                imageframe.setVisible(true);
            }
        });
        inputPanel.add(adminbutton);
        jf.add(inputPanel);
        jf.setVisible(true);
    }

    public static void process() {
        JFrame j2 = new JFrame("Processing for generating Resume..!!");
        j2.setLayout(new FlowLayout());
        j2.setBounds(500, 300, 300, 100);
        
        j2.setVisible(true);
        
        JProgressBar jp = new JProgressBar(0, 100);
        j2.add(jp);
        jp.setStringPainted(true);
        
        for(int i=0;i<=100;i+=2){
            jp.setValue(i);
            jp.setString("Progress "+i+"% ");
            if(i>20 && i<=50){
                jp.setString("Setting GUI...");
            }
            if(i>50 && i<=70){
                jp.setString("Setting Database...");
            }
            if(i>80 && i<=95){
                jp.setString("About to launch");
            }
            try {
                Thread.sleep(200);
            } catch (Exception progresse) {
                System.out.println(progresse.getMessage());
            }}
        }
        
        // Generate TXT file
    public void writeIntoTXT() throws Exception {
        try {
            String path = "D://Resume_Data";
            File f = new File(path);
            if (!f.exists()) {
                f.mkdirs();
            }

            bw = new BufferedWriter(
                    new FileWriter(path + "//" + fnamef.getText() + "_" + phonef.getText(0, 5) + "_Resume.txt"));
            bw.write("_____________________________________________________________________________\n");
            bw.write("FULL NAME: " + fnamef.getText() + " " + mnamef.getText() + " " + lnamef.getText());
            // Mon Oct 02 00:00:00 IST 2023(getDate Format)
            String x = dateChooser.getDate().toString();
            System.out.println(x);
            String temp[] = dateChooser.getDate().toString().trim().split(" ");
            String date = temp[2] + "/" + temp[1] + "/" + temp[5] + ", " + temp[0];
            bw.newLine();
            bw.write("DOB:       " + date);
            bw.newLine();

            bw.write("Phone:     " + phonef.getText());
            bw.newLine();
            bw.write("E_Mail:    " + emailf.getText());
            bw.newLine();
            bw.write("Add:       " + addr.getText());
            bw.newLine();
            bw.write(
                    "===================================================================================================================");
            bw.newLine();
            bw.write("Skills:      ");
            bw.newLine();
            bw.write("-" + (String) s1.getSelectedItem());
            bw.newLine();
            bw.write("-" + (String) s2.getSelectedItem());
            bw.newLine();
            bw.write("-" + (String) s3.getSelectedItem());
            bw.newLine();
            bw.write("-" + (String) s4.getSelectedItem());
            bw.newLine();
            bw.write("Qualification" + (String) qly.getSelectedItem());
            bw.newLine();
            bw.write(" Experience: " + (String) exp.getSelectedItem());
            bw.newLine();
            bw.write("_____________________________________________________________________________");

            bw.flush();

        } catch (FileNotFoundException e) {
            JOptionPane.showMessageDialog(null, "File not found Exception Occured", "Attention",
                    JOptionPane.INFORMATION_MESSAGE);
        } catch (IOException e) {
            JOptionPane.showMessageDialog(null, "IO Exception Occured", "Attention", JOptionPane.INFORMATION_MESSAGE);
        }
    }

    // add all details to Postgre Database

    public void addToPGDatabase() {
        Connection con = null;
        try {
            Class.forName("org.postgresql.Driver");// postgre driver name
            con = DriverManager.getConnection("jdbc:postgresql://localhost:5432/candidates_data", "postgres", "sp1024");
            if (con != null) {
                System.out.println(" pgAdmin Connection is Ok");
            } else
                System.out.println("Connection Failed");

            String insert = "insert into candidates(firstname,middlename,lastname,dob,phone,email,address,skill_1,skill_2,skill_3,skill_4,qualification_status,experience) values(?,?,?,?,?,?,?,?,?,?,?,?,?)";
            PreparedStatement pst = con.prepareStatement(insert);
            // System.out.println(fname);
            pst.setString(1, fname);
            pst.setString(2, mname);
            pst.setString(3, lname);
            pst.setString(4, date);
            pst.setLong(5, phone);
            pst.setString(6, email);
            pst.setString(7, add);
            pst.setString(8, sk1);
            pst.setString(9, sk2);
            pst.setString(10, sk3);
            pst.setString(11, sk4);
            pst.setString(12, qlyf);
            pst.setString(13, expr);
            int r = pst.executeUpdate();
            if (r > 0) {
                JOptionPane.showMessageDialog(null, "Candidate saved successfully to pgAdmin DATABSE",
                        "Save Notifiaction",
                        JOptionPane.INFORMATION_MESSAGE);
            } else {
                JOptionPane.showMessageDialog(null, "some error occured due to saving data to pgAdmin DATABSE",
                        "Save Notifiaction",
                        JOptionPane.INFORMATION_MESSAGE);
            }
        } catch (ClassNotFoundException e) {
            System.out.println("Exception: " + e.getMessage());
        } catch (SQLException e) {
            System.out.println("Exception: " + e.getMessage());
        }
    }

    // Add all data to SQL DATABASE

    public void addToSQLDatabase() {
        Connection con = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            con = DriverManager.getConnection("jdbc:mysql://localhost:3306/Candidates_data", "root", "");
            if (con != null) {
                System.out.println(" SQL Connection is Ok");
            } else {
                System.out.println("Connection Failed");
                throw new SQLException("Connection Failed..!!");
            }
            String insert = "insert into candidates(firstname,middlename,lastname,dob,phone,email,address,skill_1,skill_2,skill_3,skill_4,qualification_status,experience) values(?,?,?,?,?,?,?,?,?,?,?,?,?)";
            PreparedStatement pst = con.prepareStatement(insert);
            // System.out.println(fname);
            pst.setString(1, fname);
            pst.setString(2, mname);
            pst.setString(3, lname);
            pst.setString(4, date);
            pst.setLong(5, phone);
            pst.setString(6, email);
            pst.setString(7, add);
            pst.setString(8, sk1);
            pst.setString(9, sk2);
            pst.setString(10, sk3);
            pst.setString(11, sk4);
            pst.setString(12, qlyf);
            pst.setString(13, expr);
            int r = pst.executeUpdate();
            if (r > 0) {
                JOptionPane.showMessageDialog(null, "Candidate saved successfully to SQL DATABSE", "Save Notifiaction",
                        JOptionPane.INFORMATION_MESSAGE);
            } else {
                JOptionPane.showMessageDialog(null, "some error occured due to saving data to SQL DATABSE",
                        "Save Notifiaction",
                        JOptionPane.INFORMATION_MESSAGE);
            }
        } catch (ClassNotFoundException e) {
            System.out.println("Exception: " + e.getMessage());
        } catch (SQLException e) {
            System.out.println("Exception: " + e.getMessage());
        }
    }

    // Generate PDF using itextpdfjar

    public void writeTOPDF() throws Exception {
        try {
            String path = "D://Resume_PDFs";
            File f = new File(path);
            if (!f.exists()) {
                f.mkdirs();
            }

            String namePDF = path + "//" + fnamef.getText().toLowerCase() + "_" + phonef.getText(0, 5) + ".pdf";
            Document doc = new Document();
            PdfWriter.getInstance(doc, new FileOutputStream(namePDF));
            doc.open();
            com.itextpdf.text.Image imgd = com.itextpdf.text.Image.getInstance(imgpath.getText());
            imgd.setAbsolutePosition(473f, 750f);
            imgd.scaleAbsolute(80f, 70f);
            // doc.add(imgd);
            doc.add(imgd);
            doc.add(new Paragraph("Name: " + fnamef.getText() + " " + mnamef.getText() + " " + lnamef.getText()));
            doc.add(new Paragraph(" "));
            doc.add(new Paragraph(" "));
            doc.add(new Paragraph("CONTACT DETAILS"));

            doc.add(new Paragraph("Phone No:     " + phonef.getText()));

            doc.add(new Paragraph("E-Mail:    " + emailf.getText()));

            doc.add(new Paragraph("Address:       " + addr.getText()));

            String temp[] = dateChooser.getDate().toString().trim().split(" ");
            String date = temp[2] + "/" + temp[1] + "/" + temp[5] + ", " + temp[0];
            doc.add(new Paragraph("DOB:       " + date));
            // doc.add(new Paragraph(" "));
            doc.add(new Paragraph(" "));
            doc.add(new Paragraph(" "));

            doc.add(new Paragraph(
                    "----------------------------------------------------------------------------------------------------------------------------------"));
            doc.add(new Paragraph("SKILLS"));
            PdfPTable table = new PdfPTable(2);
            table.setHeaderRows(1);
            table.addCell((String) s1.getSelectedItem());
            table.addCell((String) s2.getSelectedItem());
            table.addCell((String) s3.getSelectedItem());
            table.addCell((String) s4.getSelectedItem());
            doc.add(table);
            doc.add(new Paragraph(" "));
            doc.add(new Paragraph(" "));
            doc.add(new Paragraph(
                    "----------------------------------------------------------------------------------------------------------------------------------"));
            doc.add(new Paragraph("QUALIFICATIONS"));
            doc.add(new Paragraph((String) qly.getSelectedItem()));
            doc.add(new Paragraph(" "));
            doc.add(new Paragraph(" "));
            doc.add(new Paragraph(
                    "----------------------------------------------------------------------------------------------------------------------------------"));
            doc.add(new Paragraph("WORK EXPERIENCE"));
            doc.add(new Paragraph((String) exp.getSelectedItem()));
            doc.close();
            JOptionPane.showMessageDialog(null, "CV was successfully generated as PDF at " + namePDF);

        } catch (DocumentException pdfex) {
            JOptionPane.showMessageDialog(null, pdfex.getMessage());
        }
    }

    public static void main(String[] args) {
        // Resume.process();
        new Resume();
    }
}
