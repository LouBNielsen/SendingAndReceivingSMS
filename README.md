## SendingAndReceivingSMS

In Android, you can use SmsManager API or devices Built-in SMS application to send a SMS's.

### SmsManager API
The following is an example of an Android app, who can send and SMS to a mobile phone.
If you uses the SmsManager API, you will have to connect your Android mobile device with your computer.
First of all, to be able to send and SMS, you need a  SEND_SMS permission.
Copy this line into your Android Manifests.xml file:

    <uses-permission android:name="android.permission.SEND_SMS" />

### Build in application
