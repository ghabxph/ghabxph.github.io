---
layout: post
title:  "Setting up a Kubernetes Cluster using Terraform to any VPS"
date:   2020-10-20 17:21:10 +0800
categories: blogging
comments: true
---

#### **This page is under construction**
So, you are seeing this latest page. I committed it to github to save my work. And I am allowing you
to see this very latest post of mine. I am constantly working on this page. Stay tuned!

## Introduction

![WordPress](/assets/images/kubernetes-terraform.webp)
In this tutorial, I am going to share with  you how to set  up  a Kubernetes cluster using Terraform
and Ansible  for any  of your VPS servers.  This is  for people like  you who  want to create a very
cheap  Kubernetes infrastructure for personal stuff.  Plus, you also want  to learn to use Terraform
for the very first time, and your first project is to create your very own Kubernetes Cluster!

I also like to  tell you that  this is my first  time  using Terraform.  I have used  Ansible before
setting up  my Kubernetes cluster. Now, I'm going to  add Terraform  to the picture.  In order to be
efficient and  hit two birds in one stone, I ambitiously create this tutorial to document my journey
on being cheap and learn Terraform at the same time.

### Prerequisites
In this tutorial,  I have  4 VPS  hosted in Contabo.  My design  is one master node and three worker
nodes.  Just a very simple setup.  You need to install Ansible and Terraform on  your local machine.

### What this tutorial is not
I am not going to teach  you how to  install ansible or terraform.  I will keep a document on how to
install  it  on  Arch  Linux as  it  is  my  machine's  operating  system.  But  that's  just it for
documentation purposes.

### My use-case
I am using Contabo VPS which is way cheaper compared to Digital Ocean or Linode.  But Contabo is not
as modern as  these aforementioned platforms.  I also like to keep  a GitOps practice of managing my
infrastructure so I chose Terraform and Ansible to do the job.  Through this article,  I am going to
share with you how to do this kind of setup.

If you're  like me  that are  on a  budget and  would like  to have  your own Kubernetes cluster the
cheapest as possible then you must continue reading this article.

### Installing Ansible and Terraform on your Local Machine
