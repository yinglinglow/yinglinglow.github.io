---
layout: post
title: "Uploading files to Google Colab"
date: 2018-03-17
---

so i'm embarking on fast.ai course (fingers crossed!!)

and the first thing is, they need a GPU for the course (duh)

guess who uses a Mac and is unwilling to set up GCP even though I still have free credits _shudders_ (i exaggerate, i'm just lazy), and am too poor for AWS (sad).

they recommended something (crestle)? again, i don't want to pay.

so... Google Colab is my new best friend!!!

FREE. slightly painful to set up (still better than GCP), and slower than a legit GPU but still a GPU nonetheless.

---

i've set it up before, but here it is again, for people who wonder - *how do I actually get my data files up onto Google Colab*? I'm not talking about one excel file or two (you can see instructions for that [here](https://stackoverflow.com/questions/47320052/load-local-data-files-to-colaboratory))

but what if i have like a thousand images (or more) for my dataset?

the easiest way is GOOGLE DRIVE.

---

copy this bunch of code below and run it - basically it will provide you with two links, one to sign in to Google, and one to allow it access to your Google Drive.

```python
!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse
from google.colab import auth
auth.authenticate_user()
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()
import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}

```

once that's done, put this in the next cell

```python
!mkdir -p drive
!google-drive-ocamlfuse drive

```

basically if you do a `!ls` now, likely you will see `datalab   drive`, and if you do a `!ls drive` you can see all the contents of your Google Drive.

so for example if i save my file called `abc.txt` in a folder called `ColabNotebooks`, i can basically access it via a path `drive/ColabNotebooks/abc.txt`

tada!!!!!

have fun guys.

---

if you need to install keras, use this:

```python
!pip install -q keras
```

