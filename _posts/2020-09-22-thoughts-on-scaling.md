---
layout: post
title:  "Thoughts on Scaling"
date:   2020-09-22 13:20:00 -0400
categories: development thoughts
---

# Scale

Pre-orders for the next generation XBox went live today. The XBox Series X and Series S. After Sony pre-orders went live last week and were a complete and utter mess, with retailers sites going down and stock being incredibly hard to get, Microsoft vowed it wouldn't go poorly for them. 

I've never been involved in a large scale product launch, and cloud systems are not my forte. But it does interest me. As a developer, it's one of the most stressful things I can imagine. Going from a system humming along nicely, taking orders and charging credit cards to bombardment! 

You get given a launch date of a product, and told to prepare for launch with pages and banners. You might have only 100 or 1000 of the product available, but when it's a popular mainstream product such as a console, you can't guarantee if you're a major retailer that you won't get hundreds of times more visitors than you have product over a period. Online retailers have to be used to this by now, but I suppose given the unpredictability of consumers, they can never truly be  prepared for what comes next. 

In trying to order one of these new consoles, I had a variety of experiences. Here they are.

#### Microsoft
As the manufacturer of the console, I figured that Microft might be ready. I found the page from which I would be able to pre-order the console beforehand, I sat in wait until the time it was meant to go live. At the time of go live;

- I refresh the page and saw a nice shiney button 'Pre-Order'. I clicked the button, nothing happened. I clicked it again, a circle, an error.  
- I stepped away for a few minutes. On returning and trying again, I was logged out.   
- I tried to add to my cart, it said it was added. Problem, I wasn't logged in. On logging in, my cart was empty.   
- I stepped away for a few more minutes while I tried other retailers.   
- XBox Series X was in my cart, I clicked check-out, error.  
- Refresh, click Check-Out. Success.   

Others reportedly had a much rougher experience than me. 

#### Best Buy
I found the page I was going to use before-hand. At the time of go live, nothing happened. Refresh. Nothing happened. Go back to search page, "No Results". Gave up.

#### Target

- At the time of go-live, I searched for the product
- On finding, I clicked "Add To Cart". 
- I go to my cart, it is empty
- I go back, I click "Add To Cart"
- I go to my cart, it is empty
- I try to login. Wait for my "forgot password" code to arrive for 3 or 4 minutes.
- I go back, after being logged in, and click "Add To Cart".
- After a small pause, I'm told "I can only add one to my cart" (My cart shows as empty)
- I click in to my cart, it's empty. 
- I refresh, nothing.
- I refresh, nothing.
- I refresh, holy moly, there's an Xbox there. 
- I click "Buy Now"
- I'm told my cart is empty. 
- Repeat for next 5 minutes. 
- Get to check out, shows my delivery address. I click "Checkout". 
- Nothing.
- Repeat next 5 minutes. Nothing.

#### Amazon
At first, I thought Amazon was just not bothering, or had sold through their allotment really quick. What they actually did was delay for long enough before listing, and the pre-order went right through once the page came up. 

#### ANTOnline 

I had never visited this retailer before, but I got in and ordered a console quickly. Now, I had to pay a little over normal, because I got a bundle with an expensive controller. But their checkout worked pretty quickly. I decided to try ordering an XBox Series S (the only retailer I tried to order an S from), and either Paypal was suffering an outage, or ANTs usage of the API had already exceeded limits. 

## Summary

These are just a few of the retailers. Gamestop actually made their entire site in to a waiting room, rather than try and scale their systems to deal with the demand it appears. This isn't great for regular consumers in my opinion, as no orders can be taken for anything but an XBox during this time. 3 hours after the pre-order window (not launch!) the site is still a waiting room. 

The Playstation pre-order mess that happened last week, with retailers jumping the gun on sales after Sony failed to message correctly, was not quite at the same level. Microsoft giving a date made little difference to the experience of trying to buy a console as retailers still struggled. These major retailers can't scale fast enough to deal with demand, or manage the user experience in such a way that it's not struggle. Scaling a website is one thing, being able to deal with demand, but being able to deal with order volume I would imagine is a whole different scaling challenge. I guess you have to weigh the cost of developing your systems to receive and respond to order requests and inventory management in a highly scaleable manner versus dealing with your average volume and trying to handle failure as best as you can when it occurs. 

Yes, I ordered multiple consoles. Will I keep them all, no. I ordered them because I've had experiences recently of retailers overselling and cancelling orders. This has happened on Amazon and Gamestop in recent memory, and they have cancelled close to delivery time. 

This makes me want to spend more time looking at distributed systems design, eventual consistency etc.

