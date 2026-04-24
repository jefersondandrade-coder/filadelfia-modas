<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Filadélfia Modas Mundial</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root{--p:#ee4d2d;--bg:#f5f5f5;--w:#fff;--t:#222}
    *{margin:0;padding:0;box-sizing:border-box;font-family:sans-serif}
    body{background:var(--bg);padding-bottom:70px}
    header{background:var(--p);padding:12px 15px;color:#fff;position:sticky;top:0}
    .h-top{display:flex;align-items:center;gap:10px;max-width:1200px;margin:0 auto}
    .logo{font-size:20px;font-weight:800}
    .search{flex:1;background:#fff;border-radius:6px;padding:8px}
    .search input{border:none;outline:none;width:100%}
    .icons{display:flex;gap:15px}
    .icons i{cursor:pointer;position:relative}
    .badge{position:absolute;top:-6px;right:-6px;background:#ffeb3b;color:#333;font-size:9px;padding:2px 5px;border-radius:10px;font-weight:700}
    .banner{width:100%;height:180px;background:#ddd;margin:12px 0}
    .banner img{width:100%;height:100%;object-fit:cover}
    .section{padding:15px}
    .section h3{font-size:16px;font-weight:700;margin-bottom:12px}
    .grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
    .card{background:#fff;border-radius:8px;overflow:hidden}
    .c-img{aspect-ratio:1;background:#f9f9f9}
    .c-img img{width:100%;height:100%;object-fit:cover}
    .c-info{padding:10px}
    .c-name{font-size:12px;font-weight:600;margin-bottom:4px}
    .c-price{color:var(--p);font-weight:700;font-size:15px}
    .c-btn{width:100%;background:var(--p);color:#fff;border:none;padding:8px;border-radius:4px;font-weight:600;cursor:pointer;font-size:11px;margin-top:6px}
    footer{background:#fff;padding:25px 15px 90px;margin-top:15px;border-top:3px solid var(--p);text-align:center;color:#666;font-size:12px}
    .b-nav{position:fixed;bottom:0;left:0;right:0;background:#fff;display:flex;justify-content:space-around;padding:8px 0;box-shadow:0 -2px 6px rgba(0,0,0,.1);z-index:100}
    .b-nav a{display:flex;flex-direction:column;align-items:center;font-size:9px;color:#666}
    .b-nav a.active{color:var(--p)}
    .b-nav i{font-size:18px}
    #admin{position:fixed;top:0;right:-100%;width:100%;height:100%;background:#fff;z-index:3000;transition:.3s;overflow-y:auto}
    #admin.active{right:0}
    .admin-h{background:#222;color:#fff;padding:12px 15px;display:flex;justify-content:space-between}
    .admin-tabs{display:flex;background:#f5f5f5}
    .admin-tab{flex:1;padding:10px;text-align:center;cursor:pointer;font-weight:600;color:#666;font-size:12px}
    .admin-tab.active{color:var(--p);border-bottom:2px solid var(--p);background:#fff}
    .admin-b{padding:12px;padding-bottom:90px}
    .upload-g{display:grid;grid-template-columns:repeat(3,1fr);gap:6px;margin:10px 0}
    .upload-s{aspect-ratio:1;border:2px dashed #ccc;border-radius:6px;display:flex;align-items:center;justify-content:center;position:relative;background:#fafafa;cursor:pointer}
    .upload-s.filled{border-style:solid;border-color:var(--p)}
    .upload-s img{width:100%;height:100%;object-fit:cover;position:absolute}
    .upload-s input{position:absolute;width:100%;height:100%;opacity:0;cursor:pointer}
    .upload-s i{color:#999;font-size:18px}
    .rm-btn{position:absolute;top:4px;right:4px;background:#f44;color:#fff;width:20px;height:20px;border-radius:50%;display:none;align-items:center;justify-content:center;cursor:pointer;font-size:10px}
    .upload-s.filled .rm-btn{display:flex}
    .capa{position:absolute;top:4px;left:4px;background:var(--p);color:#fff;padding:2px 5px;border-radius:3px;font-size:8px;cursor:pointer;display:none}
    .upload-s.filled .capa{display:block}
    .form-g{margin-bottom:10px}
    .form-g label{display:block;font-weight:600;margin-bottom:3px;font-size:11px}
    .form-g input,.form-g textarea,.form-g select{width:100%;padding:8px;border:1px solid #ddd;border-radius:5px;font-size:13px}
    .btn{width:100%;background:var(--p);color:#fff;border:none;padding:12px;border-radius:6px;font-weight:700;font-size:14px;cursor:pointer;margin-top:12px}
    .btn:disabled{background:#ccc}
    .modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,.6);z-index:2000;align-items:center;justify-content:center}
    .modal.active{display:flex}
    .modal-c{background:#fff;width:92%;max-width:350px;border-radius:10px;padding:15px}
    .modal-h{display:flex;justify-content:space-between;margin-bottom:15px}
    .close{background:none;border:none;font-size:20px;cursor:pointer}
    .load{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,.85);z-index:9999;display:none;align-items:center;justify-content:center;color:#fff;flex-direction:column}
    .load.active{display:flex}
    .load i{font-size:30px;margin-bottom:10px}
    .notif{position:fixed;top:12px;left:50%;transform:translateX(-50%);background:#333;color:#fff;padding:8px 16px;border-radius:18px;z-index:5000;display:none;font-size:12px}
    .notif.show{display:block}
  </style>
</head>
<body>

<header>
  <div class="h-top">
    <div class="logo">FILADÉLPHIA</div>
    <div class="search"><input type="text" placeholder="Buscar..."></div>
    <div class="icons">
      <i class="fas fa-shopping-cart" onclick="notif('Carrinho em breve!')"><span class="badge" id="cartN">0</span></i>
      <i class="far fa-user" id="uIcon" onclick="openLogin()"></i>
      <div id="uAvatar" style="display:none;width:28px;height:28px;background:#fff;border-radius:50%;align-items:center;justify-content:center;color:var(--p);font-weight:700;cursor:pointer;font-size:12px" onclick="toggleMenu()">A</div>
    </div>
  </div>
</header>

<div class="banner"><img src="https://images.unsplash.com/photo-1441986300917-64674bd600d8?w=800"></div>

<div class="section"><h3>MAIS VENDIDOS</h3>
<div class="grid" id="grid"><div style="grid-column:1/-1;text-align:center;padding:25px;color:#999">Carregando...</div></div></div>

<footer>© 2016-2026 Filadélfia Modas Mundial<br>CNPJ: 37.032.413/0001-68<br>(11) 98569-4216</footer>

<div class="b-nav">
  <a href="#" class="active"><i class="fas fa-home"></i>Início</a>
  <a href="#"><i class="fas fa-th-large"></i>Categorias</a>
  <a href="#"><i class="fas fa-shopping-cart"></i>Carrinho</a>
  <a href="#" onclick="openLogin()"><i class="fas fa-user"></i>Conta</a>
</div>

<div class="load" id="load"><i class="fas fa-spinner fa-spin"></i><div id="loadT">Processando...</div></div>
<div class="notif" id="notif"></div>

<div class="modal" id="loginM"><div class="modal-c">
  <div class="modal-h"><h3>Acesso</h3><button class="close" onclick="closeM('loginM')">&times;</button></div>
  <form onsubmit="doLogin(event)"><div class="form-g"><label>Email</label><input type="email" id="lE" required></div><div class="form-g"><label>Senha</label><input type="password" id="lP" required></div><button type="submit" class="btn">ENTRAR</button></form>
  <p style="text-align:center;margin-top:10px;font-size:12px;color:#666">Admin: admin@filadelphia.com / 123456</p>
</div></div>

<div id="admin">
  <div class="admin-h"><h3>🔧 GERENCIAR</h3><i class="fas fa-times" onclick="closeAdmin()" style="cursor:pointer"></i></div>
  <div class="admin-tabs">
    <div class="admin-tab active" onclick="adminTab('add')">➕ Produto</div>
    <div class="admin-tab" onclick="adminTab('list')">📋 Produtos</div>
  </div>
  <div class="admin-b" id="adminC"></div>
</div>

<script type="module">
  import{initializeApp}from"https://www.gstatic.com/firebasejs/9.23.0/firebase-app.js";
  import{getFirestore,collection,addDoc,getDocs,doc,deleteDoc,serverTimestamp}from"https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore.js";
  import{getStorage,ref,uploadBytes,getDownloadURL}from"https://www.gstatic.com/firebasejs/9.23.0/firebase-storage.js";
  import{getAuth,signInWithEmailAndPassword,createUserWithEmailAndPassword,onAuthStateChanged,signOut}from"https://www.gstatic.com/firebasejs/9.23.0/firebase-auth.js";

  const cfg={apiKey:"AIzaSyBejYQ8xnwHEq27ile9USnpHVHIw-qv4Js",authDomain:"loja-filadelphia-modas-mundial.firebaseapp.com",projectId:"loja-filadelphia-modas-mundial",storageBucket:"loja-filadelphia-modas-mundial.appspot.com",messagingSenderId:"515015337156",appId:"1:515015337156:web:79f689c6849285aa13d591"};
  const app=initializeApp(cfg),db=getFirestore(app),storage=getStorage(app),auth=getAuth(app);
  
  window.cart=[],window.user=null,window.media=[],window.cover=0;

  // VERIFICA SE JÁ ESTÁ LOGADO
  onAuthStateChanged(auth,u=>{
    window.user=u;
    if(u){
      document.getElementById('uIcon').style.display='none';
      document.getElementById('uAvatar').style.display='flex';
      document.getElementById('uAvatar').textContent=u.email.charAt(0).toUpperCase();
      // Se for admin, abre o painel automaticamente
      if(u.email==='admin@filadelphia.com'){
        document.getElementById('admin').classList.add('active');
        adminTab('add');
        renderAdd();
      }
    }else{
      document.getElementById('uIcon').style.display='block';
      document.getElementById('uAvatar').style.display='none';
    }
  });

  function notif(m){const n=document.getElementById('notif');n.textContent=m;n.classList.add('show');setTimeout(()=>n.classList.remove('show'),2500)}
  function load(t){document.getElementById('loadT').textContent=t;document.getElementById('load').classList.add('active')}
  function unload(){document.getElementById('load').classList.remove('active')}
  function openM(id){document.getElementById(id).classList.add('active')}
  function closeM(id){document.getElementById(id).classList.remove('active')}
  function toggleMenu(){if(window.user){if(window.user.email==='admin@filadelphia.com'){document.getElementById('admin').classList.add('active');adminTab('list')}else{signOut(auth);notif('Saiu')}}else{openM('loginM')}}

  async function doLogin(e){
    e.preventDefault();
    const email=document.getElementById('lE').value;
    const pass=document.getElementById('lP').value;
    load('Entrando...');
    try{
      await signInWithEmailAndPassword(auth,email,pass);
      closeM('loginM');
      if(email==='admin@filadelphia.com'){
        notif('✅ Admin logado');
        setTimeout(()=>{document.getElementById('admin').classList.add('active');adminTab('add');renderAdd()},500);
      }else{
        notif('✅ Login realizado');
      }
    }catch(err){
      notif('❌ '+err.message);
    }
    unload();
  }

  function openAdmin(){
    if(window.user && window.user.email==='admin@filadelphia.com'){
      document.getElementById('admin').classList.add('active');
      adminTab('add');
      renderAdd();
    }else{
      openM('loginM');
    }
  }

  function closeAdmin(){document.getElementById('admin').classList.remove('active')}
  
  function adminTab(t){
    document.querySelectorAll('.admin-tab').forEach(x=>x.classList.remove('active'));
    if(t==='add'){
      document.querySelectorAll('.admin-tab')[0].classList.add('active');
      renderAdd();
    }else if(t==='list'){
      document.querySelectorAll('.admin-tab')[1].classList.add('active');
      loadProdList();
    }
  }

  function renderAdd(){
    window.media=[];window.cover=0;
    let h=`<div class="form-g"><label>Título *</label><input id="pN"></div>
    <div class="form-g"><label>Fotos (clique para adicionar)</label><div class="upload-g" id="mG"></div></div>
    <div class="form-r"><div class="form-g"><label>Preço *</label><input type="number" step="0.01" id="pPr"></div><div class="form-g"><label>Estoque</label><input type="number" id="pSt" value="100"></div></div>
    <div class="form-g"><label>Categoria</label><select id="pC"><option value="feminino">Feminino</option><option value="masculino">Masculino</option><option value="acessorios">Acessórios</option></select></div>
    <div class="form-g"><label>Descrição</label><textarea id="pD" rows="2"></textarea></div>
    <button class="btn" id="btnS" onclick="saveP()">💾 SALVAR PRODUTO</button>`;
    document.getElementById('adminC').innerHTML=h;
    initGrid();
  }

  function initGrid(){
    const g=document.getElementById('mG');
    g.innerHTML='';
    for(let i=0;i<4;i++){
      const d=document.createElement('div');
      d.className='upload-s';
      d.id='s'+i;
      d.innerHTML=`<i class="fas fa-camera"></i><div class="rm-btn" onclick="clr(${i},event)">×</div><div class="capa" onclick="setCapa(${i},event)">CAPA</div><input type="file" accept="image/*" onchange="upImg(this,${i})">`;
      g.appendChild(d);
      window.media[i]=null;
    }
  }

  async function upImg(inp,i){
    if(!inp.files[0])return;
    load('Processando imagem...');
    try{
      const file=inp.files[0];
      const reader=new FileReader();
      reader.onload=async function(e){
        window.media[i]=e.target.result;
        const s=document.getElementById('s'+i);
        s.classList.add('filled');
        s.querySelector('i').style.display='none';
        if(!s.querySelector('img')){
          const img=document.createElement('img');
          s.appendChild(img);
        }
        s.querySelector('img').src=e.target.result;
        s.querySelector('.capa').style.display='block';
        if(i===0)setCapa(0);
        unload();
      };
      reader.readAsDataURL(file);
    }catch(err){
      notif('Erro: '+err.message);
      unload();
    }
  }

  function clr(i,e){
    e.stopPropagation();
    window.media[i]=null;
    const s=document.getElementById('s'+i);
    s.classList.remove('filled');
    s.querySelector('i').style.display='block';
    s.querySelector('.capa').style.display='none';
    const img=s.querySelector('img');
    if(img)img.remove();
  }

  function setCapa(i,e){
    if(e)e.stopPropagation();
    window.cover=i;
    document.querySelectorAll('.upload-s').forEach((s,idx)=>{
      s.style.borderColor=idx===i?'var(--p)':'#ccc';
      s.style.borderWidth=idx===i?'3px':'2px';
    });
  }

  async function saveP(){
    const n=document.getElementById('pN').value.trim();
    const pr=parseFloat(document.getElementById('pPr').value);
    const c=document.getElementById('pC').value;
    if(!n||!pr)return notif('Preencha título e preço');
    
    const imgs=window.media.filter(m=>m);
    if(!imgs.length)return notif('Adicione pelo menos 1 foto');
    
    document.getElementById('btnS').disabled=true;
    load('Salvando...');
    
    try{
      // Upload da imagem de capa
      const blob=await(await fetch(imgs[window.cover])).blob();
      const r=storage.ref('produtos/'+Date.now()+'.jpg');
      await uploadBytes(r,blob);
      const imgUrl=await getDownloadURL(r);
      
      // Salva no Firestore
      await addDoc(collection(db,'produtos'),{
        nome:n,
        preco:pr,
        categoria:c,
        descricao:document.getElementById('pD').value,
        estoque:parseInt(document.getElementById('pSt').value)||0,
        imagem:imgUrl,
        criado:serverTimestamp()
      });
      
      notif('✅ Produto salvo!');
      renderAdd();
      loadProdList();
    }catch(err){
      console.error(err);
      notif('❌ Erro: '+err.message);
    }finally{
      unload();
      document.getElementById('btnS').disabled=false;
    }
  }

  async function loadProdList(){
    const c=document.getElementById('adminC');
    c.innerHTML='<p style="text-align:center;padding:20px">Carregando...</p>';
    try{
      const snap=await getDocs(collection(db,'produtos'));
      if(snap.empty){
        c.innerHTML='<p style="text-align:center;padding:20px;color:#999">Nenhum produto</p>';
        return;
      }
      let h='<table style="width:100%;font-size:12px"><tr><th style="text-align:left;padding:8px">Produto</th><th>Preço</th><th>Ação</th></tr>';
      snap.forEach(d=>{
        const p=d.data();
        h+=`<tr style="border-bottom:1px solid #eee"><td style="padding:8px">${p.nome}</td><td>R$ ${p.preco.toFixed(2)}</td><td><button onclick="delP('${d.id}')" style="background:#f44;color:#fff;border:none;padding:4px 8px;border-radius:3px;cursor:pointer">Excluir</button></td></tr>`;
      });
      h+='</table>';
      c.innerHTML=h;
    }catch(err){
      c.innerHTML='<p style="color:#f44;text-align:center">Erro ao carregar</p>';
    }
  }

  async function delP(id){
    if(!confirm('Excluir este produto?'))return;
    try{
      await deleteDoc(doc(db,'produtos',id));
      notif('✅ Produto excluído');
      loadProdList();
    }catch(err){
      notif('❌ Erro: '+err.message);
    }
  }

  // Carrega produtos na loja
  async function loadStore(){
    const g=document.getElementById('grid');
    try{
      const snap=await getDocs(collection(db,'produtos'));
      if(snap.empty){
        g.innerHTML='<div style="grid-column:1/-1;text-align:center;padding:25px;color:#999">Nenhum produto</div>';
        return;
      }
      g.innerHTML='';
      snap.forEach(d=>{
        const p=d.data();
        g.innerHTML+=`<div class="card"><div class="c-img"><img src="${p.imagem||'https://via.placeholder.com/300'}"></div><div class="c-info"><div class="c-name">${p.nome}</div><div class="c-price">R$ ${p.preco.toFixed(2)}</div><button class="c-btn" onclick="notif('Adicionado!')">COMPRAR</button></div></div>`;
      });
    }catch(err){
      g.innerHTML='<div style="grid-column:1/-1;text-align:center;padding:25px;color:#f44">Erro</div>';
    }
  }

  // Inicializa
  loadStore();
</script>
</body>
</html>
