<!DOCTYPE html>
<html lang="pt-AO">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Quiz Angola — Quanto sabes?</title>
<meta name="description" content="Responde a 10 perguntas sobre Angola e desafia os teus amigos no WhatsApp.">
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@500;800&display=swap" rel="stylesheet">
<style>
:root{--red:#C8102E;--black:#000;--gold:#FFC400;--white:#fff;--grey:#EFEFEF;--ink:#111;--ok:#0a7d3b}
*{box-sizing:border-box}
html,body{margin:0;min-height:100%}
body{font-family:Archivo,system-ui,Arial,sans-serif;background:var(--grey);color:var(--ink);display:flex;justify-content:center}
main{width:100%;max-width:480px;min-height:100vh;background:var(--white);display:flex;flex-direction:column}
header{background:var(--red);color:var(--white);padding:18px 20px;border-bottom:8px solid var(--black)}
header h1{margin:0;font-size:28px;font-weight:800;letter-spacing:-.5px}
header p{margin:4px 0 0;font-size:14px;opacity:.9}
section{padding:20px;flex:1;display:none}
section.on{display:block}
.bar{height:6px;background:var(--grey);margin-bottom:16px}
.bar i{display:block;height:100%;background:var(--gold);width:0;transition:width .3s}
.meta{display:flex;justify-content:space-between;font-weight:800;font-size:14px;margin-bottom:12px}
.q{font-size:22px;font-weight:800;line-height:1.25;margin:0 0 18px}
button{font:inherit;cursor:pointer}
.opt{display:block;width:100%;text-align:left;padding:14px 16px;margin-bottom:10px;border:2px solid var(--black);background:var(--white);font-weight:500;font-size:17px;border-radius:0}
.opt:active{background:var(--gold)}
.opt.right{background:var(--ok);color:#fff;border-color:var(--ok)}
.opt.wrong{background:var(--red);color:#fff;border-color:var(--red)}
.btn{display:block;width:100%;padding:16px;background:var(--black);color:var(--white);border:0;font-weight:800;font-size:18px;text-align:center;text-decoration:none;margin-top:12px}
.btn.gold{background:var(--gold);color:var(--black)}
.btn.wa{background:#128C7E}
:focus-visible{outline:3px solid var(--gold);outline-offset:2px}
.big{font-size:72px;font-weight:800;line-height:1;margin:6px 0}
.msg{font-size:18px;margin:0 0 8px}
.ad{margin:16px 0;min-height:50px;text-align:center}
.spons{border:2px dashed var(--black);padding:14px;margin:16px 0}
.spons b{display:block;font-size:18px;margin-bottom:4px}
.spons small{opacity:.7}
.apoio{font-size:14px;margin-top:18px;padding-top:14px;border-top:1px solid var(--grey)}
footer{padding:14px 20px;font-size:12px;opacity:.7;text-align:center}
@media (prefers-reduced-motion:reduce){.bar i{transition:none}}
</style>
</head>
<body>
<main>
<header><h1>Quiz Angola</h1><p>Quanto sabes sobre o teu país?</p></header>

<section id="home" class="on">
  <p class="q">10 perguntas. 20 segundos cada. Consegues acertar todas?</p>
  <p class="msg" id="best"></p>
  <button class="btn gold" id="start">Começar o quiz</button>
  <div class="ad" data-ad></div>
</section>

<section id="game">
  <div class="bar"><i id="prog"></i></div>
  <div class="meta"><span id="num"></span><span id="time"></span><span id="pts"></span></div>
  <p class="q" id="qtext"></p>
  <div id="opts"></div>
  <button class="btn" id="next" hidden>Próxima</button>
  <div class="ad" data-ad></div>
</section>

<section id="end">
  <p class="msg">O teu resultado</p>
  <div class="big" id="score"></div>
  <p class="msg" id="verdict"></p>
  <a class="btn wa" id="share" target="_blank" rel="noopener">Desafiar amigos no WhatsApp</a>
  <button class="btn gold" id="again">Jogar outra vez</button>
  <div class="ad" data-ad></div>
  <div class="spons" id="sponsor" hidden>
    <b id="spTitle"></b><span id="spText"></span><br>
    <a class="btn" id="spLink" target="_blank" rel="sponsored noopener"></a>
    <small>Publicidade</small>
  </div>
  <div class="apoio" id="apoio" hidden>
    Gostas deste quiz? Ajuda-nos a criar mais perguntas.<br>
    <span id="apoioTxt"></span>
    <a class="btn" id="apoioWa" target="_blank" rel="noopener" hidden>Apoiar por WhatsApp</a>
  </div>
</section>

<footer>© Quiz Angola · Só para entretenimento</footer>
</main>

<script>
/* ===== CONFIGURAÇÃO DE MONETIZAÇÃO — preenche só o que tiveres ===== */
const CFG = {
  siteUrl: "https://narciso74.github.io/earnner-angola/",
  adsenseClient: "",   // ex.: "ca-pub-1234567890123456" (depois da aprovação do Google AdSense)
  adsenseSlot: "",     // ex.: "1234567890" (ID do bloco de anúncio)
  sponsor: { title: "", text: "", cta: "Ver oferta", url: "" }, // link de afiliado ou patrocinador
  apoio: { texto: "Ajude-nos a criar mais perguntas.", iban: "AO06.0040.0000.7488.7669.1019.6", whatsapp: "244949240557" }
};

/* ===== PERGUNTAS: [pergunta, [opções], índice da certa] ===== */
const BANK = [
 ["Qual é a capital de Angola?",["Luanda","Huambo","Benguela","Lubango"],0],
 ["Em que ano Angola se tornou independente?",["1965","1975","1985","1991"],1],
 ["Qual é a moeda de Angola?",["Metical","Rand","Kwanza","Dalasi"],2],
 ["Qual é a língua oficial de Angola?",["Inglês","Francês","Umbundu","Português"],3],
 ["Quem foi o primeiro presidente de Angola?",["Agostinho Neto","José Eduardo dos Santos","João Lourenço","Jonas Savimbi"],0],
 ["Que oceano banha a costa de Angola?",["Índico","Atlântico","Pacífico","Ártico"],1],
 ["As Quedas de Kalandula ficam em que província?",["Huíla","Bié","Malanje","Namibe"],2],
 ["Que país faz fronteira com Angola?",["Quénia","Moçambique","Gana","Namíbia"],3],
 ["Em que província fica o Morro do Moco, o ponto mais alto do país?",["Huambo","Cabinda","Zaire","Cuando Cubango"],0],
 ["Qual província está separada do resto do país por território da RDC?",["Zaire","Cabinda","Uíge","Cunene"],1],
 ["Em que ano Angola jogou pela primeira vez um Mundial de futebol?",["1998","2002","2006","2010"],2],
 ["Qual ritmo angolano é considerado a raiz da kizomba?",["Semba","Funaná","Coladeira","Marrabenta"],0],
 ["Que cores tem a bandeira de Angola?",["Verde e amarelo","Vermelho e preto","Azul e branco","Verde e vermelho"],1],
 ["Qual é o maior rio que corre inteiramente em território angolano?",["Zambeze","Cunene","Cuanza","Okavango"],2],
 ["Em que cidade fica a Fortaleza de São Miguel?",["Luanda","Lobito","Malanje","Saurimo"],0]
];
const ROUND = 10, SECS = 20;

const $ = id => document.getElementById(id);
const show = id => { document.querySelectorAll("section").forEach(s => s.classList.toggle("on", s.id === id)); window.scrollTo(0,0); };
const shuffle = a => { a = a.slice(); for (let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];} return a; };
const store = {
  get(k){ try { return localStorage.getItem(k); } catch(e){ return null; } },
  set(k,v){ try { localStorage.setItem(k,v); } catch(e){} }
};

let qs=[], i=0, score=0, timer=null, left=SECS, locked=false;

/* Anúncios (AdSense) — só aparecem se a configuração estiver preenchida */
function initAds(){
  if(!CFG.adsenseClient || !CFG.adsenseSlot) return;
  const s = document.createElement("script");
  s.async =
