# SOLID Princípios de design de software em Java

### Resource: https://app.pluralsight.com/library/courses/solid-software-design-principles-java/table-of-contents

***

## Parte 1 - Introdução

### Problemas que surgem quando os princípios do SOLID não são utilizados

**Fragilidade** - tendência de um software quebrar em vários lugares diferentes cada vez que ocorre um mudança no código.

**Rigidez** - tendência de um software ser difícil de mudar, mesmo quando as mudanças são simples. Cada mudança causa um efeito cascata de mudanças subsquentes em módulos dependentes.

**Débito técnico** - Custo de priorizar uma entrega rápida em detrimento de uma entrega mais demorada, porém com com um código de melhor qualidade.

**Custo do débito técnico com o passar do tempo**

![Gráfico - custo do débito técnico com o passar do tempo](/Images/SOLID_Java/custo_debito_tecnico.png)

**Custo do débito técnico com o passar do tempo em relação ao tempo de resposta**

![Gráfico - custo do débito técnico com o passar do tempo em relação ao tempo de resposta](/Images/SOLID_Java/custo_debito_tecnico_vs_resposta_para_client.png)

Os dois gráficos são bem simples de entender. Quanto maior a quantidade de débitos técnicos no projeto, maior o tempo de resposta do time em relação à qualquer solicitação, consequetemente, maior o custo de manter a aplicação.

Entretanto, independente da qualidade do time, todo projeto terá débitos técnicos, e eles irão se acumular ao longo do tempo. O importante, ao saber disso, é dedicar tempo para refatorar o código e sanar os débitos técnicos para mantê-los sob controle.

### SOLID

Acrônimo para cinco princípios de design, destinados a facilitar a compreensão, o desenvolvimento e a manutenção de software.

* Princípio da Responsabilidade Única - (SRP - Single Responsability Principle)
* Princípio Aberto-Fechado - (OCP - Open Closed Principle)
* Princípio da Substituição de Liskov (LSP - Liskov Substituition Principle)
* Princípio da Segregação de Interfaces (ISP - Interface Segregation Principle)
* Princípio da Inversão de Dependência (DIP - Dependency Inversion Principle)

***



