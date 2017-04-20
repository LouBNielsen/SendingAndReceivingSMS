## SendingAndReceivingSMS

In Android, you can use the SmsManager API or the devices Built-in SMS application to send a SMS.

### SmsManager API

The SmsManager manages SMS operations such as sending data, text, and pdu SMS messages. (pdu is a format)

 *See link for API: https://developer.android.com/reference/android/telephony/SmsManager.html*
 
The following is an example of an Android app, who can send and SMS to a mobile phone.
If you use the SmsManager API, you will have to connect your Android mobile device with your computer.
First of all, to be able to send and SMS, you need a  SEND_SMS permission.
Copy this line into your Android Manifests.xml file:

    <uses-permission android:name="android.permission.SEND_SMS" />
    

The following methods checks whether you have permission to send a SMS or not. If you have permission the phone number and the message from the GUI will be fetched and send as an SMS.


    protected void sendSMSMessage() {
        phoneNo = txtphoneNo.getText().toString();
        message = txtMessage.getText().toString();

        if (ContextCompat.checkSelfPermission(this,
                Manifest.permission.SEND_SMS)
                != PackageManager.PERMISSION_GRANTED) {
            if (ActivityCompat.shouldShowRequestPermissionRationale(this,
                    Manifest.permission.SEND_SMS)) {
            } else {
                ActivityCompat.requestPermissions(this,
                        new String[]{Manifest.permission.SEND_SMS},
                        MY_PERMISSIONS_REQUEST_SEND_SMS);
            }
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode,String permissions[], int[] grantResults) {
        switch (requestCode) {
            case MY_PERMISSIONS_REQUEST_SEND_SMS: {
                if (grantResults.length > 0
                        && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                        SmsManager smsManager = SmsManager.getDefault();
                        smsManager.sendTextMessage(phoneNo, null, message, null, null);
                    Toast.makeText(getApplicationContext(), "SMS sent.",
                            Toast.LENGTH_LONG).show();
                } else {
                    Toast.makeText(getApplicationContext(),
                            "SMS faild, please try again.", Toast.LENGTH_LONG).show();
                    return;
                }
            }
        }

    }
    
Actually, you don't have to have all the lines of codes with checks for permissions, to be able to send a SMS. 
The lines of code you need is the uses-permission:

    <uses-permission android:name="android.permission.SEND_SMS" />

and the SmsManager:

    SmsManager smsManager = SmsManager.getDefault();
    smsManager.sendTextMessage(phoneNo, null, message, null, null);



### Build in application
If you use the devices Built-in SMS application, you can send a SMS without having your Android mobile device connected to your computer.

