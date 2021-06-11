---
id: 50
title: 'How to make a login page in ASP .NET C#'
date: 2012-01-22T01:30:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/01/22/how-to-make-a-login-in-asp-net-c/
permalink: /2012/01/22/how-to-make-a-login-in-asp-net-c/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/01/how-to-make-login-in-asp-net-c.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/6305929907906487558
categories:
  - Findings
tags:
  - .net
  - asp
  - c
---
In this post, I will teach you guys about making a simple login using common page controls. First of all, you need two textboxes, one for username and another for password. I'll call them usernameTextBox and passwordTextBox for this example. Drag down a button and call it loginButton. Now, in the OnClick event of loginButton, you need to check if the provided combination of username and password is correct or not. You can only do this if you have username and passwords stored somewhere. It is recommended to use a database to do this. Create a AccountDetails table in your database. The table should have username and password fields, both as text (VARCHAR for SQL). The way login is going to work is that you select records from AccountDetails table who have the provided username and password. This query should return one row if the combination matches. If not, then it should return nothing. Here's the query you need to pass:

<code>string query = "SELECT * FROM AccountDetails WHERE username='"+usernameTextBox.Text + "' AND pass='" + passwordTextBox.Text + "'";
</code>Supply this query to the command object. This can be OleDbCommand or SqlCommand depending on your type of database. Invoke the ExecuteReader method of the command object and then check if the Read method of the DataReader object returns true. If it does then this means that the login is successful. If not, then login is invalid. Here's a sample code.

<code>string connectionString = "...";
OleDbConnection conn = new OleDbConnection(connectionString);
conn.Open();
string queryString = "SELECT * FROM AccountDetails WHERE username='"+usernameTextBox.Text + "' AND pass='" + passwordTextBox.Text + "'";
OleDbCommand command = new OleDbCommand(queryString, conn);
OleDbDataReader reader = command.ExecuteReader();
if(reader.Read()){
//Login is successful.
}else{
//Login failed.
}
</code>Thats it! Your ASP website should now have a wonderful login to allow geniune users. To extend its capabilities, store the username in a session and then in PageLoad event of rest of the pages which require login, check if the session contains something. If it is null, then the page should redirect to login page. Same if for log-out. In the PageLoad event of the login page, check if the session username contains something. If it does, then clear out the session and display some notification saying that the user has been logged out. Now, in your page, to which the user gets redirected after successful login, put a simple link to login.

Now, initially when user lands on the login page, the session username will be empty. However, on typing correct username and password, this session gets filled with the username of the logged user and he/she is redirected to some sort of dashboard page. When the user clicks on the log-out link, he/she gets redirected to the login page which first checks if there is anything in the session. Since the user has previously logged in, there will be a username inside that session. Login page clears it out and displays a message saying that the user has been logged out. I hope that was easy.

If you have any queries or doubts, feel free to comment below.
If you want to know how session works, click <a href="http://codeninjutsu.blogspot.com/2012/01/using-sessions-in-asp-net.html">here</a> to see my post on Sessions.