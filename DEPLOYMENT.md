# Deployment Guide

This guide covers deploying the Mobile Number Tracking App to various platforms.

## Table of Contents
1. [Google Play Store](#google-play-store)
2. [Apple App Store](#apple-app-store)
3. [GitHub Releases](#github-releases)
4. [Version Management](#version-management)

## Google Play Store

### Prerequisites
- Google Play Developer Account ($25 one-time fee)
- Signed APK or App Bundle
- App Store Listing prepared

### Steps

1. **Generate Signing Key**
```bash
keytool -genkey -v -keystore my-release-key.keystore \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias my-key-alias
```

2. **Build Release APK**
```bash
npm run build:android
```

3. **Sign the APK**
```bash
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 \
  -keystore my-release-key.keystore app-release.apk my-key-alias
```

4. **Upload to Google Play**
   - Go to Google Play Console
   - Create new app
   - Fill in app details
   - Upload APK/App Bundle
   - Complete store listing
   - Submit for review

### Timeline
- First release: 2-4 hours
- Updates: Usually within 1 hour

## Apple App Store

### Prerequisites
- Apple Developer Account ($99/year)
- Mac computer with Xcode
- App Store Connect access
- TestFlight ready

### Steps

1. **Create App in App Store Connect**
   - Bundle ID: `com.mobiletracker.app`
   - App Name: Mobile Number Tracking
   - Category: Utilities

2. **Build for Release**
```bash
npm run build:ios
```

3. **Create Archive**
   - Open Xcode project
   - Select Generic iOS Device
   - Product → Archive

4. **Sign and Upload**
   - Use Xcode to validate
   - Xcode → Organizer → Upload

5. **Submit for Review**
   - Go to App Store Connect
   - Add app details (screenshots, description)
   - Set pricing
   - Submit for App Review

### Timeline
- First review: 24-48 hours
- Updates: Usually within 24 hours

## GitHub Releases

### Creating a Release

1. **Update Version**
```bash
# In package.json
{
  "version": "1.0.0"
}
```

2. **Tag the Release**
```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

3. **GitHub Actions Automatic Release**
   - Automatic CI/CD triggers
   - Creates GitHub Release
   - Uploads APK and IPA
   - Generates release notes

4. **Manual Release (if needed)**
   - Go to Releases
   - Click "Draft a new release"
   - Choose tag
   - Add release notes
   - Attach APK/IPA files
   - Publish

## Version Management

### Semantic Versioning

Use format: `MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes (1.0.0)
- **MINOR**: New features (1.1.0)
- **PATCH**: Bug fixes (1.0.1)

### Version Bump Script

```bash
# Create script at scripts/bump-version.js
const fs = require('fs');
const package = JSON.parse(fs.readFileSync('package.json'));

const [major, minor, patch] = package.version.split('.').map(Number);
// Increment logic here
package.version = `${major}.${minor}.${patch + 1}`;

fs.writeFileSync('package.json', JSON.stringify(package, null, 2));
```

### Release Checklist

- [ ] Tests pass locally
- [ ] Code reviewed
- [ ] Version bumped
- [ ] CHANGELOG updated
- [ ] Documentation updated
- [ ] Tag created
- [ ] CI/CD successful
- [ ] Artifacts created
- [ ] Submitted to app stores

## Troubleshooting

### Android Issues
- **APK not signing**: Check keystore password and alias
- **Build fails**: Ensure Android SDK is installed
- **Upload rejected**: Check app version code increment

### iOS Issues
- **Certificate expires**: Renew in Apple Developer
- **Code signing fails**: Check provisioning profiles
- **App Store rejection**: Review guidelines at developer.apple.com

### CI/CD Issues
- **Workflow not triggering**: Check branch protection rules
- **Build timeout**: Increase timeout in workflow
- **Secrets not found**: Verify secrets in GitHub Settings

## Support

For deployment issues:
- Check GitHub Actions logs
- Review app store developer docs
- Open an issue on GitHub
- Contact: support@mobiletracker.co.za

---

Last updated: June 2026
