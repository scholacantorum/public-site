---
---

<script>
  let stripe, elements, paymentElement, paymentComplete
  function makeGuestLines() {
    const form = document.getElementById('gala-form')
    const qty = parseInt(form.qty.value)
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
      if (grid.childElementCount === 0) {
        row.firstElementChild.textContent = 'Host'
        row.querySelector('[name=name]').required = true
        row.querySelector('[name=email]').required = true
      } else {
        row.firstElementChild.textContent = `Guest #${grid.childElementCount/4}`
      }
      grid.appendChild(row)
    }
    const price = parseInt(form.dataset.price)
    elements.update({ amount: qty * price * 100 })
    document.getElementById('total').textContent = qty*price
  }
  function setSubmitEnabled() {
    let enabled = paymentComplete && document.getElementById('gala-form').checkValidity()
    document.getElementById('paybutton').disabled = !enabled
  }
  async function submitRegistration(evt) {
    evt.preventDefault()
    if (document.getElementById('paybutton').disabled) return
    let result = await elements.submit()
    if (result.error) {
      console.error(result.error)
      return
    }
    const form = evt.target
    result = await stripe.createPaymentMethod({
      elements: elements,
      params: {
        billing_details: {
          email: form.querySelector('[name=email]').value,
          name: form.querySelector('[name=name]').value,
        }
      }
    })
    if (result.error) {
      console.error(result.error)
      return
    }
    const fd = new FormData()
    fd.set('source', 'public')
    fd.set('name', form.querySelector('[name=name]').value)
    fd.set('email', form.querySelector('[name=email]').value)
    fd.set('payment1.type', result.paymentMethod.type)
    fd.set('payment1.subtype', result.paymentMethod.card?.wallet?.type || 'manual')
    fd.set('payment1.method', result.paymentMethod.id)
    fd.set('payment1.amount', 100*parseInt(document.getElementById('total').textContent))
    document.getElementById('guest-lines').querySelectorAll('[name=name]').forEach((name, idx) => {
      fd.set(`line${idx+1}.product`, form.dataset.product)
      fd.set(`line${idx+1}.quantity`, 1)
      fd.set(`line${idx+1}.price`, form.dataset.price+'00')
      fd.set(`line${idx+1}.guestName`, name.value)
    })
    document.getElementById('guest-lines').querySelectorAll('[name=email]').forEach((email, idx) => {
      fd.set(`line${idx+1}.guestEmail`, email.value)
    })
    document.getElementById('guest-lines').querySelectorAll('[name=entree]').forEach((entree, idx) => {
      fd.set(`line${idx+1}.option`, entree.value)
    })
    result = await fetch('https://gala-backend.scholacantorum.org/register', {
      method: 'POST', body: fd,
    })
    if (!result.ok) {
      console.error(result.status)
      return
    }
    // TODO handle errors
    document.getElementById('confirm').style.display = null
    document.getElementById('gala-form').style.display = 'none'
    document.getElementById('b-table').disabled = true
    document.getElementById('b-vip').disabled = true
    document.getElementById('b-eb').disabled = true
    document.getElementById('b-reg').disabled = true
  }
  function startGalaPayment(product, price, qty) {
    if (stripe) return
    const form = document.getElementById('gala-form')
    if (qty) {
      document.getElementById('qtyline').style.display = 'none'
      form.qty.value = qty
    } else {
      document.getElementById('tableqty').style.display = 'none'
    }
    form.dataset.price = price
    form.dataset.product = product
    const stripeKey = document.querySelector('.gala').dataset.stripeKey
    stripe = Stripe(stripeKey)
    elements = stripe.elements({
      mode: 'payment', currency: 'usd', amount: 100,
      paymentMethodCreation: 'manual',
    })
    paymentElement = elements.create('payment', {
      layout: 'tabs',
      fields: { billingDetails: { address: 'if_required' } },
    })
    paymentElement.mount('#payment-element')
    paymentElement.on('change', evt => {
      paymentComplete = evt.complete
      setSubmitEnabled()
    })
    makeGuestLines()
    document.getElementById('gala-qty').addEventListener('input', makeGuestLines)
    form.addEventListener('input', setSubmitEnabled)
    setSubmitEnabled()
    form.style.display = null
    form.scrollIntoView()
    form.addEventListener('submit', submitRegistration)
  }
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

<div id=confirm style="display:none">
  <hr class=w-100>
  <div style="font-weight:bold;margin-block:0.75rem">REGISTRATION</div>
  <div>
    Thank you for your registration.  A receipt has been mailed to you.
    We look forward to seeing you at the gala.
  </div>
</div>

<form id=gala-form style="display:none">
  <input type=hidden id=product name=product>
  <input type=hidden name=token>
  <hr class=w-100>
  <div style="font-weight:bold;margin-block:0.75rem">REGISTRATION</div>
  <div id=qtyline>
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
  <button type=submit id=paybutton class="btn btn-primary" style="margin-top:1rem">Pay $<span id=total></span></button>
  <div style="height:8rem"></div>
</form>
