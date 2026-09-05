# autounattend.xml — Windows 10/11 limpo e otimizado

Arquivo de resposta (`autounattend.xml`) para automatizar a instalação do Windows 10 ou 11, já saindo do zero **sem bloatware, sem telemetria desnecessária, mais rápido e com os apps essenciais pré-instalados**. Você não precisa clicar em nada durante a instalação — inclusive o assistente de conta Microsoft, anúncios do menu Iniciar e a maior parte da coleta de dados do Windows.

Baseado no [unattend-generator do schneegans.de](https://schneegans.de/windows/unattend-generator/), com correções e vários ajustes manuais adicionais.

> **Leia a seção [Avisos importantes](#-avisos-importantes) antes de usar.** Este arquivo desativa proteções (BitLocker, SmartScreen), cria uma conta administradora sem senha com login automático, e desinstala o Edge de forma não totalmente suportada pela Microsoft. É pensado para quem faz reinstalações frequentes (técnicos de TI, uso pessoal, VMs de teste) — avalie se cada ponto faz sentido para o seu caso antes de usar em produção.

---

## Como vai ficar o sistema

### O que já vem pronto
| Item | Detalhe |
|---|---|
| **Conta local** | Usuário `TI`, administrador, login automático, sem tela de conta Microsoft forçada |
| **Idioma/região** | pt-BR (idioma, teclado ABNT2, fuso horário) |
| **Navegador** | Google Chrome instalado e **definido como padrão** automaticamente |
| **Office** | Word, Excel e PowerPoint instalados (sem ativação — veja avisos) |
| **Microsoft Edge** | Completamente removido do sistema |
| **Bloatware (Xbox, Copilot, Cortana, Widgets, etc.)** | Removido |
| **Telemetria e coleta de dados** | Desativada na maior parte |
| **Performance** | Serviços desnecessários desligados, plano de energia de alta performance, sem indexação de busca |
| **Manutenção** | Limpeza automática de temporários (Storage Sense) + tarefa mensal de limpeza do sistema |

### 🖥️ Experiência de uso
- Instalação 100% desatendida: não pede chave de produto de forma bloqueante, não pede conta Microsoft, não mostra as telas de "personalizar sua experiência".
- Ao ligar pela primeira vez, o Windows já abre direto na área de trabalho, logado, com Chrome instalado e definido como padrão.
- Menu Iniciar limpo (sem apps fixados, sem seção "Recomendados", sem anúncios).
- Barra de tarefas alinhada à esquerda, sem campo de busca visível, sem Widgets.
- Explorador de Arquivos mostra extensões, arquivos ocultos e o caminho completo na barra de título; menu de contexto clássico (estilo Windows 10).
- Instalação funciona mesmo em hardware "não suportado" oficialmente (sem TPM 2.0 / Secure Boot / pouca RAM).

---

## O que foi removido

**Apps/UWP (Store):** Visualizador 3D, Pesquisa Bing, Calculadora, Clipchamp, Alarmes e Relógio, Copilot, Cortana, Widgets, "Office" promocional (MicrosoftOfficeHub), Gravador de Voz, Vincular ao Celular (Cross Device), Dev Home, Segurança da Família, Central de Feedback, Edge Game Assist, Obter Ajuda, Introdução, E-mail e Calendário, Mapas, Mixed Reality Portal, Notícias/Bing News, OneNote (app), novo Outlook, Paint, Contatos, Assistência Rápida, Skype, Ferramenta de Captura, Paciência, Notas Adesivas, Teams, Microsoft To Do, Carteira, Clima, todo o pacote Xbox (app, overlays, identidade, gaming app), Seu Celular, Filmes e TV.

**Recursos do Windows:** Fax e Digitalização, Reconhecimento de escrita à mão, Internet Explorer, Reconhecimento matemático, OneSync, Cliente OpenSSH, Gravador de Passos, Windows Hello Face (biometria facial), recurso legado do Snipping Tool.

**Navegador:** Microsoft Edge — desinstalado de verdade (não só "liberado para remover"), incluindo o bloqueio da reinstalação automática pelo Edge Update.

**OneDrive:** Removido (atalho, instalador e entrada de inicialização automática).

**Serviços do Windows desativados:** indexação de busca (WSearch), telemetria (DiagTrack), SysMain/Superfetch, Windows Error Reporting, P2P do Delivery Optimization, serviços do Xbox, demonstração de loja (RetailDemo), mapas offline (MapsBroker), carteira (WalletService), coletor de diagnóstico, compartilhamento de mídia (WMPNetworkSvc), telefonia (PhoneSvc), NFC/pagamentos (SEMgrSvc), Windows Insider, compartilhamento de conexão (ICS) e Registro Remoto.

> Serviços que dependem do hardware específico de cada máquina (Bluetooth, Spooler de impressão, teclado touch, biometria) **não** foram tocados, já que este arquivo serve para instalações em máquinas variadas.

**Outros:** hibernação/Fast Startup, Restauração do Sistema, Reserved Storage (~7 GB reservados pelo Windows), Windows.old (ao final da instalação).

---

## O que foi adicionado/pré-configurado

- **Google Chrome** — instalado silenciosamente e configurado como navegador padrão do sistema (associações de `.htm`, `.html`, `http` e `https`).
- **Microsoft Word, Excel e PowerPoint** — instalados via Office Deployment Tool oficial, **sem Access, Outlook, OneNote, Publisher, Teams ou OneDrive**, e **sem ativação** (fica por conta de quem for usar a máquina).
- **Plano de energia "Ultimate Performance"** — ativado automaticamente.
- **Modo de Jogo (Game Mode)** — ativado por padrão para todo novo usuário.
- **Tarefa de limpeza mensal** — roda o component cleanup do Windows (DISM) e limpa arquivos temporários todo mês, para o sistema não voltar a "engordar" com o tempo.
- **Storage Sense ativado** — limpa temporários e a lixeira automaticamente (não mexe na pasta Downloads).
- **Active Hours dinâmico** — o Windows aprende o período em que a máquina está em uso e evita reiniciar sozinho nesse intervalo.
- **Área de trabalho remota (RDP)** habilitada e liberada no firewall.
- **Atualizações do Windows mais previsíveis** — atualizações de recursos (novas versões do Windows) adiadas por 60 dias, atualizações de segurança/qualidade por 4 dias, e sem trocar drivers automaticamente via Windows Update.

---

## Privacidade

Desativados: telemetria (nível de política e serviço), ID de publicidade, Histórico de Atividades, experiências personalizadas com base em diagnóstico, destaques do Bing na busca, notificações de feedback ("Como está sua experiência?"), sugestões de apps/conteúdo patrocinado no Menu Iniciar, sugestões da caixa de pesquisa, SmartScreen (Explorer e Defender), e a política que bloqueia a análise de dados do Recall/IA em hardware Copilot+.

---

## Como usar

Você precisa de uma ISO oficial do Windows 10 ou 11 (baixada direto da Microsoft) e de um pendrive de pelo menos 8 GB.

### Opção 1 — Mais simples: colocar o arquivo no pendrive de instalação

1. Crie o pendrive bootável normalmente, com o [Rufus](https://rufus.ie/) ou a [Ferramenta de Criação de Mídia](https://www.microsoft.com/software-download/windows11) da Microsoft, usando a ISO original.
2. Baixe o [`autounattend.xml`](./autounattend.xml) deste repositório.
3. Copie o arquivo `autounattend.xml` para a **raiz** do pendrive (mesmo local onde ficam as pastas `sources`, `boot`, etc. — não dentro de nenhuma subpasta).
4. Dê boot pelo pendrive normalmente. O Windows Setup detecta o arquivo automaticamente e instala sem pedir nada.

### Opção 2 — Gravar o arquivo dentro da própria ISO

Use esta opção se você quer distribuir uma ISO já pronta, sem depender de copiar o arquivo toda vez.

1. Instale o [Windows ADK](https://learn.microsoft.com/windows-hardware/get-started/adk-install) (só precisa do componente "Deployment Tools").
2. Extraia o conteúdo da ISO original para uma pasta, por exemplo `C:\ISO`.
3. Copie o `autounattend.xml` para a raiz dessa pasta (`C:\ISO\autounattend.xml`).
4. Gere a nova ISO com o `oscdimg` (via "Deployment and Imaging Tools Environment", instalado junto com o ADK):
   ```powershell
   oscdimg -m -o -u2 -udfver102 -bootdata:2#p0,e,bC:\ISO\boot\etfsboot.com#pEF,e,bC:\ISO\efi\microsoft\boot\efisys.bin C:\ISO C:\Windows11-Limpo.iso
   ```
5. Grave essa nova ISO em pendrive normalmente (Rufus, etc.).

---

## Antes de usar — o que revisar

- **Nome/senha da conta**: por padrão o usuário é `TI`, administrador, **sem senha**, com login automático. Se a máquina vai ficar em rede ou for usada por terceiros, edite as tags `<UserAccounts>` e `<AutoLogon>` no arquivo e defina uma senha.
- **Chave de produto**: o arquivo usa uma chave genérica (`00000-00000-00000-00000-00000`) e deixa a tela de seleção de edição aparecer durante a instalação — insira sua chave real ali, ou edite a tag `<ProductKey>`.
- **Licença do Office**: o Word/Excel/PowerPoint instalam sem ativação. É necessário ter uma licença Microsoft 365 ou do Office válida e ativar depois (Configurações → Conta → Ativar Office). Se o destino usa uma licença **perpétua** (Office 2021/2024, não assinatura), é preciso trocar o `Product ID` dentro do arquivo — os comentários no próprio XML (script `unattend-05.ps1`) explicam onde.
- **Wi-Fi**: a instalação pede a senha da rede Wi-Fi normalmente durante a OOBE (não fica travado nisso, é só uma etapa interativa a mais se a máquina não tiver cabo de rede).

---

## Avisos importantes

- **BitLocker/criptografia de dispositivo fica desativada por padrão** e o **SmartScreen é desligado**. São trocas conscientes de segurança por praticidade/performance — se você precisa dessas proteções, remova essas linhas do arquivo antes de usar.
- **Conta sem senha + login automático** é adequado para uso pessoal, bancada de testes ou máquinas de uma loja de assistência técnica — **não é recomendado** para notebooks que saem de casa ou máquinas em rede corporativa sem outra camada de proteção.
- **A remoção do Edge não é um processo 100% oficial/suportado pela Microsoft.** Funciona na prática (é a mesma técnica usada por diversas ferramentas de debloat), mas pode se comportar de forma diferente em alguma build específica do Windows. Teste em uma máquina antes de replicar em massa.
- **A instalação do Office depende de internet durante a instalação** (é um download grande) e da ferramenta `winget`/download direto funcionarem — em caso de falha, o log fica em `C:\Windows\Setup\Scripts\unattend-05.log` na máquina instalada.
- Este arquivo **não ativa** o Windows nem o Office — ele só automatiza a instalação e a configuração. A ativação de ambos continua sendo responsabilidade de quem usa a licença.
- Teste sempre em uma máquina virtual antes de usar em um equipamento real.

---

## Créditos

- Baseado no [unattend-generator](https://github.com/cschneegans/unattend-generator) de [Christoph Schneegans](https://schneegans.de/).
- Correções de bugs do PowerShell gerado e todas as extensões manuais (remoção adicional de bloatware/serviços, instalação de Chrome/Office, remoção completa do Edge, tarefas de manutenção contínua, entre outras) por [mxndex7](https://github.com/mxndex7).
