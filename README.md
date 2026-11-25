# Transporte Saúde: app do motorista

Protótipo navegável do fluxo principal do motorista: conferir a viagem do dia, registrar o embarque de cada paciente e seguir para o destino.

No computador, o app aparece dentro de um celular, para facilitar a apresentação. No topo, o botão **Painel do gestor** abre o painel da secretaria em tela cheia, que mostra ao vivo o que o motorista registra. No celular de verdade, o app ocupa a tela inteira e o mesmo botão continua no topo. O link direto para o painel termina em `#gestor`.

## O que o protótipo cobre

- **Caminho principal:** Iniciar viagem → registrar *Embarcou* ou *Não embarcou* para cada paciente → *Seguir para Vacaria* → tela "Viagem em andamento".
- **Erros recuperáveis:**
  - Tocar em *Embarcou* ou registrar um não embarque por engano: cada registro mostra "Foi engano? **Corrigir**", sem prazo, até seguir viagem.
  - Tentar registrar o não embarque sem motivo: o app mostra o que falta. Ao escolher "Outro motivo", é preciso escrever qual foi.
- **Acompanhante que não veio:** no cartão de Maria, *Embarcou sem Pedro* registra o embarque dela sem o acompanhante. A contagem de pessoas e o painel levam isso em conta.
- **Confirmação:** a janela "Seguir para Vacaria?" resume quem embarcou e quantas pessoas devem estar no carro. Se ninguém embarcou, ela avisa antes de seguir.
- **Janelas:** fecham com *Voltar*, com Esc, tocando fora delas ou arrastando a alça para baixo.
- **Aviso da secretaria:** depois do primeiro embarque, a gestora muda o horário de João. O aviso aparece no topo e o cartão dele fica destacado com o horário antigo riscado.
- **Acessibilidade e inclusão:**
  - **Ajustes** com texto grande e **modo sol** (alto contraste para ler sob luz forte);
  - botões grandes para uso dentro do carro;
  - funciona com leitor de tela e teclado;
  - motivos prontos, para não precisar digitar;
  - botão de ligar para o paciente;
  - funcionamento sem sinal (a lógica está no protótipo, mas não há controle na tela para simular).

## Antes de cada participante

Para voltar ao começo, recarregue a página. Nada fica salvo, exceto os ajustes de texto e o modo sol.

## Tarefa sugerida para o teste de usabilidade

> "Você é o motorista da viagem das 5h30 para Vacaria. Inicie a viagem, registre que Maria e João embarcaram e que Teresinha não embarcou porque desistiu, e siga viagem."

Ela passa pelo aviso de alteração, pelo erro de motivo (se a pessoa tentar registrar sem escolher) e pela confirmação.

## Versões

- **Versão 1 (`/v1/`, link `.../IHC/v1/`):** a versão que passou pela avaliação da etapa 5. É o "antes" da etapa 6. Não tem a janela Ajustes (tamanho do texto e modo sol).
- **Versão 2 (raiz do site):** corrige 7 dos 13 problemas da avaliação. O quadro está em `MUDANCAS.md` e as imagens de antes e depois em `evidencias/`.

## Como publicar

O site é publicado pelo GitHub Pages a partir da raiz do branch padrão: *Settings → Pages → Deploy from a branch → (branch) / (root)*. O arquivo `.nojekyll` faz o GitHub servir os arquivos como estão.
