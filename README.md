# terminal-panacea-
dock 
<!DOCTYPE html>
<html lang="pt-PT">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PANACEA INDUSTRIES™ // TERMINAL CENTRAL DE ARQUIVOS</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: #030a03;
            color: #00ff66;
            font-family: 'Share Tech Mono', monospace;
            height: 100vh;
            overflow: hidden;
            padding: 20px;
            display: flex;
            flex-direction: column;
            text-shadow: 0 0 5px rgba(0, 255, 102, 0.4);
        }

        /* Efeito de linhas de CRT (Scanlines) */
        body::before {
            content: " ";
            display: block;
            position: fixed;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            z-index: 99999;
            background-size: 100% 4px, 6px 100%;
            pointer-events: none;
        }

        #terminal {
            flex: 1;
            overflow-y: auto;
            margin-bottom: 10px;
            padding-right: 10px;
            white-space: pre-wrap;
            word-wrap: break-word;
            line-height: 1.1; /* Linhas mais apertadas para ASCII art */
        }

        #terminal::-webkit-scrollbar {
            width: 6px;
        }
        #terminal::-webkit-scrollbar-thumb {
            background: #00ff66;
        }

        .input-line {
            display: flex;
            align-items: center;
            border-top: 1px dashed #00ff66;
            padding-top: 10px;
            margin-top: auto;
        }

        .prompt {
            margin-right: 10px;
            font-weight: bold;
        }

        input {
            background: transparent;
            border: none;
            color: #00ff66;
            font-family: 'Share Tech Mono', monospace;
            font-size: 1rem;
            outline: none;
            flex: 1;
            text-shadow: 0 0 5px rgba(0, 255, 102, 0.4);
        }

        .system-msg {
            color: #ffb700;
        }

        .error-msg {
            color: #ff3333;
        }

        .banner {
            color: #00ff66;
            margin-bottom: 15px;
            line-height: 1.4;
        }
    </style>
</head>
<body>

    <div id="terminal">
        <div class="banner">
PANACEA INDUSTRIES™ — SISTEMA OPERACIONAL CENTRAL [VERSÃO 4.12.0]
ACESSO RESTRITO — CONSELHO DOS ONZE
Estado de Segurança: <span class="error-msg">BLOQUEADO (Autenticação Necessária)</span>
Digite <span style="color: #ffb700;">'login'</span> para introduzir a credencial de acesso aos documentos.
Digite <span style="color: #ffb700;">'ajuda'</span> para ver os comandos gerais.
        </div>
    </div>

    <div class="input-line">
        <span class="prompt" id="prompt-symbol">nrcr@panacea:~#</span>
        <input type="text" id="command-input" autofocus autocomplete="off" spellcheck="false">
    </div>

    <script>
        const terminal = document.getElementById('terminal');
        const input = document.getElementById('command-input');
        const promptSymbol = document.getElementById('prompt-symbol');

        let isAuthenticated = false;
        let waitingForPassword = false;

        // SENHA DE ACESSO (Pode alterar aqui se quiser)
        const SECURE_PASSWORD = "panacea2026";

        const databases = {
            // Documentos confidenciais
            nrcr: `PANACEA INDUSTRIES™
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

            sume: `PANACEA INDUSTRIES™ — ESTAÇÃO SUMÉ
TRANSCRIÇÃO DE COMUNICAÇÃO INTERNA (SUMÉ-COM-017)
Participantes: S. Verdante, Dr. Vladimir Zurita, Laila Verdante.

S. VERDANTE: Recebemos um relatório preocupante de torres de rádio e uma emissora em Minas Gerais emitindo transmissões simultâneas com símbolos desconhecidos.
LAILA VERDANTE: Os pesquisadores locais demonstram comportamento compulsivo e estado de hipnose, recusando-se a desviar o olhar das telas durante horas.
STATUS: Investigação presencial suspensa. Instalação sob isolamento.`,

            atlas: `PANACEA INDUSTRIES™ — REGISTRO GLOBAL DE INSTALAÇÕES (PAN-GEO-001)
1. SUMÉ (Amazonas, Brasil) - Foco em mutações da fauna e flora.
2. MAXIMÓN (Durango, México) - Estudo de interferências digitais anômalas.
3. WOHPE (Alabama, EUA) - Agentes biológicos experimentais.
4. DURGĀ (Calcutá, Índia) - Sintetização farmacológica.
5. HYGIEIA (Sede Principal / Ilha Oculta) - Contenção de entidades anômalas.
6 & 7. GLACIUS & IGNEUS (Groenlândia) - Pesquisas sobre o agente infeccioso I-413 (Infecticídio) e Nidere.`,

            // Comandos não confidenciais (incluindo a logo)
            status: `PANACEA INDUSTRIES™ — RELATÓRIO DE ESTADO DE CONTENÇÃO

ESTADO DO AGENTE: <span class="error-msg">I-413 (Infecticídio) — ATIVO</span>

<span style="color: #ff3333;">         .
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

        input.addEventListener('keydown', function(event) {
            if (event.key === 'Enter') {
                const val = input.value;
                const cmd = val.trim().toLowerCase();

                const userLine = document.createElement('div');
                
                if (waitingForPassword) {
                    userLine.textContent = `auth@panacea:~# ********`;
                } else {
                    userLine.textContent = `nrcr@panacea:~# ${val}`;
                }
                terminal.appendChild(userLine);

                const responseLine = document.createElement('div');

                // 1. Processo de introdução de senha
                if (waitingForPassword) {
                    waitingForPassword = false;
                    promptSymbol.textContent = "nrcr@panacea:~#";
                    input.type = "text"; // voltar ao normal

                    if (val === SECURE_PASSWORD) {
                        isAuthenticated = true;
                        responseLine.className = 'system-msg';
                        responseLine.textContent = 'AUTENTICAÇÃO BEM-SUCEDIDA. Acesso total aos arquivos concedido.';
                    } else {
                        responseLine.className = 'error-msg';
                        responseLine.textContent = 'ACESSO NEGADO: Credencial incorreta.';
                    }
                    terminal.appendChild(responseLine);
                    input.value = '';
                    terminal.scrollTop = terminal.scrollHeight;
                    return;
                }

                // 2. Comandos normais
                if (cmd === '') {
                    // vazio
                } else if (cmd === 'login') {
                    if (isAuthenticated) {
                        responseLine.className = 'system-msg';
                        responseLine.textContent = 'O terminal já se encontra autenticado.';
                    } else {
                        waitingForPassword = true;
                        promptSymbol.textContent = "senha@panacea:~#";
                        input.type = "password"; // esconde o texto digitado
                        responseLine.className = 'system-msg';
                        responseLine.textContent = 'Introduza a credencial de acesso do Conselho:';
                    }
                } else if (cmd === 'ajuda') {
                    responseLine.textContent = `COMANDOS DISPONÍVEIS:
 - ajuda          : Exibe esta lista de comandos
 - sobre          : Informações institucionais
 - login          : Autenticar para desbloquear documentos confidenciais
 - status         : Relatório de estado do agente Infecticídio [Logo]
 - nrcr           : Abre o Arquivo Histórico NRCR-ENG-001 [Requer Login]
 - sume           : Abre a Transcrição SUMÉ-COM-017 [Requer Login]
 - atlas          : Abre o Atlas Global PAN-GEO-001 [Requer Login]
 - limpar         : Limpa o ecrã do terminal
 - sair           : Encerra a sessão atual`;
                } else if (cmd === 'sobre') {
                    responseLine.textContent = "PANACEA INDUSTRIES™\nMissão: Conhecimento, desenvolvimento e controle de fronteiras biológicas e anômalas.";
                } else if (cmd === 'limpar') {
                    terminal.innerHTML = '';
                    input.value = '';
                    return;
                } else if (cmd === 'sair') {
                    responseLine.className = 'system-msg';
                    responseLine.textContent = 'Sessão encerrada. Desligando terminal...';
                    terminal.appendChild(responseLine);
                    input.disabled = true;
                    return;
                } else if (databases[cmd]) {
                    // Verificar se está autenticado antes de abrir os documentos confidenciais
                    const secureCmds = ['nrcr', 'sume', 'atlas'];
                    if (secureCmds.includes(cmd)) {
                        if (isAuthenticated) {
                            responseLine.innerHTML = databases[cmd];
                        } else {
                            responseLine.className = 'error-msg';
                            responseLine.textContent = `ACESSO RESTRITO: O documento '${cmd}' requer autenticação prévia. Digite 'login' e insira a senha.`;
                        }
                    } else {
                        // Comandos livres (como o 'status' com a logo)
                        responseLine.innerHTML = databases[cmd];
                    }
                } else {
                    responseLine.className = 'error-msg';
                    responseLine.textContent = `Comando não reconhecido: "${val}". Digite 'ajuda' para ver os comandos válidos.`;
                }

                terminal.appendChild(responseLine);
                input.value = '';
                terminal.scrollTop = terminal.scrollHeight;
            }
        });
    </script>
</body>
</html>
