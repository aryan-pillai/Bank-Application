# Bank-Application
//JAVA CODE for a bank application



/*LOGIN CODE*/


String name=jTextField1.getText();
String pwd= jPasswordField1.getText();
               if(name.equals(""))
       {
           jLabel6.setVisible(true);
       }
       if(pwd.equals(""))
       {
           jLabel7.setVisible(true);
       }
       else
       {
 String query="select userid, password from info where userid=? and   password=?";
           try
           {
               con =MySqlConnection.connectDB();
               pstmt =con.prepareStatement(query);
               pstmt.setString(1,name);
               pstmt.setString(2,pwd);
               rst= pstmt.executeQuery();
                              if(rst.next())
                       {
                           Main welcome=new Main();
                           welcome.setVisible(true);
                           this.dispose();                       
                                                  }
                else
                       {
                           JOptionPane.showMessageDialog(null,"Invalid user");
                           jTextField1.setText(" ");
                           jPasswordField1.setText("");
                                   }              
           }
           catch(Exception e)
           {
               System.out.println(e);
           }
       }
    } 

//SIGN-UP PAGE CODE

Signup signup = new Signup();
signup.setVisible(true);
this.dispose();

//NEW BUTTON (TO ASSIGN AUTO ACCOUNT NUMBER)
int ctr=100001;
       String query="Select * from info ";       
       try
       {
           con =MySqlConnection.connectDB();
          pstmt=con.prepareStatement(query);
          rst=pstmt.executeQuery();
          
          while(rst.next())
          {
              ctr=rst.getInt("userid");
              jTextField3.setText(Integer.toString(ctr+1));
          }
       }
       catch( SQLException e)
       {
           System.out.println(e);
       }
    }                                        

//CREATE CODE(WILL TAKE IN THE DETAILS OF THE NEW COUSTOMER AND ENTER DATA INTO MYSQL TABLE)

String fname=jTextField1.getText();
       String lname=jTextField2.getText();
       String accno=jTextField3.getText();
         String pwd=jPasswordField1.getText();
         String adhar=jTextField4.getText();
       String amount=jTextField5.getText();
        String contactno=jTextField6.getText();
          String query=" insert into info(firstname,lastname,userid,password,adhar,amount,contact) values (?,?,?,?,?,?,?) ";
          try 
          {
          con =MySqlConnection.connectDB();
          pstmt=con.prepareStatement(query);
         
          
          pstmt.setString(1,fname);
          pstmt.setString(2,lname);
          pstmt.setString(3,accno);
          pstmt.setString(4,pwd);
          pstmt.setString(5,adhar);
          pstmt.setString(6,amount);
          pstmt.setString(7,contactno);
          int res=pstmt.executeUpdate();
          if(res>0)
          {
              JOptionPane.showMessageDialog(null, "Records Inserted");
               jTextField1.setText(null);
             jTextField1.setText(null);
             jTextField1.setText(null);
             jPasswordField1.setText(null);
          }
          else
          {
             JOptionPane.showMessageDialog(null, "Records Not Inserted");
             jTextField1.setText(null);
             jTextField1.setText(null);
             jTextField1.setText(null);
             jPasswordField1.setText(null);
          }                      
          pstmt.close();
          con.close();                      
          }
          catch(Exception e)
          {              System.out.println(e);          }


// CODE FOR THE DEPOSIT PAGE

// CODE FOR SHOW DETAILS BUTTON (SHOWS DETAILS OF THE USER WITH THE AFFILIATED ACCOUNT NUMBER)
int accno=Integer.parseInt(jTextField1.getText());
        try
       {	
           con=MySqlConnection.connectDB(); 
           String query="select firstname,lastname,amount from info where userid=?";
           pstmt=con.prepareStatement(query);
           pstmt.setInt(1, accno);
           rst=pstmt.executeQuery();
           while(rst.next())
           {
                jTextField3.setText( rst.getString("firstname"));
          jTextField4.setText( rst.getString("lastname"));
          jTextField5.setText( rst.getString("amount"));
               
           }
           
                 rst.close();
       pstmt.close();
       con.close();       
       }       
        catch(Exception e)
          {
              System.out.println(e);
          }
    }                               

// CODE TO ENTER MONEY INTO THE ACCOUNT
int amount=Integer.parseInt(jTextField2.getText());
         int accno=Integer.parseInt(jTextField1.getText());
         
        int a=Integer.parseInt(jTextField5.getText());
        int x=a+amount;
        
        jTextField6.setText(String.valueOf(x));
         try
       {
           con=MySqlConnection.connectDB(); 
        String query="update info set amount=? where userid=?";
        pstmt=con.prepareStatement(query);
           pstmt.setInt(1, x);
           pstmt.setInt(2, accno);
           int rst=pstmt.executeUpdate();          
       } 
        catch(Exception e)
          {
              System.out.println(e);
          }
    }                                     
    private void jButton2ActionPerformed(java.awt.event.ActionEvent evt) {                                         
        int accno=Integer.parseInt(jTextField1.getText());         
       try
       {
           con=MySqlConnection.connectDB(); 
           String query="select firstname,lastname,amount from info where userid=?";
           pstmt=con.prepareStatement(query);
           pstmt.setInt(1, accno);
           rst=pstmt.executeQuery();
           while(rst.next())
           {
                jTextField3.setText( rst.getString("firstname"));
          jTextField4.setText( rst.getString("lastname"));
          jTextField5.setText( rst.getString("amount"));               
           }                 
       rst.close();
       pstmt.close();
       con.close();       
       }       
        catch(Exception e)
          {
              System.out.println(e);
          }


// CODE FOR WITHDRAW PAGE

// CODE FOR SHOW DETAILS BUTTON (SHOWS DETAILS OF THE USER WITH THE AFFILIATED ACCOUNT NUMBER)

int accno=Integer.parseInt(jTextField1.getText());                       
       try
       {
           con=MySqlConnection.connectDB(); 
           String query="select firstname,lastname,amount from info where userid=?";
           pstmt=con.prepareStatement(query);
           pstmt.setInt(1, accno);
           rst=pstmt.executeQuery();
           while(rst.next())
           {
                jTextField3.setText( rst.getString("firstname"));
          jTextField4.setText( rst.getString("lastname"));
          jTextField5.setText( rst.getString("amount"));               
           }                     
       rst.close();
       pstmt.close();
       con.close();
       }      
        catch(Exception e)
          {
              System.out.println(e);
          }
    }                                   
    private void jButton1ActionPerformed(java.awt.event.ActionEvent evt) {                                         
          int amount=Integer.parseInt(jTextField2.getText());
                 int accno=Integer.parseInt(jTextField1.getText());

        int a=Integer.parseInt(jTextField5.getText());
        if(a<amount)
        {
        JOptionPane.showMessageDialog(this,"Your withdrawal request exceeds your current account balance");
        jTextField2.setText("");
        }
        else
        {
        int x=a-amount;
        jTextField6.setText(String.valueOf(x));
        try
       {
           con=MySqlConnection.connectDB(); 
        String query2="update info set amount=? where userid=?";
        pstmt=con.prepareStatement(query2);
           pstmt.setInt(1, x);
           pstmt.setInt(2, accno);
           
          int rst=pstmt.executeUpdate();
       }
         
        catch(Exception e)
          {
              System.out.println(e);
          }
        }
    }                         

// CODE TO WITHDRAW MONEY FROM THE BANK

int amount=Integer.parseInt(jTextField2.getText());
                 int accno=Integer.parseInt(jTextField1.getText());

        int a=Integer.parseInt(jTextField5.getText());
        if(a<amount)
        {
        JOptionPane.showMessageDialog(this,"Your withdrawal request exceeds your current account balance");
        jTextField2.setText("");
        }
        else
        {
        int x=a-amount;        
        jTextField6.setText(String.valueOf(x));
        try
       {
           con=MySqlConnection.connectDB(); 
        String query2="update info set amount=? where userid=?";
        pstmt=con.prepareStatement(query2);
           pstmt.setInt(1, x);
           pstmt.setInt(2, accno);
           
          int rst=pstmt.executeUpdate();
       }
         
        catch(Exception e)
          {
              System.out.println(e);
          }
        }


// TRANSFER PAGE(USED TO TRANFER MONEY FROM ONE ACCOUNT TO ANOTHER)
 
 //SHOW DETAILS (USED TO DISPLAY DISPLAY OF THE SENDER AND THE RECIPIENT)

int accno=Integer.parseInt(jTextField1.getText());          
        try
       {
           con=MySqlConnection.connectDB(); 
           String query="select firstname,lastname,amount from info where userid=?";
           pstmt=con.prepareStatement(query);
           pstmt.setInt(1, accno);
           rst=pstmt.executeQuery();
           while(rst.next())
           {
                jTextField4.setText( rst.getString("firstname"));
          jTextField5.setText( rst.getString("lastname"));
          jTextField6.setText( rst.getString("amount"));          
           }         
       rst.close();
       pstmt.close();
       con.close(); 
       }  
        catch(Exception e)
          {
              System.out.println(e);
          }     
int accno=Integer.parseInt(jTextField1.getText());
        int accno2=Integer.parseInt(jTextField2.getText());
        try
       {
           con=MySqlConnection.connectDB(); 
           String query="select firstname,lastname,amount from info where userid=?";
           pstmt=con.prepareStatement(query);
           pstmt.setInt(1, accno2);
           rst=pstmt.executeQuery();
           while(rst.next())
           {
                jTextField7.setText( rst.getString("firstname"));
          jTextField8.setText( rst.getString("lastname"));
          jTextField9.setText( rst.getString("amount"));
          
               
           }
           
           
          // String fname=rst.getString("firstname");
          // jTextField3.setText(fname);
          
       rst.close();
       pstmt.close();
       con.close();
       
       }
       
        catch(Exception e)
          {
              System.out.println(e);}

//CODE TO TRANSFER THE MONEY

int accno=Integer.parseInt(jTextField1.getText());
        int accno2=Integer.parseInt(jTextField2.getText());
        int amount=Integer.parseInt(jTextField3.getText());
         int a=Integer.parseInt(jTextField6.getText());
                int b=Integer.parseInt(jTextField9.getText());
        if(a<amount)
        {
        JOptionPane.showMessageDialog(this,"Your withdrawal request exceeds your current account balance");
        jTextField3.setText("");
        }
        else
        {
        int x=b+amount;
        int y=a-amount;
        
        jTextField9.setText(String.valueOf(x));
                jTextField6.setText(String.valueOf(y));

        try
       {
           con=MySqlConnection.connectDB(); 
        String query2="update info set amount=? where userid=?";
        pstmt=con.prepareStatement(query2);
           pstmt.setInt(1, y);
           pstmt.setInt(2, accno);
            String query3="update info set amount=? where userid=?";
        pstmt=con.prepareStatement(query3);
          pstmt.setInt(1, x);
           pstmt.setInt(2, accno2);
           
           
          int rst=pstmt.executeUpdate();
       }
         
        catch(Exception e)
          {
              System.out.println(e);
          }
        }     


// CODE FOR ADMIN PAGE

// SHOW DETAILS CODE

  int accno=Integer.parseInt(jTextField1.getText());
       String pwd= jTextField2.getText();
       
        if(String.valueOf(accno).equals(""))
       {
           JOptionPane.showMessageDialog(this, "Enter Valid Admin user");
       }
       if(pwd.equals(""))
       {
           JOptionPane.showMessageDialog(this, "Invalid user");
       }
       
        if(accno!=100000)
        {
        JOptionPane.showMessageDialog(this, "Invalid User");
        jTextField1.setText("");
                jTextField2.setText("");

        }
           
       else if(accno==100000)
       {
           String query="select userid, password from info where userid=? and password=?";
           try
           {
               con =MySqlConnection.connectDB();
               pstmt =con.prepareStatement(query);
               pstmt.setInt(1,accno);
               pstmt.setString(2,pwd);
               rst= pstmt.executeQuery();
               
               if(rst.next())
                       {
                           DefaultTableModel dml=new DefaultTableModel();
        int cols=0;
        con=MySqlConnection.connectDB();
        String query2="Select * from info";
        
            pstmt=con.prepareStatement(query2);
            rst=pstmt.executeQuery();
            ResultSetMetaData rsmt=rst.getMetaData();
            cols=rsmt.getColumnCount();
            String colname[]=new String[cols];
            for(int i=0;i<colname.length;i++)
                    {
                        colname[i]=rsmt.getColumnName(i+1);
                        dml.addColumn(colname[i]);
                        
                    }
            jTable1.setModel(dml);
            while(rst.next())
            {
                Object[]rowdata=new Object[cols];
                for(int i=0;i<rowdata.length;i++)
                {
                    rowdata[i]=rst.getObject(i+1);
                }
                dml.addRow(rowdata);
            }                       
                                            }
                else
                       {
                           JOptionPane.showMessageDialog(null,"Invalid user");
                           jTextField1.setText(" ");
                           jTextField2.setText("");           
                       }           
           }
           catch(Exception e)
           {
               System.out.println(e);
           }
       }

// DELETE ACCOUNT PAGE

// CODE TO AUTHINTICATE THAT IT IS THE ADMIN WHO IS ON THE PAGE

int adminaccno=Integer.parseInt(jTextField1.getText());
       String pwd= jTextField2.getText();
       
        if(String.valueOf(adminaccno).equals(""))
       {
           JOptionPane.showMessageDialog(this, "Enter Valid Admin user");
       }
       if(pwd.equals(""))
       {
           JOptionPane.showMessageDialog(this, "Invalid user");
       }
       
        if(adminaccno!=100000)
        {
        JOptionPane.showMessageDialog(this, "Invalid User");
        jTextField1.setText("");
                jTextField2.setText("");

        }
        else if(adminaccno==100000)
       {
       jTextField3.setEnabled(true);       
        }
    }                                        

    private void jButton2ActionPerformed(java.awt.event.ActionEvent evt) {                                               
        int accno=Integer.parseInt(jTextField3.getText());    
  try
       {
           con=MySqlConnection.connectDB(); 
           String query="delete from info where userid=?";
           pstmt=con.prepareStatement(query);
           pstmt.setInt(1, accno);
           int result=pstmt.executeUpdate();
           
           
          // String fname=rst.getString("firstname");
          // jTextField3.setText(fname);
          
       rst.close();
       pstmt.close();
       con.close();
       }
        catch(Exception e)
          {
              System.out.println(e);
          }


// CODE TO DELETE THE ACCOUNT
int accno=Integer.parseInt(jTextField3.getText())
   try
       {
           con=MySqlConnection.connectDB(); 
           String query="delete from info where userid=?";
           pstmt=con.prepareStatement(query);
           pstmt.setInt(1, accno);
           int result=pstmt.executeUpdate();
           
           
          // String fname=rst.getString("firstname");
          // jTextField3.setText(fname);
          
       rst.close();
       pstmt.close();
       con.close();
       
       }
       
        catch(Exception e)
          {
              System.out.println(e);
          }
           }                                





































