# BGP 101
## LAB 01

### Objetivo
Neste lab o aluno será apresentado as configurações iniciais do ***BGP***, assim como aos comandos básicos de peering ***BGP Dual-stack*** (IP v4 e v6).

O aluno deve seguir os passos para poder configurar a adjacêcia (*peering*) BGP entre os roteadores.

### Topologia do LAB

![topologia](https://raw.githubusercontent.com/leandropinheiro/BGP101/master/img/LAB01-topologia.png)

HOSTNAME | TIPO | ASN | E0/0 | E0/1
:-------:|:----:|:---:|:----:|:---:
RTA|ROUTER|100|1.1.1.1/24|-
RTA|ROUTER|100|1::1/64|-
RTB|ROUTER|100|1.1.1.2/24|2.2.2.2/24
RTB|ROUTER|100|1::2/64|2::2/64
RTC|ROUTER|200|2.2.2.1/24|-
RTC|ROUTER|200|2::1/64|-

O roteador **RTC** está pré-configurado e o aluno não precisa realizar nenhum intervenção de configuração neste equipamento, entretanto é permitido usar comandos *show*, *ping* e *tracerouter*.

Os roteadores **RTA** e **RTB** estão configurados apenas com as configurações básica, como *hostname*, *description* e endereço *IPv4* e *IPv6* das *interfaces*. Nestes equipamentos o aluno pode fazer qualquer configuração, e executar quaisquer comandos.

#### Credenciais de acesso

***Username*** = *aluno*

***Password*** = *aluno*

### Comandos utilizados no LAB

Abaixo alguns dos comandos necessários para executar o LAB, com as devidas explicações:

COMANDO | DESCRIÇÃO
:-------|:---------
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
