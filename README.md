## SendingAndReceivingSMS

In Android, you can use the SmsManager API or the devices Built-in SMS application to send a SMS.

### SmsManager API

The SmsManager manages SMS operations such as sending data, text, and pdu SMS messages. (pdu is a format)

 *See link for API: https://developer.android.com/reference/android/telephony/SmsManager.html*
 
The following is an example of an Android app, who can send and SMS to a mobile phone.
If you use the SmsManager API, you will have to connect your Android mobile device with your computer.
First of all, to be able to send a SMS, you need a  SEND_SMS permission.
Copy this line into your Android Manifests.xml file:

    <uses-permission android:name="android.permission.SEND_SMS" />
    

The following methods checks whether you have permission to send a SMS or not. If you have permission the phone number and the message from the GUI will be fetched and send as a SMS.


    protected fun sendSMSMessage() {
        phoneNo = phonenumber.text.toString()
        messageTxt = message.text.toString()

        if (ContextCompat.checkSelfPermission(this,
                Manifest.permission.SEND_SMS) != PackageManager.PERMISSION_GRANTED) {
            if (ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.SEND_SMS)) {
            } else {
                ActivityCompat.requestPermissions(this,
                        arrayOf(Manifest.permission.SEND_SMS),
                        MY_PERMISSIONS_REQUEST_SEND_SMS)
            }
        }
    }

    override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<String>, grantResults: IntArray) {
        when (requestCode) {
            MY_PERMISSIONS_REQUEST_SEND_SMS -> {
                if (grantResults.size > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    val smsManager = SmsManager.getDefault()
                    smsManager.sendTextMessage(phoneNo, null, messageTxt, null, null)
                    Toast.makeText(applicationContext, "SMS sent.",
                            Toast.LENGTH_LONG).show()
                } else {
                    Toast.makeText(applicationContext,
                            "SMS faild, please try again.", Toast.LENGTH_LONG).show()
                    return
                }
            }
        }
    }
    
Actually, you don't have to have all the lines of codes which checks for permissions to be able to send a SMS. 
Basically the lines of code you need is the uses-permission:

    <uses-permission android:name="android.permission.SEND_SMS" />

and the SmsManager:

    val smsManager = SmsManager.getDefault()
    smsManager.sendTextMessage(phoneNo, null, messageTxt, null, null)

The first argument passed to sendTextMessage() is the destination address to which the message has to be sent.

The second argument is the SMSC address (it’s also like a phone number generally) to which if you pass null, the default service center of the device’s carrier will be used.

Third argument is the text message to be sent in the SMS. 

The fourth and fifth arguments if not null must be pending intents performing broadcasts when the message is successfully sent (or failed) and delivered to the recipient.

* A pending intents performing broadcasts: is an operation which sends a broadcast message. These broadcasts are sent when an event of interest occurs.

### Build in application
If you use the devices Built-in SMS application, you can send a SMS without having your Android mobile device connected to your computer. In this example we use the Implicit Intent.

First of all, to be able to send a SMS, you need a SEND_SMS permission.
Copy this line into your Android Manifests.xml file:

    <uses-permission android:name="android.permission.SEND_SMS" />

The following method uses the ACTION_VIEW to start the SMS application. If you have multible SMS applications the user will have to choose from a list which SMS application to use. To send a SMS you need to specify the URI and the data type with the to methods setData() and setType(). 

    protected fun sendSMS() {
     Log.i("Send SMS", "")
     val smsIntent = Intent(Intent.ACTION_VIEW)

     smsIntent.data = Uri.parse("smsto:")
     smsIntent.type = "vnd.android-dir/mms-sms"
     smsIntent.putExtra("address", String("01234"))
     smsIntent.putExtra("sms_body", "Test ")

     try {
         startActivity(smsIntent)
         finish()
         Log.i("Finished sending SMS...", "")
     } catch (ex: android.content.ActivityNotFoundException) {
         Toast.makeText(this@MainActivity,
                 "SMS faild, please try again later.", Toast.LENGTH_SHORT).show()
     }
    }

By using the Build-in application you don't have to take care of permissions like in the first example. The Build-in application has already taken care of that.


But, this is also going to work and this is very simple!

        buttonSendSMS2.onClick {
            val number = "41100532"
            val text = "Jeg sender en anden sms"
            sendSMS(number, text) 
        }

        
