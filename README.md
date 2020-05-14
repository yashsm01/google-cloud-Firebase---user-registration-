# google-cloud-Firebase---user-registration-
create own application where we save user detail and provide great user experience with using google cloud service

step 1:- 

    android studio > Tools > Firebase > Authentication > create your project.

step 2:- 

    1. Create now empity Activity:
    2.name it Register

step3:- 
copy xml code in Rgister layout:-

        <?xml version="1.0" encoding="utf-8"?>
        <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context="ui.register.register">

          <TextView
              android:id="@+id/textView3"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="create new account"
              android:textColor="#000000"
              android:textSize="25sp"
              android:textStyle="bold"
              app:layout_constraintBottom_toBottomOf="parent"
              app:layout_constraintEnd_toEndOf="parent"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toTopOf="parent"
              app:layout_constraintVertical_bias="0.155" />

          <EditText
              android:id="@+id/FullName"
              android:layout_width="379dp"
              android:layout_height="44dp"
              android:layout_marginStart="16dp"
              android:layout_marginTop="24dp"
              android:layout_marginEnd="16dp"
              android:background="@android:drawable/editbox_dropdown_light_frame"
              android:ems="10"
              android:inputType="textPersonName"
              android:text="Name"
              android:textSize="18sp"
              app:layout_constraintBottom_toBottomOf="parent"
              app:layout_constraintEnd_toEndOf="parent"
              app:layout_constraintHorizontal_bias="0.0"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toBottomOf="@+id/textView3"
              app:layout_constraintVertical_bias="0.076" />

          <EditText
              android:id="@+id/Email"
              android:layout_width="379dp"
              android:layout_height="44dp"
              android:layout_marginStart="16dp"
              android:layout_marginTop="20dp"
              android:layout_marginEnd="16dp"
              android:background="@android:drawable/editbox_dropdown_light_frame"
              android:ems="10"
              android:inputType="textPersonName"
              android:text="Email"
              app:layout_constraintEnd_toEndOf="@+id/FullName"
              app:layout_constraintHorizontal_bias="0.0"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toBottomOf="@+id/FullName" />

          <EditText
              android:id="@+id/Phone"
              android:layout_width="379dp"
              android:layout_height="48dp"
              android:layout_marginStart="16dp"
              android:layout_marginTop="16dp"
              android:layout_marginEnd="16dp"
              android:background="@android:drawable/editbox_dropdown_light_frame"
              android:ems="10"
              android:inputType="textPersonName"
              android:text="Phone"
              app:layout_constraintEnd_toEndOf="parent"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toBottomOf="@+id/Email" />

          <EditText
              android:id="@+id/Password"
              android:layout_width="379dp"
              android:layout_height="45dp"
              android:layout_marginStart="16dp"
              android:layout_marginTop="16dp"
              android:layout_marginEnd="16dp"
              android:background="@android:drawable/editbox_dropdown_light_frame"
              android:ems="10"
              android:inputType="textPersonName"
              android:text="Password"
              app:layout_constraintEnd_toEndOf="parent"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toBottomOf="@+id/Phone" />

          <ProgressBar
              android:id="@+id/ProgressBar"
              style="?android:attr/progressBarStyle"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_marginTop="52dp"
              android:visibility="invisible"
              app:layout_constraintEnd_toEndOf="parent"
              app:layout_constraintHorizontal_bias="0.498"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toBottomOf="@+id/Login" />

          <Button
              android:id="@+id/LoginBtn"
              android:layout_width="0dp"
              android:layout_height="wrap_content"
              android:layout_marginStart="16dp"
              android:layout_marginTop="16dp"
              android:layout_marginEnd="16dp"
              android:text="Register"
              app:layout_constraintEnd_toEndOf="parent"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toBottomOf="@+id/Password" />

          <TextView
              android:id="@+id/Login"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_marginTop="8dp"
              android:text="Already Registred? Click Here"
              app:layout_constraintEnd_toEndOf="parent"
              app:layout_constraintHorizontal_bias="0.497"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toBottomOf="@+id/LoginBtn" />

      </androidx.constraintlayout.widget.ConstraintLayout>
      
step 4:-
copy this program in regiser Activity class:-
      
              public class register extends AppCompatActivity {
            EditText mFullName, mEmail, mPassword, mPhone;
            Button mRegisterBtn;
            TextView mLoginBtn;

            FirebaseAuth fAuth;
            FirebaseFirestore fStore;
            ProgressBar ProgressBar;

            String userId;

            @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.activity_register);
                mFullName = findViewById(R.id.FullName);
                mEmail = findViewById(R.id.Email);
                mPassword = findViewById(R.id.Password);
                mPhone =findViewById(R.id.Phone);
                mRegisterBtn = findViewById(R.id.LoginBtn);
                mLoginBtn = findViewById(R.id.Login);

                fAuth = FirebaseAuth.getInstance();
                fStore = FirebaseFirestore.getInstance();
                ProgressBar = findViewById(R.id.ProgressBar);

                if(fAuth.getCurrentUser() != null){
                    startActivity(new Intent(getApplicationContext(), home.class));
                    finish();
                }
                mRegisterBtn.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        final String email = mEmail.getText().toString().trim();
                        String password = mPassword.getText().toString().trim();
                        final String fullName = mFullName.getText().toString().trim();
                        final String Phone = mPhone.getText().toString().trim();

                        if(TextUtils.isEmpty(email)){
                            mEmail.setError("Email is Required.");
                            return;
                        }
                        if(TextUtils.isEmpty(password)){
                            mPassword.setError("Password is Required.");
                            return;
                        }
                        if(password.length() < 6){
                            mPassword.setError("Password must be greater then 6 characters");
                            return;
                        }

                        ProgressBar.setVisibility(View.VISIBLE);
                        //register the user in firebase

                        fAuth.createUserWithEmailAndPassword(email,password).addOnCompleteListener(  new OnCompleteListener<AuthResult>() {
                            @Override
                            public void onComplete(@NonNull Task<AuthResult> task) {
                                if(task.isSuccessful()){

                                    // send verification link

                                    FirebaseUser fuser= fAuth.getCurrentUser();
                                        fuser.sendEmailVerification().addOnSuccessListener(new OnSuccessListener<Void>() {
                                        @Override
                                        public void onSuccess(Void aVoid) {
                                            Toast.makeText(register.this, "Verification Email Has Been sent.", Toast.LENGTH_SHORT).show();
                                        }
                                    }).addOnFailureListener(new OnFailureListener() {
                                        @Override
                                        public void onFailure(@NonNull Exception e) {
                                            Log.d("TAG","ON Failure: Email Not Set" + e.getMessage());
                                        }
                                    });



                                    Toast.makeText(register.this, "User Created.", Toast.LENGTH_SHORT).show();

                                    // to upload the data to firebase cloud
                                    userId = fAuth.getCurrentUser().getUid();
                                    DocumentReference documentReference =  fStore.collection("Users").document(userId);
                                    Map<String, Object> user = new HashMap<>();
                                    user.put("FullName",fullName);
                                    user.put("email",email);
                                    user.put("Phone",Phone);
                                    documentReference.set(user).addOnSuccessListener(new OnSuccessListener<Void>() {
                                        @Override
                                        public void onSuccess(Void aVoid) {
                                            Log.d("Tag","On success: user profile created for"+ userId );
                                        }
                                    });

                                    startActivity(new Intent(getApplicationContext(), home.class));
                                } else {
                                    Toast.makeText(register.this, "Error !! "+ task.getException().getMessage(), Toast.LENGTH_SHORT).show();
                                    ProgressBar.setVisibility(View.GONE);
                                }
                            }
                        });
                    }
                });
                mLoginBtn.setOnClickListener(new View.OnClickListener(){
                    @Override
                    public void  onClick(View view){
                        startActivity(new Intent(getApplicationContext(), MainActivity.class));
                    }
                });
            }
        }
        
step 5:- 
create new Activity.Give Name Login:

copy Xml in login layout:-

                    <?xml version="1.0" encoding="utf-8"?>
                    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
                    xmlns:app="http://schemas.android.com/apk/res-auto"
                    xmlns:tools="http://schemas.android.com/tools"
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:background="@color/common_google_signin_btn_text_dark_default"
                    android:textAlignment="center"
                    tools:context=".MainActivity">

                <TextView
                    android:id="@+id/textView3"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/yash_industries"
                    android:textColor="#000000"
                    android:textSize="36sp"
                    android:textStyle="bold"
                    app:layout_constraintBottom_toBottomOf="parent"
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toTopOf="parent"
                    app:layout_constraintVertical_bias="0.155" />


                <EditText
                    android:id="@+id/Email"
                    android:layout_width="0dp"
                    android:layout_height="44dp"
                    android:layout_marginStart="16dp"
                    android:layout_marginTop="136dp"
                    android:layout_marginEnd="16dp"
                    android:background="@android:drawable/editbox_dropdown_light_frame"
                    android:ems="10"
                    android:inputType="textPersonName"
                    android:text="Email"
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintHorizontal_bias="0.0"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toBottomOf="@+id/textView3" />

                <EditText
                    android:id="@+id/Password"
                    android:layout_width="0dp"
                    android:layout_height="45dp"
                    android:layout_marginStart="16dp"
                    android:layout_marginTop="8dp"
                    android:layout_marginEnd="16dp"
                    android:background="@android:drawable/editbox_dropdown_light_frame"
                    android:ems="10"
                    android:inputType="textPersonName"
                    android:text="Password"
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintHorizontal_bias="0.0"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toBottomOf="@+id/Email" />

                <Button
                    android:id="@+id/LoginBtn"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_marginStart="16dp"
                    android:layout_marginTop="16dp"
                    android:layout_marginEnd="16dp"
                    android:text="Login"
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toBottomOf="@+id/Password" />

                <ProgressBar
                    android:id="@+id/ProgressBar"
                    style="?android:attr/progressBarStyle"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="76dp"
                    android:visibility="invisible"
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintHorizontal_bias="0.498"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toBottomOf="@+id/Register" />

                <TextView
                    android:id="@+id/Register"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="8dp"
                    android:text="New Here?Create Account."
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toBottomOf="@+id/LoginBtn" />

                <TextView
                    android:id="@+id/forgotPassword"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="8dp"
                    android:text="Forgot Passord ?"
                    android:textStyle="bold"
                    app:layout_constraintEnd_toEndOf="parent"
                    app:layout_constraintStart_toStartOf="parent"
                    app:layout_constraintTop_toBottomOf="@+id/Register" />


            </androidx.constraintlayout.widget.ConstraintLayout>

step 6:- 
copy program in login

                    import com.example.yashhomeautomation.ui.home.home;
                    import com.example.yashhomeautomation.ui.register.register;
                    import com.google.android.gms.tasks.OnCompleteListener;
                    import com.google.android.gms.tasks.OnFailureListener;
                    import com.google.android.gms.tasks.OnSuccessListener;
                    import com.google.android.gms.tasks.Task;
                    import com.google.firebase.auth.AuthResult;
                    import com.google.firebase.auth.FirebaseAuth;

                    public class MainActivity extends AppCompatActivity {
                    public static final String MSG = "com.example.yashhomeautomation.ORDER";


                    EditText mFullName, mEmail, mPassword, mMobileNumber;
                    TextView mRegisterBtn,mforgotPassword;
                    Button mLoginBtn;

                    FirebaseAuth fAuth;
                    ProgressBar ProgressBar;
                    @Override
                    protected void onCreate(Bundle savedInstanceState) {
                    super.onCreate(savedInstanceState);
                    setContentView(R.layout.activity_main);
                    mEmail = findViewById(R.id.Email);
                    mPassword = findViewById(R.id.Password);
                    mLoginBtn = findViewById(R.id.LoginBtn);
                    mRegisterBtn = findViewById(R.id.Register);
                    mforgotPassword = findViewById(R.id.forgotPassword);

                    fAuth = FirebaseAuth.getInstance();
                    ProgressBar = findViewById(R.id.ProgressBar);

                    if(fAuth.getCurrentUser() != null){
                        startActivity(new Intent(getApplicationContext(), home.class));
                        finish();
                    }

                    mLoginBtn.setOnClickListener(new View.OnClickListener() {
                        @Override
                        public void onClick(View view){
                            String email = mEmail.getText().toString().trim();
                            String password = mPassword.getText().toString().trim();

                            if(TextUtils.isEmpty(email)){
                                mEmail.setError("Email is Required.");
                                return;
                            }
                            if(TextUtils.isEmpty(password)){
                                mPassword.setError("Password is Required.");
                                return;
                            }
                            if(password.length() < 6){
                                mPassword.setError("Password must be greater then 6 characters");
                                return;
                            }

                            ProgressBar.setVisibility(View.VISIBLE);
                            //authenticate the user

                            fAuth.signInWithEmailAndPassword(email,password).addOnCompleteListener(new OnCompleteListener<AuthResult>() {
                                @Override
                                public void onComplete(@NonNull Task<AuthResult> task) {
                                    if(task.isSuccessful()){
                                        Toast.makeText(MainActivity.this, "Logged in successfully", Toast.LENGTH_SHORT).show();
                                        startActivity(new Intent(getApplicationContext(), home.class));

                                    }
                                    else {
                                        Toast.makeText(MainActivity.this, "Error !! "+ task.getException().getMessage(), Toast.LENGTH_SHORT).show();
                                        ProgressBar.setVisibility(View.GONE);
                                    }
                                }
                            });

                        }
                    });
                    mRegisterBtn.setOnClickListener(new View.OnClickListener(){
                        @Override
                        public void  onClick(View view){
                            startActivity(new Intent(getApplicationContext(), register.class));
                        }
                    });

                    mforgotPassword.setOnClickListener(new View.OnClickListener(){
                        @Override
                        public  void onClick(View view){
                            final EditText resetMail = new EditText(view.getContext());
                            final AlertDialog.Builder passwordResetDialog = new AlertDialog.Builder(view.getContext());
                            passwordResetDialog.setTitle("Reset Password ?");
                            passwordResetDialog.setMessage("Enter Your Email To Receive Reset Link.");
                            passwordResetDialog.setView(resetMail);

                            passwordResetDialog.setPositiveButton("yes", new DialogInterface.OnClickListener() {
                                @Override
                                public void onClick(DialogInterface dialog, int which) {
                                        //extract email and send reset link

                                    String mail = resetMail.getText().toString();
                                    fAuth.sendPasswordResetEmail(mail).addOnSuccessListener(new OnSuccessListener<Void>() {
                                        @Override
                                        public void onSuccess(Void aVoid) {
                                            Toast.makeText(MainActivity.this, "Reset Link Sent To Your Email", Toast.LENGTH_SHORT).show();
                                        }
                                    }).addOnFailureListener(new OnFailureListener() {
                                        @Override
                                        public void onFailure(@NonNull Exception e) {
                                            Toast.makeText(MainActivity.this, "Error! Reset Link is not sent "+ e.getMessage(), Toast.LENGTH_SHORT).show();
                                        }
                                    });
                                }
                            });

                            passwordResetDialog.setNegativeButton("no", new DialogInterface.OnClickListener() {
                                @Override
                                public void onClick(DialogInterface dialog, int which) {
                                    //close the dialog
                                }
                            });

                            passwordResetDialog.create().show();
                        }
                    });
                }
            }
      
step 7:-
 
create new blank Activity:- give logout intent in Login 

copy Xml code:-
                <Button
                                  android:id="@+id/logoutBtn"
                                  android:layout_width="0dp"
                                  android:layout_height="wrap_content"
                                  android:layout_marginStart="16dp"
                                  android:layout_marginTop="16dp"
                                  android:layout_marginEnd="16dp"
                                  android:background="@drawable/logout1"
                                  android:onClick="logoutBtn"
                                  app:layout_constraintEnd_toEndOf="parent"
                                  app:layout_constraintHorizontal_bias="0.0"
                                  app:layout_constraintStart_toStartOf="parent"
                                  app:layout_constraintTop_toBottomOf="@+id/button8" />

add text and button for verification process :-

               <TextView
                                  android:id="@+id/veriedMsg"
                                  android:layout_width="wrap_content"
                                  android:layout_height="wrap_content"
                                  android:text="check out your Email"
                                  android:textColor="@android:color/holo_red_dark"
                                  android:visibility="invisible"
                                  app:layout_constraintBottom_toTopOf="@+id/resendcode"
                                  app:layout_constraintEnd_toEndOf="parent"
                                  app:layout_constraintStart_toStartOf="parent"
                                  app:layout_constraintTop_toTopOf="parent" />

                              <Button
                                  android:id="@+id/resendcode"
                                  android:layout_width="wrap_content"
                                  android:layout_height="wrap_content"
                                  android:layout_marginBottom="96dp"
                                  android:background="@drawable/verify"
                                  android:visibility="invisible"
                                  app:layout_constraintBottom_toBottomOf="parent"
                                  app:layout_constraintEnd_toEndOf="parent"
                                  app:layout_constraintHorizontal_bias="0.502"
                                  app:layout_constraintStart_toStartOf="parent" />

Step 8:- copy this program

                       public void logoutBtn(View view){
                          FirebaseAuth.getInstance().signOut();
                          startActivity(new Intent(getApplicationContext(), MainActivity.class));
                          finish();
                      }

 
