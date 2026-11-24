# Mudanças: antes (v1) e depois

A **v1** é a versão "antes" da etapa 6 de IHC. Ela fica na tag `v1` do git e numa cópia navegável em [`/v1/`](v1/). A versão atual (`index.html`) é a v1 com três correções de acessibilidade.

As medições foram feitas no navegador (Playwright, tela de 390×844), lendo as cores calculadas pela página.

| # | Problema na v1 | Critério WCAG 2.2 | Antes (v1) | Depois | Como verificar |
|---|---|---|---|---|---|
| 1 | O contorno de foco (amarelo `#F2B35B`) quase não aparece sobre os cartões brancos e o fundo claro. Quem usa teclado se perde na lista. | 1.4.11 Contraste não textual (mín. 3:1); 2.4.7 Foco visível | 1,85:1 no cartão branco | 16,8:1 (foco escuro `#12201E` nas áreas claras; o amarelo continua nas áreas escuras, onde tem 4,1:1 no verde e 9,1:1 no escuro) | Navegar com Tab pela lista e medir o contraste do contorno com o fundo |
| 2 | As bordas das opções de motivo, da caixa "Qual foi o motivo?" e dos botões secundários (`#B9C6C3`) somem no branco. Não dá para ver onde tocar ou digitar. | 1.4.11 Contraste não textual (mín. 3:1) | 1,76:1 | 4,15:1 (`#6F807C`) | Abrir "Não embarcou" e medir a borda das opções contra o fundo da janela |
| 3 | O **Desfazer** sumia depois de 10 s, mesmo com o foco nele. Quem usa leitor de tela, ou está dirigindo, pode não conseguir a tempo. | 2.2.1 Ajustável por tempo | Sumia após 10 s | Fica no último registro até o próximo registro ou até seguir viagem | Registrar um embarque, esperar mais de 10 s e verificar se o Desfazer continua lá |

Imagens em [`antes-depois/`](antes-depois/): `foco-antes.png` × `foco-depois.png` e `bordas-antes.png` × `bordas-depois.png`.

## O que já estava na v1 (não entra no antes e depois)

- Desfazer depois de perder o sinal: a correção agora vai pela fila. Antes, o painel ficava com um embarque que não aconteceu.
- A confirmação "Seguir para Vacaria?" avisa quando nenhum paciente embarcou.
- Botão **Embarcou sem Pedro**, para o acompanhante que não veio. A contagem de pessoas no carro e o painel levam isso em conta.
- As janelas fecham tocando fora ou arrastando a alça para baixo (antes, a alça parecia arrastável mas não fazia nada).
- O painel do gestor destaca os registros novos com a etiqueta "novo".
- Descrição, prévia do link (para WhatsApp) e ícone da aba.
