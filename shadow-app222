name: Build Android APK
on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Unzip project files
        run: |
          unzip -q remix-shadow-care-1.zip -d extracted_app
          if [ -d "extracted_app/remix-shadow-care-1" ]; then
            cp -r extracted_app/remix-shadow-care-1/* .
          else
            cp -r extracted_app/* .
          fi

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install & Build
        run: |
          npm install --legacy-peer-deps
          npm run build

      - name: Install Capacitor & Android
        run: |
          npm install @capacitor/core @capacitor/cli @capacitor/android
          npx cap init "Shadow Care" "com.shadowcare.app" --web-dir dist
          npx cap add android
          npx cap sync android

      - name: Setup Java JDK
        uses: actions/setup-java@v4
        with:
          distribution: 'zulu'
          java-version: '17'

      - name: Build APK
        run: |
          cd android
          chmod +x gradlew
          ./gradlew assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: Shadow-Care-APK
          path: android/app/build/outputs/apk/debug/app-debug.apk
