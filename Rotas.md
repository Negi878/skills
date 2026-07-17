---
name: feature-completa-integral
description: >
  Use when building any feature where a user action (click, toggle, swipe,
  submit) must change persistent data and that change must be reflected
  consistently anywhere the affected item is shown or listed. Trigger on any
  request to add an interactive action/button/toggle, on "esse botão não
  está funcionando", or on any feature described only in visual terms
  (what it should look like or do on screen) without the data flow specified.
---

# Feature com Estado — Da Ação ao Dado, em Todas as Telas

Padrão que se aplica a qualquer ação do usuário que precisa alterar um
estado persistente e refletir esse estado consistentemente em múltiplos
lugares da interface — independente do domínio do produto.

## Como identificar que uma feature se encaixa aqui

Se a feature pode ser descrita como "o usuário faz [Ação] em um [Item], e
esse [Item] deve então aparecer/mudar de estado em [uma ou mais Telas]" —
essa skill se aplica. Não importa o nome da ação, do item, ou da tela.

## Princípio central

**Toda feature desse tipo tem 5 camadas obrigatórias.** Nenhuma feature deve
ser considerada "pronta" até as 5 estarem implementadas E testadas juntas,
na mesma tarefa — nunca uma de cada vez em conversas separadas.

## As 5 camadas obrigatórias

1. **Dado / persistência** — Existe uma tabela ou coluna que representa esse
   estado de forma permanente? Sem isso, qualquer mudança visual se perde ao
   recarregar a página.
2. **Rota / endpoint** — Existe uma function/endpoint que lê e escreve esse
   dado (criar, remover, listar)?
3. **Estado da UI ligado ao dado real** — O elemento visual reflete o estado
   vindo do banco, não um estado local que "parece" certo mas não bate com o
   servidor.
4. **Ação conectada** — A interação do usuário de fato chama a rota/endpoint,
   e só muda visualmente depois de confirmação de sucesso (ou usa estado
   otimista com rollback se a chamada falhar).
5. **Reflexo em todos os pontos onde o item aparece** — Antes de implementar,
   mapear TODOS os lugares da interface onde esse item e esse estado são
   exibidos — incluindo qualquer tela dedicada que deva listar os itens
   afetados por essa ação. Todos precisam ler do mesmo dado real.

## Como pedir a feature (prompt único, não fragmentado)

Nunca pedir em etapas separadas ("cria a tela" → "agora o botão" → "agora
corrige"). Especificar as 5 camadas explicitamente num único pedido:

```
Quero implementar [nome da ação] de ponta a ponta, para o seguinte fluxo:
- Ação: [o que o usuário faz]
- Item afetado: [o que está sendo salvo/alterado]
- Onde a ação pode ser disparada: [listar TODAS as telas]
- Onde o resultado deve aparecer refletido: [listar TODAS as telas,
  incluindo qualquer tela dedicada de listagem, se houver]

Isso inclui:
1. Estrutura de dados para persistir o estado
2. Função/rota para criar, remover e listar
3. Interface em cada tela listada, refletindo o estado real vindo do banco
4. A ação deve persistir de fato a mudança, não só alterar visualmente
5. Qualquer tela de listagem dedicada deve mostrar os itens reais do banco

Depois de implementar, teste o fluxo completo: disparar a ação num lugar →
verificar reflexo em todos os outros lugares mapeados → reverter a ação →
verificar que o reflexo também desaparece em todos os lugares.
```

## Checklist de verificação (rodar sempre, mesmo se a IA disser que terminou)

- [ ] Recarregar a página depois de disparar a ação — o estado persiste?
- [ ] O mesmo item aparece com o estado certo em TODAS as telas mapeadas
      (não só onde a ação foi disparada)?
- [ ] Se existe uma tela dedicada de listagem, ela reflete o dado real, não
      uma lista mockada ou vazia?
- [ ] Testar o caminho inverso (desfazer a ação) — ele também persiste e
      reflete em todas as telas?
- [ ] Testar com um segundo usuário/sessão — o dado é isolado corretamente
      por usuário, quando aplicável?

## Sinal de alerta (quando desconfiar que ficou só visual)

Se a descrição da implementação usa termos como "estado local", "toggle
visual", "mudei a aparência do elemento" sem mencionar tabela, rota, ou
banco de dados — é sinal de que só a camada 3 foi feita. Pedir
explicitamente para conectar às camadas 1, 2, 4 e 5.
