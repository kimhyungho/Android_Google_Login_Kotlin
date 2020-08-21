Naver Login By Using Kotlin
============
How to login/sign up to Naver Account

Official Homepage
-----------
https://console.cloud.google.com/

Version
-----------
com.google.android.gms:play-services-auth:18.1.0

https://developers.google.com/identity/sign-in/android

First Step
-----------
공식 홈페이지 접속 후 프로젝트 생성

API 및 서비스

OAuth 동의 화면 -> 앱 이름, 로고, 도메인등 지정 후 저장

사용자 인증 정보 -> 사용자 인증 정보 만들기 -> OAuth 클라이언트 ID -> 애플리케이션 유형 -> Android, 이름, 패키지 이름, SHA-1 인증서 등록

(SHA-1은 Android Studio 우측 상단 gradle -> 앱이름 -> Task -> android -> signinReport 더블클릭 후 하단 Run 탭에서 확인 가능)


사용자 인증 정보 -> 사용자 인증 정보 만들기 -> OAuth 클라이언트 ID -> 애플리케이션 유형 -> 웹 애플리케이션, 이름 등록


사용법
-----------
* 앱 수준의 build.gradle
```
dependencies {
    implementation 'com.google.android.gms:play-services-auth:18.1.0'
}
```

* AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="">

    <uses-permission android:name="android.permission.INTERNET" />

</manifest>
```
* LoginActivity
```
import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import com.google.android.gms.auth.api.signin.GoogleSignIn
import com.google.android.gms.auth.api.signin.GoogleSignInAccount
import com.google.android.gms.auth.api.signin.GoogleSignInClient
import com.google.android.gms.auth.api.signin.GoogleSignInOptions
import com.google.android.gms.common.api.ApiException
import com.google.android.gms.tasks.Task
import kotlinx.android.synthetic.main.activity_login.*

class LoginActivity : AppCompatActivity() {

    lateinit var mGoogleSignInClient: GoogleSignInClient
    private val RC_SIGN_IN = 9001

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        val gso = GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
            .requestIdToken("YOUR_CLIENT_ID")
            .requestEmail()
            .build()

        mGoogleSignInClient = GoogleSignIn.getClient(this, gso)

        google_login_button.setOnClickListener {
            signIn()
        }
    }
    private fun signIn() {
        val signInIntent = mGoogleSignInClient.signInIntent
        startActivityForResult(
            signInIntent, RC_SIGN_IN
        )

    }
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (requestCode == RC_SIGN_IN) {
            val task =
                GoogleSignIn.getSignedInAccountFromIntent(data)
            handleSignInResult(task)
        }
    }

    private fun handleSignInResult(completedTask: Task<GoogleSignInAccount>) {
        try {
            val account = completedTask.getResult(
                ApiException::class.java
            )
            // Signed in successfully
            val googleId = account?.id ?: ""
            Log.i("Google ID",googleId)

            val googleFirstName = account?.givenName ?: ""
            Log.i("Google First Name", googleFirstName)

            val googleLastName = account?.familyName ?: ""
            Log.i("Google Last Name", googleLastName)

            val googleEmail = account?.email ?: ""
            Log.i("Google Email", googleEmail)

            val googleProfilePicURL = account?.photoUrl.toString()
            Log.i("Google Profile Pic URL", googleProfilePicURL)

            val googleIdToken = account?.idToken ?: ""
            Log.i("Google ID Token", googleIdToken)

        } catch (e: ApiException) {
            // Sign in fail
            Log.e(
                "failed code=", e.statusCode.toString()
            )
        }
    }
}
```
ScreenShots
-----------
<div>
<img whdth = "50" src = "https://github.com/kimhyungho/Naver_login_Kotlin/blob/master/naver_login_image/11.JPG">
<img whdth = "50" src = "https://github.com/kimhyungho/Naver_login_Kotlin/blob/master/naver_login_image/2.JPG">
<div>
