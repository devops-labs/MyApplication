node {
// Check out the source code
 git 'https://github.com/devops-labs/MyApplication.git'
// Build the app using the 'debug' build type,
// and allow SDK components to auto-install
withAndroidSdk {
 sh './gradlew clean assembleDebug'
}
// Store the APK that was built
 archive '**/*-debug.apk'
}