<!DOCTYPE html><html lang="pt-PT"><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>PANACEA INDUSTRIES™ // TERMINAL CENTRAL DE ARQUIVOS</title><style>
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');*{box-sizing:border-box;margin:0;padding:0}body{background:#030a03;color:#00ff66;font-family:'Share Tech Mono',monospace;height:100vh;overflow:hidden;padding:20px;display:flex;flex-direction:column;text-shadow:0 0 5px #00ff6666}body:before{content:"";position:fixed;inset:0;background:linear-gradient(#12101000 50%,#0004 50%),linear-gradient(90deg,#f0000010,#00ff0005,#00f00010);background-size:100% 4px,6px 100%;z-index:99999;pointer-events:none}#terminal{flex:1;overflow-y:auto;margin-bottom:10px;padding-right:10px;white-space:pre-wrap;overflow-wrap:anywhere;line-height:1.1}#terminal::-webkit-scrollbar{width:6px}#terminal::-webkit-scrollbar-thumb{background:#00ff66}.input-line{display:flex;align-items:center;border-top:1px dashed #00ff66;padding-top:10px;margin-top:auto}.prompt{margin-right:10px;font-weight:bold}input{background:transparent;border:0;color:#00ff66;font:1rem 'Share Tech Mono',monospace;outline:0;flex:1;text-shadow:0 0 5px #00ff6666}.system-msg{color:#ffb700}.error-msg{color:#f33}.banner{margin-bottom:15px;line-height:1.4}
</style><body><div id="terminal"><div class="banner">PANACEA INDUSTRIES™ — SISTEMA OPERACIONAL CENTRAL [VERSÃO 4.12.0]
ACESSO RESTRITO — CONSELHO DOS ONZE
Estado de Segurança: <span class="error-msg">BLOQUEADO (Autenticação Necessária)</span>
Digite <span style="color:#ffb700">'login'</span> para introduzir a credencial de acesso aos documentos.
Digite <span style="color:#ffb700">'ajuda'</span> para ver os comandos gerais.</div></div><div class="input-line"><span class="prompt" id="prompt-symbol">nrcr@panacea:~#</span><input id="command-input" autofocus autocomplete="off" spellcheck="false"></div><script>
const terminal=document.getElementById('terminal'),input=document.getElementById('command-input'),prompt=document.getElementById('prompt-symbol'),PASS="C0L1S40.I413::N1D3R3";
let auth=false,waiting=false;
const db={
nrcr:`PANACEA INDUSTRIES™
COMPLEXO N.R.C.R. — ARQUIVO HISTÓRICO DE PESQUISA E DESENVOLVIMENTO
Registro: NRCR-ENG-001 | Período: 2003–2019 | Classificação: Confidencial
Responsável técnico: Dr. Sebastián Ugarte

1. CONTEXTUALIZAÇÃO
Os primeiros projetos de implantes cibernéticos visavam substituir funções orgânicas e ampliar capacidades físicas. O problema recorrente foi a incapacidade do organismo de suportar a quantidade de Energia conduzida.

2. REGISTROS EXPERIMENTAIS:
- EXP 001 [CO-PROCESSADOR NEURAL]: Sobrecarga neurológica e alucinações auditivas.
- EXP 002 [ATIVADOR CINÉTICO SANDEVITÃO-VITTAR]: Espasmos violentos e rompimento muscular.
- EXP 003 [SENSOR ÓPTICO PARIS]: Superaquecimento e sobrecarga sensorial.
- EXP 004 [GARRA ELU]: Descarga descontrolada e derretimento de componentes.
- EXP 005 [INJETOR SUBDÉRMICO VALDÉS]: Queimaduras internas e perda de controle muscular.
- EXP 006 [NÚCLEO CARDÍACO BIOMECÂNICO]: Deterioração progressiva dos tecidos.
- EXP 007 [TECIDO DE FIBRA MONOFIO]: Ruptura interna e superaquecimento nos pulsos.

PARECER DO DR. SEBASTIÁN UGARTE:
"Construímos máquinas capables de superar las limitaciones del cuerpo humano, mas ainda não conseguimos construir um corpo capaz de suportar nossas máquinas."`,
sume:`PANACEA INDUSTRIES™ — ESTAÇÃO SUMÉ
TRANSCRIÇÃO DE COMUNICAÇÃO INTERNA (SUMÉ-COM-017)
Participantes: S. Verdante, Dr. Vladimir Zurita, Laila Verdante.

S. VERDANTE: Recebemos um relatório preocupante de torres de rádio e uma emissora em Minas Gerais emitindo transmissões simultâneas com símbolos desconhecidos.
LAILA VERDANTE: Os pesquisadores locais demonstram comportamento compulsivo e estado de hipnose, recusando-se a desviar o olhar das telas durante horas.
STATUS: Investigação presencial suspensa. Instalação sob isolamento.`,
atlas:`PANACEA INDUSTRIES™ — REGISTRO GLOBAL DE INSTALAÇÕES (PAN-GEO-001)
1. SUMÉ (Amazonas, Brasil) - Foco em mutações da fauna e flora.
2. MAXIMÓN (Durango, México) - Estudo de interferências digitais anômalas.
3. WOHPE (Alabama, EUA) - Agentes biológicos experimentais.
4. DURGĀ (Calcutá, Índia) - Sintetização farmacológica.
5. HYGIEIA (Sede Principal / Ilha Oculta) - Contenção de entidades anômalas.
6 & 7. GLACIUS & IGNEUS (Groenlândia) - Pesquisas sobre o agente infeccioso I-413 (Infecticídio) e Nidere.`,
status:`PANACEA INDUSTRIES™ — RELATÓRIO DE ESTADO DE CONTENÇÃO

ESTADO DO AGENTE: <span class="error-msg">I-413 (Infecticídio) — ATIVO</span>

<span style="color:#f33">         .
       .iH.
      .HMMM.
     .HMMMMM.
    .HMMMMMMM.
   .HMMMMMMMMM.
  .HMMMMMMMMMMM.
 .HMMMMMMMMMMMMM.
.HMMMMMMMMMMMMMMM.
 VVVVMMMMMMMVVVVV
     VMMMMMV
      VMMMV
       VNV
        V</span>

ALERTA: Protocolos Glacius e Igneus operacionais. Colisão Nidere-I413 em preparação.
Monitoramento contínuo emWohpe e Hygieia.
Conhecimento, desenvolvimento e controle.`
};
input.addEventListener('keydown',e=>{if(e.key!=='Enter')return;const val=input.value,cmd=val.trim().toLowerCase(),u=document.createElement('div'),r=document.createElement('div');u.textContent=waiting?'auth@panacea:~# ********':`nrcr@panacea:~# ${val}`;terminal.appendChild(u);
if(waiting){waiting=false;prompt.textContent='nrcr@panacea:~#';input.type='text';r.className=val===PASS?'system-msg':'error-msg';if(val===PASS){auth=true;r.textContent='AUTENTICAÇÃO BEM-SUCEDIDA. Acesso total aos arquivos concedido.'}else r.textContent='ACESSO NEGADO: Credencial incorreta.'}
else if(!cmd){}
else if(cmd==='login'){if(auth){r.className='system-msg';r.textContent='O terminal já se encontra autenticado.'}else{waiting=true;prompt.textContent='senha@panacea:~#';input.type='password';r.className='system-msg';r.textContent='Introduza a credencial de acesso do Conselho:'}}
else if(cmd==='ajuda')r.textContent=`COMANDOS DISPONÍVEIS:
 - ajuda          : Exibe esta lista de comandos
 - sobre          : Informações institucionais
 - login          : Autenticar para desbloquear documentos confidenciais
 - status         : Relatório de estado do agente Infecticídio [Logo]
 - nrcr           : Abre o Arquivo Histórico NRCR-ENG-001 [Requer Login]
 - sume           : Abre a Transcrição SUMÉ-COM-017 [Requer Login]
 - atlas          : Abre o Atlas Global PAN-GEO-001 [Requer Login]
 - limpar         : Limpa o ecrã do terminal
 - sair           : Encerra a sessão atual`;
else if(cmd==='sobre')r.textContent='PANACEA INDUSTRIES™\nMissão: Conhecimento, desenvolvimento e controle de fronteiras biológicas e anômalas.';
else if(cmd==='limpar'){terminal.innerHTML='';input.value='';return}
else if(cmd==='sair'){r.className='system-msg';r.textContent='Sessão encerrada. Desligando terminal...';terminal.appendChild(r);input.disabled=true;return}
else if(db[cmd]){if(['nrcr','sume','atlas'].includes(cmd)&&!auth){r.className='error-msg';r.textContent=`ACESSO RESTRITO: O documento '${cmd}' requer autenticação prévia. Digite 'login' e insira a senha.`}else r.innerHTML=db[cmd]}
else{r.className='error-msg';r.textContent=`Comando não reconhecido: "${val}". Digite 'ajuda' para ver os comandos válidos.`}
terminal.appendChild(r);input.value='';terminal.scrollTop=terminal.scrollHeight});
</script></body></html>