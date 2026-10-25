name: Build APK
on:
  push:
  workflow_dispatch:
permissions:
  contents: write
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: unzip -q Adventure.zip
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v3
        with:
          gradle-version: 8.7
      - run: gradle assembleDebug --no-daemon
        working-directory: Adventure
      - name: Publish APK
        env:
          GH_TOKEN: ${{ github.token }}
          GH_REPO: ${{ github.repository }}
        run: |
          gh release view latest >/dev/null 2>&1 || gh release create latest --title "Himalaya Ki Ghanti" --notes "Latest APK"
          cp Adventure/app/build/outputs/apk/debug/app-debug.apk Adventure.apk
          gh release upload latest Adventure.apk --clobber
