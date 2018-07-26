# BGP 101
## LAB 04

### Índice

* [Objetivo](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#objetivo)
* [Topologia do LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#topologia-do-lab)
	* [Credenciais de acesso](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#credenciais-de-acesso)
* [Comandos utilizados no LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#comandos-utilizados-no-lab)
* [ATIVIDADES DO LAB 04](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#atividades-do-lab-03)
	* [Tarefa 01](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-01)
	* [Tarefa 02](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-02)
	* [Tarefa 03](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-03)
	* [Tarefa 04](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-04)
	* [Tarefa 05](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-05)
	* [Tarefa 06](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-06)
	* [Tarefa 07](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-07)
	* [Tarefa 08](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-08)

### Objetivo
Neste lab o aluno será apresentado a uma topologia simulando uma empresa com diversos Sites (A,B,C e D), integrados por links WAN redundantes, e utilizando um IGP (EIGRP) como protocolo de roteamento.

Cada um dos Sites conta com uma saida para a Internet por um provedor distinto, e o Bloco IPv4 e IPv6 da Empresa deve ser publicado para cada um destes roteadores, e deve ser utilizada a saída para os Prefixos externos pelo melhor provedor.

### Topologia do LAB

![topologia](https://raw.githubusercontent.com/leandropinheiro/BGP101/master/img/LAB04-topologia.png)

#### Endereeçamento IPv4

HOSTNAME | TIPO | ASN | E0/0 | E0/1 | E0/2 | E0/3 | E1/0 | E1/1 | Lo0 
:-------:|:----:|:---:|:----:|:----:|:----:|:----:|:----:|:----:|:---:
RT-SITE-A|ROUTER|100|100.10.0.1/24|10.10.20.1/29|10.10.20.5/29|10.10.30.5/29|10.10.30.1/29|172.16.20.2/29|100.0.0.1/32
PC-A|vPC|-|100.10.0.2/24|-|-|-|-|-|-
RT-SITE-B|ROUTER|100|100.20.0.1/24|10.10.20.2/29|10.10.20.6/29|10.10.40.5/29|10.10.40.1/29|172.16.30.2/29|100.0.0.2/32
PC-B|vPC|-|100.20.0.2/24|-|-|-|-|-|-
RT-SITE-C|ROUTER|100|100.30.0.1/24|10.30.40.1/29|10.30.40.5/29|10.10.30.6/29|10.10.30.2/29|172.16.40.2/29|100.0.0.3/32
PC-C|vPC|-|100.30.0.2/24|-|-|-|-|-|-
RT-SITE-D|ROUTER|100|100.40.0.1/24|10.30.40.2/29|10.30.40.6/29|10.10.20.6/29|10.10.20.2/29|172.16.50.2/29|100.0.0.4/32
PC-D|vPC|-|100.40.0.2/24|-|-|-|-|-|-
RT-ISP-A|ROUTER|20|20.20.20.1/24|172.16.20.1/29|-|-|-|-|-
PC-ISP-A|vPC|-|20.20.20.2/24|-|-|-|-|-|-
RT-ISP-B|ROUTER|30|30.30.30.1/24|172.16.30.1/29|-|-|-|-|-
PC-ISP-B|vPC|-|30.30.30.2/24|-|-|-|-|-|-
RT-ISP-C|ROUTER|40|40.40.40.1/24|172.16.40.1/29|-|-|-|-|-
PC-ISP-C|vPC|-|40.40.40.2/24|-|-|-|-|-|-
RT-ISP-D|ROUTER|50|50.50.50.1/24|172.16.50.1/29|-|-|-|-|-
PC-ISP-D|vPC|-|50.50.50.2/24|-|-|-|-|-|-

#### Endereeçamento IPv6

HOSTNAME | TIPO | ASN | E0/0 | E0/1 | E0/2 | E0/3 | E1/0 | E1/1 | Lo0 
:-------:|:----:|:---:|:----:|:----:|:----:|:----:|:----:|:----:|:---:
RT-SITE-A|ROUTER|100|2003:100 :10::1/64|10:10:20::1/126|10:10:20::5/126|10:10:30::5/126|10:10:30::1/126|172:16:20::2/126|2003:100::1/128
PC-A|vPC|-|2003:100 :10::2/64|-|-|-|-|-|-
RT-SITE-B|ROUTER|100|2003:100 :20::1/64|10:10:20::2/126|10:10:20::6/126|10:10:40::5/126|10:10:40::1/126|172:16:30::2/126|2003:100::2/128
PC-B|vPC|-|2003:100 :20::2/64|-|-|-|-|-|-
RT-SITE-C|ROUTER|100|2003:100 :30::1/64|10:30:40::1/126|10:30:40::5/126|10:10:30::6/126|10:10:30::2/126|172:16:40::2/126|2003:100::3/128
PC-C|vPC|-|2003:100 :30::2/64|-|-|-|-|-|-
RT-SITE-D|ROUTER|100|2003:100 :40::1/64|10:30:40::2/126|10:30:40::6/126|10:10:20::6/126|10:10:20::2/126|172:16:50::2/126|2003:100::4/128
PC-D|vPC|-|2003:100 :40::2/64|-|-|-|-|-|-
RT-ISP-A|ROUTER|20|2003:20.20::1/64|172:16:20::1/126|-|-|-|-|-
PC-ISP-A|vPC|-|2003:20:20::2/64|-|-|-|-|-|-
RT-ISP-B|ROUTER|30|2003:30:30::1/64|172:16:30::1/126|-|-|-|-|-
PC-ISP-B|vPC|-|2003:30:30::2/64|-|-|-|-|-|-
RT-ISP-C|ROUTER|40|2003:40:40:1/64|172:16:40::1/126|-|-|-|-|-
PC-ISP-C|vPC|-|2003:40:40::2/64|-|-|-|-|-|-
RT-ISP-D|ROUTER|50|2003:50:50::1/64|172:16:50::1/126|-|-|-|-|-
PC-ISP-D|vPC|-|2003:50:50::2/64|-|-|-|-|-|-


Os roteadores **RT-IPS-A/B/C/D** estão pré-configurados, e o aluno não precisa realizar nenhuma intervenção de configuração nestes equipamentos, entretanto é permitido usar comandos *show*, *ping* e *tracerouter*.

Os roteadores **RT-SITE-A/B/C/D** estão configurados apenas com as configurações básicas, como *hostname*, *description* e endereço *IPv4* e *IPv6* das *interfaces*. Neste equipamento o aluno pode fazer qualquer configuração, e executar quaisquer comandos.

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
*show runnning-config*|Exibe a configuração corrente
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
*neighbor 1.1.1.1 ebgp-multihop*|Permite peering BGP sem utilizar interface diretamente conectada com o roteador 1.1.1.1
*neighbor 1.1.1.1 update-soute [interface]*|Especifica que o roteador local deve utilizar a [interface] como origem dos pacotes para o roteador remoto 1.1.1.1
*address-family [ipv4][ipv6]*|Acessa a configuração BGP de uma versão especifica do protocolo IP
*neighbor 1.1.1.1 active*|Ativa uma conexão de peering BGP com o roteador 1.1.1.1
*neighbor 1.1.1.1 next-hop-self*|Substitui o endereço do *next hop* dos prefixos enviados ao peer 1.1.1.1, com o endereço ip local
*clear ip bgp \[1.1.1.1\|\*\] [soft]*|Reseta o peering BGP com o host, ou com todos \[\"\*\"\], se usar a key \[soft\] aplica as alterações de configuração sem derrubar a sessão TCP.
*debug*|Ativa informações de debug
*undebug*|Desativa informações de debug

## ATIVIDADES DO LAB 04

***ATENÇÃO!!! O ALUNO DEVE FECHAR O LAB 03 (MORE ACTIONS => STOP ALL NODES => CLOSE LAB), DEPOIS ABRIR O LAB 04 E INICIAR OS EQUIPAMENTOS (MORE ACTIONS => START ALL NODES).***

### Tarefa 01

#### Objetivo:

O aluno deve fazer a configuração Básica de BGP nos roteadores ***RT-SITE-A/B/C/D*** conforme os passos abaixo, para formar adjacencia iBGP:

1. O aluno deve verificar as configurações BGP nos equipamentos ***RT-SITE-A/B/C/D*** (executar em todos), a saída deve ser semelhante ao exemplo abaixo:

>
    RT-SITE-A#show running-config | section bgp
    RT-SITE-A#

    ! execute a verificação em todos os roteadores RT-SITE-A, B, C e D
    ! caso algum deles tenha configuração BGP, peça ajuda ao instrutor

2. Execute o script abaixo no ***RT-SITE-A*** para configurar o BGP e o *perring* com os roteadores ***RT-SITE-B*** e ***RT-SITE-C*** :

>
    configure terminal

    router bgp 100
    no bgp default ipv4-unicast
    bgp router-id 100.0.0.1
    neighbor 100.0.0.2 remote-as 100
    neighbor 100.0.0.2 ebgp-multihop
    neighbor 100.0.0.2 update-source loopback 0
    neighbor 2003:100::2 remote-as 100
    neighbor 2003:100::2 ebgp-multihop
    neighbor 2003:100::2 update-source loopback 0
    neighbor 100.0.0.3 remote-as 100
    neighbor 100.0.0.3 ebgp-multihop
    neighbor 100.0.0.3 update-source loopback 0
    neighbor 2003:100::3 remote-as 100
    neighbor 2003:100::3 ebgp-multihop
    neighbor 2003:100::3 update-source loopback 0

    address-family ipv4

    neighbor 100.0.0.2 activate
    neighbor 100.0.0.3 activate
    no synchronization
    network 100.0.0.1 mask 255.255.255.255
    network 100.10.0.0 mask 255.255.255.0
    aggregate-address 100.0.0.0 255.0.0.0 summary-only 

    address-family ipv6

    neighbor 2003:100::2 activate
    neighbor 2003:100::3 activate
    no synchronization
    network 2003:100::1/128
    network 2003:100:10::/64
    aggregate-address 2003:100::/32 summary-only

    end
    write

3. Verifique no ***RT-SITE-A*** se os peers foram configurados e estão ativos no BGP em ambas as versões do protocolo IP:

>
    RT-SITE-A#show bgp all summary 
    For address family: IPv4 Unicast
    BGP router identifier 100.0.0.1, local AS number 100
    BGP table version is 1, main routing table version 1
    2 network entries using 280 bytes of memory
    2 path entries using 160 bytes of memory
    1/0 BGP path/bestpath attribute entries using 144 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 584 total bytes of memory
    BGP activity 4/0 prefixes, 4/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    100.0.0.2       4          100       0       0        1    0    0 never    Idle
    100.0.0.3       4          100       0       0        1    0    0 never    Idle

    For address family: IPv6 Unicast
    BGP router identifier 100.0.0.1, local AS number 100
    BGP table version is 1, main routing table version 1
    2 network entries using 328 bytes of memory
    2 path entries using 208 bytes of memory
    1/0 BGP path/bestpath attribute entries using 144 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 680 total bytes of memory
    BGP activity 4/0 prefixes, 4/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    2003:100::2     4          100       0       0        1    0    0 never    Idle
    2003:100::3     4          100       0       0        1    0    0 never    Idle
    RT-SITE-A#

4. Execute o script abaixo no ***RT-SITE-B*** para configurar o BGP e o *perring* com os roteadores ***RT-SITE-A*** e ***RT-SITE-D*** :

>
    configure terminal

    router bgp 100
    no bgp default ipv4-unicast
    bgp router-id 100.0.0.2
    neighbor 100.0.0.1 remote-as 100
    neighbor 100.0.0.1 ebgp-multihop
    neighbor 100.0.0.1 update-source loopback 0
    neighbor 2003:100::1 remote-as 100
    neighbor 2003:100::1 ebgp-multihop
    neighbor 2003:100::1 update-source loopback 0
    neighbor 100.0.0.4 remote-as 100
    neighbor 100.0.0.4 ebgp-multihop
    neighbor 100.0.0.4 update-source loopback 0
    neighbor 2003:100::4 remote-as 100
    neighbor 2003:100::4 ebgp-multihop
    neighbor 2003:100::4 update-source loopback 0

    address-family ipv4

    neighbor 100.0.0.1 activate
    neighbor 100.0.0.4 activate
    no synchronization
    network 100.0.0.2 mask 255.255.255.255
    network 100.20.0.0 mask 255.255.255.0
    aggregate-address 100.0.0.0 255.0.0.0 

    address-family ipv6

    neighbor 2003:100::1 activate
    neighbor 2003:100::4 activate
    no synchronization
    network 2003:100::2/128
    network 2003:100:20::/64
    aggregate-address 2003:100::/32

    end
    write

5. Verifique no ***RT-SITE-B*** se os peers foram configurados e estão ativos no BGP em ambas as versões do protocolo IP:

>
    RT-SITE-B#show bgp all summary 
    For address family: IPv4 Unicast
    BGP router identifier 100.0.0.2, local AS number 100
    BGP table version is 1, main routing table version 1
    1 network entries using 140 bytes of memory
    1 path entries using 80 bytes of memory
    3/0 BGP path/bestpath attribute entries using 432 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 652 total bytes of memory
    BGP activity 8/6 prefixes, 10/8 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    100.0.0.1       4          100       5       2        1    0    0 00:00:09        1
    100.0.0.4       4          100       0       0        1    0    0 never    Idle

    For address family: IPv6 Unicast
    BGP router identifier 100.0.0.2, local AS number 100
    BGP table version is 1, main routing table version 1
    1 network entries using 164 bytes of memory
    1 path entries using 104 bytes of memory
    3/0 BGP path/bestpath attribute entries using 432 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 700 total bytes of memory
    BGP activity 8/6 prefixes, 10/8 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    2003:100::1     4          100       5       2        1    0    0 00:00:09        1
    2003:100::4     4          100       0       0        1    0    0 never    Idle
    RT-SITE-B#

6. Execute o script abaixo no ***RT-SITE-C*** para configurar o BGP e o *perring* com os roteadores ***RT-SITE-A*** e ***RT-SITE-D*** :

>
    configure terminal

    router bgp 100
    no bgp default ipv4-unicast
    bgp router-id 100.0.0.3
    neighbor 100.0.0.1 remote-as 100
    neighbor 100.0.0.1 ebgp-multihop
    neighbor 100.0.0.1 update-source loopback 0
    neighbor 2003:100::1 remote-as 100
    neighbor 2003:100::1 ebgp-multihop
    neighbor 2003:100::1 update-source loopback 0
    neighbor 100.0.0.4 remote-as 100
    neighbor 100.0.0.4 ebgp-multihop
    neighbor 100.0.0.4 update-source loopback 0
    neighbor 2003:100::4 remote-as 100
    neighbor 2003:100::4 ebgp-multihop
    neighbor 2003:100::4 update-source loopback 0

    address-family ipv4

    neighbor 100.0.0.1 activate
    neighbor 100.0.0.4 activate
    no synchronization
    network 100.0.0.3 mask 255.255.255.255
    network 100.30.0.0 mask 255.255.255.0
    aggregate-address 100.0.0.0 255.0.0.0 

    address-family ipv6

    neighbor 2003:100::1 activate
    neighbor 2003:100::4 activate
    no synchronization
    network 2003:100::3/128
    network 2003:100:30::/64
    aggregate-address 2003:100::/32

    end
    write

7. Verifique no ***RT-SITE-C*** se os peers foram configurados e estão ativos no BGP em ambas as versões do protocolo IP:

>
    RT-SITE-C#show bgp all summary 
    For address family: IPv4 Unicast
    BGP router identifier 100.0.0.3, local AS number 100
    BGP table version is 1, main routing table version 1
    3 network entries using 420 bytes of memory
    3 path entries using 240 bytes of memory
    2/0 BGP path/bestpath attribute entries using 288 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 948 total bytes of memory
    BGP activity 6/0 prefixes, 6/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    100.0.0.1       4          100       6       2        1    0    0 00:00:12        1
    100.0.0.4       4          100       0       0        1    0    0 never    Idle

    For address family: IPv6 Unicast
    BGP router identifier 100.0.0.3, local AS number 100
    BGP table version is 1, main routing table version 1
    3 network entries using 492 bytes of memory
    3 path entries using 312 bytes of memory
    2/0 BGP path/bestpath attribute entries using 288 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 1092 total bytes of memory
    BGP activity 6/0 prefixes, 6/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    2003:100::1     4          100       6       2        1    0    0 00:00:15        1
    2003:100::4     4          100       0       0        1    0    0 never    Idle
    RT-SITE-C#

8. Execute o script abaixo no ***RT-SITE-D*** para configurar o BGP e o *perring* com os roteadores ***RT-SITE-B*** e ***RT-SITE-C*** :

>
    configure terminal

    router bgp 100
    no bgp default ipv4-unicast
    bgp router-id 100.0.0.4
    neighbor 100.0.0.2 remote-as 100
    neighbor 100.0.0.2 ebgp-multihop
    neighbor 100.0.0.2 update-source loopback 0
    neighbor 2003:100::2 remote-as 100
    neighbor 2003:100::2 ebgp-multihop
    neighbor 2003:100::2 update-source loopback 0
    neighbor 100.0.0.3 remote-as 100
    neighbor 100.0.0.3 ebgp-multihop
    neighbor 100.0.0.3 update-source loopback 0
    neighbor 2003:100::3 remote-as 100
    neighbor 2003:100::3 ebgp-multihop
    neighbor 2003:100::3 update-source loopback 0

    address-family ipv4

    neighbor 100.0.0.2 activate
    neighbor 100.0.0.3 activate
    no synchronization
    network 100.0.0.4 mask 255.255.255.255
    network 100.40.0.0 mask 255.255.255.0
    aggregate-address 100.0.0.0 255.0.0.0 summary-only 

    address-family ipv6

    neighbor 2003:100::2 activate
    neighbor 2003:100::3 activate
    no synchronization
    network 2003:100::4/128
    network 2003:100:40::/64
    aggregate-address 2003:100::/32 summary-only

    end
    write

9. Verifique no ***RT-SITE-D*** se os peers foram configurados e estão ativos no BGP em ambas as versões do protocolo IP:

>
    RT-SITE-D#show bgp all summary 
    For address family: IPv4 Unicast
    BGP router identifier 100.0.0.4, local AS number 100
    BGP table version is 1, main routing table version 1
    7 network entries using 980 bytes of memory
    8 path entries using 640 bytes of memory
    4/0 BGP path/bestpath attribute entries using 576 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 2196 total bytes of memory
    BGP activity 14/0 prefixes, 16/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    100.0.0.2       4          100       7       2        1    0    0 00:00:16        3
    100.0.0.3       4          100       7       2        1    0    0 00:00:09        3

    For address family: IPv6 Unicast
    BGP router identifier 100.0.0.4, local AS number 100
    BGP table version is 1, main routing table version 1
    7 network entries using 1148 bytes of memory
    8 path entries using 832 bytes of memory
    4/0 BGP path/bestpath attribute entries using 576 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 2556 total bytes of memory
    BGP activity 14/0 prefixes, 16/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    2003:100::2     4          100       7       2        1    0    0 00:00:11        3
    2003:100::3     4          100       7       2        1    0    0 00:00:16        3
    RT-SITE-D#

### Tarefa 02

#### Objetivo:

O aluno deve fazer a configuração Básica de BGP nos roteadores ***RT-SITE-A/B/C/D*** conforme os passos abaixo, para formar adjacencia eBGP com so *ISPs*:

1. Execute o script abaixo no ***RT-SITE-A*** para configurar o *perring* BGP com o roteador ***RT-ISP-A*** :

>
    configure terminal

    router bgp 100

    neighbor 172.16.20.1 remote-as 20
    neighbor 172:16:20::1 remote-as 20

    address-family ipv4

    neighbor 172.16.20.1 activate

    address-family ipv6

    neighbor 172:16:20::1 activate

    end
    write

2. Verifique no ***RT-SITE-A*** se o peering foi configurado e está ativos no BGP em ambas as versões do protocolo IP com o ***RT-ISP-A*** :

>
    RT-SITE-A#show bgp all summary 
    For address family: IPv4 Unicast
    BGP router identifier 100.0.0.1, local AS number 100
    BGP table version is 18, main routing table version 18
    10 network entries using 1400 bytes of memory
    12 path entries using 960 bytes of memory
    6/4 BGP path/bestpath attribute entries using 864 bytes of memory
    1 BGP AS-PATH entries using 24 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 3248 total bytes of memory
    BGP activity 20/0 prefixes, 24/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    100.0.0.2       4          100       8       6       18    0    0 00:01:22        3
    100.0.0.3       4          100       8       6       18    0    0 00:01:26        3
    172.16.20.1     4           20       5       5       18    0    0 00:00:49        3

    For address family: IPv6 Unicast
    BGP router identifier 100.0.0.1, local AS number 100
    BGP table version is 18, main routing table version 18
    10 network entries using 1640 bytes of memory
    12 path entries using 1248 bytes of memory
    6/4 BGP path/bestpath attribute entries using 864 bytes of memory
    1 BGP AS-PATH entries using 24 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 3776 total bytes of memory
    BGP activity 20/0 prefixes, 24/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    172:16:20::1    4           20       5       5       18    0    0 00:00:58        3
    2003:100::2     4          100       8       6       18    0    0 00:01:16        3
    2003:100::3     4          100       8       6       18    0    0 00:01:18        3
    RT-SITE-A#

3. Execute o script abaixo no ***RT-SITE-B*** para configurar o *perring* BGP com o roteador ***RT-ISP-B*** :

>
    configure terminal

    router bgp 100

    neighbor 172.16.30.1 remote-as 30
    neighbor 172:16:30::1 remote-as 30

    address-family ipv4

    neighbor 172.16.30.1 activate

    address-family ipv6

    neighbor 172:16:30::1 activate

    end
    write

4. Verifique no ***RT-SITE-B*** se o peering foi configurado e está ativos no BGP em ambas as versões do protocolo IP com o ***RT-ISP-B*** :

>
    RT-SITE-B#show bgp all summary
    For address family: IPv4 Unicast
    BGP router identifier 100.0.0.2, local AS number 100
    BGP table version is 8, main routing table version 8
    9 network entries using 1260 bytes of memory
    11 path entries using 880 bytes of memory
    6/3 BGP path/bestpath attribute entries using 864 bytes of memory
    2 BGP AS-PATH entries using 48 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 3052 total bytes of memory
    BGP activity 18/0 prefixes, 22/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    100.0.0.1       4          100      15      15        8    0    0 00:08:19        4
    100.0.0.4       4          100       9      13        8    0    0 00:05:32        1
    172.16.30.1     4           30       8       9        8    0    0 00:02:57        3

    For address family: IPv6 Unicast
    BGP router identifier 100.0.0.2, local AS number 100
    BGP table version is 8, main routing table version 8
    9 network entries using 1476 bytes of memory
    11 path entries using 1144 bytes of memory
    6/3 BGP path/bestpath attribute entries using 864 bytes of memory
    2 BGP AS-PATH entries using 48 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 3532 total bytes of memory
    BGP activity 18/0 prefixes, 22/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    172:16:30::1    4           30       8       9        8    0    0 00:03:03        3
    2003:100::1     4          100      15      14        8    0    0 00:08:27        4
    2003:100::4     4          100      10      13        8    0    0 00:05:26        1
    RT-SITE-B#

5. Execute o script abaixo no ***RT-SITE-C*** para configurar o *perring* BGP com o roteador ***RT-ISP-C*** :

>
    configure terminal

    router bgp 100

    neighbor 172.16.40.1 remote-as 40
    neighbor 172:16:40::1 remote-as 40

    address-family ipv4

    neighbor 172.16.40.1 activate

    address-family ipv6

    neighbor 172:16:40::1 activate

    end
    write

6. Verifique no ***RT-SITE-C*** se o peering foi configurado e está ativos no BGP em ambas as versões do protocolo IP com o ***RT-ISP-C*** :

>
    RT-SITE-C#show bgp all summary
    For address family: IPv4 Unicast
    BGP router identifier 100.0.0.3, local AS number 100
    BGP table version is 8, main routing table version 8
    9 network entries using 1260 bytes of memory
    11 path entries using 880 bytes of memory
    6/3 BGP path/bestpath attribute entries using 864 bytes of memory
    2 BGP AS-PATH entries using 48 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 3052 total bytes of memory
    BGP activity 18/0 prefixes, 22/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    100.0.0.1       4          100      16      15        8    0    0 00:08:49        4
    100.0.0.4       4          100      12      16        8    0    0 00:07:30        1
    172.16.40.1     4           40       5       6        5    0    0 00:00:06        3

    For address family: IPv6 Unicast
    BGP router identifier 100.0.0.3, local AS number 100
    BGP table version is 8, main routing table version 8
    9 network entries using 1476 bytes of memory
    11 path entries using 1144 bytes of memory
    6/3 BGP path/bestpath attribute entries using 864 bytes of memory
    2 BGP AS-PATH entries using 48 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 3532 total bytes of memory
    BGP activity 18/0 prefixes, 22/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    172:16:40::1    4           40       5       6        5    0    0 00:00:02        3
    2003:100::1     4          100      16      15        8    0    0 00:08:45        4
    2003:100::4     4          100      12      16        8    0    0 00:07:34        1
    RT-SITE-C#

7. Execute o script abaixo no ***RT-SITE-D*** para configurar o *perring* BGP com o roteador ***RT-ISP-D*** :

>
    configure terminal

    router bgp 100

    neighbor 172.16.50.1 remote-as 50
    neighbor 172:16:50::1 remote-as 50

    address-family ipv4

    neighbor 172.16.50.1 activate

    address-family ipv6

    neighbor 172:16:50::1 activate

    end
    write

8. Verifique no ***RT-SITE-D*** se o peering foi configurado e está ativos no BGP em ambas as versões do protocolo IP com o ***RT-ISP-D*** :

>
    RT-SITE-D#show bgp all summary
    For address family: IPv4 Unicast
    BGP router identifier 100.0.0.4, local AS number 100
    BGP table version is 18, main routing table version 18
    16 network entries using 2240 bytes of memory
    18 path entries using 1440 bytes of memory
    8/4 BGP path/bestpath attribute entries using 1152 bytes of memory
    3 BGP AS-PATH entries using 72 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 4904 total bytes of memory
    BGP activity 32/0 prefixes, 36/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    100.0.0.2       4          100      18      16       18    0    0 00:10:18        6
    100.0.0.3       4          100      18      16       18    0    0 00:10:07        6
    172.16.50.1     4           50       5       5       15    0    0 00:00:01        3

    For address family: IPv6 Unicast
    BGP router identifier 100.0.0.4, local AS number 100
    BGP table version is 18, main routing table version 18
    16 network entries using 2624 bytes of memory
    18 path entries using 1872 bytes of memory
    8/4 BGP path/bestpath attribute entries using 1152 bytes of memory
    3 BGP AS-PATH entries using 72 bytes of memory
    0 BGP route-map cache entries using 0 bytes of memory
    0 BGP filter-list cache entries using 0 bytes of memory
    BGP using 5720 total bytes of memory
    BGP activity 32/0 prefixes, 36/0 paths, scan interval 60 secs

    Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    172:16:50::1    4           50       5       5       15    0    0 00:00:08        3
    2003:100::2     4          100      18      16       18    0    0 00:10:12        6
    2003:100::3     4          100      18      15       18    0    0 00:10:11        6
    RT-SITE-D#

### Tarefa 03

#### Objetivo:

O aluno deve realizar testes de conectividade do ***PC-A*** para os equipamentos ***PC-ISP-A, PC-ISP-B, PC-ISP-C e PC-ISP-D*** :

1. Na console do ***PC-A*** execute ping para os equipamentos:

>
    PC-A> ping 20.20.20.2

    84 bytes from 20.20.20.2 icmp_seq=1 ttl=62 time=0.718 ms
    84 bytes from 20.20.20.2 icmp_seq=2 ttl=62 time=0.555 ms
    84 bytes from 20.20.20.2 icmp_seq=3 ttl=62 time=0.759 ms
    84 bytes from 20.20.20.2 icmp_seq=4 ttl=62 time=0.455 ms
    84 bytes from 20.20.20.2 icmp_seq=5 ttl=62 time=0.472 ms

    PC-A>

    PC-A> ping 30.30.30.2

    *100.10.0.1 icmp_seq=1 ttl=255 time=0.325 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=2 ttl=255 time=0.327 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=3 ttl=255 time=0.267 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=4 ttl=255 time=0.219 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=5 ttl=255 time=0.339 ms (ICMP type:3, code:1, Destination host unreachable)

    PC-A>

    PC-A> ping 40.40.40.2

    *100.10.0.1 icmp_seq=1 ttl=255 time=0.334 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=2 ttl=255 time=0.291 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=3 ttl=255 time=0.247 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=4 ttl=255 time=0.243 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=5 ttl=255 time=0.249 ms (ICMP type:3, code:1, Destination host unreachable)

    PC-A>

    PC-A> ping 50.50.50.2

    *100.10.0.1 icmp_seq=1 ttl=255 time=0.185 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=2 ttl=255 time=0.218 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=3 ttl=255 time=0.302 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=4 ttl=255 time=0.293 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=5 ttl=255 time=0.274 ms (ICMP type:3, code:1, Destination host unreachable)

    PC-A>  

    ! veja que o PC-A só obteve sucesso ao executar o ping para o PC-SITE-A
    ! veja que para os demais a mensagem indica que o Destino é inalcançavél
    ! isso indica que o RT-SITE-A não tem rota para entregar os pacotes para o Next Hop

2. Na console do ***RT-SITE-A*** verifique o prefixo **20.20.20.0** no BGP:

>
    RT-SITE-A#show bgp ipv4 unicast 20.20.20.0
    BGP routing table entry for 20.20.20.0/24, version 3
    Paths: (1 available, best #1, table default)
      Advertised to update-groups:
         3
      Refresh Epoch 1
      20
        172.16.20.1 from 172.16.20.1 (172.16.20.1)
          Origin IGP, metric 0, localpref 100, valid, external, best
          rx pathid: 0, tx pathid: 0x0
    RT-SITE-A#

    ! observe que o Next Hop é o IP 172.16.20.1

3. Na console do ***RT-SITE-A*** verifique se há entradas na RIB para os prefixos **20.20.20.0** e **172.16.20.0**:

>
    RT-SITE-A#show ip route 20.20.20.0
    Routing entry for 20.20.20.0/24
      Known via "bgp 100", distance 20, metric 0
      Tag 20, type external
      Last update from 172.16.20.1 00:39:34 ago
      Routing Descriptor Blocks:
      * 172.16.20.1, from 172.16.20.1, 00:39:34 ago
          Route metric is 0, traffic share count is 1
          AS Hops 1
          Route tag 20
          MPLS label: none
    RT-SITE-A#
    RT-SITE-A#show ip route 172.16.20.0
    Routing entry for 172.16.20.0/30
      Known via "connected", distance 0, metric 0 (connected, via interface)
      Routing Descriptor Blocks:
      * directly connected, via Ethernet1/1
          Route metric is 0, traffic share count is 1
    RT-SITE-A#

    ! veja que para o prefixo 20.20.20.0 exise uma entrada na RIB, e se trata de uma rota indireta
    ! apontando para o Next Hop 172.16.20.1
    ! observe ainda que temos uma entrada na RIB para que o RT-SITE-A possa alcançar o Next Hop 172.16.20.1, neste caso se trata de uma rota diretamente conectada
    ! isso explica o porque de o RT-SITE-A ser capaz de encaminhar o ping do PC-A para o PC-ISP-A

4. Na console do ***RT-SITE-A*** verifique o prefixo **30.30.30.0** no BGP:

>
    RT-SITE-A#show bgp ipv4 unicast 30.30.30.0
    BGP routing table entry for 30.30.30.0/24, version 0
    Paths: (1 available, no best path)
      Not advertised to any peer
      Refresh Epoch 1
      30
        172.16.30.1 (inaccessible) from 100.0.0.2 (100.0.0.2)
          Origin IGP, metric 0, localpref 100, valid, internal
          rx pathid: 0, tx pathid: 0
    RT-SITE-A#

    ! observe que logo podemos verificar que o Next Hop 172.16.30.1 está marcado como Inacessível
    ! com isso o BGP não instala uma rota para o prefixo na RIB
    ! se repetirmos este teste para o prefixos 40.40.40.0 teremos o mesmo resultado

### Tarefa 04

#### Objetivo:

O aluno deve realizar as modificações necessárias nos IGPs (EIGRP) dos roteadores ***RT-SITE-A/B/C/D*** para divulgar a subnet de interconexão com o respectivos ISPs:

1. Na console do ***RT-SITE-A*** faça as verificações abaixo:

>
    RT-SITE-A#sh run interface ethernet 1/1
    Building configuration...

    Current configuration : 130 bytes
    !
    interface Ethernet1/1
     description TO-RT-ISP-A-e0/1
     ip address 172.16.20.2 255.255.255.252
     ipv6 address 172:16:20::2/126
    end

    ! tome nota dos IPs v4 e v6 na interface que conecta com o ISP-A

    RT-SITE-A#show running-config | section router eigrp
    router eigrp 100
     network 10.10.20.0 0.0.0.3
     network 10.10.20.4 0.0.0.3
     network 10.10.30.0 0.0.0.3
     network 10.10.30.4 0.0.0.3
     network 100.0.0.1 0.0.0.0
     network 100.10.0.0 0.0.0.255
     eigrp router-id 100.0.0.1
    ipv6 router eigrp 100
     eigrp router-id 100.0.0.1
    RT-SITE-A#

2. Na console do ***RT-SITE-A*** execute o script abaixo para habilitar o EIGRP na inteface E1/1:

>
    configure terminal

    interface e1/1
    ipv6 eigrp 100
    exit

    ipv6 router eigrp 100
    passive-interface ethernet 1/1
    exit

    router eigrp 100
    network 172.16.20.0 0.0.0.3
    passive-interface ethernet 1/1
    end

    write

    ! Adicionamos a Interface E1/1 no IGP, e tornamos a interface Passiva, para evitar
    ! que a mesma envie pocaotes EIGRP para o roteador do ISP

3. Na console do ***RT-SITE-B*** faça as verificações abaixo:

>
    RT-SITE-B#show running-config interface ethernet 1/1
    Building configuration...

    Current configuration : 130 bytes
    !
    interface Ethernet1/1
     description TO-RT-ISP-B-e0/1
     ip address 172.16.30.2 255.255.255.252
     ipv6 address 172:16:30::2/126
    end

    ! tome nota dos IPs v4 e v6 na interface que conecta com o ISP-B

    RT-SITE-B#sh run | sec route eigrp
    router eigrp 100
     network 10.10.20.0 0.0.0.3
     network 10.10.20.4 0.0.0.3
     network 10.20.40.0 0.0.0.3
     network 10.20.40.4 0.0.0.3
     network 100.0.0.2 0.0.0.0
     network 100.20.0.0 0.0.0.255
     eigrp router-id 100.0.0.2
    ipv6 router eigrp 100
     eigrp router-id 100.0.0.2
    RT-SITE-B#

4. Na console do ***RT-SITE-B*** execute o script abaixo para habilitar o EIGRP na inteface E1/1:

>
    configure terminal

    interface e1/1
    ipv6 eigrp 100
    exit

    ipv6 router eigrp 100
    passive-interface ethernet 1/1
    exit

    router eigrp 100
    network 172.16.30.0 0.0.0.3
    passive-interface ethernet 1/1
    end

    write

    ! Adicionamos a Interface E1/1 no IGP, e tornamos a interface Passiva, para evitar
    ! que a mesma envie pocaotes EIGRP para o roteador do ISP

5. Na console do ***RT-SITE-C*** faça as verificações abaixo:

>
    RT-SITE-C#show run interface e1/1
    Building configuration...

    Current configuration : 130 bytes
    !
    interface Ethernet1/1
     description TO-RT-ISP-C-e0/1
     ip address 172.16.40.2 255.255.255.252
    ipv6 address 172:16:40::2/126
    end

    ! tome nota dos IPs v4 e v6 na interface que conecta com o ISP-C

    RT-SITE-C#show running-config | sec router eigrp
    router eigrp 100
     network 10.10.30.0 0.0.0.3
     network 10.10.30.4 0.0.0.3
     network 10.30.40.0 0.0.0.3
     network 10.30.40.4 0.0.0.3
     network 100.0.0.3 0.0.0.0
     network 100.30.0.0 0.0.0.255
     eigrp router-id 100.0.0.3
    ipv6 router eigrp 100
     eigrp router-id 100.0.0.3
    RT-SITE-C#

6. Na console do ***RT-SITE-C*** execute o script abaixo para habilitar o EIGRP na inteface E1/1:

>
    configure terminal

    interface e1/1
    ipv6 eigrp 100
    exit

    ipv6 router eigrp 100
    passive-interface ethernet 1/1
    exit

    router eigrp 100
    network 172.16.40.0 0.0.0.3
    passive-interface ethernet 1/1
    end

    write

    ! Adicionamos a Interface E1/1 no IGP, e tornamos a interface Passiva, para evitar
    ! que a mesma envie pocaotes EIGRP para o roteador do ISP

7. Na console do ***RT-SITE-D*** faça as verificações abaixo:

>
    RT-SITE-D#show run interface e1/1
    Building configuration...

    Current configuration : 130 bytes
    !
    interface Ethernet1/1
     description TO-RT-ISP-D-e0/1
     ip address 172.16.50.2 255.255.255.252
     ipv6 address 172:16:50::2/126
    end

    ! tome nota dos IPs v4 e v6 na interface que conecta com o ISP-D

    RT-SITE-D#show running-config | sec router eigrp
    router eigrp 100
     network 10.20.40.0 0.0.0.3
     network 10.20.40.4 0.0.0.3
     network 10.30.40.0 0.0.0.3
     network 10.30.40.4 0.0.0.3
     network 100.0.0.4 0.0.0.0
     network 100.40.0.0 0.0.0.255
     eigrp router-id 100.0.0.4
    ipv6 router eigrp 100
     eigrp router-id 100.0.0.4
    RT-SITE-D#

8. Na console do ***RT-SITE-D*** execute o script abaixo para habilitar o EIGRP na inteface E1/1:

>
    configure terminal

    interface e1/1
    ipv6 eigrp 100
    exit

    ipv6 router eigrp 100
    passive-interface ethernet 1/1
    exit

    router eigrp 100
    network 172.16.50.0 0.0.0.3
    passive-interface ethernet 1/1
    end

    write

    ! Adicionamos a Interface E1/1 no IGP, e tornamos a interface Passiva, para evitar
    ! que a mesma envie pocaotes EIGRP para o roteador do ISP

### Tarefa 04

#### Objetivo:

O aluno deve repetir os testes de conectividade do ***PC-A*** para os equipamentos ***PC-ISP-A, PC-ISP-B, PC-ISP-C e PC-ISP-D*** :

1. Na console do ***PC-A*** execute ping para os equipamentos:

>
    PC-A> ping 20.20.20.2

    84 bytes from 20.20.20.2 icmp_seq=1 ttl=62 time=0.680 ms
    84 bytes from 20.20.20.2 icmp_seq=2 ttl=62 time=0.568 ms
    84 bytes from 20.20.20.2 icmp_seq=3 ttl=62 time=0.654 ms
    84 bytes from 20.20.20.2 icmp_seq=4 ttl=62 time=0.624 ms
    84 bytes from 20.20.20.2 icmp_seq=5 ttl=62 time=0.604 ms

    PC-A> ping 30.30.30.2

    84 bytes from 30.30.30.2 icmp_seq=1 ttl=61 time=0.981 ms
    84 bytes from 30.30.30.2 icmp_seq=2 ttl=61 time=0.609 ms
    84 bytes from 30.30.30.2 icmp_seq=3 ttl=61 time=0.635 ms
    84 bytes from 30.30.30.2 icmp_seq=4 ttl=61 time=0.662 ms
    84 bytes from 30.30.30.2 icmp_seq=5 ttl=61 time=0.704 ms

    PC-A> ping 40.40.40.2

    84 bytes from 40.40.40.2 icmp_seq=1 ttl=61 time=1.639 ms
    84 bytes from 40.40.40.2 icmp_seq=2 ttl=61 time=0.899 ms
    84 bytes from 40.40.40.2 icmp_seq=3 ttl=61 time=0.781 ms
    84 bytes from 40.40.40.2 icmp_seq=4 ttl=61 time=0.823 ms
    84 bytes from 40.40.40.2 icmp_seq=5 ttl=61 time=0.823 ms

    PC-A> ping 50.50.50.2

    *100.10.0.1 icmp_seq=1 ttl=255 time=0.321 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=2 ttl=255 time=0.353 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=3 ttl=255 time=0.333 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=4 ttl=255 time=0.347 ms (ICMP type:3, code:1, Destination host unreachable)
    *100.10.0.1 icmp_seq=5 ttl=255 time=0.314 ms (ICMP type:3, code:1, Destination host unreachable)

    PC-A>

    ! agora podemos observar que com excessão do 50.50.50.2, todos os outros
    ! testes foram bem sucedidos.

2. Na console do ***RT-SITE-A*** verifique os prefixos **50.50.50.0** e ***2003:50:50::/64*** no BGP:

>
    RT-SITE-A#show bgp ipv4 unicast 50.50.50.0
    % Network not in table
    RT-SITE-A#show bgp ipv6 unicast 2003:50:50::/64
    % Network not in table
    RT-SITE-A#

    ! veja que os pefixos provenientes do ISP-D não estão chegando via BGP no RT-SITE-A

2. Na console do ***RT-SITE-B*** verifique os prefixos **50.50.50.0** e ***2003:50:50::/64*** no BGP:

>
    RT-SITE-B#show bgp ipv4 unicast 50.50.50.0
    BGP routing table entry for 50.50.50.0/24, version 13
    Paths: (1 available, best #1, table default)
      Advertised to update-groups:
         2
      Refresh Epoch 1
      50
        172.16.50.1 (metric 307200) from 100.0.0.4 (100.0.0.4)
          Origin IGP, metric 0, localpref 100, valid, internal, best
          rx pathid: 0, tx pathid: 0x0
    RT-SITE-B#show bgp ipv6 unicast 2003:50:50::/64
    BGP routing table entry for 2003:50:50::/64, version 13
    Paths: (1 available, best #1, table default)
      Advertised to update-groups:
     2
      Refresh Epoch 1
      50
        172:16:50::1 (metric 307200) from 2003:100::4 (100.0.0.4)
          Origin IGP, metric 0, localpref 100, valid, internal, best
          rx pathid: 0, tx pathid: 0x0
    RT-SITE-B#

    ! observe que os prefixos existem na topologia BGP do RT-SITE-B, e que
    ! eles estão sendo propagados pelo RT-SITE-D!!!
    !
    ! PERGUNTA: Porque esse prefixos não chegam no RT-SITE-A?
    !
    ! PERGUNTA: O que precisamos fazer para resolver esse problema?

### Tarefa 05

#### Objetivo:

O aluno deve peering iBGP Full Mesh entre os equipamentos ***RT-SITE-A/B/C/D*** :

1. Na console do ***RT-SITE-A*** execute o script abaixo para configura um peering com o ***RT-SITE-D***:

>
    configure terminal

    router bgp 100

    neighbor 100.0.0.4 remote-as 100
    neighbor 100.0.0.4 ebgp-multihop
    neighbor 100.0.0.4 update-source loopback 0

    neighbor 2003:100::4 remote-as 100
    neighbor 2003:100::4 ebgp-multihop
    neighbor 2003:100::4 update-source loopback 0

    address-family ipv4

    neighbor 100.0.0.4 activate
    exit

    address-family ipv6

    neighbor 2003:100::4 activate
    
    end

    write

2. Na console do ***RT-SITE-B*** execute o script abaixo para configura um peering com o ***RT-SITE-C***:

>
    configure terminal

    router bgp 100

    neighbor 100.0.0.3 remote-as 100
    neighbor 100.0.0.3 ebgp-multihop
    neighbor 100.0.0.3 update-source loopback 0

    neighbor 2003:100::3 remote-as 100
    neighbor 2003:100::3 ebgp-multihop
    neighbor 2003:100::3 update-source loopback 0

    address-family ipv4

    neighbor 100.0.0.3 activate
    exit

    address-family ipv6

    neighbor 2003:100::3 activate
    
    end

    write

3. Na console do ***RT-SITE-C*** execute o script abaixo para configura um peering com o ***RT-SITE-B***:

>
    configure terminal

    router bgp 100

    neighbor 100.0.0.2 remote-as 100
    neighbor 100.0.0.2 ebgp-multihop
    neighbor 100.0.0.2 update-source loopback 0

    neighbor 2003:100::2 remote-as 100
    neighbor 2003:100::2 ebgp-multihop
    neighbor 2003:100::2 update-source loopback 0

    address-family ipv4

    neighbor 100.0.0.2 activate
    exit

    address-family ipv6

    neighbor 2003:100::2 activate
    
    end

    write

4. Na console do ***RT-SITE-D*** execute o script abaixo para configura um peering com o ***RT-SITE-A***:

>
    configure terminal

    router bgp 100

    neighbor 100.0.0.1 remote-as 100
    neighbor 100.0.0.1 ebgp-multihop
    neighbor 100.0.0.1 update-source loopback 0

    neighbor 2003:100::1 remote-as 100
    neighbor 2003:100::1 ebgp-multihop
    neighbor 2003:100::1 update-source loopback 0

    address-family ipv4

    neighbor 100.0.0.1 activate
    exit

    address-family ipv6

    neighbor 2003:100::1 activate
    
    end

    write

### Tarefa 06

#### Objetivo:

O aluno deve repetir os testes de conectividade do ***PC-A*** para os equipamentos ***PC-ISP-A, PC-ISP-B, PC-ISP-C e PC-ISP-D*** :

1. Na console do ***PC-A*** execute ping para os equipamentos:

>
    PC-A> ping 20.20.20.2

    84 bytes from 20.20.20.2 icmp_seq=1 ttl=62 time=0.699 ms
    84 bytes from 20.20.20.2 icmp_seq=2 ttl=62 time=0.640 ms
    84 bytes from 20.20.20.2 icmp_seq=3 ttl=62 time=0.618 ms
    84 bytes from 20.20.20.2 icmp_seq=4 ttl=62 time=0.461 ms
    84 bytes from 20.20.20.2 icmp_seq=5 ttl=62 time=0.503 ms

    PC-A> ping 30.30.30.2

    84 bytes from 30.30.30.2 icmp_seq=1 ttl=61 time=0.604 ms
    84 bytes from 30.30.30.2 icmp_seq=2 ttl=61 time=0.544 ms
    84 bytes from 30.30.30.2 icmp_seq=3 ttl=61 time=0.575 ms
    84 bytes from 30.30.30.2 icmp_seq=4 ttl=61 time=0.588 ms
    84 bytes from 30.30.30.2 icmp_seq=5 ttl=61 time=0.664 ms

    PC-A> ping 40.40.40.2

    84 bytes from 40.40.40.2 icmp_seq=1 ttl=61 time=0.529 ms
    84 bytes from 40.40.40.2 icmp_seq=2 ttl=61 time=0.551 ms
    84 bytes from 40.40.40.2 icmp_seq=3 ttl=61 time=0.804 ms
    84 bytes from 40.40.40.2 icmp_seq=4 ttl=61 time=0.785 ms
    84 bytes from 40.40.40.2 icmp_seq=5 ttl=61 time=0.688 ms

    PC-A> ping 50.50.50.2

    84 bytes from 50.50.50.2 icmp_seq=1 ttl=60 time=1.669 ms
    84 bytes from 50.50.50.2 icmp_seq=2 ttl=60 time=1.047 ms
    84 bytes from 50.50.50.2 icmp_seq=3 ttl=60 time=0.779 ms
    84 bytes from 50.50.50.2 icmp_seq=4 ttl=60 time=0.902 ms
    84 bytes from 50.50.50.2 icmp_seq=5 ttl=60 time=1.086 ms

    PC-A>

    ! agora temos conectividade com todas as saídas para os ISPs

2. Na console do ***PC-B*** execute ping para os equipamentos:

>
    PC-B> ping 20.20.20.2

    84 bytes from 20.20.20.2 icmp_seq=1 ttl=61 time=1.786 ms
    84 bytes from 20.20.20.2 icmp_seq=2 ttl=61 time=0.882 ms
    84 bytes from 20.20.20.2 icmp_seq=3 ttl=61 time=0.880 ms
    84 bytes from 20.20.20.2 icmp_seq=4 ttl=61 time=0.896 ms
    84 bytes from 20.20.20.2 icmp_seq=5 ttl=61 time=0.687 ms

    PC-B> ping 30.30.30.2

    84 bytes from 30.30.30.2 icmp_seq=1 ttl=62 time=1.676 ms
    84 bytes from 30.30.30.2 icmp_seq=2 ttl=62 time=0.470 ms
    84 bytes from 30.30.30.2 icmp_seq=3 ttl=62 time=0.399 ms
    84 bytes from 30.30.30.2 icmp_seq=4 ttl=62 time=0.367 ms
    84 bytes from 30.30.30.2 icmp_seq=5 ttl=62 time=0.439 ms

    PC-B> ping 40.40.40.2

    84 bytes from 40.40.40.2 icmp_seq=1 ttl=60 time=2.127 ms
    84 bytes from 40.40.40.2 icmp_seq=2 ttl=60 time=1.290 ms
    84 bytes from 40.40.40.2 icmp_seq=3 ttl=60 time=0.717 ms
    84 bytes from 40.40.40.2 icmp_seq=4 ttl=60 time=1.072 ms
    84 bytes from 40.40.40.2 icmp_seq=5 ttl=60 time=1.028 ms
    
    PC-B> ping 50.50.50.2

    84 bytes from 50.50.50.2 icmp_seq=1 ttl=61 time=1.843 ms
    84 bytes from 50.50.50.2 icmp_seq=2 ttl=61 time=0.568 ms
    84 bytes from 50.50.50.2 icmp_seq=3 ttl=61 time=0.819 ms
    84 bytes from 50.50.50.2 icmp_seq=4 ttl=61 time=0.777 ms
    84 bytes from 50.50.50.2 icmp_seq=5 ttl=61 time=0.823 ms

    PC-B>
    
3. Na console do ***PC-C*** execute ping para os equipamentos:

>
    PC-C> ping 20.20.20.2

    84 bytes from 20.20.20.2 icmp_seq=1 ttl=61 time=1.648 ms
    84 bytes from 20.20.20.2 icmp_seq=2 ttl=61 time=0.557 ms
    84 bytes from 20.20.20.2 icmp_seq=3 ttl=61 time=0.762 ms
    84 bytes from 20.20.20.2 icmp_seq=4 ttl=61 time=0.623 ms
    84 bytes from 20.20.20.2 icmp_seq=5 ttl=61 time=0.728 ms

    PC-C> ping 30.30.30.2

    84 bytes from 30.30.30.2 icmp_seq=1 ttl=60 time=1.783 ms
    84 bytes from 30.30.30.2 icmp_seq=2 ttl=60 time=0.669 ms
    84 bytes from 30.30.30.2 icmp_seq=3 ttl=60 time=1.069 ms
    84 bytes from 30.30.30.2 icmp_seq=4 ttl=60 time=1.045 ms
    84 bytes from 30.30.30.2 icmp_seq=5 ttl=60 time=0.786 ms

    PC-C> ping 40.40.40.2

    84 bytes from 40.40.40.2 icmp_seq=1 ttl=62 time=1.535 ms
    84 bytes from 40.40.40.2 icmp_seq=2 ttl=62 time=0.462 ms
    84 bytes from 40.40.40.2 icmp_seq=3 ttl=62 time=0.349 ms
    84 bytes from 40.40.40.2 icmp_seq=4 ttl=62 time=0.446 ms
    84 bytes from 40.40.40.2 icmp_seq=5 ttl=62 time=0.411 ms

    PC-C> ping 50.50.50.2

    84 bytes from 50.50.50.2 icmp_seq=1 ttl=61 time=1.710 ms
    84 bytes from 50.50.50.2 icmp_seq=2 ttl=61 time=0.623 ms
    84 bytes from 50.50.50.2 icmp_seq=3 ttl=61 time=0.639 ms
    84 bytes from 50.50.50.2 icmp_seq=4 ttl=61 time=0.798 ms
    84 bytes from 50.50.50.2 icmp_seq=5 ttl=61 time=0.590 ms

    PC-C>

4. Na console do ***PC-D*** execute ping para os equipamentos:

>
    PC-D> ping 20.20.20.2

    84 bytes from 20.20.20.2 icmp_seq=1 ttl=60 time=1.813 ms
    84 bytes from 20.20.20.2 icmp_seq=2 ttl=60 time=0.731 ms
    84 bytes from 20.20.20.2 icmp_seq=3 ttl=60 time=1.001 ms
    84 bytes from 20.20.20.2 icmp_seq=4 ttl=60 time=1.075 ms
    84 bytes from 20.20.20.2 icmp_seq=5 ttl=60 time=0.954 ms

    PC-D> ping 30.30.30.2

    84 bytes from 30.30.30.2 icmp_seq=1 ttl=61 time=1.764 ms
    84 bytes from 30.30.30.2 icmp_seq=2 ttl=61 time=0.588 ms
    84 bytes from 30.30.30.2 icmp_seq=3 ttl=61 time=0.649 ms
    84 bytes from 30.30.30.2 icmp_seq=4 ttl=61 time=0.563 ms
    84 bytes from 30.30.30.2 icmp_seq=5 ttl=61 time=0.605 ms

    PC-D> ping 40.40.40.2

    84 bytes from 40.40.40.2 icmp_seq=1 ttl=61 time=1.639 ms
    84 bytes from 40.40.40.2 icmp_seq=2 ttl=61 time=0.564 ms
    84 bytes from 40.40.40.2 icmp_seq=3 ttl=61 time=0.532 ms
    84 bytes from 40.40.40.2 icmp_seq=4 ttl=61 time=0.546 ms
    84 bytes from 40.40.40.2 icmp_seq=5 ttl=61 time=0.786 ms

    PC-D> ping 50.50.50.2

    84 bytes from 50.50.50.2 icmp_seq=1 ttl=62 time=1.704 ms
    84 bytes from 50.50.50.2 icmp_seq=2 ttl=62 time=0.589 ms
    84 bytes from 50.50.50.2 icmp_seq=3 ttl=62 time=0.498 ms
    84 bytes from 50.50.50.2 icmp_seq=4 ttl=62 time=0.584 ms
    84 bytes from 50.50.50.2 icmp_seq=5 ttl=62 time=0.989 ms

    PC-D>

    ! temos certeza que todos os SITES saem por todas as saídas dos ISPs que a companhia possui

### Tarefa 07

#### Objetivo:

O aluno deve realizar testes de conectividade do ***PC-ISP-A*** para os equipamentos ***PC-A/B/C/D e PC-ISP-B/C/D*** :

1. Na console do ***PC-ISP-A*** execute ping para os equipamentos ***PC-A/B/C/D*** :

>
    PCISPA> ping 100.10.0.2

    84 bytes from 100.10.0.2 icmp_seq=1 ttl=62 time=1.534 ms
    84 bytes from 100.10.0.2 icmp_seq=2 ttl=62 time=0.596 ms
    84 bytes from 100.10.0.2 icmp_seq=3 ttl=62 time=0.689 ms
    84 bytes from 100.10.0.2 icmp_seq=4 ttl=62 time=1.087 ms
    84 bytes from 100.10.0.2 icmp_seq=5 ttl=62 time=0.526 ms

    PCISPA> ping 100.20.0.2

    84 bytes from 100.20.0.2 icmp_seq=1 ttl=61 time=2.042 ms
    84 bytes from 100.20.0.2 icmp_seq=2 ttl=61 time=0.652 ms
    84 bytes from 100.20.0.2 icmp_seq=3 ttl=61 time=0.598 ms
    84 bytes from 100.20.0.2 icmp_seq=4 ttl=61 time=0.653 ms
    84 bytes from 100.20.0.2 icmp_seq=5 ttl=61 time=0.632 ms

    PCISPA> ping 100.30.0.2

    84 bytes from 100.30.0.2 icmp_seq=1 ttl=61 time=1.731 ms
    84 bytes from 100.30.0.2 icmp_seq=2 ttl=61 time=0.647 ms
    84 bytes from 100.30.0.2 icmp_seq=3 ttl=61 time=0.641 ms
    84 bytes from 100.30.0.2 icmp_seq=4 ttl=61 time=0.654 ms
    84 bytes from 100.30.0.2 icmp_seq=5 ttl=61 time=0.600 ms

    PCISPA> ping 100.40.0.2

    84 bytes from 100.40.0.2 icmp_seq=1 ttl=60 time=1.908 ms
    84 bytes from 100.40.0.2 icmp_seq=2 ttl=60 time=0.693 ms
    84 bytes from 100.40.0.2 icmp_seq=3 ttl=60 time=0.763 ms
    84 bytes from 100.40.0.2 icmp_seq=4 ttl=60 time=1.174 ms
    84 bytes from 100.40.0.2 icmp_seq=5 ttl=60 time=1.021 ms

    PCISPA>

    ! até aqui tudo execente, o ISP está recebendo o Bloco da Companhia e acessa 
    ! os recursos com IP Publico em todos os Sites

2. Na console do ***PC-ISP-A*** execute ping e traceroute para os equipamentos ***PC-ISP-B/C/D*** :

>
    PCISPA> ping 30.30.30.2

    84 bytes from 30.30.30.2 icmp_seq=1 ttl=60 time=3.342 ms
    84 bytes from 30.30.30.2 icmp_seq=2 ttl=60 time=0.714 ms
    84 bytes from 30.30.30.2 icmp_seq=3 ttl=60 time=1.031 ms
    84 bytes from 30.30.30.2 icmp_seq=4 ttl=60 time=0.752 ms
    84 bytes from 30.30.30.2 icmp_seq=5 ttl=60 time=1.105 ms

    PCISPA> trace 30.30.30.2
    trace to 30.30.30.2, 8 hops max, press Ctrl+C to stop
     1   20.20.20.1   0.290 ms  0.193 ms  0.203 ms
     2   172.16.20.2   0.471 ms  0.433 ms  0.429 ms
     3   10.10.20.6   0.632 ms  0.594 ms  0.556 ms
     4   172.16.30.1   0.856 ms  0.758 ms  0.807 ms
     5   *30.30.30.2   0.780 ms (ICMP type:3, code:3, Destination port unreachable)

    PCISPA> ping 40.40.40.2

    84 bytes from 40.40.40.2 icmp_seq=1 ttl=60 time=1.764 ms
    84 bytes from 40.40.40.2 icmp_seq=2 ttl=60 time=0.702 ms
    84 bytes from 40.40.40.2 icmp_seq=3 ttl=60 time=0.740 ms
    84 bytes from 40.40.40.2 icmp_seq=4 ttl=60 time=0.707 ms
    84 bytes from 40.40.40.2 icmp_seq=5 ttl=60 time=0.864 ms

    PCISPA> trace 40.40.40.2
    trace to 40.40.40.2, 8 hops max, press Ctrl+C to stop
     1   20.20.20.1   0.245 ms  0.132 ms  0.142 ms
     2   172.16.20.2   0.431 ms  0.283 ms  0.305 ms
     3   10.10.30.6   0.592 ms  0.444 ms  0.507 ms
     4   172.16.40.1   0.609 ms  0.632 ms  0.567 ms
     5   *40.40.40.2   0.636 ms (ICMP type:3, code:3, Destination port unreachable)

    PCISPA> ping 50.50.50.2

    84 bytes from 50.50.50.2 icmp_seq=1 ttl=59 time=2.477 ms
    84 bytes from 50.50.50.2 icmp_seq=2 ttl=59 time=0.967 ms
    84 bytes from 50.50.50.2 icmp_seq=3 ttl=59 time=1.023 ms
    84 bytes from 50.50.50.2 icmp_seq=4 ttl=59 time=0.904 ms
    84 bytes from 50.50.50.2 icmp_seq=5 ttl=59 time=0.772 ms

    PCISPA> trace 50.50.50.2
    trace to 50.50.50.2, 8 hops max, press Ctrl+C to stop
     1   20.20.20.1   0.230 ms  0.143 ms  0.153 ms
     2   172.16.20.2   0.327 ms  0.277 ms  0.260 ms
     3   10.10.30.6   0.476 ms  0.494 ms  0.443 ms
     4   10.30.40.6   0.577 ms  0.527 ms  0.577 ms
     5   172.16.50.1   0.781 ms  0.694 ms  0.679 ms
     6   *50.50.50.2   0.938 ms (ICMP type:3, code:3, Destination port unreachable)

    PCISPA>

    ! agora já não está tão bom, os ISPs estão usando a rede da companhia
    ! como trasito para atingir uns aos outros
    ! isso não é desejável, a não ser que sejamos um projedor de internet
    ! interligando nossos clientes

### Tarefa 08

#### Objetivo:

O aluno deve aplicar filtros nos equipamentos ***RT-SITE-A/B/C/D*** para permitir os prefixos IPv4 e IPv6 do ASN 100, e filtrar as demais redes de serem propagadas para os ISPs:

1. Na console do ***RT-ISP-A*** verifique as a topologia BGP e a RIB :

>
    RT-ISP-A#show bgp all
    For address family: IPv4 Unicast

    BGP table version is 31, local router ID is 172.16.20.1
    Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
                  r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
                  x best-external, a additional-path, c RIB-compressed,
    Origin codes: i - IGP, e - EGP, ? - incomplete
    RPKI validation codes: V valid, I invalid, N Not found

         Network          Next Hop            Metric LocPrf Weight Path
     *>  20.0.0.0         0.0.0.0                  0         32768 i
     *>  20.20.20.0/24    0.0.0.0                  0         32768 i
     *>  30.0.0.0         172.16.20.2                            0 100 30 i
     *>  30.30.30.0/24    172.16.20.2                            0 100 30 i
     *>  40.0.0.0         172.16.20.2                            0 100 40 i
     *>  40.40.40.0/24    172.16.20.2                            0 100 40 i
     *>  100.0.0.0        172.16.20.2              0             0 100 i
     *>  172.16.20.0/30   0.0.0.0                  0         32768 i
     *>  172.16.30.0/30   172.16.20.2                            0 100 30 i
     *>  172.16.40.0/30   172.16.20.2                            0 100 40 i

    For address family: IPv6 Unicast

    BGP table version is 24, local router ID is 172.16.20.1
    Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
                  r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
                  x best-external, a additional-path, c RIB-compressed,
    Origin codes: i - IGP, e - EGP, ? - incomplete
    RPKI validation codes: V valid, I invalid, N Not found

         Network          Next Hop            Metric LocPrf Weight Path
     *>  172:16:20::/126  ::                       0         32768 i
     *>  172:16:30::/126  172:16:20::2                           0 100 30 i
     *>  172:16:40::/126  172:16:20::2                           0 100 40 i
     *>  172:16:50::/126  172:16:20::2                           0 100 50 i
     *>  2003:20::/32     ::                       0         32768 i
     *>  2003:20:20::/64  ::                       0         32768 i
     *>  2003:30::/32     172:16:20::2                           0 100 30 i
     *>  2003:30:30::/64  172:16:20::2                           0 100 30 i
     *>  2003:40::/32     172:16:20::2                           0 100 40 i
     *>  2003:40:40::/64  172:16:20::2                           0 100 40 i
     *>  2003:50::/32     172:16:20::2                           0 100 50 i
     *>  2003:50:50::/64  172:16:20::2                           0 100 50 i
     *>  2003:100::/32    172:16:20::2             0             0 100 i

    For address family: IPv4 Multicast


    For address family: MVPNv4 Unicast

    RT-ISP-A#

    RT-ISP-A#sh ip route
    Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
           D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
           N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
           E1 - OSPF external type 1, E2 - OSPF external type 2
           i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
           ia - IS-IS inter area, * - candidate default, U - per-user static route
           o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
           a - application route
           + - replicated route, % - next hop override

    Gateway of last resort is not set

          20.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
    S        20.0.0.0/8 is directly connected, Null0
    C        20.20.20.0/24 is directly connected, Ethernet0/0
    L        20.20.20.1/32 is directly connected, Ethernet0/0
          30.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
    B        30.0.0.0/8 [20/0] via 172.16.20.2, 02:30:10
    B        30.30.30.0/24 [20/0] via 172.16.20.2, 02:30:10
          40.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
    B        40.0.0.0/8 [20/0] via 172.16.20.2, 10:20:15
    B        40.40.40.0/24 [20/0] via 172.16.20.2, 10:20:15
    B     100.0.0.0/8 [20/0] via 172.16.20.2, 10:27:33
          172.16.0.0/16 is variably subnetted, 4 subnets, 2 masks
    C        172.16.20.0/30 is directly connected, Ethernet0/1
    L        172.16.20.1/32 is directly connected, Ethernet0/1
    B        172.16.30.0/30 [20/0] via 172.16.20.2, 02:30:10
    B        172.16.40.0/30 [20/0] via 172.16.20.2, 10:20:15
    RT-ISP-A#

2. Na console do ***RT-SITE-A*** execute o script abaixo para configura o filtro no peering com o ***RT-ISP-A***:

>
    configure terminal

    ip prefix-list MY_IPV4_BLOCK description PERMITE O BLOCO 100.0.0.0/8 e suas subnets
    ip prefix-list MY_IPV4_BLOCK seq 5 permit 100.0.0.0/8 le 32

    ipv6 prefix-list MY_IPV6_BLOCK description PERMITE O BLOCO 2003:100::/32 e suas subnets
    ipv6 prefix-list MY_IPV6_BLOCK permit 2003:100::/32 le 128

    route-map BGP_IPV4_FILTER_ISP_OUT permit 10
    description PERMITE O BLOCO LOCAL
    match ip address prefix-list MY_IPV4_BLOCK

    route-map BGP_IPV6_FILTER_ISP_OUT permit 10
    description PERMITE O BLOCO LOCAL
    match ipv6 address prefix-list MY_IPV6_BLOCK

    exit

    router bgp 100

    address-family ipv4
    neighbor 172.16.20.1 route-map BGP_IPV4_FILTER_ISP_OUT out
    exit

    address-family ipv6
    neighbor 172:16:20::1 route-map BGP_IPV6_FILTER_ISP_OUT out

    end

    write

    ! precisa fazer um Soft Reset na conexão BGP com o RT-ISP-A para
    ! aplicar as alterações de filtros no peering

    clear ip bgp 172.16.20.1 soft

3. Na console do ***RT-ISP-A*** verifique as a topologia BGP e a RIB :

>
    RT-ISP-A#show bgp all
    For address family: IPv4 Unicast

    BGP table version is 51, local router ID is 172.16.20.1
    Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
                  r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
                  x best-external, a additional-path, c RIB-compressed,
    Origin codes: i - IGP, e - EGP, ? - incomplete
    RPKI validation codes: V valid, I invalid, N Not found

         Network          Next Hop            Metric LocPrf Weight Path
     *>  20.0.0.0         0.0.0.0                  0         32768 i
     *>  20.20.20.0/24    0.0.0.0                  0         32768 i
     *>  100.0.0.0        172.16.20.2              0             0 100 i
     *>  172.16.20.0/30   0.0.0.0                  0         32768 i

    For address family: IPv6 Unicast

    BGP table version is 35, local router ID is 172.16.20.1
    Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
                  r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
                  x best-external, a additional-path, c RIB-compressed,
    Origin codes: i - IGP, e - EGP, ? - incomplete
    RPKI validation codes: V valid, I invalid, N Not found

         Network          Next Hop            Metric LocPrf Weight Path
     *>  172:16:20::/126  ::                       0         32768 i
     *>  2003:20::/32     ::                       0         32768 i
     *>  2003:20:20::/64  ::                       0         32768 i
     *>  2003:100::/32    172:16:20::2             0             0 100 i

    For address family: IPv4 Multicast


    For address family: MVPNv4 Unicast

    RT-ISP-A#

    RT-ISP-A#show ip route
    Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
           N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
           E1 - OSPF external type 1, E2 - OSPF external type 2
           i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
           ia - IS-IS inter area, * - candidate default, U - per-user static route
           o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
           a - application route
           + - replicated route, % - next hop override

    Gateway of last resort is not set

          20.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
    S        20.0.0.0/8 is directly connected, Null0
    C        20.20.20.0/24 is directly connected, Ethernet0/0
    L        20.20.20.1/32 is directly connected, Ethernet0/0
    B     100.0.0.0/8 [20/0] via 172.16.20.2, 00:03:16
          172.16.0.0/16 is variably subnetted, 2 subnets, 2 masks
    C        172.16.20.0/30 is directly connected, Ethernet0/1
    L        172.16.20.1/32 is directly connected, Ethernet0/1
    RT-ISP-A#

    RT-ISP-A#show ipv6 route
    IPv6 Routing Table - default - 7 entries
    Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
           B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
           H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
           IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
           ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
           O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
           ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, ls - LISP site
           ld - LISP dyn-EID, a - Application
    C   172:16:20::/126 [0/0]
         via Ethernet0/1, directly connected
    L   172:16:20::1/128 [0/0]
         via Ethernet0/1, receive
    S   2003:20::/32 [1/0]
         via Null0, directly connected
    C   2003:20:20::/64 [0/0]
         via Ethernet0/0, directly connected
    L   2003:20:20::1/128 [0/0]
         via Ethernet0/0, receive
    B   2003:100::/32 [20/0]
         via FE80::A8BB:CCFF:FE00:1011, Ethernet0/1
    L   FF00::/8 [0/0]
         via Null0, receive
    RT-ISP-A#

4. Na console do ***PC-ISP-A*** execute ping para os equipamentos ***PC-A/B/C/D*** :

>
    PCISPA> ping 100.10.0.2

    84 bytes from 100.10.0.2 icmp_seq=1 ttl=62 time=1.569 ms
    84 bytes from 100.10.0.2 icmp_seq=2 ttl=62 time=0.483 ms
    84 bytes from 100.10.0.2 icmp_seq=3 ttl=62 time=0.530 ms
    84 bytes from 100.10.0.2 icmp_seq=4 ttl=62 time=0.586 ms
    84 bytes from 100.10.0.2 icmp_seq=5 ttl=62 time=0.368 ms

    PCISPA> ping 100.20.0.2

    84 bytes from 100.20.0.2 icmp_seq=1 ttl=61 time=1.704 ms
    84 bytes from 100.20.0.2 icmp_seq=2 ttl=61 time=0.592 ms
    84 bytes from 100.20.0.2 icmp_seq=3 ttl=61 time=0.834 ms
    84 bytes from 100.20.0.2 icmp_seq=4 ttl=61 time=0.716 ms
    84 bytes from 100.20.0.2 icmp_seq=5 ttl=61 time=0.480 ms

    PCISPA> ping 100.30.0.2

    84 bytes from 100.30.0.2 icmp_seq=1 ttl=61 time=1.674 ms
    84 bytes from 100.30.0.2 icmp_seq=2 ttl=61 time=0.578 ms
    84 bytes from 100.30.0.2 icmp_seq=3 ttl=61 time=0.688 ms
    84 bytes from 100.30.0.2 icmp_seq=4 ttl=61 time=0.553 ms
    84 bytes from 100.30.0.2 icmp_seq=5 ttl=61 time=0.519 ms

    PCISPA> ping 100.40.0.2

    84 bytes from 100.40.0.2 icmp_seq=1 ttl=60 time=2.241 ms
    84 bytes from 100.40.0.2 icmp_seq=2 ttl=60 time=1.389 ms
    84 bytes from 100.40.0.2 icmp_seq=3 ttl=60 time=1.028 ms
    84 bytes from 100.40.0.2 icmp_seq=4 ttl=60 time=1.188 ms
    84 bytes from 100.40.0.2 icmp_seq=5 ttl=60 time=0.931 ms

    PCISPA>

    ! veja que a nossa mudança não comprometeu a capacidade do PC-ISP-A em
    ! acessar os recursos dentro da Comapanhia

5. Na console do ***PC-ISP-A*** execute ping e traceroute para os equipamentos ***PC-ISP-B/C/D*** :

>
    PCISPA> ping 30.30.30.2

    *20.20.20.1 icmp_seq=1 ttl=255 time=0.178 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=2 ttl=255 time=0.325 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=3 ttl=255 time=0.290 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=4 ttl=255 time=0.201 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=5 ttl=255 time=0.199 ms (ICMP type:3, code:1, Destination host unreachable)

    PCISPA> trace 30.30.30.2
    trace to 30.30.30.2, 8 hops max, press Ctrl+C to stop
     1   20.20.20.1   0.370 ms  0.203 ms  0.194 ms
     2   *20.20.20.1   0.191 ms (ICMP type:3, code:1, Destination host unreachable)  *

    PCISPA> ping 40.40.40.2

    *20.20.20.1 icmp_seq=1 ttl=255 time=0.228 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=2 ttl=255 time=0.215 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=3 ttl=255 time=0.295 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=4 ttl=255 time=0.327 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=5 ttl=255 time=0.317 ms (ICMP type:3, code:1, Destination host unreachable)

    PCISPA> trace 40.40.40.2
    trace to 40.40.40.2, 8 hops max, press Ctrl+C to stop
     1   20.20.20.1   0.296 ms  0.197 ms  0.203 ms
     2   *20.20.20.1   0.235 ms (ICMP type:3, code:1, Destination host unreachable)  *

    PCISPA> ping 50.50.50.2

    *20.20.20.1 icmp_seq=1 ttl=255 time=0.287 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=2 ttl=255 time=0.270 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=3 ttl=255 time=0.264 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=4 ttl=255 time=0.314 ms (ICMP type:3, code:1, Destination host unreachable)
    *20.20.20.1 icmp_seq=5 ttl=255 time=0.253 ms (ICMP type:3, code:1, Destination host unreachable)

    PCISPA> trace 50.50.50.2
    trace to 50.50.50.2, 8 hops max, press Ctrl+C to stop
     1   20.20.20.1   0.368 ms  0.204 ms  0.194 ms
     2   *20.20.20.1   0.194 ms (ICMP type:3, code:1, Destination host unreachable)  *

    PCISPA>

    ! observer que agora o ISP A não consegue mais utilizar a rede da companhia como
    ! transito para atingir os demias ISPs

6. Na console do ***RT-SITE-B*** execute o script abaixo para configura o filtro no peering com o ***RT-ISP-B***:

>
    configure terminal

    ip prefix-list MY_IPV4_BLOCK description PERMITE O BLOCO 100.0.0.0/8 e suas subnets
    ip prefix-list MY_IPV4_BLOCK seq 5 permit 100.0.0.0/8 le 32

    ipv6 prefix-list MY_IPV6_BLOCK description PERMITE O BLOCO 2003:100::/32 e suas subnets
    ipv6 prefix-list MY_IPV6_BLOCK permit 2003:100::/32 le 128

    route-map BGP_IPV4_FILTER_ISP_OUT permit 10
    description PERMITE O BLOCO LOCAL
    match ip address prefix-list MY_IPV4_BLOCK

    route-map BGP_IPV6_FILTER_ISP_OUT permit 10
    description PERMITE O BLOCO LOCAL
    match ipv6 address prefix-list MY_IPV6_BLOCK

    exit

    router bgp 100

    address-family ipv4
    neighbor 172.16.30.1 route-map BGP_IPV4_FILTER_ISP_OUT out
    exit

    address-family ipv6
    neighbor 172:16:30::1 route-map BGP_IPV6_FILTER_ISP_OUT out

    end

    write

    ! precisa fazer um Soft Reset na conexão BGP com o RT-ISP-A para
    ! aplicar as alterações de filtros no peering

    clear ip bgp 172.16.30.1 soft

6. Usando como base o script acima, Na console dos equipamentos ***RT-SITE-C*** e ***RT-SITE-D** execute o seu script para configura o filtro no peering com os respectivos ISPs.

7. Acesse a console dos equipamentos ***PC-ISP-B/C/D*** e verifique se o resultado da aplicação dos Filtros é semelhante ao que ocorreu com o ***PC-ISP-A***.