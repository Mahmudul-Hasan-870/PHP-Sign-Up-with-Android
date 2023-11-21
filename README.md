# PHP-Sign-Up-with-Android

> Creating a complete PHP signup page with Android Studio, Volley for network requests, HashMap for parameters, and a loading bar involves multiple components. I'll provide you with a basic example, but keep in mind that this is a simplified version, and you may need to adapt it based on your specific requirements and server setup.

## PHP Signup Script (signup.php):

```sh
<?php
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    // Assuming you have a MySQL database setup
    $servername = "your_mysql_server";
    $username = "your_mysql_username";
    $password = "your_mysql_password";
    $dbname = "your_mysql_database";

    // Create connection
    $conn = new mysqli($servername, $username, $password, $dbname);

    // Check connection
    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }

    $username = $_POST['username'];
    $password = password_hash($_POST['password'], PASSWORD_DEFAULT); // Hash the password for security

    // You might want to add additional validation and error handling here

    $sql = "INSERT INTO users (username, password) VALUES ('$username', '$password')";

    if ($conn->query($sql) === TRUE) {
        echo "Registration successful";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }

    $conn->close();
}
?>

```

## Android Studio Code:

Create a new Android Studio project.
Add the following dependencies to your build.gradle file:

```sh
implementation 'com.android.volley:volley:1.2.1'
```

### Create your layout (activity_signup.xml):

```sh
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingLeft="16dp"
    android:paddingTop="16dp"
    android:paddingRight="16dp"
    android:paddingBottom="16dp"
    tools:context=".SignupActivity">

    <EditText
        android:id="@+id/editTextUsername"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Username" />

    <EditText
        android:id="@+id/editTextPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextUsername"
        android:layout_marginTop="16dp"
        android:hint="Password"
        android:inputType="textPassword" />

    <Button
        android:id="@+id/btnSignUp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/editTextPassword"
        android:layout_marginTop="16dp"
        android:text="Sign Up" />

    <ProgressBar
        android:id="@+id/progressBar"
        style="?android:attr/progressBarStyle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:visibility="gone" />

</RelativeLayout>

```

### Create your SignupActivity.java:

```sh
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ProgressBar;
import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;
import java.util.HashMap;
import java.util.Map;

public class SignupActivity extends AppCompatActivity {

    private EditText editTextUsername, editTextPassword;
    private Button btnSignUp;
    private ProgressBar progressBar;
    private RequestQueue requestQueue;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signup);

        editTextUsername = findViewById(R.id.editTextUsername);
        editTextPassword = findViewById(R.id.editTextPassword);
        btnSignUp = findViewById(R.id.btnSignUp);
        progressBar = findViewById(R.id.progressBar);

        requestQueue = Volley.newRequestQueue(this);

        btnSignUp.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                signUp();
            }
        });
    }

    private void signUp() {
        final String username = editTextUsername.getText().toString().trim();
        final String password = editTextPassword.getText().toString().trim();

        progressBar.setVisibility(View.VISIBLE);

        String url = "http://your_server/signup.php";

        StringRequest stringRequest = new StringRequest(Request.Method.POST, url,
                new Response.Listener<String>() {
                    @Override
                    public void onResponse(String response) {
                        progressBar.setVisibility(View.GONE);
                        // Handle the response from the server
                    }
                },
                new Response.ErrorListener() {
                    @Override
                    public void onErrorResponse(VolleyError error) {
                        progressBar.setVisibility(View.GONE);
                        // Handle error
                    }
                }) {
            @Override
            protected Map<String, String> getParams() {
                Map<String, String> params = new HashMap<>();
                params.put("username", username);
                params.put("password", password);
                return params;
            }
        };

        requestQueue.add(stringRequest);
    }
}
```

> Remember to replace "http://your_server/signup.php" with the actual URL of your PHP script. Also, make sure to handle responses and errors appropriately in the Android code. This example uses a simple StringRequest; for a more robust solution, you might want to consider using Gson for JSON parsing and a custom model for response handling. Additionally, don't forget to add internet permission to your AndroidManifest.xml:

```sh
<uses-permission android:name="android.permission.INTERNET" />
```

This is a basic example, and in a real-world scenario, you would need to implement better security practices, error handling, and possibly use HTTPS for secure communication.

## Thank you
