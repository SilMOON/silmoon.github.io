---
layout: post
title: Fix DNS resolution issue for my own domain
categories: [Website]
tags: [Website]
fullview: true
---

Yesterday I found that my domain name for my Mastodon instance couldn't be resolved. So I check my Whois information on ICANN Lookup and found that my status became "serverHold". Generally this code means your domain name potentially have risks of spam or other illegal matters, but I only use this domain for my small Mastodon instance and the email service it runs is simply for sending verification email to new users. But I still had to email to the registry of my domain and when they reactivate my domain name, I was told that my domain was also blacklisted by another anti-spam organization. As a result, I had to contact that organization in order to get my domain out of their blacklist. I was told that the reason I was blacklisted is that I ran an outgoing email service without setting verified rDNS. To solve this problem, I firstly set rDNS of my server, this step is quite easy thanks to Vultr's easy to use interface. Next I need to apply for a delist key from their delist tool. In order to get the key, the anti-spam website need a postmaster email address with my domain name, so I create it using the email-forwarding tool provided by my domain name provider. And then I simply get the key and get my domain out of the blacklist successfully.