---
title: "How I passed CKA"
date: 2021-12-23T17:01:00+02:00
tags: ["K8s","Certification"]
draft: false
ShowToc: true
TocOpen: true
cover:
  image: "/img/cka.png"
  alt: "cka_cover"
---

Some time ago I passed the Certified Kubernetes Administrator certification exam
and decide to write an article how to prepare for it.

Certified Kubernetes Administrator is certification assures that CKAs
have the skills, knowledge, and competency to perform the responsibilities of
Kubernetes administrators.

First of all, I need to say that CKA is a tough exam. Mainly because it checks your
ability to perform practical tasks instead of a bunch of multi-choice questions
for testing your knowledge.
Here are some tips how to prepare for an examination.

### My Background

I'm working with K8s almost 2 years. By this time I passed a way from a person who doesn't know what is it to
person who can work with k8s resources without any problem and understand how it works.

### Preparing

First advice: buy an exam on Cyber Monday.
CKA is one of the most expensive exams in the industry. It costs $375.
But one time in the year, on Cyber Monday you may buy an exam for only $187. It saves you 50% of costs.
After that, you will have a full year for preparation and examination.

Second advice: don't buy the course + exam bundle.
This course is expensive and it not enough
for exam preparation. Instead, I recommended buying Certified Kubernetes Administrator (CKA)
with Practise Tests on Udemy. This is a very detailed course help to learn what you need for
examination and practice tests give you test environments for honing your skills. I recommend
passing all labs several times.

Third advice: create bookmarks of Kubernetes docs.
Exam rules allow you to use Kubernetes documentation.
I recommend creating bookmarks for some difficult places like
updating cluster, backup and restore etcd, persistent volumes, etc.
It saves you a lot of time.

Fourth advice: use kubectl.
kubectl is a powerful CLI tool for Kubernetes. This tool can create resources by command and
always faster than creating manifests. By the way, you may use kubectl to generate a manifest and
edit it later. Learn how to use kubectl. This [doc](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) will help you

Fifth advice: learn base VIM's functionality.
By default, you will have Vim as a main editor. This is not a simple editor and without knowledge,
you will now even be able to exit it. So that's why you should know how to use VIM.
First of all pass vimtutor. It helps you to learn the basic functions of VIM. And who knows maybe
you will like VIM and it will become your main editor(BTW this article was written on VIM).

Sixth advice: use killer.sh simulator
A lot of people don't know that certification includes 2 attempts in [killer.sh](https://killer.sh). It simulates real
examination and help you to understand how it will take place.
This simulator is harder than the real exam and after passing you will have a result with a detail explanation.

### Result

![cka](/img/cka_cert.png)

As result after 2 hours of examination and 24 hours of expectation, I received the desired message of congratulations.
I hope this article with bits of advice will help you to pass the exam and become a Certified Kubernetes Administrator.   
Good luck.
