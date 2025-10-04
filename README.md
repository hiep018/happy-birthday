<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Đêm Trăng Sinh Nhật</title>
  <style>
    :root{ --bg-1:#0f1013; --bg-2:#1a1c22; --cam:#ff8c42; --white:#f6f7f9; --glow:0 0 8px rgba(255,140,66,.7),0 0 24px rgba(255,140,66,.35) }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;color:var(--white);font-family:system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial}
    body{background:radial-gradient(1200px 600px at 70% 20%,#21242c 0%,var(--bg-2) 50%,var(--bg-1) 100%);overflow:hidden}

    /* Sky + moon (đặt TRÊN mây để đèn bay không bị khuất) */
    .sky{position:fixed;inset:0;z-index:3;pointer-events:auto}
    .moon{position:absolute;top:6vh;right:8vw;width:200px;height:200px;border-radius:50%;
      box-shadow:0 0 40px 8px rgba(255,198,112,.35);filter:drop-shadow(0 0 20px rgba(255,200,130,.6));
      animation:pulse 6s ease-in-out infinite}
    .moon img{width:100%;height:100%;object-fit:cover;border-radius:50%}
    @keyframes pulse{0%,100%{filter:drop-shadow(0 0 20px rgba(255,200,130,.6))}50%{filter:drop-shadow(0 0 40px rgba(255,200,130,.9))}}

    /* Fireflies */
    .firefly{position:absolute;width:6px;height:6px;border-radius:50%;
      background:radial-gradient(circle,#fff,#ffd27a 40%,transparent 60%);opacity:.9;filter:blur(.2px);
      animation:drift 16s linear infinite, twinkle 2.2s ease-in-out infinite}
    @keyframes drift{0%{transform:translate(0,0)}25%{transform:translate(30px,-20px)}50%{transform:translate(60px,10px)}75%{transform:translate(30px,30px)}100%{transform:translate(0,0)}}
    @keyframes twinkle{0%,100%{opacity:.2}50%{opacity:1}}

    /* Header */
    header{position:relative;z-index:2;text-align:center;padding:6vh 6vw 2vh}
    .title{font-weight:800;letter-spacing:.5px;line-height:1.15;margin:0}
    .title .top{display:block;font-size:clamp(28px,6vw,64px);text-transform:uppercase;color:var(--cam);text-shadow:var(--glow)}
    .title .sub{display:block;font-size:clamp(20px,3vw,40px);color:#d9dbdf;opacity:.9}

    /* Scene (để có bàn & bánh) */
    .scene{position:absolute;inset:auto 0 0 0;height:46vh;min-height:320px;z-index:1;pointer-events:none}
    .ground-gradient{position:absolute;inset:0;background:linear-gradient(0deg,var(--bg-1) 0%,rgba(15,16,19,0) 70%)}

    /* ====== MÂY – kên cao gần trăng ====== */
    .clouds-wrap{position:fixed; left:0; right:0; top:10vh; height:32vh; pointer-events:none; z-index:1;}
    .cloud{
      position:absolute; width:520px; height:170px; border-radius:999px;
      background:
        radial-gradient(ellipse at 20% 60%, rgba(255,255,255,.5), rgba(255,255,255,0) 60%),
        radial-gradient(ellipse at 45% 40%, rgba(255,255,255,.45), rgba(255,255,255,0) 60%),
        radial-gradient(ellipse at 70% 60%, rgba(255,255,255,.4), rgba(255,255,255,0) 60%),
        radial-gradient(ellipse at 85% 45%, rgba(255,255,255,.35), rgba(255,255,255,0) 60%);
      filter: blur(6px) saturate(1.05);
      opacity: var(--o,.36);
      top: var(--y, 6vh);
      transform: translateX(-30vw) scale(var(--s,1));
      animation: cloudMove var(--dur,80s) linear var(--delay,0s) infinite;
    }
    @keyframes cloudMove{0%{transform:translateX(-30vw) scale(var(--s,1))}100%{transform:translateX(130vw) scale(var(--s,1))}}

    /* Dây & 3 đèn giữa màn hình */
    .lantern-line, .lantern{z-index:2}
    .lantern-line{position:absolute;left:18vw;right:18vw;top:12vh;height:2px;
      background:linear-gradient(90deg,transparent 0 8%,rgba(255,255,255,.12) 8% 92%,transparent 92% 100%);
      transform:skewY(-3deg);filter:blur(.2px);opacity:.7}
    .lantern{position:absolute;top:10vh;width:44px;height:64px;border-radius:12px;pointer-events:auto;
      background:linear-gradient(180deg,#ffd9b0 0%,var(--cam) 60%,#dd6f2f 100%);
      box-shadow:0 8px 22px rgba(255,140,66,.35), inset 0 -8px 18px rgba(0,0,0,.25);
      animation:sway 3.6s ease-in-out infinite; transform-origin:top center}
    .lantern .glow{position:absolute;inset:-20px;background:radial-gradient(circle,rgba(255,166,72,.35),transparent 70%);border-radius:18px;mix-blend-mode:screen}
    .lantern .tassel{position:absolute;bottom:-18px;left:50%;width:3px;height:18px;background:linear-gradient(180deg,#f7b06a,#a14f1d);transform:translateX(-50%);border-radius:2px}
    @keyframes sway{0%,100%{transform:rotate(-2deg)}50%{transform:rotate(2deg)}}

    .lantern-line.fixed, .lantern.fixed{position:fixed; z-index:4;}
    .lantern-line.fixed{top:48vh}
    .lantern.fixed{top:42vh !important}

    /* Đèn thông điệp */
    .lantern.note{width:52px;height:74px;box-shadow:0 10px 28px rgba(255,140,66,.45),inset 0 -10px 20px rgba(0,0,0,.28)}
    .lantern.note .halo{position:absolute;inset:-44px;border-radius:20px;background:radial-gradient(circle at 50% 60%,rgba(255,188,102,.22),rgba(255,170,80,.12) 45%,transparent 70%);animation:haloPulse 3.2s ease-in-out infinite}
    @keyframes haloPulse{0%,100%{opacity:.6;transform:scale(1)}50%{opacity:1;transform:scale(1.08)}}
    .lantern .label{position:absolute;left:50%;bottom:86px;transform:translate(-50%,0);background:rgba(26,28,34,.92);color:#fff;font-size:13px;padding:6px 10px;border:1px solid rgba(255,140,66,.45);border-radius:10px;box-shadow:0 8px 18px rgba(0,0,0,.35);white-space:nowrap;opacity:0;transition:.25s ease;pointer-events:none}
    .lantern .label::after{content:"";position:absolute;left:50%;bottom:-6px;transform:translateX(-50%) rotate(45deg);width:10px;height:10px;background:rgba(26,28,34,.92);border-left:1px solid rgba(255,140,66,.45);border-bottom:1px solid rgba(255,140,66,.45)}
    .lantern.show-label .label{opacity:1;transform:translate(-50%,-6px)}
    .lantern.note::after{content:"";position:absolute;left:50%;top:56%;transform:translate(-50%,-50%);width:8px;height:8px;border-radius:50%;background:rgba(255,200,150,.9);filter:blur(1px);box-shadow:0 0 18px rgba(255,180,90,.8);animation:pulseDot 2.6s infinite}
    @keyframes pulseDot{0%{box-shadow:0 0 0 0 rgba(255,180,90,.55)}70%{box-shadow:0 0 0 16px rgba(255,180,90,0)}100%{box-shadow:0 0 0 0 rgba(255,180,90,0)}}

    /* ĐÈN LỒNG BAY (hiển thị TRÊN mây) */
    .fly-lantern{position:absolute;left:var(--x,50vw);top:100vh;pointer-events:auto;animation:flyUp var(--dur,22s) linear var(--delay,0s) infinite; z-index:3;}
    .lantern-body{position:relative;width:36px;height:52px;border-radius:12px;background:linear-gradient(180deg,#ffd9b0 0%,var(--cam) 60%,#dd6f2f 100%);box-shadow:0 8px 22px rgba(255,140,66,.35),inset 0 -8px 18px rgba(0,0,0,.25);animation:sway 3.2s ease-in-out infinite}
    .lantern-body .glow{position:absolute;inset:-24px;border-radius:18px;background:radial-gradient(circle,rgba(255,166,72,.35),transparent 70%);mix-blend-mode:screen}
    .lantern-body .tassel{position:absolute;bottom:-18px;left:50%;width:3px;height:18px;transform:translateX(-50%);border-radius:2px;background:linear-gradient(180deg,#f7b06a,#a14f1d)}
    @keyframes flyUp{0%{transform:translateY(0) translateX(-8px);opacity:0}5%{opacity:1}50%{transform:translateY(-55vh) translateX(8px)}100%{transform:translateY(-110vh) translateX(-8px);opacity:.95}}

    /* Bàn & bánh */
    .table{position:absolute;right:8vw;bottom:6vh;width:min(380px,46vw);height:140px;border-radius:20px;background:linear-gradient(180deg,#2c2f35,#1f2127);box-shadow:0 10px 24px rgba(0,0,0,.45);display:flex;align-items:flex-end;justify-content:center; z-index:2}
    .table::before{content:"";position:absolute;inset:-2px;z-index:-1;border-radius:22px;background:linear-gradient(135deg,rgba(255,140,66,.45),rgba(255,140,66,0));filter:blur(8px)}
    .cake{position:relative;bottom:16px;width:140px;height:90px;background:linear-gradient(180deg,#f6f0e9,#e9dccf);border-radius:10px;box-shadow:inset 0 -6px 0 #d8c4ad}
    .cake::after{content:"";position:absolute;left:0;right:0;bottom:-14px;height:14px;border-radius:0 0 12px 12px;background:repeating-linear-gradient(90deg,#ffb37a 0 18px,#ffd7b8 18px 36px)}
    .candle{position:absolute;left:50%;top:-38px;width:10px;height:38px;transform:translateX(-50%);background:repeating-linear-gradient(180deg,#ffb37a,#ffb37a 6px,#ffd1a8 6px 12px);border-radius:4px}
    .flame{position:absolute;left:50%;top:-18px;width:16px;height:26px;transform:translateX(-50%);border-radius:50% 50% 50% 50%/60% 60% 40% 40%;background:radial-gradient(ellipse at 50% 40%,#fff8c9 0%,#ffd66b 40%,#ff8c42 70%,rgba(0,0,0,0) 72%);filter:saturate(1.2) drop-shadow(0 0 10px rgba(255,169,70,.8));animation:flame 1.4s ease-in-out infinite}

    /* Controls + Toast (góc dưới phải) */
    .controls{position:fixed;right:14px;bottom:14px;z-index:10;display:flex;gap:8px}
    .btn{pointer-events:auto;background:#1f2127;border:1px solid #3a3f47;color:#fff;padding:8px 12px;border-radius:10px;font-size:13px;opacity:.9;cursor:pointer}
    .btn:hover{border-color:#555c66}
    .btn-accent{border-color:rgba(255,140,66,.6);box-shadow:0 0 0 2px rgba(255,140,66,.12)}
    .toast{position:fixed;z-index:11;max-width:72vw;background:rgba(26,28,34,.92);border:1px solid rgba(255,140,66,.4);border-radius:12px;padding:10px 14px;font-size:14px;line-height:1.35;box-shadow:0 10px 26px rgba(0,0,0,.45),0 0 0 2px rgba(255,140,66,.15) inset;backdrop-filter:blur(4px);opacity:0;transform:translateY(-6px);transition:.18s ease}
    .toast.show{opacity:1;transform:translateY(0)}

    @media (max-width:780px){
      .table{right:4vw;width:min(320px,60vw)}
      .lantern{width:38px;height:56px}
      .moon{width:130px;height:130px}
    }
  </style>
</head>
<body>
  <div class="sky" id="sky">
    <div class="moon"><img src="img/90ee9375c9cbcb24c5ccb75108a2905a.jpg" alt="moon"></div>
  </div>

  <header>
    <h1 class="title">
      <span class="top">🎉 Chúc Mừng Sinh Nhật, <span id="receiver"></span>!</span>
      <span class="sub">Hương Giang 🌕✨</span>
    </h1>
  </header>

  <div class="scene">
    <div class="ground-gradient"></div>

    <!-- Mây cao -->
    <div class="clouds-wrap" id="clouds"></div>

    <!-- Dây & 3 đèn CỐ ĐỊNH giữa màn hình -->
    <div class="lantern-line fixed"></div>
    <div class="lantern fixed" style="left:34vw;"><div class="glow"></div><div class="tassel"></div></div>
    <div class="lantern fixed" style="left:50vw;"><div class="glow"></div><div class="tassel"></div></div>
    <div class="lantern fixed" style="left:66vw;"><div class="glow"></div><div class="tassel"></div></div>

    <!-- Bánh -->
    <div class="table"><div class="cake"><div class="candle"></div><div class="flame"></div></div></div>
  </div>

  <!-- Nút góc dưới phải -->
  <div class="controls">
    <button class="btn btn-accent" id="btnName">Đổi tên người nhận</button>
    <button class="btn" id="btnStars">Ẩn/Hiện lồng đèn bay</button>
  </div>

  <script>
    // tên
    const RECEIVER_NAME="Bạn";
    document.getElementById('receiver').textContent=RECEIVER_NAME;

    // đom đóm
    const sky=document.getElementById('sky');
    for(let i=0;i<16;i++){
      const f=document.createElement('div'); f.className='firefly';
      f.style.left=(5+Math.random()*90)+'vw';
      f.style.top=(20+Math.random()*60)+'vh';
      f.style.animationDelay=(Math.random()*4)+'s';
      sky.appendChild(f);
    }

    // lồng đèn bay (trên mây)
    const flyCount=20, flyers=[];
    for(let i=0;i<flyCount;i++){
      const wrap=document.createElement('div'); wrap.className='fly-lantern';
      wrap.style.setProperty('--x',(Math.random()*100)+'vw');
      wrap.style.setProperty('--dur',(18+Math.random()*12)+'s');
      wrap.style.setProperty('--delay',(Math.random()*12)+'s');
      const body=document.createElement('div'); body.className='lantern-body';
      body.innerHTML='<div class="glow"></div><div class="tassel"></div>';
      wrap.appendChild(body);
      wrap.title='Nhấp để nhận lời chúc ✨';
      wrap.addEventListener('click',(e)=>showToast(e.clientX,e.clientY));
      sky.appendChild(wrap); flyers.push(wrap);
    }
    let flyOn=true;
    document.getElementById('btnStars').addEventListener('click',()=>{
      flyOn=!flyOn; flyers.forEach(el=>el.style.display=flyOn?'block':'none')
    });

    // MÂY CAO
    const clouds=document.getElementById('clouds');
    function addCloud(y,s,dur,o,delay=Math.random()*20){
      const c=document.createElement('div');
      c.className='cloud';
      c.style.setProperty('--y', y+'vh'); c.style.setProperty('--s', s);
      c.style.setProperty('--dur', dur+'s'); c.style.setProperty('--o', o);
      c.style.setProperty('--delay', delay+'s');
      clouds.appendChild(c);
    }
    const layers=[[2,1.18,92,.26],[8,1.05,82,.30],[14,0.95,74,.34],[20,0.86,66,.38],[26,0.78,60,.42]];
    layers.forEach(([y,s,dur,o])=>addCloud(y,s,dur,o));
    for(let i=0;i<3;i++) addCloud(6+Math.random()*20,0.55+Math.random()*0.55,48+Math.random()*38,0.30+Math.random()*0.20);

    // 3 đèn giữa màn hình -> thông điệp
    const LINE_MESSAGES=[
      ["Chúc mừng sinh nhật! 🎉","Ước gì được nấy ✨","Luôn bình an 💛"],
      ["Thắp sáng tuổi mới 🔥","Công việc thăng hoa 🚀","Gia đình hạnh phúc 👨‍👩‍👧‍👦"],
      ["Thêm tuổi thêm yêu ❤️","Sức khỏe dồi dào 💪","Niềm vui trọn vẹn 😊"]
    ];
    const fixedLanterns=document.querySelectorAll('.scene .lantern.fixed');
    fixedLanterns.forEach((lantern,i)=>{
      lantern.classList.add('note');
      lantern.insertAdjacentHTML('beforeend','<div class="halo"></div><div class="label"></div>');
      const label=lantern.querySelector('.label');
      const msgs=LINE_MESSAGES[i%LINE_MESSAGES.length]; let k=0;
      const showMsg=()=>{label.textContent=msgs[k%msgs.length];k++;lantern.classList.add('show-label');setTimeout(()=>lantern.classList.remove('show-label'),2200)};
      showMsg(); setInterval(showMsg,3600+i*250); lantern.addEventListener('click',showMsg);
    });

    // toast
    const WISHES=["Chúc tuổi mới rực rỡ như trăng rằm!","Sức khỏe – Bình an – Hạnh phúc ✨","Mọi điều ước đều tới đúng lúc 🎁","Thêm tuổi thêm yêu thương!","Work hard, shine harder!","Trung Thu sum vầy – Sinh nhật đong đầy 💛","Điều tốt lành luôn ở bên nhé!"];
    let toastEl=null, toastTimer=null;
    function showToast(x,y){
      const msg=WISHES[Math.floor(Math.random()*WISHES.length)];
      if(!toastEl){toastEl=document.createElement('div');toastEl.className='toast';document.body.appendChild(toastEl)}
      toastEl.textContent=msg;
      const pad=16,maxX=window.innerWidth-pad-220;
      const tx=Math.min(Math.max(x,pad),maxX),ty=Math.max(y-40,pad);
      toastEl.style.left=tx+'px';toastEl.style.top=ty+'px';
      requestAnimationFrame(()=>toastEl.classList.add('show'));
      clearTimeout(toastTimer);toastTimer=setTimeout(()=>toastEl.classList.remove('show'),2200);
    }

    // đổi tên
    document.getElementById('btnName').addEventListener('click',()=>{
      const cur=document.getElementById('receiver').textContent.trim();
      const name=prompt('Tên người nhận:',cur||'');
      if(name!==null){document.getElementById('receiver').textContent=name||'Bạn'}
    });
  </script>
</body>
</html>
