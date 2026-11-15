# TeinBooster

TeinBooster's Android source project is stored in the repository as `TeinBooster.zip`. The GitHub Actions workflow extracts the archive, builds a release APK, and publishes it as a GitHub Release asset.

## Build a release APK

1. Open **Actions** in this repository.
2. Select **TeinBooster - Build and Release APK**.
3. Choose **Run workflow**.
4. Enter a release tag, for example `v1.0.0`. Use a new tag for each release.
5. Open the completed run and follow the link to the created GitHub Release.
6. Download `TeinBooster.apk` from the release assets.

The workflow is manually triggered; it does not run automatically on every push.

## Source archive requirements

The repository must contain `TeinBooster.zip` at its root. The archive is expected to contain an Android Gradle project with:

- `gradlew` (the Gradle wrapper script);
- `gradle/wrapper/gradle-wrapper.jar`;
- `settings.gradle` or `settings.gradle.kts`;
- an `app` module that can run `assembleRelease`.

The workflow extracts the archive into the runner's temporary directory and locates the project by finding `gradlew`.

## CI build environment

The current workflow uses:

- Ubuntu GitHub-hosted runner.
- Eclipse Temurin JDK 17.
- Android SDK platform 34 and Build Tools 34.0.0.
- The Gradle wrapper included in the source archive.
- Gradle task `assembleRelease`.

The workflow checks for a release APK under `app/build/outputs/apk/` and copies the selected APK to the release asset name `TeinBooster.apk`.

## Troubleshooting

**The source ZIP or Gradle wrapper is missing**  
Check that the root archive is named exactly `TeinBooster.zip` and that the wrapper files are included in it.

**The workflow cannot find the Android project**  
Make sure the archive contains both a Gradle settings file and the application module. The workflow searches for `gradlew` and uses its parent directory as the project root.

**The release APK is not produced**  
Open the failed Actions run and inspect the first Gradle error. Check that the app module has a valid `release` build type and that any required signing configuration is available to the build.

Do not commit signing keystores, passwords, API keys, or other private credentials to the repository. Configure sensitive values as GitHub Actions secrets when the project requires them.
