# CLAUDE.md: Transporte Saúde

Contexto do projeto para o Claude Code. Leia antes de qualquer alteração.

## O que é este repositório

Repositório `ihc`, publicado no GitHub Pages. Contém o **protótipo navegável** de um sistema de gestão de viagens de transporte de pacientes de uma Secretaria Municipal de Saúde (município pequeno, Esmeralda/RS, que leva pacientes para atendimento em outras cidades, como Vacaria).

O mesmo sistema é usado em **dois trabalhos da faculdade**, feitos em dupla:

1. **Engenharia de Software (projeto final):** relatório em Word com problema, requisitos, viabilidade, diagramas UML (classes, sequência, estados) e matriz de rastreabilidade, mais uma apresentação em slides. Esses arquivos **não ficam neste repositório**.
2. **IHC (projeto final, "Redesenho de uma experiência digital"):** este protótipo é a entrega da etapa 4. O protótipo precisa ser **coerente com o relatório de Engenharia de Software**: mesmos nomes, estados e regras.

O repositório já teve um trabalho de IHC sobre a Cantina (recuperação). **Ele não tem relação com este projeto.** Não reaproveite nada dele.

## Arquivos

| Arquivo | O que é |
|---|---|
| `index.html` | O protótipo inteiro: HTML, CSS e JS num arquivo só, sem build e sem dependências. Só carrega as fontes Inter e Space Grotesk do Google Fonts, com fallback para fontes do sistema. |
| `README.md` | Como usar o protótipo, a tarefa sugerida para o teste de usabilidade e como publicar. |
| `CLAUDE.md` | Este arquivo. |
| `MUDANCAS.md` | Quadro das correções C1 a C7 (versão 1 → versão 2), com os problemas PR da etapa 5 e as medições. |
| `v1/index.html` | Cópia congelada da versão 1, a que passou pela avaliação da etapa 5 (o "antes"). **Não editar.** |
| `evidencias/` | Imagens de antes e depois das correções C1 a C4 (etapa 6). |
| `.nojekyll` | Faz o GitHub Pages servir os arquivos como estão. |

Para rodar localmente, abra o `index.html` no navegador ou use `python3 -m http.server` e acesse `http://localhost:8000`. O link direto para o painel do gestor termina em `#gestor`.

---

## Domínio (resumo do relatório de Engenharia de Software)

### Problema
Hoje a secretaria organiza as viagens em agenda de papel, tabela no Word e folha impressa entregue ao motorista. As mudanças de última hora vão por WhatsApp e se perdem. Não há registro de quem embarcou, do que aconteceu na viagem nem histórico. O caso de risco é o **paciente que fica sem transporte de volta**.

### Atores
- **Gestor** (secretaria; no protótipo, "Rita, gestora"): cadastra pacientes, motoristas e veículos; cria viagens e missões; escala; altera a programação; acompanha em tempo real; consulta o histórico.
- **Motorista** (no protótipo, "Carlos Menezes"): consulta a lista do dia no celular, registra checks (embarque, não embarque, entrega, retirada, missão, pontos de checagem) e funciona sem sinal.
- Pacientes e acompanhantes **não usam o sistema**; isso está fora do escopo.

### Conceitos (glossário)
- **Viagem:** deslocamento de um veículo, com um motorista, até uma cidade, incluindo ida e volta.
- **Embarque:** a participação de um paciente em uma viagem, com seus trechos (ida e volta, só ida, só volta), etapas e checks. É a entidade central.
- **Missão:** tarefa extra da viagem (ex.: buscar exames, entregar malote). Pode ser cumprida ou não cumprida; se não for cumprida, exige justificativa.
- **Check:** registro feito pelo motorista de algo que aconteceu (embarque, não embarque, entrega, retirada, missão), guardado com a **hora do aparelho**.
- **Ponto de checagem:** registro de horário em momentos definidos (saída, entrega no destino, retirada, chegada em Esmeralda). Guarda só o horário, nunca a localização.
- **Acompanhante:** sempre vinculado a um paciente e aparece junto dele na lista do motorista.

### Estados do Embarque (diagrama de estados)
`Pendente → Embarcado → Entregue no destino → Aguardando retorno → Retirado para volta → Concluído`

- Saídas por exceção, todas estados finais: **Não embarcado** (a partir de Pendente), **Cancelado** (a partir de Pendente) e **Não retirado** (a partir de Aguardando retorno).
- O embarque "só volta" começa em Aguardando retorno.
- O embarque "só ida" vai de Entregue no destino direto para Concluído.
- Quando a volta é transferida para outra viagem, o embarque passa de Aguardando retorno para Concluído.

### Regras de negócio que o protótipo usa
Os números são os do relatório. **Não invente números novos.** Se precisar de uma regra nova, avise o usuário para incluí-la no relatório também.

- **RN02:** motorista ou veículo não fica em duas viagens no mesmo horário.
- **RN04:** as etapas do embarque seguem a ordem, sem pular. As exceções são Não embarcado (RN05), Não retirado (RN19) e Cancelado (RN17, RN18).
- **RN05:** não embarque exige motivo, com opção pronta ou texto livre.
- **RN06:** se o paciente não embarcou na ida e a volta dele está em outra viagem, o gestor é avisado para revisar essa volta.
- **RN09:** o embarque "só ida" gera o alerta "volta sem viagem definida", até a volta ser incluída em outra viagem ou marcada como "sem retorno previsto".
- **RN10:** a chegada em Esmeralda só pode ser registrada quando nenhum embarque está Pendente ou Aguardando retorno. Ela conclui a viagem.
- **RN11:** o gestor não altera embarques encerrados (Concluído, Não embarcado, Não retirado, Cancelado).
- **RN12:** toda alteração do gestor notifica o motorista afetado e vai para o histórico.
- **RN13:** o horário da volta pode ser ajustado até o paciente ser retirado.
- **RN14:** os pontos de checagem guardam só o horário.
- **RN15 (conflito offline):** os checks do motorista registram fatos e **nunca são descartados**. Na sincronização, são aplicados **na ordem da hora do aparelho**. Alterações de **programação** do gestor (horário, local, missão, motorista, veículo) prevalecem. Se o gestor cancelou um embarque, transferiu uma volta ou cancelou a viagem sem o motorista ter recebido essa decisão, e o motorista registrou um check que a contradiz, **o check prevalece**: a decisão do gestor é desfeita e ele recebe a notificação "Conflito de sincronização".
- **RN16:** na transferência da volta, o embarque passa a Concluído na viagem de origem e entra como "só volta" na outra viagem.
- **RN17:** o gestor só cancela embarques Pendentes.
- **RN18:** o gestor só cancela viagens Agendadas.
- **RN19:** se o paciente não é encontrado para a volta, o motorista registra o não comparecimento com motivo obrigatório. O embarque passa a Não retirado e o gestor é notificado.
- **RN20:** o painel mostra a última sincronização de cada motorista e avisa o gestor antes de cancelar ou transferir algo de um motorista que está há mais de 10 minutos sem sincronizar.

### Requisitos (códigos do relatório)
- **Funcionais:**
  - **Cadastros:** RF01–RF04 (paciente, acompanhante, motorista, veículo).
  - **Planejamento:** RF05–RF11 e RF20 (viagem, escala, inclusão de paciente, missão, alteração, notificação ao motorista, volta sem viagem, cancelamento de viagem).
  - **Execução:** RF12–RF17 (lista do dia, embarque, não embarque, missão, pontos de checagem, conclusão da viagem).
  - **Acompanhamento:** RF18 (tempo real) e RF19 (histórico).
- **Não funcionais:**
  - RNF01: lista do dia em até 3 s.
  - RNF02: alterações e checks chegam em até 30 s com conexão.
  - RNF03: checks offline enviados até 1 min depois de o sinal voltar.
  - RNF04: consistência na sincronização (ver RN15).
  - RNF05: perfis; o motorista vê só as suas viagens.
  - RNF06: LGPD; sem dados clínicos e com HTTPS.
  - RNF07: auditoria com quem, quando, valor anterior e valor novo.
  - RNF08: check em até 3 toques.
  - RNF09: Chrome atualizado em Android 8+, como PWA; painel nos navegadores atuais.
  - RNF10: backup diário.

### Modelo de classes (nomes a manter)
`Usuário` (abstrata) → `Gestor`, `Motorista`; `Viagem` (composta por `Embarque`, `Missão` e `PontoChecagem`); `Paciente` (autoassociação acompanhante/acompanhado); `Veículo`; `Notificação`; `RegistroHistórico`; `Check/FilaSincronizacao` (atributos `evento`, `horaNoAparelho` e `enviado`).
- O `Embarque` tem `aplicarCheck()` e dependências «cria» com `Notificação` e «registra» com `RegistroHistórico`.
- Os tipos de notificação são: Alteração, Volta sem viagem definida, Revisar volta e Conflito de sincronização.

---

## O protótipo (`index.html`)

### Escopo
O enunciado de IHC pede **só o fluxo principal**, com caminho principal, um erro recuperável e uma confirmação. O fluxo escolhido é o **do motorista no início do dia**:

Iniciar viagem → registrar *Embarcou* ou *Não embarcou* para cada paciente → *Seguir para Vacaria* → tela "Viagem em andamento".

As entregas no destino, as retiradas para a volta e a chegada ficam **fora do protótipo**, e a tela final diz isso. O painel do gestor é **somente leitura**: os botões fora do fluxo mostram o aviso "não faz parte do protótipo".

### Dois modos (barra escura no topo)
- **App do motorista:** no desktop, aparece dentro de uma moldura de celular de 390×844 que se ajusta à altura da janela (`ajustarEscala()`). Em telas de até 500 px, ocupa a tela inteira, sem moldura.
- **Painel do gestor:** abre em tela cheia (`#gestor`) e mostra ao vivo o que o motorista registra.
- Os modos são alternados por `mudarModo()`, com `history.pushState` e `popstate`.

### Dados fictícios (manter consistentes com o canvas, os slides e o relatório)
- Terça, 17 de novembro. Viagem Esmeralda → **Vacaria**, saída **05:30**, veículo **Spin · IXY2B34** (7 lugares), motorista Carlos Menezes.
- Missões: buscar resultados de exames no Laboratório Central; entregar malote no Hospital de Vacaria.
- Pacientes da ida:
  - **Maria Aparecida Lopes** (com acompanhante Pedro Lopes, filho), Hospital de Vacaria às 07:30, ida e volta;
  - **João Fulano**, Linha São José em frente à capela, Clínica de Imagem às 08:00, ida e volta;
  - **Teresinha Borges**, Hospital de Vacaria às 09:00, **só ida**, volta em outro carro.
- Na volta: **Ana Ribeiro**, só volta, retirar no Hospital de Vacaria às 15:00.
- A alteração simulada é feita pela **Rita (gestora)**: o horário do João muda de **08:00 para 10:30**.
- Outras viagens do painel:
  - Passo Fundo, 04:45, Lucas Prado, Van JZK4C21;
  - Vacaria, 13:00, Sérgio Dal Molin, Gol IWE7A09;
  - Lagoa Vermelha, 07:00, cancelada.
- O alerta fixo do painel é o de RN09: Luiz Carvalho, só ida para Passo Fundo.
- Todos os nomes, placas e telefones são fictícios.

### Arquitetura do JS (IIFE, vanilla, ES5)
- **Estado:** `E` é o estado do app do motorista, criado por `novoEstado()`. `E.relogio` é um relógio simulado em minutos, que avança com `passa(min)` a cada ação.
- **Servidor simulado:** `E.servidor` é o que o painel "sabe": situação da viagem, estado de cada paciente, motivos, eventos do feed, `ultimaSync`, `semSinalDesde` e `revisarVolta`.
- **`registrar(ev)`:** todo check passa por aqui. Com sinal, chama `entregar([ev])`, que atualiza `E.servidor` e redesenha o painel. Sem sinal, o check vai para `E.fila`.
- **Volta do sinal:** a fila é ordenada pela hora do aparelho (RN15) e entregue de uma vez.
- **Corrigir (`data-a="desfazer"`):** aparece em cada registro até seguir viagem. Se o check do paciente ainda está na fila, é retirado dela; se já foi entregue, gera um evento de correção.
- **Acompanhante:** *Embarcou sem (nome)* (`data-a="embarcou-sem"`) registra o embarque com `semAcomp:true`. Não é um estado novo do Embarque; é um detalhe do check de embarque. **Precisa constar no relatório** (RF13), junto com a contagem de pessoas no carro.
- **Renderização:** `render(foco)` redesenha o cabeçalho, a faixa de sinal, a lista e a barra inferior, e move o foco para o seletor indicado. `renderPainel()` só desenha se o painel estiver visível.
- **Janelas** (`dlg-nao`, `dlg-seguir`, `dlg-ajustes`): são `<dialog>` abertos com `.show()`, não com `showModal()`, para ficarem **dentro do celular**. `abrir()` e `fechar()` cuidam do véu e de `inert` no resto da tela. O Esc, o toque no véu e arrastar a alça para baixo chamam `fecharAtual()`, que devolve o foco a quem abriu a janela.
- **Elementos posicionados:** a barra inferior, o toast e as janelas usam `position:absolute` dentro de `.moldura` (que tem `transform`), não `fixed`.
- **Comportamentos temporizados:**
  - A alteração do João dispara **2,5 s depois do primeiro embarque** (`agendarAlteracao()`). Se o celular estiver sem sinal, fica pendente até o sinal voltar.
  - **Corrigir** não tem prazo (WCAG 2.2.1; na versão 1 eram 10 s).
- **Ajustes de leitura:** texto grande (118%), muito grande (135%) e **modo sol** (alto contraste). São classes no `<html>`, aplicadas **só no modo motorista** e salvas em `localStorage` (chave `transporte-ajustes`, sempre dentro de try/catch).
- **Controles do teste:** removidos da versão 2 a pedido da dupla (Ajustes só tem texto e modo sol). Para reiniciar, recarregue a página. A lógica sem sinal (`E.offline`, `E.fila`) continua no código, mas não há mais como ativá-la pela tela. A versão 1 ainda tem os controles.

### Design
- Foco em dois tons: contorno `#12201E` com margem branca (no modo sol, amarelo com margem preta). Bordas `--line` `#738380` (≥3:1).
- O aviso da secretaria fica preso no topo (`position:sticky`) até tocar em Entendi.
- Cor de destaque `#0E5E5A` (verde-petróleo); fundo `#EEF2F0`; texto `#12201E`.
- Alerta e alteração em laranja `#9A3F07`/`#7A3106` sobre `#FFF7EC`.
- "Só volta" em azul `#1D4ED8`.
- Não embarcado e cancelado em cinza `#5B6664`.
- Tipografia: **Space Grotesk** em títulos de destaque e **Inter** no resto.
- As cores são variáveis CSS em `:root`; o modo sol só redefine as variáveis.
- Botões com pelo menos 52 px de altura (`--toque`), porque o app é usado dentro do carro.

### Acessibilidade (não regredir)
- Foco visível em tudo e foco levado ao lugar certo depois de cada ação.
- Avisos por `aria-live`: `#anuncio` (polite) e `#anuncio-urgente` (assertive, para o aviso da secretaria).
- Erros de formulário com texto e `aria-invalid`/`aria-describedby`, nunca só com cor.
- Estados sempre com texto ("Embarcou 05:36", "Não embarcou"), nunca só com cor.
- `prefers-reduced-motion` respeitado.
- Textos de interface em **português do Brasil**, curtos e diretos (UX writing): verbo no botão, consequência explicada ("O motivo vai para o histórico e para a secretaria").

---

## Enunciado de IHC (para saber o que ainda falta)

| Etapa | Entrega | Situação |
|---|---|---|
| 1. Investigar | Pesquisa com pelo menos 3 pessoas, método e achados | A fazer pela dupla |
| 2. Personas | Duas personas, jornada, achados que sustentam cada perfil e hipóteses | A fazer |
| 3. Barreiras → requisitos | Quadro com pelo menos uma barreira de usabilidade, uma de acessibilidade e uma de inclusão, com forma de verificação | A fazer (o protótipo já oferece candidatos: modo sol, botões grandes, motivos prontos, ligar para o paciente, offline) |
| 4. Protótipo | Link navegável com caminho principal, erro recuperável e confirmação | **Feito** (este repositório) |
| 5. Avaliar | Heurísticas de Nielsen, teste com pelo menos 3 participantes e verificação WCAG | Feito no relatório (13 problemas, PR1 a PR13) |
| 6. Refinar | Corrigir pelo menos 2 problemas (1 de acessibilidade ou inclusão), refazer a verificação e mostrar antes e depois | **Feito na versão 2** (C1 a C7, ver `MUDANCAS.md` e `evidencias/`). Ficam para depois: PR7 a PR10, PR12 e PR13 |

Para a etapa 6, guarde a versão "antes" (por exemplo, uma tag git ou cópia em `/v1/`) **antes** de aplicar as correções, para poder mostrar a comparação.

Onde estão os elementos exigidos:
- **Erro recuperável:** Corrigir depois de um registro; não embarque sem motivo; "Outro motivo" sem texto.
- **Confirmação:** a janela "Seguir para Vacaria?".

## Como trabalhar neste repositório

- Mantenha **um único `index.html`**, sem frameworks, sem build e sem CDNs além das fontes. Ele precisa funcionar direto no GitHub Pages.
- Antes de mudar nomes, estados, regras ou números, confira esta página. Se a mudança afetar o relatório ou os slides, **avise o usuário** para atualizar lá também.
- Depois de mudar algo, teste o fluxo completo no navegador, de preferência com Playwright, nas larguras 1440×900 e 390×844:
  - caminho principal;
  - desfazer;
  - erro de motivo;
  - modo sol;
  - painel do gestor.

  O console deve ficar sem erros.
- Não use `position:fixed` dentro do celular nem `showModal()`, porque os dois escapam da moldura.

## Pendências conhecidas fora deste repositório
- Os slides de Engenharia de Software ainda têm o texto antigo da RNF04 e da RN15 e a versão antiga do diagrama de classes (sem `aplicarCheck()` e sem as dependências).
- Não existe diagrama de sequência para o conflito offline; ele está descrito só em texto no relatório.
