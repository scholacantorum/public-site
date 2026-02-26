---
title: Paris After Dark
description: >
  A Night of Elegance and Joie de Vivre to benefit Schola Cantorum
weight: 4
layout: gala
galaForm: true
resources:
- src: image.png
  title: Paris After Dark

aliases:
- /gala
- /Gala
- /GALA
---

<h1>Paris After Dark: La Belle Époque</h1>

A Night of Elegance & Joie de Vivre to benefit _Schola Cantorum_  
featuring Dinner, Auctions, & Entertainment

**Saturday, April 25, 2026**  
6:00 – 10:00 PM  
Saratoga Country Club

Transport yourself to turn-of-the-century Paris—a golden age of art, music, and
joie de vivre. Join Schola Cantorum Silicon Valley for an enchanting evening of
lively entertainment, fine dining, and the unmistakable sparkle of the City of
Lights.

<script>
  window.addEventListener('load', function() {
    const now = new Date().toISOString()
    if (now >= '2026-03-26T07') document.getElementById('b-table').disabled = true
    if (now >= '2026-03-09T07') document.getElementById('b-vip').disabled = true
    if (now < '2026-03-09T07' || now >= '2026-03-26T07') document.getElementById('b-eb').disabled = true
    if (now < '2026-03-26T07' || now >= '2026-04-13T07') document.getElementById('b-reg').disabled = true
    document.getElementById('b-table').addEventListener('click', function() {
      document.getElementById('b-vip').disabled = true
      document.getElementById('b-eb').disabled = true
      document.getElementById('b-reg').disabled = true
      startGalaPayment('registration-2026-04-25-table', 180, 10)
    });
    ['vip', 'eb', 'reg'].forEach(tag => {
      document.getElementById(`b-${tag}`).addEventListener('click', function() {
        document.getElementById('b-table').disabled = true
        document.getElementById('b-vip').disabled = tag !== 'vip'
        document.getElementById('b-eb').disabled = tag !== 'eb'
        document.getElementById('b-reg').disabled = tag !== 'reg'
        startGalaPayment(
          `registration-2026-04-25${tag === 'reg' ? '' : ('-'+tag)}`,
          tag === 'vip' ? 190 : tag === 'eb' ? 200 : 225)
      })
    })
  })
</script>

<div style="font-weight:bold">TICKET PRICING</div>
<div style="display:grid;grid:auto/1fr max-content;gap:0.25rem;width:max-content">
  <div>
    <div>Table Sponsor   <i>$1800 per table for 10</i></div>
    <div style="margin-left:2rem">
      <div>+ Complimentary signature cocktails</div>
      <div>+ Sneak peek at Auction offerings</div>
      <div>+ VIP table location</div>
      <div style="font-style:italic">Available until March 25 or until 5 sold</div>
    </div>
  </div>
  <div><button id=b-table class="btn btn-primary">Buy</button></div>
  <div>
    <div>VIP Ticket   <i>$190 per person</i></div>
    <div style="margin-left:2rem">
      <div>+ Complimentary signature cocktail</div>
      <div>+ Sneak peek at Auction offerings</div>
      <div>+ VIP table location</div>
      <div style="font-style:italic">Available until March 8</div>
    </div>
  </div>
  <div><button id=b-vip class="btn btn-primary">Buy</button></div>
  <div>
    <div>Early Bird   <i>$200 per person</i></div>
    <div style="margin-left:2rem">
      <div>+ Complimentary signature cocktail</div>
      <div style="font-style:italic">Available March 9 to 25</div>
    </div>
  </div>
  <div><button id=b-eb class="btn btn-primary">Buy</button></div>
  <div>
    <div>Regular   <i>$225 per person</i></div>
    <div style="margin-left:2rem">
      <div style="font-style:italic">Available March 26 to April 12</div>
    </div>
  </div>
  <div><button id=b-reg class="btn btn-primary">Buy</button></div>
</div>

<div style="margin-top:1rem;font-weight:bold">THE EVENING INCLUDES</div>
<ul style="margin:0 0 1rem 0">
  <li>Master of Ceremonies: Jeff Applebaum</li>
  <li>Entertainment: Le Cancan Bijou &amp; DJ Frank Morales</li>
  <li>Photography: George Paganini</li>
  <li>Complimentary Wine &amp; Champagne</li>
  <li>Signature Cocktail</li>
  <li>Seated Dining</li>
  <li>Silent &amp; Live Auctions</li>
  <li>Fund-A-Need</li>
  <li>Cash bar</li>
</ul>

**DRESS CODE**  
Evening wear, period-inspired, or art-inspired. Channel your favorite Impressionist painting, Art Nouveau poster, or full Belle Époque elegance, however the era moves you.

**CONTACT**  
Questions? Reach out! 
info@ScholaCantorum.org | (650) 254-1700
