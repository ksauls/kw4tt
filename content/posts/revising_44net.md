+++
date = '2026-11-13T18:12:30-05:00'
draft = 
title = 'Moving my 44Net from Raspberry Pi to Mikrotik'
+++

A few days ago I documented the process I followed for creating a 44Net account, installing the 44Net WireGuard service on an RPI, and then creating a /28 subnet.  The RPI setup, using a USB network adapter was rather janky and I knew that that would not be a long-term solution. In order to actually use the subnet I would  need to at least add a switch. My desk is messy enough without adding more wires and devices.  A single device with multiple ethernet ports, that is a router, made a lot more sense. 

Over the years I've worked with a lot of different router software including Tomato, OpenWRT, EdgerouterOS, and other OSes that come on consumer grade devices. I hadn't used a Mikrotik router, though I had heard a lot of positive things about them. Because the email from 44Net included a configuration file for Mikrotik I thought it might be time to see what it's all about.

Now, to be honest, the UI is a bit confusing. Once you dig into it a bit more you realize the functions are pretty much the same as you'd find on other routers. It's just not quite a intuitive as some (though it struck me as a hell of a lot more intuitive than EdgerouterOS from Ubiqiti!)

For the most part the transition went fairly smoothly. Until I went to setup the firewall.  My goal was to block all access from the internet to the router. And, while I wanted to block all access from the 44Net router to my main LAN, I wanted to be able to access the devices on the 44Net from my main LAN. I was able to do this on the RPI without too much difficulty using UFW. The Mikrotik, though, apparently uses it's own proprietary firewall. Well, crap. 

I spent most of a day trying to work out exactly what I needed to do to accomplish my goal. With no joy.

Now the thing is I enjoy learning so taking some time to learn a bit about how to configure Mikrotik's firewall was kinda fun. For a while. But then you get to a point where frustration mounts and you just want to move on to the next thing. 

### Turning to Claude for Guidance

Early the next morning my brain kicked into gear and I thought, "I wonder if Claude could help me out here." Now I'm not a huge fan of AI and have spent almost no time using it, so I wasn't even sure I knew how to create prompt to get the information I needed.  The instructions said to tell Claude what I wanted in plain language, just as I might tell a friend. So I did. 

I described my setup, including IP addresses, and then described what I wanted to accomplish. Badda-bing!  Claude gave me detailed instructions on exactly what I needed to do. And they worked!

Well, almost.  Everything worked except that I couldn't get to the internet from the network. I'm not a complete dolt, so I walked through the process again and realized that while Claude had named the wireguard tunnel 'wireguard1' the 44Net configuration file had named it 'WireGuardPOP'.  I went through the configuration file, changed those references to match and suddenly everything worked.

Everything, that is, except web browsing. A quick search suggested that the MTU was set too high. I lowered it to 1300 and boom! I was browsing the web.

### I'm just a bit annoyed with the Mikrotik firewall.

I have to say that despite the glitches, I enjoyed the process and think I'm really going to like this little router.  I'm not sure about their firewall, though. It seems to me that a vendor would want to make their product as user-friendly as possible, whether that user is a basic consumer or a well-experienced pro.  Using standard processes e.g. iptables, ufw, or some other well-known firewall makes more sense than requiring the user to learn a proprietary set of instructions.

That's just my 2 cents.

### And what about Claude?

I'm still not a great fan of AI, but I can see the utility of it when you're working on a complex (at least to you) process. In the long run I still think I'd rather just learn stuff so that  I can have the pride of accomplishment in doing something that I know how to do rather than simply getting things done.

But that's just me ...
