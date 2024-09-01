---
title: What is a 2-Factor Authentication(2FA)?
subtitle: A lesson from a popular fable for children
date: 2023-01-16T09:19:42+01:00
image: 0_qmGPBazgUYGQQXE0.jpg
draft: false
categories: [Tech Related]
tags: [Security, Authentication, 2FA, Password, MFA]
medium: https://medium.com/geekculture/what-is-a-2-factor-authentication-2fa-ee3248b0bb74
---

# The Wolf, the Kid, and the Goat

You probably have heard of this popular fable before.

Mother Goat in her wisdom had carefully _secured_ her house and her Kid before going out. She even reminded her Kid to check the _password_ before opening the door. Her password was good. It was _very long_ and contained _non-alphabets_ and _special characters._

As fate had it, a Wolf was nearby, he _eavesdropped_ and learned of the mother’s password. He was so sure of filling his belly. He even _mimicked_ the way mother Goat softly said her password

Yet the Kid was clever. He asked to see a white paw, _a feature only the mother Goat has_.

Not having that _extra authentication_ factor, the Wolf knew he has failed and left.

{{< figure src="0_qmGPBazgUYGQQXE0.webp" caption="“Two sureties are better than one ”— The Wolf, the Kid, and the mother Goat" >}}


# What is 2-Factors Authentication in plain English?

## The 2 Factors

As the fable demonstrates, mother Goat and her Kid used 2 factors to successfully secure their home and fend off attempts by the big bad Wolf. The 2 factors are:

*   **The password**: “Down with the Wolf and all his race!”
*   **The white paw**: a biological feature that only mother Goat has

Coincidentally, modern IT systems also commonly use this combination of a password and a biological factor (iris, fingerprint, face) to _authenticate_ users.

## What is Authentication?

Authentication is the Kid doing what he needed to do to be sure the one knocking on his door is indeed his mother.

In our modern context, authentication is a security system showing a login form, asking for user name, password, and an extra OTP via SMS or push notification… to ascertain the user attempting to log in is indeed a true and verified user.

In other words, authentication is the process of determining if users are indeed who they claim to be.

## So in plain English…

2FA is the Kid using a combination of a secured password and a biological feature to let only his true mother in the house.

It is also the modern secured system using another factor besides the traditional password mechanism to validate its logon user. The extra factor is commonly chosen from something _the user has_ (e.g. the phone number able to receive SMS) or something _the user is_ (e.g. fingerprint).