---
layout: post
title: "trying the AWS spot instance"
date: 2018-01-24
---

click spot requests

request spot instances

AMI: search for ami

select instance type

select the default security group

auto-assign IPv4 public IP: Enable

select key-pair name

set maximum price

launch

---

on the spot instance page, click on the request, history to track the status.

it should take around 4-5 minutes to launch the instance.

once launched, click to navigate to the instance page as per the usual instance.

---

if you have an error to ssh in (timed out), check the error message to see if it is because of a port error.

mine was port 22. 

to resolve this:
check the security group name of the instance you are running (scroll to the right to check).

click security groups under network and security.
select the security group that your instance is running on.
select inbound, click edit.

add custom, and type 22 in the port range.

save.


---

for people who want to sync from local to s3 quicker - use `aws s3 sync <source> <target>`

for example: `aws s3 sync . s3://my-bucket/path`