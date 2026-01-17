<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Quiz Mundial</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body {
  font-family: 'Arial', sans-serif;
  background: linear-gradient(135deg,#667eea,#764ba2);
  min-height: 100vh;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
.card {
  background: white;
  padding: 25px;
  border-radius: 18px;
  width: 100%;
  max-width: 420px;
  box-shadow: 0 15px 30px rgba(0,0,0,0.2);
  text-align: center;
  animation: fade 0.5s ease;
}
@keyframes fade {
  from {opacity:0; transform:translateY(10px);}
  to {opacity:1; transform:translateY(0);}
}
h2 {
  margin-top: 0;
}
button {
  width: 100%;
  padding: 14px;
  margin: 8px 0;
  font-size: 16px;
  border: none;
  border-radius: 10px;
  background: #667eea;
  color: white;
  cursor: pointer;
}
button:hover {
  background: #5a6fdc;
}
select {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  margin: 10px 0;
  border-radius: 8px;
}
.small {
  font-size: 14px;
  opacity: 0.8;
}
</style>
</head>
<body>

<audio id="musica" loop>
  <source src="https://cdn.pixabay.com/audio/2022/03/15/audio_5e0d7c6c89.mp3" type="audio/mpeg">
</audio>

<div class="card" id="inicio">
  <h2>🌍 Quiz Mundial</h2>

  <select id="idioma">
    <option value="pt">🇧🇷 Português</option>
    <option value="fr">🇫🇷 Français</option>
    <option value="en">🇬🇧 English</option>
  </select>

  <select id="nivel">
    <option value="facil">⭐ Fácil</option>
    <option value="medio">⭐⭐ Médio</option>
    <option value="dificil">⭐⭐⭐ Difícil</option>
  </select>

  <button onclick="toggleMusica()">🎵 Música ON/OFF</button>
  <button onclick="iniciar()">▶ Começar</button>

  <p class="small">Simples • Educativo • Divertido</p>
</div>

<div class="card" id="jogo" style="display:none">
  <h2 id="pergunta"></h2>
  <div id="respostas"></div>
  <p id="info"></p>
</div>

<script>
let lang, nivel, atual = 0, pontos = 0;
const musica = document.getElementById("musica");

const perguntas = {
 pt: {
  facil: [
   {q:"Capital da França?", r:["Paris","Roma","Madri"], c:0},
   {q:"Planeta vermelho?", r:["Marte","Júpiter","Vênus"], c:0}
  ],
  medio: [
   {q:"Quem pintou a Mona Lisa?", r:["Da Vinci","Picasso","Van Gogh"], c:0}
  ],
  dificil: [
   {q:"Em que ano começou a Segunda Guerra Mundial?", r:["1939","1918","1945"], c:0}
  ]
 },
 fr: {
  facil: [
   {q:"Capitale de la France ?", r:["Paris","Rome","Madrid"], c:0}
  ],
  medio: [
   {q:"Qui a peint la Joconde ?", r:["Da Vinci","Picasso","Van Gogh"], c:0}
  ],
  dificil: [
   {q:"Début de la Seconde Guerre mondiale ?", r:["1939","1918","1945"], c:0}
  ]
 },
 en: {
  facil: [
   {q:"Capital of France?", r:["Paris","Rome","Madrid"], c:0}
  ],
  medio: [
   {q:"Who painted the Mona Lisa?", r:["Da Vinci","Picasso","Van Gogh"], c:0}
  ],
  dificil: [
   {q:"When did WWII start?", r:["1939","1918","1945"], c:0}
  ]
 }
};

function toggleMusica() {
  musica.paused ? musica.play() : musica.pause();
}

function iniciar() {
 lang = idioma.value;
 nivel = document.getElementById("nivel").value;
 document.getElementById("inicio").style.display="none";
 document.getElementById("jogo").style.display="block";
 carregar();
}

function carregar() {
 const lista = perguntas[lang][nivel];
 document.getElementById("info").innerText = `Pontos: ${pontos}`;
 const p = lista[atual];
 document.getElementById("pergunta").innerText = p.q;
 const r = document.getElementById("respostas");
 r.innerHTML="";
 p.r.forEach((op,i)=>{
  const b=document.createElement("button");
  b.innerText=op;
  b.onclick=()=>{
   if(i===p.c) pontos++;
   atual++;
   atual<lista.length ? carregar() : fim();
  }
  r.appendChild(b);
 });
}

function fim() {
 document.body.innerHTML = `
 <div class="card">
  <h2>🏆 Fim do jogo</h2>
  <p>Você fez ${pontos} ponto(s)</p>
  <button onclick="location.reload()">Jogar novamente</button>
 </div>`;
}
</script>

</body>
</html>
