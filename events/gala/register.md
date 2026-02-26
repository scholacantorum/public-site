---
---

<script>
  let stripe, elements, paymentElement
  function makeGuestLines() {
    const qty = parseInt(document.getElementById('gala-qty').value)
    if (!qty || qty < 1) return
    const grid = document.getElementById('guest-lines')
    if (grid.childElementCount === 4*qty) return
    while (grid.childElementCount > 4*qty) {
      grid.lastElementChild.remove()
      grid.lastElementChild.remove()
      grid.lastElementChild.remove()
      grid.lastElementChild.remove()
    }
    while (grid.childElementCount < 4*qty) {
      const row = document.getElementById('guestline').content.cloneNode(true)
      row.firstElementChild.textContent = (grid.childElementCount === 0) ? 'Host' : `Guest #${grid.childElementCount/4}`
      grid.appendChild(row)
    }
    const price = parseInt(document.getElementById('price').textContent)
    elements.update({ amount: qty * price * 100 })
  }
  window.addEventListener('load', function() {
    const stripeKey = document.querySelector('.gala').dataset.stripeKey
    stripe = Stripe(stripeKey)
    elements = stripe.elements({ mode: 'payment', currency: 'usd', amount: 100 })
    paymentElement = elements.create('payment', { layout: 'tabs', fields: { billingDetails: { address: 'if_required' } } })
    paymentElement.mount('#payment-element')
    makeGuestLines()
    document.getElementById('gala-qty').addEventListener('input', makeGuestLines)
  })
</script>

<style>
  #guestinfo { grid: auto-flow/max-content 1fr 1fr max-content }
  .guestinput label { display: none }
  #note { grid-column: span 3 }
  @media (max-width: 44em) {
    #guestinfo { grid:auto-flow/max-content 1fr }
    .widehead { display: none }
    .guestnum { grid-column: span 2 }
    .guestnum:not(:first-child) { margin-top: 0.75rem }
    .guestinput { display: contents }
    .guestinput label { display: inline }
    #notespace { display: none }
    #note { grid-column: span 2 }
  }
</style>

<form id=gala-form style="display:none">
<hr class=w-100>
<div style="font-weight:bold;margin-block:0.75rem">REGISTRATION</div>
<div id=confirm style="display:none">
  Thank you for your registration.  A receipt has been mailed to you.
  We look forward to seeing you at the gala.
</div>  <div id=qtyline>
    Registering
    <input type=number id=gala-qty name=qty min=1 value=1 class=form-control style="display:inline;width:4rem">
    guest(s) at $<span id=price>1</span> each:
  </div>
  <div id=tableqty>
    Registering a table of 10 guests for $1800:
  </div>
  <div id=guestinfo style="margin-block:1rem;display:grid;align-items:baseline;gap:0.25rem">
    <div class=widehead></div>
    <div class=widehead style="font-weight:bold">Name</div>
    <div class=widehead style="font-weight:bold">Email</div>
    <div class=widehead style="font-weight:bold">Entree</div>
    <div id=guest-lines style="display:contents">
    </div>
    <template id=guestline>
      <div class=guestnum style="font-weight:bold">Host</div>
      <div class=guestinput><label>Name</label><input name=name class=form-control></div>
      <div class=guestinput><label>Email</label><input type=email name=email class=form-control></div>
      <div class=guestinput><label>Entree</label><select name=entree class=form-control>
        <option value="" disabled selected>(choose one)</option>
        <option value="beef">Filet Mignon with bordelaise sauce</option>
        <option value="fish">Pan-seared Sea Bass with beurre blanc</option>
        <option value="vegan">Curry Fried Rice (vegan)</option>
      </select></div>
    </template>
    <div id=notespace></div>
    <div id=note style="font-size:0.875rem;font-style:italic">
      If you don't have names or entree choices yet, that’s OK.
      Email them to us by April 12.
    </div>
  </div>
  <div style="font-weight:bold">Any special requests?</div>
  <textarea name=requests placeholder="Seating preferences, dietary restrictions, etc." class=form-control></textarea>
  <div style="margin-top:1rem;font-weight:bold">Payment Information</div>
  <div id=payment-element></div>
</form>
