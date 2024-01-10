+++ 
draft = false
date = 2024-01-11T10:37:52+13:00
title = "Reading TOTP QR codes with Python and OpenCV"
description = "Read QR codes with TOTP URL's in them and then use them"
slug = ""
authors = ["Srđan Đukić"]
tags = ["python", "opencv", "qrcode", "totp"]
categories = []
externalLink = ""
series = []
+++
# Read a QR code and extract the data

In our case, we want to get the TOTP URL out of a QR code. Documentation for the OTP URL format can be found [here on the Yubico website](https://docs.yubico.com/yesdk/users-manual/application-oath/uri-string-format.html).

Sample code to get the data out of a QRCode image using the OpenCV2 library is [here](https://gist.github.com/srkiNZ84/a0a634f69584f964b8226bb452e8a034).

This should produce some output such as the following:

  otpauth://totp/Example%20Company%3AJohn.Doe%40example.com?secret=aaaabbbbcccceee1123m&issuer=Google

we can then use the [PyOTP library](https://github.com/pyauth/pyotp) to extract the TOTP values as needed:

```
import pyotp
import time

totp = pyotp.parse_uri("otpauth://totp/Example%20Company%3AJohn.Doe%40example.com?secret=aaaabbbbcccceee1123m&issuer=Google")

totp.now() # => '609257'
time.sleep(30)
totp.now() # => '924749'
```
