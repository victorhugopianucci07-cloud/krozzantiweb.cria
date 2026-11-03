<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#090909">
<title>CalcDesconto — Calculadora de descontos</title>
<style>
:root{--red:#ff1425;--red2:#c90018;--bg:#080808;--panel:#151515;--muted:#b9b9b9}
*{box-sizing:border-box}
body{margin:0;background:radial-gradient(ellipse at 50% 0%,#3b080e 0,#100608 34%,var(--bg) 75%);color:#fff;font-family:Inter,Arial,sans-serif;min-height:100vh}
header{border-bottom:1px solid #a40c1b;box-shadow:0 4px 25px #ff142522;padding:20px clamp(18px,5vw,70px);display:flex;align-items:center;justify-content:space-between;gap:16px}
.brand{display:flex;align-items:center;gap:13px}.logo{font-size:38px;color:var(--red)}.brand h1{font-size:clamp(25px,4vw,40px);margin:0;font-weight:900;letter-spacing:-1.5px}.brand h1 span{color:var(--red)}.brand p{margin:2px 0 0;color:#ccc;font-size:14px}
main{width:min(1100px,92%);margin:38px auto 60px}
.hero{text-align:center;margin-bottom:28px}.hero h2{font-size:clamp(30px,5vw,48px);margin:0 0 8px}.hero h2 span{color:var(--red)}.hero p{color:#ccc;margin:0;font-size:clamp(14px,2vw,18px)}
.panel{background:linear-gradient(145deg,#1b1b1b,#0d0d0d);border:1px solid #e21a2c;border-radius:20px;padding:25px;box-shadow:0 12px 35px #0008,0 0 25px #ff142515}
.fields{display:grid;grid-template-columns:1.4fr 1fr 1fr auto;gap:18px;align-items:end}
label{display:block;font-weight:700;margin-bottom:9px;color:#eee}
input{width:100%;padding:15px 14px;border:1px solid #555;border-radius:11px;background:#080808;color:white;font-size:20px;outline:none}
input:focus{border-color:var(--red);box-shadow:0 0 0 3px #ff142522}
button{border:0;border-radius:12px;padding:16px 25px;background:linear-gradient(135deg,#ff2434,#d00019);color:white;font-size:18px;font-weight:800;cursor:pointer;box-shadow:0 5px 0 #79000d;transition:.15s;white-space:nowrap}
button:hover{filter:brightness(1.12);transform:translateY(-1px)}button:active{transform:translateY(2px);box-shadow:0 2px 0 #79000d}
.results{margin-top:26px;padding:0;overflow:hidden}
.results-head{padding:17px 22px;background:linear-gradient(90deg,#c90018,#24070a 70%);font-size:25px;font-weight:900;border-bottom:1px solid #9e1020}
.result-row{display:grid;grid-template-columns:55px 1fr auto;gap:16px;align-items:center;padding:16px 20px;border-bottom:1px solid #5d1019}
.result-row:last-child{border-bottom:0}.result-icon{font-size:30px;color:var(--red);text-align:center}
.result-title{font-size:18px;font-weight:800}.result-detail{font-size:14px;color:#bbb;margin-top:4px}
.amount{font-size:clamp(19px,3vw,27px);font-weight:900;background:#c90018;padding:12px 18px;border-radius:10px;min-width:205px;text-align:center}
.summary{margin-top:20px;border:1px solid var(--red);border-radius:15px;padding:18px 22px;background:#12090a;display:flex;justify-content:space-between;align-items:center;gap:16px}
.summary strong{color:var(--red);font-size:19px}.summary p{margin:5px 0 0;color:#eee}.summary-value{font-size:clamp(24px,4vw,34px);font-weight:900;color:var(--red);white-space:nowrap}
.note{color:#aaa;text-align:center;font-size:13px;margin-top:22px}
@media(max-width:760px){header{align-items:flex-start}.fields{grid-template-columns:1fr 1fr}.fields .principal{grid-column:1/-1}button{width:100%}.result-row{grid-template-columns:38px 1fr}.amount{grid-column:2;justify-self:stretch;min-width:0}.summary{align-items:flex-start;flex-direction:column}}
@media(max-width:430px){.fields{grid-template-columns:1fr}.fields .principal{grid-column:auto}.panel{padding:17px}.result-row{padding:14px 12px;gap:10px}.results-head{font-size:21px}.summary{padding:16px}}
</style>
</head>
<body>
<header>
  <div class="brand"><div class="logo">▦</div><div><h1>Calc<span>Desconto</span></h1><p>Sua calculadora de descontos avançada!</p></div></div>
</header>
<main>
  <section class="hero">
    <h2>Calculadora de <span>Descontos</span></h2>
    <p>Digite o valor e personalize as porcentagens para calcular tudo rapidamente.</p>
  </section>
  <section class="panel">
    <div class="fields">
      <div class="principal"><label for="valor">Valor principal (R$)</label><input id="valor" type="number" min="0" step="0.01" value="100000"></div>
      <div><label for="desc1">Desconto 1 (%)</label><input id="desc1" type="number" min="0" max="100" step="0.01" value="25"></div>
      <div><label for="desc2">Desconto 2 (%)</label><input id="desc2" type="number" min="0" max="100" step="0.01" value="5"></div>
      <button id="calcular" type="button">▦ &nbsp; Calcular</button>
    </div>
  </section>
  <section class="panel results" aria-live="polite">
    <div class="results-head">✓ &nbsp; Resultados</div>
    <div class="result-row">
      <div class="result-icon">％</div>
      <div><div class="result-title" id="linha1titulo">100.000 menos 25%</div><div class="result-detail" id="linha1detalhe">25% = R$ 25.000,00</div></div>
      <div class="amount" id="resultado1">R$ 75.000,00</div>
    </div>
    <div class="result-row">
      <div class="result-icon">％</div>
      <div><div class="result-title" id="linha2titulo">100.000 menos 5%</div><div class="result-detail" id="linha2detalhe">5% = R$ 5.000,00</div></div>
      <div class="amount" id="resultado2">R$ 95.000,00</div>
    </div>
    <div class="result-row">
      <div class="result-icon">◎</div>
      <div><div class="result-title" id="linha3titulo">10% do resultado (R$ 95.000,00)</div><div class="result-detail" id="linha3detalhe">10% = R$ 9.500,00</div></div>
      <div class="amount" id="resultado3">R$ 9.500,00</div>
    </div>
  </section>
  <section class="summary">
    <div><strong>✦ &nbsp; Resumo final</strong><p>Valor de 10% calculado sobre o resultado do segundo cálculo.</p></div>
    <div class="summary-value" id="resumo">R$ 9.500,00</div>
  </section>
  <p class="note">Os cálculos são feitos separadamente sobre o valor principal. O percentual final é aplicado ao resultado do segundo cálculo.</p>
</main>
<script>
const el = id => document.getElementById(id);
const dinheiro = n => n.toLocaleString('pt-BR',{style:'currency',currency:'BRL'});
const numero = n => n.toLocaleString('pt-BR',{maximumFractionDigits:2});
function calcular(){
  const valor = Number(el('valor').value);
  const p1 = Number(el('desc1').value);
  const p2 = Number(el('desc2').value);
  if(!Number.isFinite(valor)||!Number.isFinite(p1)||!Number.isFinite(p2)||valor<0||p1<0||p1>100||p2<0||p2>100){
    alert('Informe um valor válido e percentuais entre 0% e 100%.'); return;
  }
  const desconto1=valor*p1/100, resultado1=valor-desconto1;
  const desconto2=valor*p2/100, resultado2=valor-desconto2;
  const final=resultado2*0.10;
  el('linha1titulo').textContent=`${numero(valor)} menos ${numero(p1)}%`;
  el('linha1detalhe').textContent=`(${numero(p1)}% = ${dinheiro(desconto1)})`;
  el('resultado1').textContent=dinheiro(resultado1);
  el('linha2titulo').textContent=`${numero(valor)} menos ${numero(p2)}%`;
  el('linha2detalhe').textContent=`(${numero(p2)}% = ${dinheiro(desconto2)})`;
  el('resultado2').textContent=dinheiro(resultado2);
  el('linha3titulo').textContent=`10% do resultado (${dinheiro(resultado2)})`;
  el('linha3detalhe').textContent=`(10% = ${dinheiro(final)})`;
  el('resultado3').textContent=dinheiro(final);
  el('resumo').textContent=dinheiro(final);
}
el('calcular').addEventListener('click',calcular);
['valor','desc1','desc2'].forEach(id=>el(id).addEventListener('input',calcular));
calcular();
</script>
</body>
</html>
