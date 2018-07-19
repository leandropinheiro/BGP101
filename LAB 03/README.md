# BGP 101
## LAB 03

### Índice

* [Objetivo](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2003#objetivo)
* [Topologia do LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2003#topologia-do-lab)
	* [Credenciais de acesso](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2003#credenciais-de-acesso)
* [Comandos utilizados no LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2003#comandos-utilizados-no-lab)
* [ATIVIDADES DO LAB 03](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2003#atividades-do-lab-03)
	* [Tarefa 01](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2003#tarefa-01)

### Objetivo
Neste lab o aluno será apresentado aos metodos para injetar rotas no ***BGP***, assim como aos comandos *network*, *aggregate* e *redistribution*.

O aluno deve seguir os passos para poder configurar os roteadores para injetar as rotas na topologia BGP.

### Topologia do LAB

![topologia](https://raw.githubusercontent.com/leandropinheiro/BGP101/master/img/LAB03-topologia.png)

HOSTNAME | TIPO | ASN | E0/0 | E0/1 | E0/2 | Lo0 | Lo1 
:-------:|:----:|:---:|:----:|:----:|:----:|:---:|:---:
RTA|ROUTER|100|1.1.1.1/24|-|-|100.0.0.1/24|101.0.0.1/24
RTA|ROUTER|100|1::1/64|-|-|100::1/64|101::1/64
RTB|ROUTER|100|1.1.1.2/24|2.2.2.2/24|3.3.3.2/24|110.0.0.1/24|111.0.0.1/24
RTB|ROUTER|100|1::2/64|2::2/64|3::2/64|110::1/64|111::1/64
RTC|ROUTER|200|2.2.2.1/24|3.3.3.1/24|-|200.0.0.1/24|201.0.0.1/24
RTC|ROUTER|200|2::1/64|3::1/64|-|200::1/64|201::1/64

O roteador **RTC** está pré-configurado e o aluno não precisa realizar nenhuma intervenção de configuração neste equipamento, entretanto é permitido usar comandos *show*, *ping* e *tracerouter*.

Os roteadores **RTA** e **RTB** estão configurados apenas com as configurações básicas, como *hostname*, *description* e endereço *IPv4* e *IPv6* das *interfaces*, assim como a configuração BGP do LAB 01. Nestes equipamentos o aluno pode fazer qualquer configuração, e executar quaisquer comandos.

#### Credenciais de acesso

***Username*** = *aluno*

***Password*** = *aluno*

### Comandos utilizados no LAB

Abaixo alguns dos comandos necessários para executar o LAB, com as devidas explicações:

COMANDO | DESCRIÇÃO
:-------|:---------
*no*|Nega/Desabilita/Inverte a função de um determinado comando
*do*|Executa um comando do modo *EXEC* a partir do modo de Configuração
*configure terminal*|Entra no modo de configuração global
*end*|Sai do mode de configuração global
*write*|Salva as alterações nas configura
*show runnning-config*|Exibe a configuração currente
*show startup-config*|Exibe a configuração salva
*show ip protocols*|Exibe as configurações IPv4 globais
*show ipv6 protocols*|Exibe as configurações IPv6 globais
*show ip interface brief*|Exibe os endereços IPv4 das Interfaces
*show ipv6 interface brief*|Exibe os endereços IPv6 das Interfaces
*show ip bgp summary*|Exibe o resumo dos neighbors IPv4 do protocolo BGP
*show bgp ipv4 unicast summary*|Exibe o resumo dos neighbors IPv4 do protocolo BGP
*show bgp ipv6 unicast summary*|Exibe o resumo dos neighbors IPv6 do protocolo BGP
*show ip bgp*|Exibe os Prefixos IPv4 do protocolo BGP
*show bgp ipv4 unicast*|Exibe os Prefixos IPv4 do protocolo BGP
*show bgp ipv6 unicast*|Exibe os Prefixos IPv6 do protocolo BGP
*show ip route*|Exibe as rotas IPv4 na RIB (tabela de rotas)
*show ipv6 route*|Exibe as rotas IPv6 na RIB (tabela de rotas)
*show all bgp summary*|Exibe o resumo dos neighbors IPv4 e IPv6 do protocolo BGP
*show all bgp*|Exibe os Prefixos IPv4 e IPv6 do protocolo BGP
*router bgp 100*|Cria/configura um processo BGP com o ASN 100
*bgp default ipv4-unicast*|Inclui os neighbors BGP automaticamente no *address-family ipv4* e ativa os mesmos, mesmo que o neighbor seja IPv6
*syncronization*|Sincroniza o FIB do BGP com a RIB criada por um IGP
*neighbor 1.1.1.1 remote-as 100*|Cria peering BGP no ASN 100 com o roteador 1.1.1.1
*address-family [ipv4][ipv6]*|Acessa a configuração BGP de uma versão especifica do protocolo IP
*neighbor 1.1.1.1 active*|Ativa uma conexão de peering BGP com o roteador 1.1.1.1
*neighbor 1.1.1.1 next-hop-self*|Subititui o endereço do *next hop* dos prefixos enviados ao peer 1.1.1.1, com o endereço ip local
*clear ip bgp \[1.1.1.1\|\*\] [soft]*|Reseta o peering BGP com o host, ou com todos \[\"\*\"\], se usar a key \[soft\] aplica as alterações de onfiguração sem derrubar a sessão TCP.
*debug*|Ativa informações de debug
*undebug*|Desativa informações de debug

## ATIVIDADES DO LAB 02
### Tarefa 01
1. Acessar a console do ***RTA***.

