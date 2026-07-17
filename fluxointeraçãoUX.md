---
name: fluxo-interacao-ux
description: >
  Use when deciding the sequence, order, or structure of a multi-step flow —
  what screen or question comes first vs. last, whether to ask for
  confirmation before an action, how many steps a task should take, or how
  to structure a wizard, checkout, onboarding, or any process with more than
  one screen. Trigger on requests like "qual a melhor ordem para essas
  telas", "deveria pedir confirmação antes de X", "como estruturar esse
  fluxo", "essa tela está com muitos passos/perguntas", or any request
  describing a sequence of user actions without the order specified.
---

# Fluxo de Interação — Decisões de Sequência e Estrutura

Regras para decidir a ordem, o número de passos e o momento certo de pedir
uma decisão ao usuário — a camada que fica entre "como a tela parece" e "o
que o produto faz". Aplica-se a qualquer fluxo com mais de uma etapa:
checkout, onboarding, wizard, formulário longo, fluxo de resposta/feedback
educacional, cadastro.

## Princípio central

**O produto deve agir, não interrogar.** A maioria das interfaces trata
decisões do usuário como se fossem prováveis quando na verdade são raras —
e isso gera perguntas e confirmações desnecessárias que quebram o fluxo.
Antes de adicionar qualquer pergunta, diálogo de confirmação, ou passo
extra, perguntar: essa situação é realmente provável, ou é só tecnicamente
possível?

## 1. Prefira ação com desfazer a pergunta com confirmação

Pedir confirmação antes de agir ("Tem certeza que quer salvar?") assume que
a ação indesejada é tão provável quanto a desejada — o que quase nunca é
verdade. Na prática, a esmagadora maioria das vezes o usuário quer a ação
óbvia (salvar, continuar, confirmar). Interromper todo mundo com uma
pergunta pra proteger contra o raro caso do 1 em 1000 é pior do que deixar
a ação acontecer e oferecer desfazer depois.

- Regra prática: se a ação é reversível (pode desfazer com um clique), não
  pedir confirmação — executar direto e oferecer "desfazer" logo depois.
- Reservar confirmação só pra ações genuinamente destrutivas e
  irreversíveis (deletar conta, cancelar assinatura, apagar permanentemente).
- Nunca usar confirmação como reflexo padrão pra qualquer ação "importante"
  — isso ensina o usuário a clicar "confirmar" no automático sem ler, o que
  anula completamente a proteção pretendida.

## 2. Prefira oferecer opções sempre visíveis a interromper com pergunta

Uma pergunta modal (popup que trava a tela até responder) para o fluxo do
usuário e exige atenção total imediatamente. Uma opção sempre visível
(botão, toggle, item de menu) deixa o usuário escolher no momento que ele
quiser, sem interromper o que estava fazendo.

- Preferir controles persistentes (botão visível, menu, toggle) a
  diálogos que aparecem sozinhos perguntando algo.
- Um diálogo é apropriado quando a decisão está genuinamente fora do fluxo
  principal (ex.: configuração avançada rara) — não para decisões comuns do
  caminho principal da tarefa.

## 3. Ordene o fluxo pela forma como o usuário pensa a tarefa, não pela forma como o sistema é organizado internamente

A ordem das telas/perguntas deve seguir a sequência natural com que a
pessoa pensa sobre a tarefa no mundo real — não a ordem que é mais
conveniente pra estrutura do banco de dados ou da API.

- Antes de definir a ordem de um fluxo novo, descrever em uma frase como uma
  pessoa comum narraria essa tarefa fora do software ("primeiro eu escolho
  o produto, depois decido a forma de pagamento, depois confirmo o
  endereço") — e alinhar a ordem das telas a essa narrativa.
- Quando a ordem "óbvia" do sistema (ex.: criar conta → depois escolher
  produto) diverge da ordem natural da tarefa (escolher produto → só depois
  criar conta pra finalizar), priorizar a ordem da tarefa: pedir cadastro
  antes da hora certa é um dos motivos mais comuns de abandono de fluxo.

## 4. Elimine passos que não avançam o objetivo do usuário

Todo passo de um fluxo se encaixa em uma de duas categorias: passo que
avança diretamente o que o usuário veio fazer, ou passo administrativo que
existe só por exigência do sistema (login repetido, confirmação de dado que
o sistema já tinha, preencher informação que poderia ser inferida).

- Antes de adicionar qualquer campo, tela ou clique a um fluxo, identificar
  em qual categoria ele cai. Passos administrativos devem ser eliminados,
  automatizados, ou adiados pro momento em que realmente importam — nunca
  colocados na frente do caminho principal só porque "é mais fácil pro
  sistema pedir isso agora".
- Sinal de alerta: se um passo existe só porque "precisamos desse dado em
  algum momento", questionar se ele precisa estar ANTES do valor da
  ferramenta ter sido demonstrado ao usuário, ou se pode vir depois.

## 5. Informe continuamente em vez de reportar sob demanda

Informação sobre o estado atual (progresso, quantidade restante, o que já
foi preenchido) deve estar sempre visível na tela, não exigir uma ação
separada do usuário pra descobrir ("clicar em ver status"). Isso reduz a
necessidade de perguntas de confirmação, porque o usuário já sabe o que vai
acontecer antes de precisar perguntar.

- Em fluxos de múltiplas etapas, sempre mostrar onde o usuário está na
  sequência (passo 2 de 4) e o que falta.
- Mostrar consequência da ação em tempo real quando possível (preview,
  cálculo atualizado) em vez de só revelar o resultado depois de confirmar.

## 6. Design pensado pro caso provável, com plano pro caso possível

Uma situação tecnicamente possível não é a mesma coisa que uma situação
provável. Desenhar o caminho principal do fluxo para o caso comum, sem
adicionar pergunta ou verificação extra pra cobrir uma exceção rara — e
tratar a exceção separadamente, sem penalizar todo mundo por ela.

- Antes de adicionar uma verificação, pergunta ou trava no fluxo principal,
  perguntar: isso está protegendo contra algo que acontece raramente? Se
  sim, mover essa verificação para fora do caminho principal (ex.: uma
  mensagem de erro tratável depois, não uma pergunta bloqueante antes).

## Como pedir a estruturação de um fluxo novo

```
Quero estruturar o fluxo de [nome da tarefa]. Antes de desenhar as telas:
1. Descreva a sequência como uma pessoa narraria a tarefa fora do sistema
2. Identifique quais passos avançam a tarefa e quais são só administrativos
   — administrativos devem ser eliminados ou adiados
3. Para cada decisão do usuário no fluxo, diga se deve ser: ação direta com
   desfazer, opção sempre visível, ou confirmação bloqueante — e justifique
   com base em quão reversível e quão provável é o erro
4. Mostre o progresso do usuário na sequência de forma sempre visível
```

## Checklist de revisão de um fluxo existente

- [ ] Alguma confirmação bloqueante existe para uma ação reversível? Se sim,
      remover e oferecer desfazer no lugar.
- [ ] A ordem das telas segue a forma como o usuário pensa a tarefa, ou a
      forma como o sistema está organizado internamente?
- [ ] Existe algum passo que só serve pra satisfazer o sistema, sem avançar
      o que o usuário veio fazer? Pode ser eliminado ou adiado?
- [ ] O usuário sabe em que ponto do fluxo está sem precisar perguntar?
- [ ] Alguma verificação está bloqueando o caminho principal por causa de
      um caso raro que poderia ser tratado separadamente?
