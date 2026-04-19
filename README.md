<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Filadélfia Modas Mundial</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root{--p:#ee4d2d;--s:#ff6b35;--bg:#f5f5f5;--w:#fff;--t:#222;--b:#e8e8e8}
    *{margin:0;padding:0;box-sizing:border-box;font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif}
    body{background:var(--bg);color:var(--t);padding-bottom:70px}
    a{text-decoration:none;color:inherit}
    header{background:linear-gradient(135deg,var(--p),var(--s));padding:12px 15px;position:sticky;top:0;z-index:100}
    .h-top{display:flex;align-items:center;gap:10px;max-width:1200px;margin:0 auto}
    .logo{color:#fff;font-size:20px;font-weight:800}
    .search{flex:1;background:#fff;border-radius:6px;padding:8px 12px;display:flex;align-items:center}
    .search input{border:none;outline:none;width:100%;font-size:14px}
    .icons{color:#fff;display:flex;gap:15px;font-size:18px}
    .icons i{cursor:pointer;position:relative}
    .badge{position:absolute;top:-6px;right:-6px;background:#ffeb3b;color:#333;font-size:9px;padding:2px 5px;border-radius:10px;font-weight:700}
    .cats{background:#fff;padding:10px 0;overflow-x:auto}
    .cats-grid{display:flex;gap:15px;padding:0 15px}
    .cat{display:flex;flex-direction:column;align-items:center;gap:4px;cursor:pointer;min-width:55px}
    .cat-i{width:42px;height:42px;background:#fff5f2;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:16px;color:var(--p)}
    .cat span{font-size:10px;color:#666}
    .banner{width:100%;height:180px;background:#ddd;overflow:hidden;margin:12px 0}
    .banner img{width:100%;height:100%;object-fit:cover}
    .section{padding:15px}
    .section h3{font-size:16px;font-weight:700;margin-bottom:12px;color:#333;display:flex;align-items:center;gap:6px}
    .grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;padding:0 15px 15px}
    .card{background:#fff;border-radius:8px;overflow:hidden;box-shadow:0 1px 4px rgba(0,0,0,.08)}
    .c-img{aspect-ratio:1;background:#f9f9f9;position:relative}
    .c-img img{width:100%;height:100%;object-fit:cover}
    .badge-p{position:absolute;top:6px;left:6px;background:var(--p);color:#fff;padding:3px 6px;border-radius:3px;font-size:9px;font-weight:700}
    .c-info{padding:10px}
    .c-name{font-size:12px;font-weight:600;margin-bottom:4px;color:#333;display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden;height:32px}
    .c-price{color:var(--p);font-weight:700;font-size:15px;margin:6px 0}
    .c-btn{width:100%;background:var(--p);color:#fff;border:none;padding:8px;border-radius:4px;font-weight:600;cursor:pointer;font-size:11px}
    footer{background:#fff;padding:25px 15px 90px;margin-top:15px;border-top:3px solid var(--p)}
    .f-grid{display:grid;grid-template-columns:1fr;gap:15px;max-width:1200px;margin:0 auto}
    .f-col h4{margin-bottom:8px;color:#333;font-size:14px}
    .f-col p,.f-col a{color:#666;font-size:12px;margin-bottom:5px;display:block;line-height:1.4}
    .f-bottom{text-align:center;margin-top:15px;padding-top:12px;border-top:1px solid #eee;color:#999;font-size:10px}
    .b-nav{position:fixed;bottom:0;left:0;right:0;background:#fff;display:flex;justify-content:space-around;padding:8px 0;box-shadow:0 -2px 6px rgba(0,0,0,.08);z-index:100}
    .b-nav a{display:flex;flex-direction:column;align-items:center;font-size:9px;color:#666}
    .b-nav a.active{color:var(--p)}
    .b-nav i{font-size:18px}
    #admin{position:fixed;top:0;right:-100%;width:100%;max-width:100%;height:100%;background:#fff;z-index:3000;transition:.3s;overflow-y:auto}
    #admin.active{right:0}
    .admin-h{background:#222;color:#fff;padding:12px 15px;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;z-index:10}
    .admin-tabs{display:flex;background:#f5f5f5;border-bottom:1px solid #eee;position:sticky;top:48px;z-index:9}
    .admin-tab{flex:1;padding:10px;text-align:center;cursor:pointer;font-weight:600;color:#666;font-size:12px}
    .admin-tab.active{color:var(--p);border-bottom:2px solid var(--p);background:#fff}
    .admin-b{padding:12px;padding-bottom:90px}
    .upload-g{display:grid;grid-template-columns:repeat(3,1fr);gap:6px;margin:10px 0}
    .upload-s{aspect-ratio:1;border:2px dashed #ccc;border-radius:6px;display:flex;align-items:center;justify-content:center;position:relative;overflow:hidden;background:#fafafa;cursor:pointer}
    .upload-s.filled{border-style:solid;border-color:var(--p)}
    .upload-s img{width:100%;height:100%;object-fit:cover;position:absolute;top:0;left:0}
    .upload-s input{position:absolute;width:100%;height:100%;opacity:0;cursor:pointer;z-index:10}
    .upload-s i{color:#999;font-size:18px}
    .rm-btn{position:absolute;top:4px;right:4px;background:rgba(0,0,0,.7);color:#fff;width:20px;height:20px;border-radius:50%;display:none;align-items:center;justify-content:center;cursor:pointer;font-size:10px;z-index:20}
    .upload-s.filled .rm-btn{display:flex}
    .capa{position:absolute;top:4px;left:4px;background:var(--p);color:#fff;padding:2px 5px;border-radius:3px;font-size:8px;cursor:pointer;display:none;z-index:15}
    .upload-s.filled .capa{display:block}
    .vars{background:#f9f9f9;padding:10px;border-radius:6px;margin:12px 0}
    .var-i{background:#fff;padding:10px;border-radius:6px;margin-bottom:8px;border:1px solid #eee}
    .var-h{display:flex;align-items:center;gap:8px;margin-bottom:6px}
    .var-img{width:45px;height:45px;object-fit:cover;border-radius:4px;border:2px solid var(--p)}
    .var-in{flex:1}
    .var-in input{width:100%;padding:6px;border:1px solid #ddd;border-radius:4px;font-size:12px;margin-bottom:4px}
    .tams{display:flex;flex-wrap:wrap;gap:5px;margin-top:4px}
    .tam{display:inline-flex;align-items:center;gap:3px;background:#f5f5f5;padding:3px 7px;border-radius:3px;font-size:10px}
    .tam input{width:auto;margin:0}
    .add-var{width:100%;padding:8px;border:2px dashed var(--p);background:#fff;color:var(--p);border-radius:6px;cursor:pointer;font-weight:600;font-size:11px}
    .form-g{margin-bottom:10px}
    .form-g label{display:block;font-weight:600;margin-bottom:3px;font-size:11px;color:#555}
    .form-g input,.form-g textarea,.form-g select{width:100%;padding:8px;border:1px solid #ddd;border-radius:5px;font-size:13px}
    .form-r{display:flex;gap:8px}
    .form-r .form-g{flex:1}
    .btn{width:100%;background:var(--p);color:#fff;border:none;padding:12px;border-radius:6px;font-weight:700;font-size:14px;cursor:pointer;margin-top:12px}
    .btn:disabled{background:#ccc;cursor:not-allowed}
    .modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,.6);z-index:2000;align-items:center;justify-content:center}
    .modal.active{display:flex}
    .modal-c{background:#fff;width:92%;max-width:400px;border-radius:10px;max-height:90vh;overflow-y:auto}
    .modal-h{padding:12px 15px;border-bottom:1px solid #eee;display:flex;justify-content:space-between;align-items:center}
    .modal-b{padding:15px}
    .close{background:none;border:none;font-size:20px;cursor:pointer;color:#999}
    .load{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,.85);z-index:9999;display:none;align-items:center;justify-content:center;color:#fff;flex-direction:column;gap:8px}
    .load.active{display:flex}
    .load i{font-size:30px}
    .notif{position:fixed;top:12px;left:50%;transform:translateX(-50%);background:#333;color:#fff;padding:8px 16px;border-radius:18px;z-index:5000;display:none;font-size:12px;text-align:center;max-width:90%}
    .notif.show{display:block;animation:fadeIn .2s}
    @keyframes fadeIn{from{opacity:0;transform:translateX(-50%) translateY(-8px)}to{opacity:1;transform:translateX(-50%) translateY(0)}}
  </style>
</head>
<body>

<header>
  <div class="h-top">
    <div class="logo">FILADÉLPHIA</div>
    <div class="search"><i class="fas fa-search" style="color:#999;margin-right:6px"></i><input type="text" placeholder="Buscar..." id="search"></div>
    <div class="icons">
      <i class="far fa-heart"></i>
      <i class="fas fa-shopping-cart" onclick="openCart()"><span class="badge" id="cartN">0</span></i>
      <i class="far fa-user" id="uIcon" onclick="openLogin()"></i>
      <div id="uAvatar" style="display:none;width:28px;height:28px;background:#fff;border-radius:50%;align-items:center;justify-content:center;color:var(--p);font-weight:700;cursor:pointer;font-size:12px" onclick="toggleMenu()">A</div>
    </div>
  </div>
</header>

<div class="cats"><div class="cats-grid">
  <div class="cat" onclick="filter('ofertas')"><div class="cat-i"><i class="fas fa-bolt"></i></div><span>Ofertas</span></div>
  <div class="cat" onclick="filter('feminino')"><div class="cat-i"><i class="fas fa-female"></i></div><span>Feminino</span></div>
  <div class="cat" onclick="filter('masculino')"><div class="cat-i"><i class="fas fa-male"></i></div><span>Masculino</span></div>
  <div class="cat" onclick="filter('lancamentos')"><div class="cat-i"><i class="fas fa-star"></i></div><span>Novidades</span></div>
</div></div>

<div class="banner"><img src="https://images.unsplash.com/photo-1441986300917-64674bd600d8?w=800" alt="Banner"></div>

<div class="section"><h3><i class="fas fa-fire" style="color:var(--p)"></i> MAIS VENDIDOS</h3>
<div class="grid" id="grid"><div style="grid-column:1/-1;text-align:center;padding:25px;color:#999">Carregando...</div></div></div>

<footer>
  <div class="f-grid">
    <div class="f-col"><h4>FILADÉLPHIA MODAS</h4><p>Desde 2016 trazendo qualidade e tendência.</p></div>
    <div class="f-col"><h4>ATENDIMENTO</h4><a href="https://wa.me/5511985694216"><i class="fab fa-whatsapp"></i> (11) 98569-4216</a><a href="mailto:contato@filadelphiamodas.com.br"><i class="fas fa-envelope"></i> contato@filadelphiamodas.com.br</a></div>
  </div>
  <div class="f-bottom">© 2016-2026 Filadélfia Modas Mundial. CNPJ: 37.032.413/0001-68</div>
</footer>

<div class="b-nav">
  <a href="#" class="active" onclick="home()"><i class="fas fa-home"></i>Início</a>
  <a href="#"><i class="fas fa-th-large"></i>Categorias</a>
  <a href="#" onclick="openCart()"><i class="fas fa-shopping-cart"></i>Carrinho</a>
  <a href="#" onclick="openLogin()"><i class="fas fa-user"></i>Conta</a>
</div>

<div class="load" id="load"><i class="fas fa-spinner fa-spin"></i><div id="loadT">Processando...</div></div>
<div class="notif" id="notif"></div>

<div class="modal" id="loginM"><div class="modal-c">
  <div class="modal-h"><h3>Acesso</h3><button class="close" onclick="closeM('loginM')">&times;</button></div>
  <div class="modal-b">
    <form onsubmit="doLogin(event)"><div class="form-g"><label>Email</label><input type="email" id="lE" required></div><div class="form-g"><label>Senha</label><input type="password" id="lP" required></div><button type="submit" class="btn">ENTRAR</button></form>
  </div>
</div></div>

<div class="modal" id="cartM"><div class="modal-c">
  <div class="modal-h"><h3>Carrinho</h3><button class="close" onclick="closeM('cartM')">&times;</button></div>
  <div id="cartB" class="modal-b"></div>
</div></div>

<div id="admin">
  <div class="admin-h"><h3>🔧 GERENCIAR</h3><i class="fas fa-times" onclick="closeAdmin()" style="font-size:20px;cursor:pointer"></i></div>
  <div class="admin-tabs">
    <div class="admin-tab active" onclick="adminTab('add')">➕ Produto</div>
    <div class="admin-tab" onclick="adminTab('pedidos')">📦 Pedidos</div>
  </div>
  <div class="admin-b" id="adminC"></div>
</div>

<script type="module">
  import{initializeApp}from"https://www.gstatic.com/firebasejs/9.23.0/firebase-app.js";
  import{getFirestore,collection,addDoc,getDocs,query,where,orderBy,serverTimestamp}from"https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore.js";
  import{getStorage,ref,uploadBytes,getDownloadURL}from"https://www.gstatic.com/firebasejs/9.23.0/firebase-storage.js";
  import{getAuth,signInWithEmailAndPassword,onAuthStateChanged}from"https://www.gstatic.com/firebasejs/9.23.0/firebase-auth.js";

  const cfg={apiKey:"AIzaSyBejYQ8xnwHEq27ile9USnpHVHIw-qv4Js",authDomain:"loja-filadelphia-modas-mundial.firebaseapp.com",projectId:"loja-filadelphia-modas-mundial",storageBucket:"loja-filadelphia-modas-mundial.firebasestorage.app",messagingSenderId:"515015337156",appId:"1:515015337156:web:79f689c6849285aa13d591"};
  const app=initializeApp(cfg),db=getFirestore(app),storage=getStorage(app),auth=getAuth(app);

  window.cart=[],window.user=null,window.media=[],window.cover=0,window.vars=[];

  window.onload=()=>{initGrid();loadProd();onAuthStateChanged(auth,u=>{window.user=u;document.getElementById('uIcon').style.display=u?'none':'block';document.getElementById('uAvatar').style.display=u?'flex':'none';document.getElementById('uAvatar').textContent=u?u.email.charAt(0).toUpperCase():'A'})};

  function notif(m){const n=document.getElementById('notif');n.textContent=m;n.classList.add('show');setTimeout(()=>n.classList.remove('show'),2500)}
  function load(t){document.getElementById('loadT').textContent=t;document.getElementById('load').classList.add('active')}
  function unload(){document.getElementById('load').classList.remove('active')}
  function openM(id){document.getElementById(id).classList.add('active')}
  function closeM(id){document.getElementById(id).classList.remove('active')}
  function toggleMenu(){if(window.user)openAdmin();else openM('loginM')}

  async function doLogin(e){e.preventDefault();const email=document.getElementById('lE').value;const pass=document.getElementById('lP').value;try{await signInWithEmailAndPassword(auth,email,pass);if(email==='admin@filadelphiamodas.com.br'){closeM('loginM');document.getElementById('admin').classList.add('active');adminTab('add');notif('✅ Admin logado')}else{closeM('loginM');notif('✅ Login realizado')}}catch(err){notif('❌ '+err.message)}}

  function openAdmin(){if(window.user){if(window.user.email==='admin@filadelphiamodas.com.br'){document.getElementById('admin').classList.add('active');adminTab('add')}else{notif('❌ Acesso restrito')}}else{const s=prompt('🔐 Senha:');if(s==='123'||s==='admin'){document.getElementById('admin').classList.add('active');adminTab('add')}else if(s)notif('Senha errada')}}
  function closeAdmin(){document.getElementById('admin').classList.remove('active')}
  function adminTab(t){document.querySelectorAll('.admin-tab').forEach(x=>x.classList.remove('active'));if(t==='add'){document.querySelectorAll('.admin-tab')[0].classList.add('active');renderAdd()}else if(t==='pedidos'){document.querySelectorAll('.admin-tab')[1].classList.add('active');loadPed()}}

  function renderAdd(){window.media=[];window.cover=0;window.vars=[];let h=`<div class="form-g"><label>Título *</label><input id="pN"></div><div class="form-g"><label>Fotos (até 8)</label><div class="upload-g" id="mG"></div><small style="color:#666;display:block;margin-top:4px">Clique para upload. Toque CAPA para definir principal.</small></div><div class="vars" id="varS" style="display:none"><h4 style="margin-bottom:8px">🎨 Variações</h4><div id="varL"></div><button class="add-var" onclick="addVar()">+ Adicionar Variação</button></div><div class="form-r"><div class="form-g"><label>Preço *</label><input type="number" step="0.01" id="pPr"></div><div class="form-g"><label>Promoção</label><input type="number" step="0.01" id="pOld"></div></div><div class="form-g"><label>Categoria *</label><select id="pC"><option value="">Selecione</option><option value="feminino">Feminino</option><option value="masculino">Masculino</option><option value="acessorios">Acessórios</option></select></div><div class="form-r"><div class="form-g"><label>Cor</label><input id="pCo"></div><div class="form-g"><label>Estoque</label><input type="number" id="pSt"></div></div><div class="form-g"><label>Descrição</label><textarea id="pD" rows="3"></textarea></div><button class="btn" id="btnS" onclick="saveP()">PUBLICAR PRODUTO</button>`;document.getElementById('adminC').innerHTML=h;initGrid()}

  function initGrid(){const g=document.getElementById('mG');g.innerHTML='';for(let i=0;i<8;i++){const d=document.createElement('div');d.className='upload-s';d.id='s'+i;d.innerHTML=`<i class="fas fa-camera"></i><div class="rm-btn" onclick="clr(${i},event)">×</div><div class="capa" onclick="setCapa(${i},event)">CAPA</div><input type="file" accept="image/*" onchange="upImg(this,${i})">`;g.appendChild(d);window.media[i]=null}}

  async function upImg(inp,i){if(!inp.files[0])return;load('Comprimindo...');try{const b64=await compress(inp.files[0],1024);window.media[i]=b64;const s=document.getElementById('s'+i);s.classList.add('filled');s.querySelector('i').style.display='none';if(!s.querySelector('img')){const img=document.createElement('img');s.appendChild(img)}s.querySelector('img').src=b64;s.querySelector('.capa').style.display='block';if(i===0)s.classList.add('is-capa');document.getElementById('varS').style.display='block';renderVars()}catch(e){notif('Erro: '+e.message)}unload()}

  function compress(file,max){return new Promise((res,rej)=>{const r=new FileReader();r.onload=e=>{const img=new Image();img.onload=()=>{const c=document.createElement('canvas');let w=img.width,h=img.height;if(w>max){h=(h*max)/w;w=max}c.width=w;c.height=h;c.getContext('2d').drawImage(img,0,0,w,h);res(c.toDataURL('image/jpeg',.8))};img.onerror=rej;img.src=e.target.result};r.onerror=rej;r.readAsDataURL(file)})}

  function clr(i,e){e.stopPropagation();window.media[i]=null;const s=document.getElementById('s'+i);s.classList.remove('filled','is-capa');s.querySelector('i').style.display='block';s.querySelector('.capa').style.display='none';const img=s.querySelector('img');if(img)img.remove();renderVars()}
  function setCapa(i,e){e.stopPropagation();window.cover=i;document.querySelectorAll('.upload-s').forEach((s,idx)=>s.classList.toggle('is-capa',idx===i));notif('Capa definida')}

  function addVar(){window.vars.push({cor:'',tams:[]});renderVars()}
  function renderVars(){const c=document.getElementById('varL');c.innerHTML='';window.vars.forEach((v,idx)=>{c.innerHTML+=`<div class="var-i"><div class="var-h"><div class="var-img" style="background:#eee;display:flex;align-items:center;justify-content:center"><i class="fas fa-image" style="color:#999"></i></div><div class="var-in"><input placeholder="Cor (Ex: Preto)" value="${v.cor}" onchange="window.vars[${idx}].cor=this.value"></div><button onclick="window.vars.splice(${idx},1);renderVars()" style="background:#f44;color:#fff;border:none;padding:4px;border-radius:3px">×</button></div><label style="font-size:10px;color:#666">Tamanhos:</label><div class="tams">${['PP','P','M','G','GG'].map(t=>`<label class="tam"><input type="checkbox" ${v.tams.includes(t)?'checked':''} onchange="togTam(${idx},'${t}')">${t}</label>`).join('')}</div><input type="text" placeholder="Ou numeração: 34, 36..." value="${v.num||''}" onchange="window.vars[${idx}].num=this.value" style="margin-top:6px;width:100%;padding:6px;border:1px solid #ddd;border-radius:4px"></div>`})}
  function togTam(idx,t){const v=window.vars[idx];if(v.tams.includes(t))v.tams=v.tams.filter(x=>x!==t);else v.tams.push(t)}

  async function saveP(){const n=document.getElementById('pN').value.trim(),pr=parseFloat(document.getElementById('pPr').value),c=document.getElementById('pC').value;if(!n||!pr||!c)return notif('Preencha título, preço e categoria');const imgs=window.media.filter(m=>m);if(!imgs.length)return notif('Adicione pelo menos 1 foto');document.getElementById('btnS').disabled=true;load('Enviando imagens...');try{const urls=[];for(let i=0;i<imgs.length;i++){document.getElementById('loadT').textContent=`Enviando ${i+1}/${imgs.length}...`;const blob=await(await fetch(imgs[i])).blob(),r=storage.ref('produtos/'+Date.now()+'_'+i+'.jpg');await uploadBytes(r,blob);urls.push(await getDownloadURL(r))}load('Salvando...');await addDoc(collection(db,'produtos'),{nome:n,preco:pr,precoAntigo:parseFloat(document.getElementById('pOld').value)||0,categoria:c,cor:document.getElementById('pCo').value,estoque:parseInt(document.getElementById('pSt').value)||0,descricao:document.getElementById('pD').value,imagens:urls,capaIdx:window.cover,variacoes:window.vars.filter(v=>v.cor),ativo:true,criado:serverTimestamp()});notif('✅ Produto publicado!');renderAdd();loadProd();closeAdmin()}catch(err){console.error(err);notif('❌ Erro: '+err.message)}finally{unload();document.getElementById('btnS').disabled=false}}

  async function loadProd(cat='all'){const g=document.getElementById('grid');try{let q=collection(db,'produtos');if(cat!=='all')q=query(q,where('categoria','==',cat));q=query(q,orderBy('criado','desc'));const snap=await getDocs(q);if(snap.empty){g.innerHTML='<div style="grid-column:1/-1;text-align:center;padding:25px;color:#999">Nenhum produto.</div>';return}g.innerHTML='';snap.forEach(d=>{const p=d.data(),img=p.imagens&&p.imagens.length?(p.imagens[p.capaIdx]||p.imagens[0]):'',desc=p.precoAntigo?`<span style="text-decoration:line-through;color:#999;font-size:10px;margin-left:4px">R$${p.precoAntigo.toFixed(2)}</span>`:'';g.innerHTML+=`<div class="card" onclick="det('${d.id}')"><div class="c-img"><img src="${img}" loading="lazy" onerror="this.src='https://via.placeholder.com/300'"></div><div class="c-info"><div class="c-name">${p.nome}</div><div class="c-price">R$ ${p.preco.toFixed(2)} ${desc}</div><button class="c-btn" onclick="event.stopPropagation();qCart('${d.id}','${p.nome}',${p.preco},'${img}')">COMPRAR</button></div></div>`})}catch(e){console.error(e);g.innerHTML='<div style="grid-column:1/-1;text-align:center;padding:25px;color:#f44">Erro ao carregar.</div>'}}

  function filter(c){loadProd(c)}
  function home(){loadProd()}

  function det(id){getDocs(query(collection(db,'produtos'),where('__name__','==',id))).then(snap=>{if(snap.empty)return;const p=snap.docs[0].data();window.cP={id,...p};const img=p.imagens&&p.imagens.length?(p.imagens[p.capaIdx]||p.imagens[0]):'';let vH='';if(p.variacoes&&p.variacoes.length){vH=`<div style="margin:12px 0"><b>Cor:</b><div id="cO" style="display:flex;gap:6px;flex-wrap:wrap;margin:8px 0">${p.variacoes.map((v,i)=>`<div class="color-opt" style="padding:6px 12px;border:1px solid #ddd;border-radius:5px;cursor:pointer;${i===0?'background:#fff5f2;border-color:var(--p);color:var(--p);font-weight:700':''}" onclick="selC(${i})">${v.cor}</div>`).join('')}</div></div><div style="margin:12px 0"><b>Tamanho:</b><div id="sO" style="display:flex;gap:6px;flex-wrap:wrap;margin:8px 0">${p.variacoes[0].tams.map(t=>`<div class="size-opt" style="width:42px;height:42px;border:1px solid #ddd;border-radius:5px;display:flex;align-items:center;justify-content:center;cursor:pointer" onclick="selT('${t}')">${t}</div>`).join('')}</div></div>`}document.getElementById('cartB').innerHTML=`<div style="width:100%;aspect-ratio:1;background:#f5f5f5;border-radius:6px;overflow:hidden;margin-bottom:12px"><img src="${img}" style="width:100%;height:100%;object-fit:cover"></div><div style="font-size:16px;font-weight:700;margin-bottom:6px">${p.nome}</div><div style="font-size:20px;color:var(--p);font-weight:700;margin:8px 0">R$ ${p.preco.toFixed(2)}</div>${p.descricao?`<p style="color:#666;font-size:12px;margin:8px 0;line-height:1.4">${p.descricao}</p>`:''}${vH}<button class="btn" onclick="addCD()">ADICIONAR</button>`;window.selC=p.variacoes?.[0]?.cor||null;window.selT=null;openM('cartM')}).catch(e=>notif('Erro: '+e.message))}

  function selC(i){document.querySelectorAll('.color-opt').forEach((e,idx)=>{e.style.background=idx===i?'#fff5f2':'#fff';e.style.borderColor=idx===i?'var(--p)':'#ddd';e.style.color=idx===i?'var(--p)':'#333';e.style.fontWeight=idx===i?'700':'400'});window.selC=window.cP.variacoes[i].cor;const so=document.getElementById('sO');so.innerHTML=window.cP.variacoes[i].tams.map(t=>`<div class="size-opt" style="width:42px;height:42px;border:1px solid #ddd;border-radius:5px;display:flex;align-items:center;justify-content:center;cursor:pointer" onclick="selT('${t}')">${t}</div>`).join('')}
  function selT(t){document.querySelectorAll('.size-opt').forEach(e=>{e.style.background=e.textContent===t?'#fff5f2':'#fff';e.style.borderColor=e.textContent===t?'var(--p)':'#ddd';e.style.color=e.textContent===t?'var(--p)':'#333'});window.selT=t}
  function qCart(id,n,p,i){window.cart.push({id,nome:n,preco:p,img:i,qtd:1});updCart();notif('✅ Adicionado')}
  function addCD(){if(!window.selT)return notif('Selecione um tamanho');window.cart.push({id:window.cP.id,nome:window.cP.nome,preco:window.cP.preco,img:window.cP.imagens[window.cP.capaIdx],qtd:1,cor:window.selC,tam:window.selT});updCart();closeM('cartM');notif('✅ Adicionado')}

  function openCart(){const b=document.getElementById('cartB');if(!window.cart.length){b.innerHTML='<p style="text-align:center;color:#999;padding:15px">Vazio</p>'}else{let h='',t=0;window.cart.forEach((it,i)=>{t+=it.preco*it.qtd;h+=`<div style="display:flex;gap:8px;padding:8px 0;border-bottom:1px solid #eee;align-items:center"><img src="${it.img}" style="width:50px;height:50px;object-fit:cover;border-radius:5px"><div style="flex:1"><div style="font-weight:600;font-size:12px">${it.nome}</div><div style="font-size:10px;color:#666">${it.cor||''} ${it.tam?'Tam:'+it.tam:''}</div><div style="color:var(--p);font-weight:700">R$ ${it.preco.toFixed(2)}</div><div style="display:flex;align-items:center;gap:6px;margin-top:4px"><button onclick="chgQ(${i},-1)" style="width:22px;height:22px;border:1px solid #ddd;background:#fff;border-radius:3px">-</button>${it.qtd}<button onclick="chgQ(${i},1)" style="width:22px;height:22px;border:1px solid #ddd;background:#fff;border-radius:3px">+</button><button onclick="rmC(${i})" style="margin-left:auto;background:none;border:none;color:#f44;font-size:16px">🗑</button></div></div></div>`});h+=`<div style="text-align:right;font-size:16px;font-weight:700;margin:12px 0">Total: R$ ${t.toFixed(2)}</div><button class="btn" onclick="checkout()">FINALIZAR NO WHATSAPP</button>`;b.innerHTML=h}openM('cartM')}
  function chgQ(i,d){window.cart[i].qtd+=d;if(window.cart[i].qtd<1)window.cart.splice(i,1);updCart();openCart()}
  function rmC(i){window.cart.splice(i,1);updCart();openCart()}
  function updCart(){document.getElementById('cartN').textContent=window.cart.reduce((a,b)=>a+b.qtd,0)}

  async function checkout(){if(!window.cart.length)return notif('Carrinho vazio');const t=window.cart.reduce((a,b)=>a+b.preco*b.qtd,0);let m=`🛍️ *PEDIDO FILADÉLPHIA*\n\n`;window.cart.forEach(it=>m+=`• ${it.nome} ${it.cor?'('+it.cor+')':''} ${it.tam?'Tam:'+it.tam:''} x${it.qtd} - R$ ${(it.preco*it.qtd).toFixed(2)}\n`);m+=`\n*Total: R$ ${t.toFixed(2)}*\n_Aguardo confirmação do PIX!_`;window.open(`https://wa.me/5511985694216?text=${encodeURIComponent(m)}`,'_blank');try{await addDoc(collection(db,'pedidos'),{items:window.cart,total:t,cliente:window.user?.email||'Visitante',status:'novo',data:serverTimestamp()})}catch(e){}window.cart=[];updCart();closeM('cartM');notif('✅ Pedido enviado!')}

  async function loadPed(){const c=document.getElementById('adminC');c.innerHTML='<p style="text-align:center;padding:15px">Carregando...</p>';try{const snap=await getDocs(query(collection(db,'pedidos'),orderBy('data','desc')));if(snap.empty){c.innerHTML='<p style="text-align:center;padding:15px;color:#999">Nenhum pedido.</p>';return}let h='';snap.forEach(d=>{const o=d.data();h+=`<div style="background:#f9f9f9;padding:10px;border-radius:5px;margin-bottom:8px;border-left:3px solid var(--p)"><div style="display:flex;justify-content:space-between;font-size:11px"><b>#${d.id.substr(0,6).toUpperCase()}</b><span style="background:#e3f2fd;color:#1976d2;padding:2px 5px;border-radius:3px;font-size:9px">${o.status}</span></div><div style="font-size:10px;color:#666;margin-top:3px">${o.cliente} | R$ ${o.total.toFixed(2)}</div></div>`});c.innerHTML=h}catch(e){c.innerHTML='<p style="color:#f44;text-align:center">Erro</p>'}}
html>
