
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>MadaTransport - Tickets Madagascar</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
    body { background: linear-gradient(145deg, #f5f7fc, #eef2f5); color: #1a2c3e; }
    .navbar { background: #0b2b26; padding: 1rem; display: flex; flex-wrap: wrap; justify-content: center; gap: 0.5rem; position: sticky; top: 0; z-index: 100; }
    .nav-btn { background: transparent; border: none; color: #f0e6a0; font-size: 1rem; font-weight: 600; padding: 0.5rem 1.2rem; border-radius: 40px; cursor: pointer; transition: 0.2s; }
    .nav-btn:hover, .nav-btn.active { background: #e9b741; color: #0b2b26; }
    .page-container { max-width: 1200px; margin: 2rem auto; padding: 0 1.5rem 2rem; }
    .page { display: none; animation: fade 0.3s ease; }
    .active-page { display: block; }
    @keyframes fade { from { opacity: 0; transform: translateY(8px);} to { opacity: 1; transform: translateY(0);} }
    h1 { font-size: 2rem; background: linear-gradient(135deg, #0b2b26, #1e6b5e); background-clip: text; -webkit-background-clip: text; color: transparent; }
    h2 { color: #1f5e55; border-left: 4px solid #e9b741; padding-left: 1rem; margin: 1.5rem 0 1rem; }
    .card-grid { display: flex; flex-wrap: wrap; gap: 1.5rem; margin-top: 1.5rem; }
    .card { background: white; border-radius: 24px; padding: 1.2rem; box-shadow: 0 8px 20px rgba(0,0,0,0.08); flex: 1 1 250px; }
    .btn-primary { background: #e9b741; border: none; color: #0b2b26; font-weight: bold; padding: 0.6rem 1.2rem; border-radius: 60px; cursor: pointer; transition: 0.2s; }
    .btn-primary:hover { background: #d9a32e; transform: scale(0.97); }
    .trajet-item { background: white; border-radius: 20px; padding: 0.8rem 1.2rem; margin-bottom: 0.8rem; display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 0.8rem; }
    .price-badge { background: #1f5e55; color: #ffe3a3; padding: 0.2rem 0.8rem; border-radius: 40px; font-weight: bold; }
    .contact-card { background: white; border-radius: 28px; padding: 1.5rem; text-align: center; max-width: 450px; margin: 1rem auto; }
    .whatsapp-link, .tel-link { display: inline-flex; align-items: center; gap: 10px; background: #25D366; color: white; padding: 0.7rem 1.5rem; border-radius: 60px; text-decoration: none; font-weight: bold; margin: 0.5rem; }
    .tel-link { background: #3b5999; }
    .cart-icon { position: fixed; bottom: 20px; right: 20px; background: #e9b741; width: 55px; height: 55px; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; z-index: 200; font-size: 1.6rem; box-shadow: 0 4px 15px rgba(0,0,0,0.2); }
    .cart-count { position: absolute; top: -5px; right: -5px; background: #dc2626; color: white; border-radius: 50%; width: 22px; height: 22px; font-size: 0.7rem; display: flex; align-items: center; justify-content: center; }
    .cart-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 300; justify-content: center; align-items: center; }
    .cart-modal.active { display: flex; }
    .cart-content { background: white; width: 90%; max-width: 500px; max-height: 70vh; border-radius: 28px; padding: 1.2rem; overflow-y: auto; position: relative; }
    .cart-item { display: flex; justify-content: space-between; align-items: center; padding: 0.6rem 0; border-bottom: 1px solid #e2e8f0; }
    .cart-total { font-size: 1.2rem; font-weight: bold; margin: 0.8rem 0; text-align: right; border-top: 2px solid #e9b741; padding-top: 0.8rem; }
    .close-cart { position: absolute; top: 10px; right: 15px; font-size: 1.5rem; cursor: pointer; }
    .remove-btn { background: #ef4444; color: white; border: none; padding: 0.2rem 0.6rem; border-radius: 20px; cursor: pointer; font-size: 0.7rem; }
    .footer-note { text-align: center; margin-top: 2rem; font-size: 0.75rem; color: #4a627a; border-top: 1px solid #cbd5e1; padding-top: 1.2rem; }
    @media (max-width: 680px) { .nav-btn { padding: 0.3rem 0.8rem; font-size: 0.8rem; } h1 { font-size: 1.5rem; } }
  </style>
</head>
<body>
<div class="navbar">
  <button class="nav-btn" data-page="home">🏠 Accueil</button>
  <button class="nav-btn" data-page="trajets">🚍 Trajets</button>
  <button class="nav-btn" data-page="contact">📞 Contact</button>
  <button class="nav-btn" data-page="about">📖 À propos</button>
  <button class="nav-btn" data-page="paiement">💳 Paiement</button>
</div>

<div class="cart-icon" id="cartIcon">🛒<span class="cart-count" id="cartCount">0</span></div>
<div id="cartModal" class="cart-modal"><div class="cart-content"><span class="close-cart" id="closeCartBtn">&times;</span><h3>🛍️ Mon panier</h3><div id="cartItemsList"></div><div id="cartTotal" class="cart-total"></div><button id="checkoutBtn" class="btn-primary" style="width:100%;">💳 Payer</button></div></div>

<div class="page-container">
  <div id="home" class="page active-page">
    <h1>✈️ MadaTransport 🚌</h1>
    <p style="margin-bottom:1rem;">Voyagez à Madagascar — billets vers Tana, Toamasina, Mahajanga, Fianar.</p>
    <div class="card-grid">
      <div class="card"><strong>🚌 +700 trajets/mois</strong><br>Réseau national fiable</div>
      <div class="card"><strong>🎫 Réservation instantanée</strong><br>Place garantie</div>
      <div class="card"><strong>💰 Meilleurs prix</strong><br>Promo -15% aller-retour</div>
    </div>
    <div style="background:#eef1e0; border-radius: 24px; padding: 1.2rem; margin-top: 1.5rem;">
      <h2>🇲🇬 Bienvenue</h2>
      <p>Réservez vos tickets bus/taxi-brousse. Paiement sécurisé, WhatsApp 24/7.</p>
      <button class="btn-primary" style="margin-top:0.8rem;" data-page="trajets">📅 Voir trajets →</button>
    </div>
  </div>

  <div id="trajets" class="page">
    <h1>🚍 Trajets disponibles</h1>
    <p>Ajoutez au panier et payez en un clic.</p>
    <div style="margin: 1rem 0;">
      <div class="trajet-item" data-id="1" data-name="Tana ➡ Toamasina" data-price="35000"><div><strong>🏙️ Tana ➡ Toamasina</strong><br>7h | 6h30/13h00</div><div class="price-badge">35 000 Ar</div><button class="btn-primary add-to-cart" style="width:120px;">➕ Ajouter</button></div>
      <div class="trajet-item" data-id="2" data-name="Tana ➡ Mahajanga" data-price="48000"><div><strong>🌴 Tana ➡ Mahajanga</strong><br>12h | 7h00/20h30</div><div class="price-badge">48 000 Ar</div><button class="btn-primary add-to-cart" style="width:120px;">➕ Ajouter</button></div>
      <div class="trajet-item" data-id="3" data-name="Fianar ➡ Tana" data-price="32000"><div><strong>⛰️ Fianarantsoa ➡ Tana</strong><br>9h | 5h45/14h15</div><div class="price-badge">32 000 Ar</div><button class="btn-primary add-to-cart" style="width:120px;">➕ Ajouter</button></div>
      <div class="trajet-item" data-id="4" data-name="Toamasina ➡ Morondava" data-price="68000"><div><strong>🏖️ Toamasina ➡ Morondava</strong><br>14h | 4h00</div><div class="price-badge">68 000 Ar</div><button class="btn-primary add-to-cart" style="width:120px;">➕ Ajouter</button></div>
      <div class="trajet-item" data-id="5" data-name="Antsirabe ➡ Tana" data-price="15500"><div><strong>🏞️ Antsirabe ➡ Tana</strong><br>3h30 | départs toutes 2h</div><div class="price-badge">15 500 Ar</div><button class="btn-primary add-to-cart" style="width:120px;">➕ Ajouter</button></div>
    </div>
    <div class="card" style="background:#fff6e5;"><p>✍️ <strong>Panier :</strong> Ajoutez plusieurs trajets → panier (icône flottante) → paiement WhatsApp.</p></div>
  </div>

  <div id="contact" class="page">
    <h1>📱 Contactez-nous</h1>
    <div class="contact-card">
      <h3>💬 Assistance rapide</h3>
      <a href="https://wa.me/261334127027?text=Bonjour%2C%20je%20souhaite%20r%C3%A9server" target="_blank" class="whatsapp-link">📲 WhatsApp : +261 33 41 270 27</a>
      <a href="tel:+261385907180" class="tel-link">📞 Téléphone : +261 38 59 071 80</a>
      <p>📍 Antananarivo - Gare MadaTransport</p>
      <p>📧 contact@madatransport.mg</p>
    </div>
    <div class="card-grid"><div class="card">📢 Horaires: Lun-Ven 6h-21h</div><div class="card">💳 Mobile Money: Orange, Airtel, MVola</div></div>
  </div>

  <div id="about" class="page">
    <h1>📖 À propos</h1>
    <div style="background:white; border-radius: 24px; padding: 1.2rem;">
      <p>Fondée en 2024, MadaTransport est la 1ère plateforme de vente de tickets interurbains à Madagascar. +35 compagnies partenaires, +150k voyageurs satisfaits.</p>
      <p style="margin-top:0.8rem;">✅ Trajets vers 22 villes ✅ Annulation flexible ✅ Paiement sécurisé</p>
    </div>
    <div class="card-grid">
      <div class="card"><strong>🌿 Mission</strong><br>Transport accessible et digital</div>
      <div class="card"><strong>🏅 Engagement</strong><br>Annulation jusqu'à 2h avant</div>
    </div>
  </div>

  <div id="paiement" class="page">
    <h1>💳 Paiement sécurisé</h1>
    <div class="card-grid">
      <div class="card"><strong>📱 Mobile Money</strong><br>Orange, Airtel, MVola - 0% frais</div>
      <div class="card"><strong>💳 Carte bancaire</strong><br>Visa/Mastercard (prochainement)</div>
      <div class="card"><strong>🏧 Espèce en agence</strong><br>8 points de vente</div>
    </div>
    <div class="contact-card" style="background:#fff2df;">
      <h3>⚡ Paiement via panier</h3>
      <p>Ajoutez vos billets au panier (🛒) puis cliquez "Payer"</p>
      <button class="btn-primary" id="whatsappPayBtn" style="width:70%;">💸 Payer via WhatsApp</button>
    </div>
  </div>
  <div class="footer-note">© 2026 MadaTransport - Voyagez malin, voyagez serein.</div>
</div>

<script>
  // PANIER
  let cart = [];
  function saveCart() { localStorage.setItem('transportCart', JSON.stringify(cart)); updateCartUI(); }
  function loadCart() { const saved = localStorage.getItem('transportCart'); cart = saved ? JSON.parse(saved) : []; updateCartUI(); }
  function updateCartUI() { document.getElementById('cartCount').textContent = cart.reduce((s,i)=>s+i.quantity,0); renderCartModal(); }
  function showNotification(msg) { let n=document.createElement('div'); n.textContent=msg; n.style.cssText='position:fixed;bottom:90px;right:20px;background:#0b2b26;color:#e9b741;padding:8px 16px;border-radius:40px;z-index:999'; document.body.appendChild(n); setTimeout(()=>n.remove(),1800); }
  function addToCart(id,name,price) { let ex = cart.find(i=>i.id===id); ex ? ex.quantity++ : cart.push({id,name,price,quantity:1}); saveCart(); showNotification(`✅ ${name} ajouté`); }
  function deleteItem(id) { cart = cart.filter(i=>i.id!==id); saveCart(); showNotification(`❌ Article retiré`); }
  function decreaseItem(id) { let idx = cart.findIndex(i=>i.id===id); if(idx!==-1){ if(cart[idx].quantity===1) cart.splice(idx,1); else cart[idx].quantity--; saveCart(); showNotification(`🔄 Panier mis à jour`); } }
  function renderCartModal() { let container = document.getElementById('cartItemsList'), totalDiv = document.getElementById('cartTotal'); if(!container) return;
    if(cart.length===0){ container.innerHTML='<div style="text-align:center;padding:1.5rem;">🛒 Panier vide</div>'; if(totalDiv) totalDiv.innerHTML=''; return; }
    let html='', total=0;
    cart.forEach(i=>{ let it=i.price*i.quantity; total+=it; html+=`<div class="cart-item"><div><strong>${i.name}</strong><br><small>${i.price.toLocaleString()} Ar x ${i.quantity}</small></div><div>${it.toLocaleString()} Ar</div><div><button class="remove-btn" data-id="${i.id}" data-act="dec">-1</button> <button class="remove-btn" data-id="${i.id}" data-act="del" style="background:#dc2626;">🗑️</button></div></div>`; });
    container.innerHTML=html; totalDiv.innerHTML=`💰 Total : ${total.toLocaleString()} Ar`;
    document.querySelectorAll('.remove-btn').forEach(btn=>{ btn.addEventListener('click',(e)=>{ e.stopPropagation(); let id=parseInt(btn.dataset.id); btn.dataset.act==='dec' ? decreaseItem(id) : deleteItem(id); }); });
  }
  // MODALE PANIER
  let modal = document.getElementById('cartModal'), cartIcon = document.getElementById('cartIcon'), closeBtn = document.getElementById('closeCartBtn');
  cartIcon.onclick = ()=>{ renderCartModal(); modal.classList.add('active'); };
  closeBtn.onclick = ()=> modal.classList.remove('active');
  modal.onclick = (e)=> { if(e.target===modal) modal.classList.remove('active'); };
  // PAIEMENT
  document.getElementById('checkoutBtn').onclick = ()=>{
    if(cart.length===0){ alert('Panier vide'); return; }
    let total = cart.reduce((s,i)=>s+(i.price*i.quantity),0);
    let articles = cart.map(i=>`${i.name} x${i.quantity}`).join(', ');
    let msg = `Bonjour, je souhaite payer ma commande MadaTransport : ${articles} pour ${total.toLocaleString()} Ar.`;
    window.open(`https://wa.me/261334127027?text=${encodeURIComponent(msg)}`, '_blank');
    alert(`Redirection WhatsApp pour finaliser le paiement de ${total.toLocaleString()} Ar.`);
  };
  document.getElementById('whatsappPayBtn').onclick = ()=>{
    if(cart.length===0){ alert('Ajoutez des trajets au panier'); return; }
    let total = cart.reduce((s,i)=>s+(i.price*i.quantity),0);
    let items = cart.map(i=>`${i.name} (${i.quantity}x)`).join(', ');
    window.open(`https://wa.me/261334127027?text=${encodeURIComponent(`Paiement MadaTransport : ${items} | Total : ${total.toLocaleString()} Ar.`)}`, '_blank');
  };
  // BOUTONS AJOUTER PANIER
  function bindAddButtons() { document.querySelectorAll('.add-to-cart').forEach(btn=>{ btn.removeEventListener('click', addHandler); btn.addEventListener('click', addHandler); }); }
  function addHandler(e) { let parent = e.currentTarget.closest('.trajet-item'); if(parent){ let id=parseInt(parent.dataset.id), name=parent.dataset.name, price=parseInt(parent.dataset.price); addToCart(id,name,price); } }
  // NAVIGATION PAGES
  const pages = { home:document.getElementById('home'), trajets:document.getElementById('trajets'), contact:document.getElementById('contact'), about:document.getElementById('about'), paiement:document.getElementById('paiement') };
  const navBtns = document.querySelectorAll('.nav-btn');
  function showPage(id) { Object.values(pages).forEach(p=>p.classList.remove('active-page')); pages[id]?.classList.add('active-page'); navBtns.forEach(btn=>{ btn.classList.toggle('active', btn.dataset.page===id); }); window.scrollTo({top:0}); if(id==='trajets') setTimeout(bindAddButtons,30); }
  navBtns.forEach(btn=>{ btn.onclick = ()=> showPage(btn.dataset.page); });
  document.querySelectorAll('[data-page]').forEach(el=>{ if(!el.classList.contains('nav-btn')) el.onclick = ()=> showPage(el.dataset.page); });
  if(!document.querySelector('.page.active-page') || document.querySelector('.page.active-page').id!=='home') showPage('home');
  else document.querySelector('.nav-btn[data-page="home"]').classList.add('active');
  loadCart(); bindAddButtons();
</script>
</body>
</html>
