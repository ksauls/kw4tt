+++
date = '2026-10-27T20:48:35-04:00'
draft = false
title = 'Recovering From a Failure of my Truenas OS Drive'
+++


There's nothing quite like waking up to find that the server that you rely on for most of your services, and on which you store your writing, is not responding and is making that sickening clicky noise that says "dead drive". Welp, that was my morning.

Now I've only been using Truenas about a year so I'm not really familiar with a lot of the nuances of managing these kinds of events. I was very concerned that the process was going to be long, convoluted, and would end up borking my data.  

I was wrong.

The wonderful thing about Truenas, I discovered, is that because the data does not live on the OS drive the data itself is safe.  That said, I have run into similar issues before (Windows, I'm looking at you) where even though the data was on another drive the pointers to that data didn't. As a result even though the data was on another drive, accessing that data was a huge mess.

Not so with Truenas.

### What I did

So, after a quick internet search I downloaded the latest version of Truenas to a USB drive, pulled the old, dead drive out and popped in a new one.  I booted the machine up to install Truenas on the new drive and five minutes later I was back in business.

Now I had not made a backup copy of my configuration (I thought I had, but couldn't find it.) Fortunately, my configuration is really simple so it only took a couple of minutes to recreate my account. Once that was done I imported the data drives, rebooted (which I don't think was actually necessary) and everything back on line. All my apps were there, complete with data and correct setting. Nothing required adjustment.

Literally, the process took less than 30 minutes from dead drive to fully operational!  That was absolutely freaking awesome.  In my forty years of mucking around with computers I've never had as smooth a recovery as this turned out to be.

When I first moved over to Truenas as an experiment I wasn't sure I'd stay with it. Now there is no question. I may never leave it!
