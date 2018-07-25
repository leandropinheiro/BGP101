# BGP 101
## LAB 04

### Índice

* [Objetivo](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#objetivo)
* [Topologia do LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#topologia-do-lab)
	* [Credenciais de acesso](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#credenciais-de-acesso)
* [Comandos utilizados no LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#comandos-utilizados-no-lab)
* [ATIVIDADES DO LAB 04](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#atividades-do-lab-03)
	* [Tarefa 01](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2004#tarefa-01)

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
RT-SITE-A|ROUTER|100|2003:100:10::1/64|10:10:20::1/126|10:10:20::5/126|10:10:30::5/126|10:10:30::1/126|172:16:20::2/126|2003:100::1/128
PC-A|vPC|-|2003:100:10::2/64|-|-|-|-|-|-
RT-SITE-B|ROUTER|100|2003:100:20::1/64|10:10:20::2/126|10:10:20::6/126|10:10:40::5/126|10:10:40::1/126|172:16:30::2/126|2003:100::2/128
PC-B|vPC|-|2003:100:20::2/64|-|-|-|-|-|-
RT-SITE-C|ROUTER|100|2003:100:30::1/64|10:30:40::1/126|10:30:40::5/126|10:10:30::6/126|10:10:30::2/126|172:16:40::2/126|2003:100::3/128
PC-C|vPC|-|2003:100:30::2/64|-|-|-|-|-|-
RT-SITE-D|ROUTER|100|2003:100:40::1/64|10:30:40::2/126|10:30:40::6/126|10:10:20::6/126|10:10:20::2/126|172:16:50::2/126|2003:100::4/128
PC-D|vPC|-|2003:100:40::2/64|-|-|-|-|-|-
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

2. Execute o script abaixo no ***RT-SITE-A*** para configurar o BGP e o *perring* com os roteadores **RT-SITE-B*** e ***RT-SITE-C*** :

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

4. Execute o script abaixo no ***RT-SITE-B*** para configurar o BGP e o *perring* com os roteadores **RT-SITE-A*** e ***RT-SITE-D*** :

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

6. Execute o script abaixo no ***RT-SITE-C*** para configurar o BGP e o *perring* com os roteadores **RT-SITE-A*** e ***RT-SITE-D*** :

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

8. Execute o script abaixo no ***RT-SITE-D*** para configurar o BGP e o *perring* com os roteadores **RT-SITE-B*** e ***RT-SITE-C*** :

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

