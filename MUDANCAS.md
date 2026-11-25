# Mudanças da versão 1 para a versão 2

A versão 1 (`/v1/`) é a que passou pela avaliação da etapa 5 (heurísticas de Nielsen, WCAG 2.2 e teste de usabilidade). A versão 2 (raiz do site) corrige 7 dos 13 problemas encontrados. As medições de antes e depois estão na pasta `evidencias/` e na etapa 6 do relatório.

| ID | Problema (etapa 5) | Correção na versão 2 | Requisito | Antes | Depois |
|---|---|---|---|---|---|
| C1 | PR1: registro errado só podia ser desfeito por 10 s | "Foi engano? Corrigir" em cada registro, sem prazo, até seguir viagem. Sem sinal, a correção tira o registro da fila | U1, WCAG 2.2.1 | Sem como corrigir após 10 s | Corrigir disponível após 15 s |
| C2 | PR2: aviso da secretaria fora da área visível | Aviso preso no topo da tela até tocar em Entendi | Achados A2, A3 | 309 px acima da tela | Visível, a 39 px do topo |
| C3 | PR3: bordas e foco com contraste baixo | Bordas `#738380`; foco em dois tons (contorno escuro e margem branca) | A1, A3, WCAG 1.4.11 e 2.4.7 | Bordas 1,76:1; foco 1,85:1 | Bordas 3,97:1; foco 16,79:1 |
| C4 | PR4: texto a 200% cortava botões | Botões, cabeçalho e etiquetas quebram linha e empilham | A2, WCAG 1.4.4 e 1.4.10 | 465 px em tela de 390 px | 390 px, nada cortado |
| C5 | PR5: alvos de 44 px | Ajustes, Ligar e Corrigir com 52 px | A2 | 44 px | 52 px |
| C6 | PR6: tabela do painel sem teclado | Tabela recebe foco e rola com as setas | A3, WCAG 2.1.1 | axe: 1 ocorrência | axe: 0 |
| C7 | PR11: dois cabeçalhos "banner" | Barra do protótipo virou a região "Controles do protótipo" | A3 (boa prática) | axe: 2 regras | axe: 0 |

Verificação automática (axe-core 4.10, 10 situações): a versão 1 teve 3 regras com falha; a versão 2 não teve nenhuma.

Ficam para a próxima versão: PR7 a PR10, PR12 e PR13 (ver etapa 5.4 do relatório).

## Só na versão 2, fora do quadro

- A janela **Ajustes**, com tamanho do texto (grande e muito grande) e **modo sol** (alto contraste). A versão 1 não tem esses recursos.

## O que vale para as duas versões (não entra no antes e depois)

- Botão **Embarcou sem Pedro**, para o acompanhante que não veio; a contagem de pessoas no carro e o painel levam isso em conta.
- A confirmação "Seguir para Vacaria?" avisa quando nenhum paciente embarcou.
- As janelas fecham tocando fora ou arrastando a alça para baixo.
- O painel destaca os registros novos com a etiqueta "novo" e mostra se o aviso da secretaria foi lido.
- O estado aparece como **Pendente**, os motivos de não embarque são os do RF14, e "Iniciar viagem" e "Seguir para Vacaria" correspondem a `iniciar()` e `registrarSaida()` do relatório de Engenharia de Software.
- Descrição, prévia do link (para WhatsApp) e ícone da aba.
