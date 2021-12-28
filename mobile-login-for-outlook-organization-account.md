# Mobile login for outlook organization account

### SSO login failure

For microsoft organization account, sometime it will direct you to your organization's SSO page when you try to login from a mobile email app (e.g. outlook app or other 3rd party email app like Spark). However not all organization's SSO work on mobile device, for example I got this error when try to login an Northeastern outlook account on my phone:

&#x20;![](.gitbook/assets/IMG\_1812.PNG)

### Solution

We can use [microsoft authenticator app](https://support.microsoft.com/en-us/account-billing/download-and-install-the-microsoft-authenticator-app-351498fc-850a-45da-b7b6-27e523b8702a) to workaround this. First you need to add authenticator app to you sign-in options:

1. login to you organization account on laptop, go to account settings -> security info -> add method -> select authenticator app and continue.&#x20;
2. Follow the steps to enable authenticator app. You will probably need to download the authenticator app on you phone and login through QR code in the app.

![](<.gitbook/assets/Screen Shot 2021-12-28 at 10.26.33 AM.png>)

Then you are good to go, try to login from your outlook client again, it will direct you to your authenticator app for authentication.
