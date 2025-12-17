<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Mini Call of Duty (2D)</title>
  <style>
    body{margin:0;background:#000;color:#fff;font-family:Arial;text-align:center}
    h2{margin:12px 0 4px}
    .sub{opacity:.85;margin:0 0 8px}
    canvas{background:#0f1115;display:block;margin:10px auto;border:2px solid #fff;border-radius:12px;touch-action:none}
    .hint{font-size:12px;opacity:.7;margin-bottom:10px}
  </style>
</head>
<body>
  <h2>🎖️ Mini Call of Duty (2D)</h2>
  <p class="sub">WASD move • Mouse aim • Click shoot • R reload • Shift sprint • Space roll • P pause • R restart on death</p>
  <div class="hint">Survive waves. Headshots do bonus damage. Pick up ammo/medkits.</div>
  <canvas id="c" width="900" height="560"></canvas>

<script>
(() => {
  const canvas = document.getElementById("c");
  const ctx = canvas.getContext("2d");
  const W = canvas.width, H = canvas.height;

  // ---------- helpers ----------
  const clamp = (v,a,b)=>Math.max(a,Math.min(b,v));
  const rnd = (a,b)=>a+Math.random()*(b-a);
  const dist = (ax,ay,bx,by)=>Math.hypot(ax-bx, ay-by);

  function segCircleHit(x1,y1,x2,y2,cx,cy,r){
    // line segment vs circle (for bullet trail hit)
    const dx=x2-x1, dy=y2-y1;
    const len2=dx*dx+dy*dy || 1;
    let t=((cx-x1)*dx+(cy-y1)*dy)/len2;
    t=clamp(t,0,1);
    const px=x1+t*dx, py=y1+t*dy;
    return dist(px,py,cx,cy) <= r;
  }

  function rectCircleCollide(rx,ry,rw,rh,cx,cy,cr){
    const px = clamp(cx, rx, rx+rw);
    const py = clamp(cy, ry, ry+rh);
    return dist(px,py,cx,cy) <= cr;
  }

  // ---------- input ----------
  const keys = {};
  const mouse = {x:W/2, y:H/2, down:false};

  addEventListener("keydown", e=>{
    keys[e.key.toLowerCase()] = true;
    if (e.key.toLowerCase()==="p") paused = !paused;
    if (gameOver && e.key.toLowerCase()==="r") reset();
    if (!gameOver && e.key.toLowerCase()==="r") tryReload();
  });
  addEventListener("keyup", e=> keys[e.key.toLowerCase()] = false);

  canvas.addEventListener("pointermove", e=>{
    const r=canvas.getBoundingClientRect();
    mouse.x=(e.clientX-r.left)*(W/r.width);
    mouse.y=(e.clientY-r.top)*(H/r.height);
  });
  canvas.addEventListener("pointerdown", ()=> mouse.down=true);
  addEventListener("pointerup", ()=> mouse.down=false);

  // ---------- world ----------
  const walls = [
    {x:120,y:80,w:260,h:30},
    {x:520,y:90,w:260,h:30},
    {x:220,y:210,w:30,h:220},
    {x:650,y:210,w:30,h:220},
    {x:350,y:320,w:200,h:30},
    {x:380,y:440,w:140,h:30},
  ];

  // ---------- game state ----------
  let player, enemies, particles, drops, shots, score, wave, paused, gameOver;
  let lastEnemySpawn = 0, lastEnemyFire = 0;

  function reset(){
    player = {
      x: W/2, y: H/2, r: 14,
      hp: 100, maxHp: 100,
      spd: 2.35,
      sprint: 1.55,
      rollCd: 0,
      rollT: 0,
      invuln: 0,
      gun: {
        magSize: 14,
        ammo: 14,
        reserve: 70,
        reloadMs: 1100,
        reloading: 0,
        fireMs: 140,
        lastShot: 0,
        spread: 0.045, // radians
        damage: 18
      }
    };
    enemies = [];
    particles = [];
    drops = [];
    shots = [];
    score = 0;
    wave = 1;
    paused = false;
    gameOver = false;
    lastEnemySpawn = 0;
    lastEnemyFire = 0;
  }

  // ---------- effects ----------
  function puff(x,y,n=10){
    for(let i=0;i<n;i++){
      particles.push({x,y,vx:rnd(-2.2,2.2),vy:rnd(-2.2,2.2),life:rnd(14,28)});
    }
  }

  function addDrop(x,y){
    if (Math.random()<0.22){
      drops.push({
        x,y,r:10,
        kind: Math.random()<0.55 ? "ammo" : "med",
        t: 0
      });
    }
  }

  // ---------- enemies ----------
  function spawnEnemy(){
    // spawn outside edges
    const side = Math.floor(Math.random()*4);
    let x,y;
    if (side===0){ x=rnd(0,W); y=-20; }
    if (side===1){ x=W+20; y=rnd(0,H); }
    if (side===2){ x=rnd(0,W); y=H+20; }
    if (side===3){ x=-20; y=rnd(0,H); }

    const typeRoll = Math.random();
    let type="grunt";
    if (typeRoll>0.72) type="rifle";
    if (typeRoll>0.90) type="tank";

    const base = {
      grunt:{r:13,hp:40,spd:rnd(1.35,1.75),score:15,shoot:false},
      rifle:{r:13,hp:55,spd:rnd(1.05,1.45),score:25,shoot:true},
      tank:{r:18,hp:120,spd:rnd(0.75,1.05),score:60,shoot:true}
    }[type];

    enemies.push({
      type,
      x,y,
      r: base.r,
      hp: base.hp + Math.floor((wave-1)*6),
      maxHp: base.hp + Math.floor((wave-1)*6),
      spd: base.spd + (wave-1)*0.05,
      score: base.score,
      shoot: base.shoot,
      aimNoise: rnd(0.0, 1.0)
    });
  }

  // ---------- collisions ----------
  function pushOutOfWalls(obj){
    for (const w of walls){
      if (rectCircleCollide(w.x,w.y,w.w,w.h,obj.x,obj.y,obj.r)){
        // minimal push based on closest side
        const leftDist = Math.abs(obj.x - w.x);
        const rightDist = Math.abs((w.x+w.w) - obj.x);
        const topDist = Math.abs(obj.y - w.y);
        const botDist = Math.abs((w.y+w.h) - obj.y);
        const m = Math.min(leftDist, rightDist, topDist, botDist);
        if (m===leftDist) obj.x = w.x - obj.r;
        else if (m===rightDist) obj.x = w.x + w.w + obj.r;
        else if (m===topDist) obj.y = w.y - obj.r;
        else obj.y = w.y + w.h + obj.r;
      }
    }
    obj.x = clamp(obj.x, obj.r+4, W-obj.r-4);
    obj.y = clamp(obj.y, obj.r+4, H-obj.r-4);
  }

  // ---------- shooting ----------
  function tryReload(){
    const g = player.gun;
    if (g.reloading || g.ammo===g.magSize || g.reserve<=0) return;
    g.reloading = g.reloadMs;
  }

  function shoot(t){
    const g = player.gun;
    if (g.reloading || g.ammo<=0) return;
    if (t - g.lastShot < g.fireMs) return;
    g.lastShot = t;
    g.ammo--;

    // direction
    const ang = Math.atan2(mouse.y - player.y, mouse.x - player.x);
    const spread = (Math.random()-0.5)*g.spread*(keys["shift"]?1.3:1.0);
    const a = ang + spread;

    // hitscan: ray until wall or max range
    const maxRange = 750;
    const step = 8;
    let x = player.x, y = player.y;
    let endX = x, endY = y;
    let hitEnemy = null, hitPoint = null, headshot = false;

    for (let d=0; d<maxRange; d+=step){
      const nx = player.x + Math.cos(a)*d;
      const ny = player.y + Math.sin(a)*d;

      // stop at wall
      let blocked=false;
      for (const w of walls){
        if (nx>=w.x && nx<=w.x+w.w && ny>=w.y && ny<=w.y+w.h){ blocked=true; break; }
      }
      if (blocked){ endX=nx; endY=ny; break; }

      // check enemy
      for (const e of enemies){
        const r = e.r;
        if (dist(nx,ny,e.x,e.y) <= r){
          hitEnemy = e;
          hitPoint = {x:nx, y:ny};
          // "head": upper part bonus (simple rule)
          const headY = e.y - e.r*0.35;
          headshot = dist(nx,ny,e.x,headY) <= e.r*0.45;
          endX=nx; endY=ny;
          d=maxRange; // exit outer loop
          break;
        }
      }
    }

    shots.push({x1:player.x,y1:player.y,x2:endX,y2:endY,life:6});

    if (hitEnemy){
      const dmg = g.damage * (headshot ? 1.75 : 1.0);
      hitEnemy.hp -= dmg;
      puff(endX,endY, headshot ? 16 : 10);
      if (hitEnemy.hp<=0){
        score += hitEnemy.score;
        puff(hitEnemy.x, hitEnemy.y, 26);
        addDrop(hitEnemy.x, hitEnemy.y);
        enemies.splice(enemies.indexOf(hitEnemy),1);
      }
    } else {
      puff(endX,endY, 6);
    }
  }

  // ---------- enemy fire ----------
  function enemyFire(t){
    const interval = Math.max(520, 1450 - wave*55);
    if (t - lastEnemyFire < interval) return;
    lastEnemyFire = t;

    const shooters = enemies.filter(e=>e.shoot && dist(e.x,e.y,player.x,player.y) < 520);
    if (!shooters.length) return;
    const e = shooters[Math.floor(Math.random()*shooters.length)];

    // if line of sight blocked by wall, skip
    const ang = Math.atan2(player.y - e.y, player.x - e.x);
    const step = 10;
    let blocked=false;
    for (let d=0; d<600; d+=step){
      const nx = e.x + Math.cos(ang)*d;
      const ny = e.y + Math.sin(ang)*d;
      for (const w of walls){
        if (nx>=w.x && nx<=w.x+w.w && ny>=w.y && ny<=w.y+w.h){ blocked=true; break; }
      }
      if (blocked) break;
      if (dist(nx,ny,player.x,player.y) < player.r){ break; }
    }
    if (blocked) return;

    // enemy hitscan
    const noise = (Math.random()-0.5) * (0.08 + e.aimNoise*0.06);
    const a = ang + noise;
    let endX = e.x + Math.cos(a)*620;
    let endY = e.y + Math.sin(a)*620;

    // clip at wall
    for (let d=0; d<620; d+=10){
      const nx = e.x + Math.cos(a)*d;
      const ny = e.y + Math.sin(a)*d;
      let stop=false;
      for (const w of walls){
        if (nx>=w.x && nx<=w.x+w.w && ny>=w.y && ny<=w.y+w.h){ stop=true; break; }
      }
      if (stop){ endX=nx; endY=ny; break; }
      if (dist(nx,ny,player.x,player.y) <= player.r){
        endX=nx; endY=ny;
        if (player.invuln<=0){
          player.hp -= (e.type==="tank" ? 16 : 10);
          player.invuln = 220; // ms i-frames
          puff(player.x, player.y, 18);
          if (player.hp<=0){ gameOver=true; }
        }
        break;
      }
    }
    shots.push({x1:e.x,y1:e.y,x2:endX,y2:endY,life:6,enemy:true});
    puff(endX,endY, 6);
  }

  // ---------- player movement ----------
  function updatePlayer(dt){
    const up=keys["w"]||keys["arrowup"];
    const dn=keys["s"]||keys["arrowdown"];
    const lf=keys["a"]||keys["arrowleft"];
    const rt=keys["d"]||keys["arrowright"];

    let vx=0, vy=0;
    if (up) vy-=1;
    if (dn) vy+=1;
    if (lf) vx-=1;
    if (rt) vx+=1;

    const moving = vx||vy;
    const len = moving ? Math.hypot(vx,vy) : 1;
    vx/=len; vy/=len;

    let spd = player.spd;
    if (keys["shift"] && player.rollT<=0) spd *= player.sprint;

    // roll (short dash + i-frames)
    if ((keys[" "] || keys["space"]) && player.rollCd<=0 && moving && player.rollT<=0){
      player.rollT = 180;    // ms
      player.rollCd = 750;   // ms
      player.invuln = 240;   // ms
    }

    if (player.rollT>0){
      // dash faster
      player.x += vx * 7.5;
      player.y += vy * 7.5;
    } else {
      player.x += vx * spd;
      player.y += vy * spd;
    }

    pushOutOfWalls(player);
  }

  // ---------- update loop ----------
  let lastT = performance.now();

  function update(t){
    if (paused) return;

    const dt = t - lastT;
    lastT = t;

    // reload timer
    const g = player.gun;
    if (g.reloading>0){
      g.reloading -= dt;
      if (g.reloading<=0){
        const need = g.magSize - g.ammo;
        const take = Math.min(need, g.reserve);
        g.reserve -= take;
        g.ammo += take;
        g.reloading = 0;
      }
    }

    // timers
    player.invuln = Math.max(0, player.invuln - dt);
    player.rollCd = Math.max(0, player.rollCd - dt);
    player.rollT  = Math.max(0, player.rollT  - dt);

    // player move
    updatePlayer(dt);

    // shoot
    if (!gameOver && mouse.down) shoot(t);
    if (!gameOver && g.ammo<=0 && g.reserve>0 && !g.reloading) tryReload();

    // waves + spawning
    const spawnGap = Math.max(260, 950 - wave*55);
    if (!gameOver && t - lastEnemySpawn > spawnGap){
      lastEnemySpawn = t;
      spawnEnemy();
      if (score > wave*220) wave++;
    }

    // enemy AI (move toward player)
    for (const e of enemies){
      const a = Math.atan2(player.y - e.y, player.x - e.x);
      const stopDist = (e.type==="rifle" ? 210 : e.type==="tank" ? 170 : 60);
      const d = dist(e.x,e.y,player.x,player.y);
      if (d > stopDist){
        e.x += Math.cos(a)*e.spd;
        e.y += Math.sin(a)*e.spd;
        pushOutOfWalls(e);
      }
      // melee damage if too close
      if (d < e.r + player.r + 2 && player.invuln<=0){
        player.hp -= (e.type==="tank" ? 22 : 12);
        player.invuln = 220;
        puff(player.x,player.y,18);
        if (player.hp<=0) gameOver=true;
      }
    }

    // enemy fire
    if (!gameOver) enemyFire(t);

    // drops pickup
    for (let i=drops.length-1;i>=0;i--){
      const d = drops[i];
      d.t += dt;
      if (dist(d.x,d.y,player.x,player.y) < d.r + player.r){
        if (d.kind==="ammo") player.gun.reserve = Math.min(250, player.gun.reserve + 24);
        if (d.kind==="med")  player.hp = Math.min(player.maxHp, player.hp + 30);
        drops.splice(i,1);
      } else if (d.t > 12000){
        drops.splice(i,1);
      }
    }

    // particles
    for (let i=particles.length-1;i>=0;i--){
      const p = particles[i];
      p.x += p.vx; p.y += p.vy; p.life -= 1;
      if (p.life<=0) particles.splice(i,1);
    }

    // shot trails
    for (let i=shots.length-1;i>=0;i--){
      shots[i].life -= 1;
      if (shots[i].life<=0) shots.splice(i,1);
    }
  }

  // ---------- draw ----------
  function draw(t){
    ctx.clearRect(0,0,W,H);

    // floor
    ctx.globalAlpha = 0.12;
    ctx.strokeStyle = "#fff";
    for(let y=0;y<H;y+=40){ ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(W,y); ctx.stroke(); }
    for(let x=0;x<W;x+=40){ ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,H); ctx.stroke(); }
    ctx.globalAlpha = 1;

    // walls
    ctx.fillStyle = "#2b2f3a";
    for (const w of walls){
      ctx.fillRect(w.x,w.y,w.w,w.h);
    }

    // drops
    for (const d of drops){
      ctx.fillStyle = d.kind==="ammo" ? "#ffd54a" : "#ff4ad6";
      ctx.beginPath(); ctx.arc(d.x,d.y,d.r,0,Math.PI*2); ctx.fill();
      ctx.fillStyle="#000";
      ctx.font="12px Arial";
      ctx.fillText(d.kind==="ammo" ? "AM" : "+", d.x-7, d.y+4);
    }

    // enemies
    for (const e of enemies){
      ctx.fillStyle = e.type==="grunt" ? "#ff5a5a" : e.type==="rifle" ? "#b57bff" : "#9cff57";
      ctx.beginPath(); ctx.arc(e.x,e.y,e.r,0,Math.PI*2); ctx.fill();

      // HP bar
      const pct = clamp(e.hp/e.maxHp,0,1);
      ctx.fillStyle="#000";
      ctx.fillRect(e.x-e.r, e.y-e.r-10, e.r*2, 6);
      ctx.fillStyle="#fff";
      ctx.fillRect(e.x-e.r, e.y-e.r-10, e.r*2*pct, 6);
    }

    // player
    // body
    ctx.fillStyle = player.invuln>0 ? "rgba(0,229,255,0.55)" : "#00e5ff";
    ctx.beginPath(); ctx.arc(player.x,player.y,player.r,0,Math.PI*2); ctx.fill();

    // aim line
    const ang = Math.atan2(mouse.y-player.y, mouse.x-player.x);
    ctx.strokeStyle="#fff";
    ctx.globalAlpha=0.35;
    ctx.beginPath();
    ctx.moveTo(player.x,player.y);
    ctx.lineTo(player.x+Math.cos(ang)*40, player.y+Math.sin(ang)*40);
    ctx.stroke();
    ctx.globalAlpha=1;

    // shots
    for (const s of shots){
      ctx.strokeStyle = s.enemy ? "#ff9b2f" : "#ffffff";
      ctx.globalAlpha = 0.25 + (s.life/6)*0.55;
      ctx.beginPath();
      ctx.moveTo(s.x1,s.y1);
      ctx.lineTo(s.x2,s.y2);
      ctx.stroke();
    }
    ctx.globalAlpha=1;

    // particles
    ctx.fillStyle="#fff";
    for (const p of particles){
      ctx.fillRect(p.x,p.y,2,2);
    }

    // HUD
    const g = player.gun;
    ctx.fillStyle="#fff";
    ctx.font="16px Arial";
    ctx.fillText(`Score: ${score}`, 12, 22);
    ctx.fillText(`Wave: ${wave}`, 12, 42);

    // HP bar
    ctx.fillText(`HP`, 12, 68);
    ctx.fillStyle="#000";
    ctx.fillRect(42, 56, 200, 14);
    ctx.fillStyle="#fff";
    ctx.fillRect(42, 56, 200*clamp(player.hp/player.maxHp,0,1), 14);
    ctx.fillStyle="#fff";
    ctx.fillText(`${Math.max(0,Math.floor(player.hp))}/100`, 250, 68);

    // ammo
    ctx.fillText(`Ammo: ${g.ammo}/${g.magSize}  Reserve: ${g.reserve}`, 12, 94);
    if (g.reloading){
      ctx.globalAlpha=0.8;
      ctx.fillText(`Reloading...`, 12, 116);
      ctx.globalAlpha=1;
    }

    if (paused && !gameOver){
      ctx.fillStyle="rgba(0,0,0,0.65)";
      ctx.fillRect(0,0,W,H);
      ctx.fillStyle="#fff";
      ctx.font="32px Arial";
      ctx.fillText("PAUSED", W/2-70, H/2-10);
      ctx.font="16px Arial";
      ctx.fillText("Press P to resume", W/2-75, H/2+20);
    }

    if (gameOver){
      ctx.fillStyle="rgba(0,0,0,0.75)";
      ctx.fillRect(0,0,W,H);
      ctx.fillStyle="#fff";
      ctx.font="36px Arial";
      ctx.fillText("YOU DIED", W/2-85, H/2-20);
      ctx.font="18px Arial";
      ctx.fillText(`Final Score: ${score}`, W/2-70, H/2+10);
      ctx.fillText("Press R to restart", W/2-80, H/2+40);
    }
  }

  // ---------- loop ----------
  function loop(t){
    if (!gameOver) update(t);
    draw(t);
    requestAnimationFrame(loop);
  }

  reset();
  requestAnimationFrame(loop);
})();
</script>
</body>
</html>
