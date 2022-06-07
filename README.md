<div id="top"></div>

<!-- HEAD -->
<br />
<div align="center">
  <h3 align="center">react-native-build-ios</h3>

  <p align="center">
    GitHub Action to build React Native iOS app and publish to Testflight
  </p>
</div>

<!-- GETTING STARTED -->
# Getting Started

### Prerequisites

By using this GitHub Action I already assume you know how to build your React Native app and publish it to Testflight manually.

I also recommend making a folder to save files that we will get creating below.

### Get Certificate Request
Open the Keychain Access application on your Mac, then click on 'Keychain Access' top left to open the drop down, go to 'Certificate Assistant' and click 'Request a Certificate From a Certificate Authority'. Check 'Saved to disk' and click Continue. Save the file.

### Create Certificate
1. Go to [your Apple Developer Dashboard](https://developer.apple.com/account/).
2. Select 'Certificates, IDs & Profiles' from the sidebar.
3. Click on Certificates and then the + button beside Certificates. Select Apple Distribution under Software and then Continue. 
4. Upload the file we created from 'Get Certificate Request'.
5. Click the Download button.

### Create Identifier
1. After downloading the file from the steps above, select 'Identifiers' from the side nav and then the + button beside 'Identifiers'.
2. Select 'App IDs' and then Continue.
3. Select 'App' as the type and then Continue.
4. Add a Description and a Bundle ID. You can also select any capabilities your app uses and then Continue.
5. Once again, select any capabilities your app uses and then Register.

### Create Profile 
1. Select 'Profiles' from the nav bar and then select the + button beside 'Profiles'.
2. Select 'App Store' under Distribution and then Continue.
3. Select the Bundle ID we created in the above steps and then Continue.
4. Select the Certificate we created in the above steps and then Continue.
5. Add a Provisioning Profile Name and then select Generate.
6. Click Download.

### Xcode Setup

1. Open your project in Xcode by using the file ios/XXXX.xcworkspace (not the .xcodeproj file).
2. Select your project under the Targets section > Signing & Capabilities > Release.
3. Uncheck Automatically manage singing.
4. In Provisioning Profile, import the profile file that be generated in the above steps.
5. If the certificate associated with the profile is already installed on your device you should see it in Signing Certificate also, you should see the team name appear automatically.

### Generate App Store API Key
1. Go [here](https://appstoreconnect.apple.com/access/api) to generate a App Store Connect API Key.
2. Click on the + button.
3. Give it a name and admin access.
4. Download the API Key, take note of the Issuer ID at the top and the Key ID in the table.


# Action Secrets

For the GitHub action to run, you will need to add these variables to your repository.

#### `APP_PROJECT_NAME`

Go to your ios folder in your project and its the name that comes before the .xcworkspace and .xcodeproj files.

#### `IOS_MOBILE_PROVISION_BASE64`

Open a terminal in the folder where you downloaded the Profile to and run the following command after changing CHANGE_ME to the name of your file.

```sh
  openssl base64 < CHANGE_ME.mobileprovision | tr -d '\n' | tee my-profile.base64.txt
```

#### `IOS_P12_BASE64`

1. Open the Certificate file we created earlier in the section 'Create Certificate'.
2. Open the Keychain Access app ,select "My Certificates" at the top and locate the cetificate we created.
3. Expand the cerificate to see the corresponding private key.
4. Select both items, the pight click and choose "Export 2 items...".
5. Save the file as a .p12 to the folder we created earlier and chose a strong password for the file. Take note of the password as we will need it later.
6. Run the following command to generate the base64(Change Certificates.p12 if you renamed the file).

```sh
  openssl base64 < Certificates.p12 | tr -d '\n' | tee Certificates.base64.txt
```

#### `IOS_CERTIFICATE_PASSWORD`

Enter the above password we used while exporting the .p12 file.

#### `IOS_TEAM_ID`

Your Team ID can be found [here](https://developer.apple.com/account/#!/membership/).

#### `APPSTORE_ISSUER_ID`

Issuer ID can be found [here](https://appstoreconnect.apple.com/access/api).

#### `APPSTORE_API_KEY_ID`

API KEY ID can be found [here](https://appstoreconnect.apple.com/access/api).

#### `APPSTORE_API_PRIVATE_KEY`

In a text editor, open the file we generated from steps 'Generate App Store API Key' and paste in the whole file contents.




<!-- TIPS -->
## Tips

To get rid of the 'Missing Compliance' warning every time you deploy a build to Testflight, add the following to the end of your Info.plist file.

```js
   <key>ITSAppUsesNonExemptEncryption</key>
   <false />
```

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments
I created this action after reading a blog post by Youssouf EL Azizi which can be found [here](https://www.obytes.com/blog/react-native-github-action)
