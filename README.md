# Hello World flutter web

Following this guide :<br/>
https://medium.com/@zonble/use-github-pages-to-host-your-flutter-web-app-as-an-example-of-your-flutter-package-cb7b5b726eb1

1. Generate a github access token in https://github.com/settings/tokens
2. Add this token in your project, in YourProject > Settings > Secrets. This secret should be called ACCESS_TOKEN
3. Create .github/workflows/workflow.yml and paste. Please replace [YOUR_USER_NAME] and [YOUR_REPO_NAME]
```bash
mkdir .github;
mkdir .github/workflows;
touch .github/workflows/workflow.yml;
```
```yaml
name: Flutter Web

on:
  push:
    branches:
    - master

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
        #flutter-version: '1.12.x'
        channel: 'beta' # Currently you have to use beta channel for Flutter web.
    - run: flutter upgrade # You also get the flutter version
    - run: flutter config --enable-web
    - run: flutter packages get
    - run: flutter build web
    - run: cd build/web
    - run: git init
    - run: git config user.name  "CI"
    - run: git config user.email "flutter-ci@github.com"
    - run: git remote add secure-origin https://${{ secrets.ACCESS_TOKEN }}@github.com/dleurs/flutter_hello_world_web.git
    - run: git checkout -b gh-pages
    - run: git add .
    - run: git commit -m "Updated docs" --allow-empty
    - run: git push --force secure-origin gh-pages
```
4. YourProject > Settings > Option > Github Pages with branch gh-pages