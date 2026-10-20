const STORAGE_KEY = 'polis-prototype-v1';

const defaults = {
  step: 0,
  name: '',
  motto: '',
  geography: '',
  values: [],
  reason: '',
  decisions: {},
  agora: '',
  emergency: '',
  metrics: {fairness:50, freedom:50, trust:50, stability:50, resources:50},
  completed: [],
  continueIntent: false
};

const steps = [
  'Welcome','Ancient Greece','Found Your Polis','Choose Values','Founding Decisions','The Agora','Water Emergency','Consequences'
];

const app = document.getElementById('app');
const progressDialog = document.getElementById('progressDialog');
const progressContent = document.getElementById('progressContent');
let state = load();

function load(){
  try { return {...structuredClone(defaults), ...(JSON.parse(localStorage.getItem(STORAGE_KEY))||{})}; }
  catch { return structuredClone(defaults); }
}
function save(){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
  const s=document.getElementById('saveStatus'); if(s) s.textContent='Saved on this device';
}
function reset(){ localStorage.removeItem(STORAGE_KEY); state=structuredClone(defaults); render(0); }
function complete(key){ if(!state.completed.includes(key)) state.completed.push(key); }
function clamp(n){ return Math.max(0,Math.min(100,n)); }
function apply(delta){ Object.entries(delta).forEach(([k,v])=>state.metrics[k]=clamp((state.metrics[k]||50)+v)); }
function metricsHTML(){
  return `<div class="metrics">${Object.entries(state.metrics).map(([k,v])=>`<div><span>${cap(k)}</span><strong>${v}</strong></div>`).join('')}</div>`;
}
function cap(s){return s.charAt(0).toUpperCase()+s.slice(1)}
function progressPct(){ return Math.round((Math.min(state.step,steps.length-1)/(steps.length-1))*100); }
function shell(title, subtitle, body, kicker='Chapter One'){
  return `<section class="screen"><div class="chapter-header"><div><div class="kicker">${kicker}</div><h2>${title}</h2><p>${subtitle}</p></div><div><div class="help">Journey progress</div><div class="progress-track" style="width:180px"><div class="progress-fill" style="width:${progressPct()}%"></div></div></div></div>${body}</section>`;
}
function btn(label,next,cls='primary'){return `<button class="${cls}" data-next="${next}">${label}</button>`}
function toast(msg){const el=document.createElement('div');el.className='toast';el.textContent=msg;document.body.appendChild(el);setTimeout(()=>el.remove(),1600)}

function render(step=state.step){ state.step=step; save(); window.scrollTo({top:0,behavior:'instant'});
  const views=[homeView,storyView,foundView,valuesView,decisionsView,agoraView,emergencyView,resultView];
  app.innerHTML=views[Math.min(step,views.length-1)]();
  bind();
  app.focus({preventScroll:true});
}

function homeView(){
  return `<section class="hero">
    <div class="hero-copy">
      <div class="eyebrow">Interactive prototype • ages 11–15</div>
      <h1>POLIS</h1>
      <h2>Build a society.<br>Make the rules.<br>Face the consequences.</h2>
      <p>An island is waiting. Thousands of people will soon arrive. They will need homes, laws, water, schools and a government. Ancient Greece gives you ideas — but <strong>you</strong> decide what to keep, change or reject.</p>
      <div class="button-row">
        <button class="primary" data-next="1">${state.completed.length ? 'Continue Chapter One' : 'Begin Chapter One'}</button>
        ${state.completed.length?'<button class="secondary" id="viewCharter">View my progress</button>':''}
      </div>
      <p class="help">No account. No score to chase. Your choices are saved only on this device in this prototype.</p>
    </div>
    <div class="hero-art"><img src="assets/market.png" alt="Ancient Greek market street beside the sea"></div>
  </section>`;
}

function storyView(){
  return shell('Wake up in another world','Before you create a society, you need to understand the world that inspired this one.',`
    <div class="card story-grid">
      <div class="story-text">
        <p>Imagine waking more than 2,400 years ago. Sunlight slips through a wooden shutter onto white plaster walls. Your home might be a modest room with clay jars, woven mats and a small courtyard — or, if your family is wealthy, a larger house with several rooms and servants doing much of the work.</p>
        <p>You smell <strong>wood smoke, olive oil and warm bread</strong>. Breakfast is simple: bread, olives, figs, goat's cheese and water — perhaps sweetened with honey. Outside, the smells change: fresh fish, animals, dust, herbs crushed under sandals, smoke from workshops and bread baking nearby.</p>
        <p>The street contains everyone: labourers carrying baskets, craftspeople opening workshops, traders bargaining, children weaving through crowds and better-dressed citizens heading towards public business. Ancient Greek society is full of ideas — and full of inequality.</p>
      </div>
      <div class="story-image"><img src="assets/breakfast.png" alt="A household eating a simple breakfast"></div>
    </div>
    <div class="sense-row">
      <div class="sense"><strong>YOU CAN SMELL</strong> Bread, fish, olive oil, smoke, herbs, animals and the sea.</div>
      <div class="sense"><strong>YOU CAN HEAR</strong> Traders calling, sandals on stone, hammers, carts, animals and arguments.</div>
      <div class="sense"><strong>YOU CAN SEE</strong> Rich and poor living in the same city — but with very different power and choices.</div>
    </div>
    <div class="card" style="margin-top:1rem">
      <h3 style="color:var(--purple);font-size:1.6rem;margin-top:0">One Greece? Not quite.</h3>
      <p class="story-text">Ancient Greece is made up of many independent <strong>city-states</strong>, or <strong>poleis</strong>. Athens, Sparta and Corinth each have their own laws, leaders, customs and armies. Athens experiments with democracy, but most people living there cannot vote. Greek thinkers ask enormous questions about justice, power and the good life. One of them, Socrates, will eventually be put on trial by his own city.</p>
      <p class="quote">You are not going to copy Ancient Greece. You are going to learn from it — and decide what your society should do differently.</p>
      <div class="button-row">${btn('I’m ready to found my polis',2)}</div>
    </div>`, 'Before You Begin');
}

function foundView(){
  return shell('Found your polis','Give your new society an identity. Nothing is permanent yet — good governments revise ideas.',`
    <div class="card">
      <div class="form-grid">
        <div><label for="polisName">Name of your polis</label><input id="polisName" type="text" maxlength="32" placeholder="e.g. Asteria" value="${escapeAttr(state.name)}"></div>
        <div><label for="motto">Motto</label><input id="motto" type="text" maxlength="70" placeholder="e.g. Think freely. Act fairly." value="${escapeAttr(state.motto)}"></div>
      </div>
      <h3 style="margin-top:1.5rem;color:var(--purple)">Where will it be?</h3>
      <div class="choice-grid" id="geoChoices">
        ${geoChoice('island','Island Harbour','Trade and fishing are strong, but fresh water is limited.','Sea power')}
        ${geoChoice('valley','Fertile Valley','Plenty of farmland and water, but neighbours can reach you easily.','Food security')}
        ${geoChoice('mountain','Mountain Stronghold','Easier to defend, but travel and farming are harder.','Defence')}
      </div>
      <div><label for="foundReason">Why would people choose to live here?</label><textarea id="foundReason" placeholder="Write a few sentences. What can your polis offer its people?">${escapeHtml(state.reason)}</textarea></div>
      <div class="button-row"><button class="primary" id="saveFound">Continue</button></div>
    </div>`);
}
function geoChoice(id,title,desc,tag){return `<button class="choice-card ${state.geography===id?'selected':''}" data-geo="${id}"><span class="choice-tag">${tag}</span><h3>${title}</h3><p>${desc}</p></button>`}

const valueDefs={
  fairness:'Fairness', freedom:'Freedom', security:'Security', learning:'Learning', prosperity:'Prosperity', community:'Community', responsibility:'Responsibility', compassion:'Compassion'
};
function valuesView(){
  return shell('Choose your founding principles','Pick exactly three. Later, the difficult part will be deciding what happens when your values conflict.',`
    <div class="card">
      <p class="quote">A value sounds easy until it costs you something.</p>
      <div class="value-grid">${Object.entries(valueDefs).map(([k,v])=>`<button class="value-chip ${state.values.includes(k)?'selected':''}" data-value="${k}">${v}</button>`).join('')}</div>
      <p class="help"><strong id="valueCount">${state.values.length}/3 selected.</strong> Try to choose principles you genuinely want the government to protect.</p>
      <div class="button-row"><button class="primary" id="saveValues" ${state.values.length===3?'':'disabled'}>Seal the Founding Charter</button></div>
    </div>`);
}

const decisions=[
  {
    id:'voice', title:'Who gets a voice?', text:'Your first citizens are gathering in the agora. They want to know who gets a say in public decisions.',
    options:[
      {id:'few',title:'Only experienced property-owning citizens',desc:'Fewer people decide, on the grounds that they have more at stake.',d:{stability:7,freedom:-9,fairness:-10,trust:-4}},
      {id:'adults',title:'All adult citizens',desc:'Every adult citizen gets an equal vote.',d:{freedom:8,fairness:8,stability:-2,trust:4}},
      {id:'assembly',title:'Citizen assembly plus elected council',desc:'Big decisions go to citizens; day-to-day decisions go to a council.',d:{freedom:5,fairness:4,stability:5,resources:-2}}
    ]
  },
  {
    id:'newcomers', title:'Who may become a citizen?', text:'Traders and craftspeople from other places want to settle permanently in your polis.',
    options:[
      {id:'closed',title:'Citizenship by birth only',desc:'Protect a strong common identity and limit political membership.',d:{stability:5,freedom:-5,fairness:-7,resources:-2}},
      {id:'path',title:'Create a path to citizenship',desc:'Long-term residents can qualify after meeting clear rules.',d:{fairness:7,freedom:5,trust:4,resources:3}},
      {id:'open',title:'Citizenship after one year',desc:'Make it easy for newcomers to join the society quickly.',d:{freedom:8,fairness:5,resources:5,stability:-5}}
    ]
  },
  {
    id:'water', title:'Who controls fresh water?', text:'Your geography gives you limited springs and wells. Water is essential for homes, farms and workshops.',
    options:[
      {id:'public',title:'Water belongs to the polis',desc:'The government manages access and guarantees a basic amount to every household.',d:{fairness:8,trust:5,resources:-5,stability:3}},
      {id:'private',title:'Wells can be privately owned',desc:'Owners invest in wells and sell water at prices they choose.',d:{resources:7,freedom:4,fairness:-7,trust:-3}},
      {id:'mixed',title:'Basic water is public; extra use is paid',desc:'Everyone gets a minimum supply. Heavy users pay more.',d:{fairness:5,resources:4,stability:5,freedom:-1}}
    ]
  },
  {
    id:'law', title:'Can leaders break the rules?', text:'The founding council says leaders sometimes need special freedom to act quickly.',
    options:[
      {id:'same',title:'The same law applies to leaders and citizens',desc:'Power never puts anyone above the law.',d:{fairness:8,trust:8,stability:-1}},
      {id:'emergency',title:'Limited emergency powers',desc:'Leaders can temporarily gain extra powers, but only under written rules.',d:{stability:7,trust:2,freedom:-3}},
      {id:'broad',title:'Leaders decide when exceptions are needed',desc:'Speed and flexibility matter more than strict limits.',d:{stability:4,freedom:-8,fairness:-7,trust:-8}}
    ]
  }
];

function decisionsView(){
  const idx = decisions.findIndex(d=>!state.decisions[d.id]);
  if(idx===-1){complete('decisions');save();setTimeout(()=>render(5),0);return '<div></div>'}
  const d=decisions[idx];
  return shell(d.title, d.text, `<div class="card">${metricsHTML()}<div class="decision"><div class="decision-context"><div class="decision-number">Founding decision ${idx+1} of ${decisions.length}</div><h3>${d.title}</h3><p>${d.text}</p><p class="help" style="color:#dfd8ff">There is no perfect answer. Choose what best fits the society you are trying to build.</p></div><div class="decision-options">${d.options.map(o=>`<button class="option" data-decision="${d.id}" data-option="${o.id}"><strong>${o.title}</strong><span>${o.desc}</span></button>`).join('')}</div></div></div>`,'Founding Decisions');
}

function agoraView(){
  return shell('The Agora','A society does not only need decisions. It needs reasons.',`
    <div class="card agora">
      <div class="agora-image"><img src="assets/agora-scene.png" alt="Citizens listening to a public speaker in an agora"></div>
      <div>
        <div class="kicker">Your first public challenge</div>
        <h3 style="font-size:2rem;color:var(--purple);margin:.3rem 0">Should everyone have the same political voice?</h3>
        <p class="story-text">A wealthy landowner argues that people who own property and pay more towards the city should have more influence. A dock worker says a law affects his life just as much as anyone else's. A young resident asks why age should automatically make someone's opinion wiser.</p>
        <p class="quote">Your task is not to give the “school answer”. Tell the citizens what you believe — and why.</p>
        <label for="agoraAnswer">Your argument</label><textarea id="agoraAnswer" placeholder="My position is... My strongest reason is... A good argument against me would be...">${escapeHtml(state.agora)}</textarea>
        <div class="button-row"><button class="primary" id="saveAgora">Address the Agora</button></div>
      </div>
    </div>`,'The Agora');
}

function emergencyView(){
  const waterDecision=state.decisions.water||'mixed';
  const setup={
    public:'You promised every household a basic public water supply. The reservoirs are falling fast, and farmers say rationing will ruin their crops.',
    private:'Several wealthy well owners still have water. Prices have tripled. Poor households are queuing outside public fountains that are nearly dry.',
    mixed:'Your basic public allowance is still running, but farms and workshops say the paid extra supply has become unaffordable.'
  }[waterDecision];
  return shell('The wells are drying up',setup,`
    <div class="emergency-banner"><div class="kicker">Government Emergency</div><h2>Water Crisis</h2><p>${setup}</p></div>
    <div class="card">
      ${metricsHTML()}
      <h3 style="color:var(--purple);font-size:1.7rem">Your council gives you three options.</h3>
      <div class="decision-options">
        <button class="option" data-emergency="ration"><strong>Emergency rationing</strong><span>Guarantee every person a minimum amount. Farms and businesses receive strict limits until rainfall returns.</span></button>
        <button class="option" data-emergency="market"><strong>Let price control demand</strong><span>Do not ration. Allow scarce water to become more expensive so people naturally use less.</span></button>
        <button class="option" data-emergency="works"><strong>Build emergency water works</strong><span>Spend heavily on cisterns, transport and new wells. Preserve supply, but drain much of the treasury.</span></button>
      </div>
      <div class="consequence"><strong>Remember:</strong> your earlier rules still matter. You cannot pretend you made different choices when the crisis arrived.</div>
    </div>`,'Crisis Centre');
}

function resultView(){
  const e=state.emergency;
  const outcomes={
    ration:{title:'You ration the water.',text:'Basic access becomes more equal and public confidence rises among poorer households. Farmers and workshops are angry about lost income. You have protected fairness, but placed limits on individual choice.',delta:{fairness:8,trust:5,freedom:-5,stability:3,resources:-2}},
    market:{title:'You let prices rise.',text:'Total water use falls quickly. Families with money adapt; poorer households struggle. The treasury is protected, but arguments erupt over whether an essential resource should depend on ability to pay.',delta:{resources:7,freedom:4,fairness:-9,trust:-5,stability:-2}},
    works:{title:'You build emergency water works.',text:'The polis avoids the worst shortages. Construction creates work and public confidence rises — but your reserves are badly depleted. A future crisis may now be harder to afford.',delta:{trust:6,stability:7,resources:-12,fairness:3}}
  }[e] || {title:'No emergency choice recorded.',text:'Return to the crisis and make a decision.',delta:{}};
  const values=state.values.map(v=>valueDefs[v]).join(', ');
  const notable=lowestMetric();
  return shell('Chapter One complete','Your polis now exists. More importantly, it already contains tensions you created yourself.',`
    <div class="card">${metricsHTML()}
      <div class="result-grid">
        <div class="result-card"><div class="kicker">Immediate consequence</div><h3>${outcomes.title}</h3><p class="story-text">${outcomes.text}</p><p class="help">Your weakest civic indicator is currently <strong>${cap(notable[0])}</strong> at ${notable[1]}. That is not a failure. It is a problem your future government may have to solve.</p></div>
        <div class="charter"><h3>Founding Charter of ${escapeHtml(state.name||'Your Polis')}</h3><p><strong>Motto:</strong> ${escapeHtml(state.motto||'—')}</p><p><strong>Founding principles:</strong> ${values||'—'}</p><p><strong>Geography:</strong> ${geoLabel(state.geography)}</p><p><strong>Purpose:</strong> ${escapeHtml(state.reason||'—')}</p><p><strong>First public argument:</strong> ${escapeHtml(state.agora||'—')}</p></div>
      </div>
      <div class="consequence"><strong>The question for Chapter Two:</strong> Now that your citizens have seen your government act under pressure, who should have the power to change its decisions?</div>
      <div class="button-row"><button class="primary" id="continueTeaser">${state.continueIntent?'✓ I want Chapter Two':'I want to continue'}</button><button class="secondary" id="downloadCharter">Download my Charter</button><button class="secondary" id="exportTest">Export test result</button><button class="secondary" id="restartChapter">Try a different society</button></div>
    </div>`,'Chapter Complete');
}

function bind(){
  document.querySelectorAll('[data-next]').forEach(b=>b.onclick=()=>render(Number(b.dataset.next)));
  document.querySelectorAll('[data-geo]').forEach(b=>b.onclick=()=>{state.geography=b.dataset.geo;save();render(2)});
  document.querySelectorAll('[data-value]').forEach(b=>b.onclick=()=>{
    const v=b.dataset.value; const i=state.values.indexOf(v);
    if(i>=0) state.values.splice(i,1); else if(state.values.length<3) state.values.push(v); else return toast('Choose only three founding principles');
    save();render(3);
  });
  const sf=document.getElementById('saveFound'); if(sf) sf.onclick=()=>{
    state.name=document.getElementById('polisName').value.trim(); state.motto=document.getElementById('motto').value.trim(); state.reason=document.getElementById('foundReason').value.trim();
    if(!state.name||!state.geography){toast('Give your polis a name and choose its geography');return} complete('identity');save();render(3);
  };
  const sv=document.getElementById('saveValues'); if(sv) sv.onclick=()=>{ if(state.values.length!==3)return; complete('values');save();render(4)};
  document.querySelectorAll('[data-decision]').forEach(b=>b.onclick=()=>chooseDecision(b.dataset.decision,b.dataset.option));
  const sa=document.getElementById('saveAgora'); if(sa) sa.onclick=()=>{state.agora=document.getElementById('agoraAnswer').value.trim();if(state.agora.length<20){toast('Give the citizens a little more reasoning first');return}complete('agora');save();render(6)};
  document.querySelectorAll('[data-emergency]').forEach(b=>b.onclick=()=>chooseEmergency(b.dataset.emergency));
  const vc=document.getElementById('viewCharter'); if(vc) vc.onclick=showProgress;
  const ct=document.getElementById('continueTeaser'); if(ct) ct.onclick=()=>{state.continueIntent=true;save();render(7);toast('Recorded: you want to continue.');};
  const dc=document.getElementById('downloadCharter'); if(dc) dc.onclick=downloadCharter;
  const et=document.getElementById('exportTest'); if(et) et.onclick=exportTest;
  const rc=document.getElementById('restartChapter'); if(rc) rc.onclick=()=>{if(confirm('Start Chapter One again with a fresh society?')) reset()};
}

function chooseDecision(id,opt){
  const d=decisions.find(x=>x.id===id); const o=d.options.find(x=>x.id===opt); state.decisions[id]=opt; apply(o.d); complete(id);save();render(4);
}
function chooseEmergency(id){
  state.emergency=id; const delta={ration:{fairness:8,trust:5,freedom:-5,stability:3,resources:-2},market:{resources:7,freedom:4,fairness:-9,trust:-5,stability:-2},works:{trust:6,stability:7,resources:-12,fairness:3}}[id]; apply(delta); complete('emergency');save();render(7);
}
function lowestMetric(){return Object.entries(state.metrics).sort((a,b)=>a[1]-b[1])[0]}
function geoLabel(v){return ({island:'Island Harbour',valley:'Fertile Valley',mountain:'Mountain Stronghold'})[v]||'—'}
function escapeHtml(s=''){return String(s).replace(/[&<>'"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;',"'":'&#39;','"':'&quot;'}[c]))}
function escapeAttr(s=''){return escapeHtml(s)}


function downloadText(filename,text){
  const blob=new Blob([text],{type:'text/plain;charset=utf-8'}); const url=URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download=filename; document.body.appendChild(a); a.click(); a.remove(); setTimeout(()=>URL.revokeObjectURL(url),500);
}
function downloadCharter(){
  const values=state.values.map(v=>valueDefs[v]).join(', ');
  const text=`FOUNDING CHARTER OF ${state.name||'MY POLIS'}\n\nMotto: ${state.motto||'—'}\nGeography: ${geoLabel(state.geography)}\nFounding principles: ${values||'—'}\n\nWhy people would live here:\n${state.reason||'—'}\n\nMy first Agora argument:\n${state.agora||'—'}\n\nCivic indicators after Chapter One:\n${Object.entries(state.metrics).map(([k,v])=>`${cap(k)}: ${v}`).join('\n')}\n`;
  downloadText(`${(state.name||'polis').replace(/[^a-z0-9_-]+/gi,'_')}_Founding_Charter.txt`,text);
}
function exportTest(){
  const text=`POLIS PROTOTYPE TEST RESULT\n\nPolis: ${state.name||'—'}\nCompleted chapter: ${state.completed.includes('emergency')?'Yes':'No'}\nWould continue to Chapter Two: ${state.continueIntent?'YES':'Not recorded'}\nFounding values: ${state.values.map(v=>valueDefs[v]).join(', ')||'—'}\nGeography: ${geoLabel(state.geography)}\nWater decision: ${state.decisions.water||'—'}\nEmergency response: ${state.emergency||'—'}\n\nAgora response:\n${state.agora||'—'}\n`;
  downloadText(`POLIS_test_${(state.name||'anonymous').replace(/[^a-z0-9_-]+/gi,'_')}.txt`,text);
}

function showProgress(){
  const list=[['Opening story',state.step>=2],['Polis identity',state.completed.includes('identity')],['Founding principles',state.completed.includes('values')],['Founding decisions',state.completed.includes('decisions')],['Agora argument',state.completed.includes('agora')],['Water crisis',state.completed.includes('emergency')]];
  progressContent.innerHTML=`<h2>${escapeHtml(state.name||'Your Polis')}</h2><p class="help">Chapter One progress</p><div class="progress-list">${list.map(([n,d])=>`<div class="progress-item ${d?'done':''}"><span>${n}</span><strong>${d?'Complete':'Not yet'}</strong></div>`).join('')}</div>${metricsHTML()}`;
  progressDialog.showModal();
}

document.getElementById('progressBtn').onclick=showProgress;
document.getElementById('resetBtn').onclick=()=>{if(confirm('Erase this prototype progress and start over?'))reset()};
document.querySelector('.brand').onclick=()=>render(0);

if('serviceWorker' in navigator){navigator.serviceWorker.register('sw.js').catch(()=>{});}
render(state.step||0);
