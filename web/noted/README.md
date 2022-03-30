# Noted
Web Exploitation - 500p
## Abstract
We use a CSRF through the unprotected login page to access stored XSS attack in our notes to exfiltrate the flag from the admin's notepad. This bypasses the SOP restrictions due to the unset CORS, which is a primary feature of the challenge.
## Problem Description

> I made a nice web app that lets you take notes. I'm pretty sure I've followed all the best practices so its definitely secure right? Note that the headless browser used for the "report" feature does not have access to the internet.

## Introduction
Noted was a problem designed by Ehhthing, which involved a web app containing notes, and a "report URL" function which would paste the flag in an anonymous account, then visits the reported URL via puppeteer.
## Research
We are provided with 3 hints:
> Are you sure I followed all the best practices?

> There's more than just HTTP(S)!

> Things that require user interaction normally in Chrome might not require it in Headless Chrome.

The first hint suggests that it may not be using best practices everywhere, and in fact, by quickly scanning through the source code, which is provided to us in the challenge, we notice that there is no CSRF token requirement for the login page, hinting to a possible CSRF attack.

While the challenge description states that the instance doesn't have internet access, due to infrastructure reasons at the time of the competition, the instance was indeed connected to the internet, though this may be changed when the challenge moves to picoGym. Therefore, the second hint likely points to the `javascript:` URI scheme possibly being used instead of visiting our reverse proxy.

The third hint stumped our team, but one of our members suggested an idea known as "transient activation" from the HTML standard. Essentially, headless chrome **does not** require user interaction for interactions such as `window.open()` and playing media.

## Attack
This problem involved a 2-pronged attack, one being CSRF, and one involving stored XSS. Early on, we became quite frustrated by the fact that our attacks did not work due to something known as SOP (or CORS because the error message would be a CORS policy violation). Some people reported that their attack worked on Firefox, but not on headless chrome. This is due to the different ways Firefox and Chrome handle the origin of `about:blank` as Firefox derives the origin from the previous website, while Chrome's origin becomes NULL. However, we quickly came to the realization that we likely needed to open new windows, and use a *CSRF attack* to access our own notepad. We took inspiration from a writeup on a previous problem, *TBDXSS* from pbctf 2021. By copying the login form and setting `target="_blank"` to our reverse proxy, we can login to our own account via the javascript `formID.submit()`, and **execute a stored XSS** on our notes. 

Next, we realized that we could use `window.open()` to "cache" the admin notepad, and use the name feature of the window to access it from our XSS, which would bypass the SOP/CORS policy.

## Execution
Both payloads can be found in this directory, one which is stored on a notepad of our choice [`notepad.payload`](notepad.payload), executing a POST request to our reverse proxy with the raw HTML data of the admin notepad, and one on a reverse proxy executing this exploit [`exploit.payload`](exploit.payload).

We created an account on the noted server with the username 'a' and the password 'a'. We created a note, and pasted our payload on it. Then, we reported our reverse proxy, giving us the flag `picoCTF{p00rth0s_parl1ment_0f_p3p3gas_386f0184}`.

## Afterthoughts
While this attack may be possible without internet, it would certainly prove some difficulty attempting to encode our payload into a `javascript:` URI scheme, especially with being limited to a 1024 char limit.

It should be noted that this exploit is extremely easy to patch, sanitization of the notes or adding a CSRF token to the login page would almost completely shut down this exploit.