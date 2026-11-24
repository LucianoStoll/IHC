# Transporte Saúde: app do motorista

Protótipo navegável do fluxo principal do motorista: conferir a viagem do dia, registrar o embarque de cada paciente e seguir para o destino.

No computador, o app aparece dentro de um celular, para facilitar a apresentação. No topo, o botão **Painel do gestor** abre o painel da secretaria em tela cheia, que mostra ao vivo o que o motorista registra. No celular de verdade, o app ocupa a tela inteira e o mesmo botão continua no topo. O link direto para o painel termina em `#gestor`.

## O que o protótipo cobre

- **Caminho principal:** Iniciar viagem → registrar *Embarcou* ou *Não embarcou* para cada paciente → *Seguir para Vacaria* → tela "Viagem em andamento".
- **Erros recuperáveis:**
  - Tocar em *Embarcou* ou registrar um não embarque por engano: o botão **Desfazer** fica disponível por 10 segundos.
  - Tentar registrar o não embarque sem motivo: o app mostra o que falta. Ao escolher "Outro motivo", é preciso escrever qual foi.
- **Confirmação:** a janela "Seguir para Vacaria?" resume quem embarcou e quantas pessoas devem estar no carro.
- **Aviso da secretaria:** depois do primeiro embarque, a gestora muda o horário de João. O aviso aparece no topo e o cartão dele fica destacado com o horário antigo riscado.
- **Acessibilidade e inclusão:**
  - **Ajustes** com texto grande e **modo sol** (alto contraste para ler sob luz forte);
  - botões grandes para uso dentro do carro;
  - funciona com leitor de tela e teclado;
  - motivos prontos, para não precisar digitar;
  - botão de ligar para o paciente;
  - funcionamento sem sinal.

## Controles do teste (em Ajustes)

- **Simular celular sem sinal:** os registros ficam guardados e aparece o aviso "Sem sinal". O painel do gestor mostra que o motorista está sem sinal e só recebe os registros quando a simulação é desligada, na ordem em que foram feitos.
- **Reiniciar protótipo:** volta ao começo para o próximo participante.

## Tarefa sugerida para o teste de usabilidade

> "Você é o motorista da viagem das 5h30 para Vacaria. Inicie a viagem, registre que Maria e João embarcaram e que Teresinha não embarcou porque desistiu, e siga viagem."

Ela passa pelo aviso de alteração, pelo erro de motivo (se a pessoa tentar registrar sem escolher) e pela confirmação.

## Como publicar

Coloque o `index.html` na raiz do repositório `ihc` e ative o GitHub Pages em *Settings → Pages → Deploy from a branch → main / (root)*.
