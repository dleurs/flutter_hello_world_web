# Hello World flutter web

Following this guide :<br/>
https://medium.com/@zonble/use-github-pages-to-host-your-flutter-web-app-as-an-example-of-your-flutter-package-cb7b5b726eb1

1. Generate a github access token in https://github.com/settings/tokens
2. Add this token in your project, in YourProject > Settings > Secrets. This secret should be called ACCESS_TOKEN
3. Create .github/workflows/workflow.yml and paste. Please replace [YOUR_USER_NAME] and [YOUR_REPO_NAME]
```bash
mkdir .github;
mkdir .github/workflow;
touch .github/workflows/workflow.yml;
```
```yaml
name: Flutter Web

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest 
    steps:
    - uses: actions/checkout@v1
    - uses: actions/setup-java@v1
      with:
        java-version: '12.x'
    - uses: subosito/flutter-action@v1
      with:
        flutter-version: '1.12.x' # you can use 1.12
        channel: 'beta' # Currently you have to use beta channel for Flutter web.
    - name: Upgrades flutter
      run: flutter upgrade
      working-directory: ./example
    - name: Enable Web
      run: flutter config --enable-web
      working-directory: ./example
    - name: Install dependencies
      run: flutter packages get
      working-directory: ./example
    - name: Build Web
      run: flutter build web
      working-directory: ./example
    - name: Deploy
      run: |
        cd example/build/web
        git init
        git config user.name  "CI"
        git config user.email "flutter-ci@github.com"
        git remote add secure-origin https://${{ secrets.ACCESS_TOKEN }}@github.com/dleurs/flutter_hello_world_web.git
        git checkout -b gh-pages
        git add .
        git commit -m "Updated docs"
        git push --force secure-origin gh-pages
```