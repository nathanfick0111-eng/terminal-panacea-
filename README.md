<!DOCTYPE html>
<html lang="pt-PT">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PANACEA INDUSTRIES™ // TERMINAL CENTRAL</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');

        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            background: #030a03; color: #00ff66;
            font-family: 'Share Tech Mono', monospace;
            height: 100vh; padding: 20px;
            display: flex; flex-direction: column;
            text-shadow: 0 0 5px rgba(0, 255, 102, 0.4);
            overflow: hidden;
        }

        /* Efeito Scanlines CRT */
        body::before {
            content: " "; display: block; position: fixed;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            z-index: 99; background-size: 100% 4px, 6px 100%; pointer-events: none;
        }

        #terminal {
            flex: 1; overflow-y: auto; white-space: pre-wrap; line-height: 1.2;
        }
        #terminal::-webkit-scrollbar { width: 4px; }
        #terminal::-webkit-scrollbar-thumb { background: #00ff66; }

        .input-line {
            display: flex; border-top: 1px dashed #00ff66; padding-top: 10px; margin-top: 10px;
        }
        .prompt { margin-right: 10px; font-weight: bold; }
        input {
            background: transparent; border: none; color: #00ff66;
            font-family: inherit; font-size: 1rem; outline: none; flex: 1;
        }
        .warn { color: #ffb700; }
        .danger { color: #ff3333; }
    </style>
</head>
<body>

    <div id="terminal">
PANACEA INDUSTRIES™ — SISTEMA OPERACIONAL CENTRAL [v4.12]
Estado de Segurança: <span class="danger">BLOQUEADO (Requer Autenticação)</span>
Digite <span class="warn">'login'</span> para introduzir a credencial de acesso.
Digite <span class="warn">'ajuda'</span> para ver os comandos gerais.
    </div>

    <div class="input-line">
        <span class="prompt" id="prompt-symbol">nrcr@panacea:~#</span>
        <input type="text" id="cmd-input" autofocus autocomplete="off" spellcheck="false">
    </div>

    <script>
        const terminal = document.getElementById('terminal');
        const input = document.getElementById('cmd-input');
        const promptSymbol = document.getElementById('prompt-symbol');

        let isAuth = false;
        let isWritingPassword = false;
        const PASS = "N3URO1MPLANT3S";

        const docs = {
            nrcr: `[REGISTRO NRCR-ENG-001] — IMPLANTES CIBERNÉTICOS
---------------------------------------------------
EXP 001 [NEURAL LINK]: Sobrecarga neurológica e alucinações.
EXP 002 [SANDEVITÃO-VITTAR]: Espasmos e rotura de fibras musculares.
EXP 003 [SENSOR PARIS]: Superaquecimento ocular e perda de equilíbrio.
EXP 004 [GARRA ELU]: Descargas descontroladas e derretimento de componentes.
EXP 005 [INJETOR VALDÉS]: Queimaduras internas generalizadas.
EXP 006 [NÚCLEO CARDÍACO]: Deterioração progressiva dos tecidos.
EXP 007 [FIBRA MONOFIO]: Descontrolo total dos filamentos nos pulsos.

PARECER DO DR. UGARTE: "Construímos máquinas capazes de superar o corpo, mas não um corpo capaz de as suportar."`,

            sume: `[SUMÉ-COM-017] — TRANSCRIÇÃO DE EMERGÊNCIA
--------------------------------------------
S. Verdante: Torres de rádio em MG a transmitir sinais anómalos.
Laila Verdante: Pesquisadores em estado hipnótico, atraídos pelos símbolos nas telas.
STATUS: Instalação sob isolamento estrito.`,

            atlas: `[PAN-GEO-001] — ATLAS GLOBAL DE INSTALAÇÕES
--------------------------------------------
1. SUMÉ (Brasil) - Mutações na ecologia amazónica.
2. MAXIMÓN (México) - Interferências digitais anómalas.
3. WOHPE (EUA) - Agentes biológicos experimentais.
4. DURGĀ (Índia) - Sintetização farmacológica.
5. HYGIEIA (Sede Oculta) - Contenção de entidades anômalas.
6 & 7. GLACIUS / IGNEUS (Groenlândia) - Estudo do agente I-413 (Infecticídio).`,

            status: `RELATÓRIO DE ESTADO: <span class="danger">AGENTE I-413 (Infecticídio) — ATIVO</span>

<span class="danger">         .
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
Conhecimento, desenvolvimento e controle.`
        };

        // Função dinâmica para efeito de digitação rápida
        function printText(text, isHtml = false, speed = 4) {
            const line = document.createElement('div');
            terminal.appendChild(line);
            
            if (isHtml) {
                line.innerHTML = text;
                terminal.scrollTop = terminal.scrollHeight;
                return;
            }

            let i = 0;
            input.disabled = true;
            const timer = setInterval(() => {
                line.textContent += text.charAt(i);
                i++;
                terminal.scrollTop = terminal.scrollHeight;
                if (i >= text.length) {
                    clearInterval(timer);
                    input.disabled = false;
                    input.focus();
                }
            }, speed);
        }

        input.addEventListener('keydown', (e) => {
            if (e.key !== 'Enter') return;
            const val = input.value.trim();
            const cmd = val.toLowerCase();
            input.value = '';

            // Registar comando no ecrã
            printText(`${promptSymbol.textContent} ${isWritingPassword ? '********' : val}`, false, 0);

            // Validação de Senha
            if (isWritingPassword) {
                isWritingPassword = false;
                promptSymbol.textContent = "nrcr@panacea:~#";
                input.type = "text";
                if (val === PASS) {
                    isAuth = true;
                    printText('<span class="warn">AUTENTICAÇÃO ACEITE. Documentos desbloqueados.</span>', true);
                } else {
                    printText('<span class="danger">ACESSO NEGADO: Credencial incorreta.</span>', true);
                }
                return;
            }

            // Processamento de Comandos
            if (!cmd) return;

            if (cmd === 'login') {
                if (isAuth) {
                    printText('<span class="warn">Terminal já autenticado.</span>', true);
                } else {
                    isWritingPassword = true;
                    promptSymbol.textContent = "senha@panacea:~#";
                    input.type = "password";
                    printText('<span class="warn">Introduza a senha do Conselho:</span>', true);
                }
            } else if (cmd === 'ajuda') {
                printText(`COMANDOS:
 - ajuda   : Lista de comandos
 - login   : Autenticar terminal [Senha: N3URO1MPLANT3S]
 - status  : Logo Panacea & Estado do Agente I-413
 - nrcr    : Arquivo Histórico de Cibernética (Confidencial)
 - sume    : Transcrição Estação Sumé (Confidencial)
 - atlas   : Atlas de Instalações (Confidencial)
 - limpar  : Limpar ecrã`, false, 2);
            } else if (cmd === 'limpar') {
                terminal.innerHTML = '';
            } else if (cmd === 'sobre') {
                printText("PANACEA INDUSTRIES™ — Conhecimento, desenvolvimento e controle.", false, 2);
            } else if (docs[cmd]) {
                const confidenciais = ['nrcr', 'sume', 'atlas'];
                if (confidenciais.includes(cmd) && !isAuth) {
                    printText(`<span class="danger">ACESSO RESTRITO. Digite 'login' e use a senha.</span>`, true);
                } else {
                    printText(docs[cmd], cmd === 'status', 2);
                }
            } else {
                printText(`<span class="danger">Comando não reconhecido: "${val}". Digite 'ajuda'.</span>`, true);
            }
        });
    </script>
</body>
</html>
