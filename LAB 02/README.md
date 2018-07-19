# BGP 101
## LAB 02

### Índice

* [Objetivo](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2002#objetivo)
* [Topologia do LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2002#topologia-do-lab)
	* [Credenciais de acesso](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2002#credenciais-de-acesso)
* [Comandos utilizados no LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2002#comandos-utilizados-no-lab)
* [ATIVIDADES DO LAB 02](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2002#atividades-do-lab-02)
	* [Tarefa 01](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2002#tarefa-01)
	* [Tarefa 02](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2002#tarefa-02)
	* [Tarefa 03](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2002#tarefa-03)

### Objetivo
Neste lab o aluno será apresentado aos métodos para injetar rotas no ***BGP***, assim como aos comandos *network*, *aggregate* e *redistribution*.

O aluno deve seguir os passos para poder configurar os roteadores para injetar as rotas na topologia BGP.

### Topologia do LAB

![topologia](https://raw.githubusercontent.com/leandropinheiro/BGP101/master/img/LAB02-topologia.png)

HOSTNAME | TIPO | ASN | E0/0 | E0/1 | Lo0 | Lo1 
:-------:|:----:|:---:|:----:|:----:|:---:|:---:
RTA|ROUTER|100|1.1.1.1/24|-|100.0.0.1/24|101.0.0.1/24
RTA|ROUTER|100|1::1/64|-|100::1/64|101::1/64
RTB|ROUTER|100|1.1.1.2/24|2.2.2.2/24|110.0.0.1/24|111.0.0.1/24
RTB|ROUTER|100|1::2/64|2::2/64|110::1/64|111::1/64
RTC|ROUTER|200|2.2.2.1/24|-|200.0.0.1/24|201.0.0.1/24
RTC|ROUTER|200|2::1/64|-|200::1/64|201::1/64

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

2. Verificar prefixos IPv4/IPv6 no ***RTA***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTA#sh bgp all       
	For address family: IPv4 Unicast
	
	BGP table version is 1, local router ID is 101.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 * i 2.2.2.0/24       2.2.2.1                  0    100      0 200 ?
	 * i 200.0.0.0        2.2.2.1                  0    100      0 200 ?
	 * i 201.0.0.0        2.2.2.1                  0    100      0 200 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 1, local router ID is 101.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 * i 2::/64           2::1                     0    100      0 200 ?
	 * i 200::/64         2::1                     0    100      0 200 ?
	 * i 201::/64         2::1                     0    100      0 200 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTA#

3. Verificar prefixos IPv4 tabela de rotas (RIB) do ***RTA***, execute o comando ***show ip route***, compare com a saída de exemplo abaixo:

>
	RTA#sh ip route   
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
	
	      1.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        1.1.1.0/24 is directly connected, Ethernet0/0
	L        1.1.1.1/32 is directly connected, Ethernet0/0
	      100.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        100.0.0.0/24 is directly connected, Loopback0
	L        100.0.0.1/32 is directly connected, Loopback0
	      101.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        101.0.0.0/24 is directly connected, Loopback1
	L        101.0.0.1/32 is directly connected, Loopback1
	RTA#
	
	! veja que apesar de termos os refixos 2.2.2.0/24, 200.0.0.0/24 e 201.0.0.0/24 na
	! topologia do BGP, estes prefixos não estão presetes na tabela de rotas (RIB).
	
	!      Network          Next Hop            Metric LocPrf Weight Path
	!	 * i 2.2.2.0/24       2.2.2.1                  0    100      0 200 ?
	
	! veja que  Next Hop é 2.2.2.1, e o RTA não tem na RIB uma rota para alcançar
	! este destino, como o BGP não consegue validar o Next Hop, ele não envia a
	! rota para ser inserida na RIB.

4. Acessar a console do ***RTB***.

5. Verificar prefixos IPv4/IPv6 no ***RTB***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTB#sh bgp all
	For address family: IPv4 Unicast
	
	BGP table version is 4, local router ID is 2.2.2.2
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 r>  2.2.2.0/24       2.2.2.1                  0             0 200 ?
	 *>  200.0.0.0        2.2.2.1                  0             0 200 ?
	 *>  201.0.0.0        2.2.2.1                  0             0 200 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 4, local router ID is 2.2.2.2
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 r>  2::/64           2::1                     0             0 200 ?
	 *>  200::/64         2::1                     0             0 200 ?
	 *>  201::/64         2::1                     0             0 200 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTB#

6. Verificar prefixos IPv4 tabela de rotas (RIB) do ***RTB***, execute o comando ***show ip route***, compare com a saída de exemplo abaixo:

>
	RTB#sh ip route
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
	
	      1.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        1.1.1.0/24 is directly connected, Ethernet0/0
	L        1.1.1.2/32 is directly connected, Ethernet0/0
	      2.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        2.2.2.0/24 is directly connected, Ethernet0/1 <<<===
	L        2.2.2.2/32 is directly connected, Ethernet0/1 
	      110.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        110.0.0.0/24 is directly connected, Loopback0
	L        110.0.0.1/32 is directly connected, Loopback0
	      111.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        111.0.0.0/24 is directly connected, Loopback1
	L        111.0.0.1/32 is directly connected, Loopback1
	B     200.0.0.0/24 [20/0] via 2.2.2.1, 00:11:33
	B     201.0.0.0/24 [20/0] via 2.2.2.1, 00:11:33
	RTB#
	
	! veja que no caso do RTB, como o mesmo tem uma rota para a rede 2.2.2.0/24,
	! o BGP consegue validar o Next Hop, e envia os prefixos para serem inseridos
	! na RIB.

7. Verificar a configuração BGP no ***RTB***, execute o comando ***show running-config | section bgp***, compare com a saída de exemplo abaixo:

>
	RTB#show running-config | section bgp
	router bgp 100
	 bgp log-neighbor-changes
	 no bgp default ipv4-unicast
	 neighbor 1::1 remote-as 100
	 neighbor 2::1 remote-as 200
	 neighbor 1.1.1.1 remote-as 100
	 neighbor 2.2.2.1 remote-as 200
	 !
	 address-family ipv4
	  neighbor 1.1.1.1 activate
	  neighbor 2.2.2.1 activate
	 exit-address-family
	 !
	 address-family ipv6
	  neighbor 1::1 activate
	  neighbor 2::1 activate
	 exit-address-family
	RTB#
	
	! para resolver a falta de rotas para o RTA atingir as rotas do RTC, e para
	! do RTC para o RTA, vamos usar o next-hop-self para alterar a informação
	! dos prefixos quando o RTB enviá-los para o RTA.

8. Execute o script abaixo:

>
	configure terminal
	
	router bgp 100
	address-family ipv4
	neighbor 1.1.1.1 next-hop-self
	neighbor 2.2.2.1 next-hop-self
	address-family ipv6
	neighbor 1::1 next-hop-self
	neighbor 2::1 next-hop-self
	end
	write
	
	! veja que a alteração é realizada por neighbor.

9. Acessar a console do ***RTA***.

10. Verificar prefixos IPv4/IPv6 no ***RTA***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTA#show bgp all 
	For address family: IPv4 Unicast
	
	BGP table version is 4, local router ID is 101.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>i 2.2.2.0/24       1.1.1.2                  0    100      0 200 ?
	 *>i 200.0.0.0        1.1.1.2                  0    100      0 200 ?
	 *>i 201.0.0.0        1.1.1.2                  0    100      0 200 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 4, local router ID is 101.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>i 2::/64           1::2                     0    100      0 200 ?
	 *>i 200::/64         1::2                     0    100      0 200 ?
	 *>i 201::/64         1::2                     0    100      0 200 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTA#
	
	! veja se não houve nenhuma alteração.
	
	! quando uma conexão BGP está ativa, dependendo da alteração é necessário
	! fazer um reset na sessão BGP entre os roteadores.

11. Acessar a console do ***RTB***.

12. Execute o comando abaixo:

>
	cleair ip bgp * soft
	
	! esse comando vai fazer um Soft Reset na Sessão BGP, sem derrubar a mesma
	! apenas vai alterar os parametros da mesma e aplicar as alterações.

	! CUIDADO!!! O * vai aplicar o reset em todas as Sessões BGP locais do
	! roteador, para aplicar a um neighbor especifico, usar o IP do mesmo.
	
	! se não for utilizado o keyword "soft", a sessão BGP é derrubada, e uma
	! nova sessão será estabelecida entre os roteadores, implicando em nova
	! troca de todas as informações de prefixos entre os mesmos, em produção
	! com Full BGP, esse procedimento pode levar de 15 a 30 minutos para
	! completar.

13. Verificar prefixos IPv4 tabela de rotas (RIB) do ***RTA***, execute o comando ***show ip route***, compare com a saída de exemplo abaixo:

>
	RTA#show ip route 
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
	
	      1.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        1.1.1.0/24 is directly connected, Ethernet0/0
	L        1.1.1.1/32 is directly connected, Ethernet0/0
	      2.0.0.0/24 is subnetted, 1 subnets
	B        2.2.2.0 [200/0] via 1.1.1.2, 00:06:06
	      100.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        100.0.0.0/24 is directly connected, Loopback0
	L        100.0.0.1/32 is directly connected, Loopback0
	      101.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        101.0.0.0/24 is directly connected, Loopback1
	L        101.0.0.1/32 is directly connected, Loopback1
	B     200.0.0.0/24 [200/0] via 1.1.1.2, 00:06:06	
	B     201.0.0.0/24 [200/0] via 1.1.1.2, 00:06:06
	RTA#
	
	! veja que agora as rotas estão na RIB, e o next hop é o endereço do RTB
	! na rede em comum com o RTA (e0/0 - 1.1.1.0/24)

14. Verificar prefixos IPv6 tabela de rotas (RIB) do ***RTA***, execute o comando ***show ip route***, compare com a saída de exemplo abaixo:

>
	RTA#show ipv6 route 
	IPv6 Routing Table - default - 10 entries
	Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
	       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
	       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
	       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
	       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
	       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
	       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, ls - LISP site
	       ld - LISP dyn-EID, a - Application
	C   1::/64 [0/0]
	     via Ethernet0/0, directly connected
	L   1::1/128 [0/0]
	     via Ethernet0/0, receive
	B   2::/64 [200/0]
	     via 1::2
	C   100::/64 [0/0]
	     via Loopback0, directly connected
	L   100::1/128 [0/0]
	     via Loopback0, receive
	C   101::/64 [0/0]
	     via Loopback1, directly connected
	L   101::1/128 [0/0]
	     via Loopback1, receive
	B   200::/64 [200/0]
	     via 1::2
	B   201::/64 [200/0]
	     via 1::2
	L   FF00::/8 [0/0]
	     via Null0, receive
	RTA#
	
	! veja que o mesmo que ocorreu no IPv4 também está valendo para o IPv6.

### Tarefa 02
1. Acessar a console do ***RTA***.

2. Execute o script abaixo:

>
	configure terminal
	
	router bgp 100
	address-family ipv4
	network 1.1.1.0 mask 255.255.255.0
	network 100.0.0.0 mask 255.255.255.0
	network 101.0.0.0 mask 255.255.0.0
	address-family ipv6
	network 1::/64
	network 100::/64
	network 101::/64
	end
	write
	
	! estamos aplicando o comando network para injetar os prefixos das interfaces
	! do RTA na topologia do BGP.

3. Acessar a console do ***RTC***.

4. Verificar prefixos IPv4/IPv6 no ***RTC***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTC#sh bgp all
	For address family: IPv4 Unicast
	
	BGP table version is 6, local router ID is 201.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1.1.1.0/24       2.2.2.2                                0 100 i
	 *>  2.2.2.0/24       0.0.0.0                  0         32768 ?
	 *>  100.0.0.0/24     2.2.2.2                                0 100 i
	 *>  200.0.0.0        0.0.0.0                  0         32768 ?
	 *>  201.0.0.0        0.0.0.0                  0         32768 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 7, local router ID is 201.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1::/64           2::2                                   0 100 i
	 *>  2::/64           ::                       0         32768 ?
	 *>  100::/64         2::2                                   0 100 i
	 *>  101::/64         2::2                                   0 100 i
	 *>  200::/64         ::                       0         32768 ?
	 *>  201::/64         ::                       0         32768 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTC#
	
	! devemos observar três coisas:
	
	! a primeira é que quando o Next Hop é 0.0.0.0 (IPv4) ou :: (IPv6),
	! significa que o prefixo se originou no roteador local.
	
	! a segunda é que os prefixos originados no RTA agora estão aparecendo na
	! topologia do BGP, e o Next Hop é o endereço local do RTB, devido a utilização
	! da opção next-hop-self.
	
	! a terceira é que o prefixo 101.0.0.0/24 não consta da topologia BGP no RTC.

5. Acessar a console do ***RTA***.

6. Verificar a configuração BGP no ***RTA***, execute o comando ***show running-config | section bgp***, compare com a saída de exemplo abaixo:

>
	RTA#show running-config | section bgp
	router bgp 100
	 bgp log-neighbor-changes
	 no bgp default ipv4-unicast
	 neighbor 1::2 remote-as 100
	 neighbor 1.1.1.2 remote-as 100
	 !
	 address-family ipv4
	  network 1.1.1.0 mask 255.255.255.0
	  network 100.0.0.0 mask 255.255.255.0
	  network 101.0.0.0 mask 255.255.0.0
	  neighbor 1.1.1.2 activate
	 exit-address-family
	 !
	 address-family ipv6
	  network 1::/64
	  network 100::/64
	  network 101::/64
	  neighbor 1::2 activate
	 exit-address-family
	RTA#

7. Verificar prefixos IPv4 tabela de rotas (RIB) do ***RTA***, execute o comando ***show ip route***, compare com a saída de exemplo abaixo:

>
	RTA#show ip route 
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
	
	      1.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        1.1.1.0/24 is directly connected, Ethernet0/0
	L        1.1.1.1/32 is directly connected, Ethernet0/0
	      2.0.0.0/24 is subnetted, 1 subnets
	B        2.2.2.0 [200/0] via 1.1.1.2, 00:22:02
	      100.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        100.0.0.0/24 is directly connected, Loopback0
	L        100.0.0.1/32 is directly connected, Loopback0
	      101.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
	C        101.0.0.0/24 is directly connected, Loopback1
	L        101.0.0.1/32 is directly connected, Loopback1
	B     200.0.0.0/24 [200/0] via 1.1.1.2, 00:22:02
	B     201.0.0.0/24 [200/0] via 1.1.1.2, 00:22:02
	RTA#
	
	! observe que na configuração BGP do RTA o network está 101.0.0.0/16.
	! na RIP não temos nenhum prefixo 101.0.0.0/16, apenas /8, /24.
	
	! o comportamento do comando network no BGP é diferente dos IGPs.
	! no BGP ele verifica a RIB e injeta no BGP o prefixo extado que foi
	! configurado.

8. Execute o script abaixo:

>
	configure terminal
	
	router bgp 100
	address-family ipv4
	no network 101.0.0.0 mask 255.255.0.0
	network 101.0.0.0 mask 255.255.255.0
	end
	write
	
	! removemos o prefixo 101.0.0.0/16, e adicionamos o 101.0.0.0/24 que existe
	! na RIB do equipamento.

9. Acessar a console do ***RTC***.

10. Verificar prefixos IPv4 no ***RTC***, execute o comando ***show ip bgp***, compare com a saída de exemplo abaixo:

>
	RTC#show ip bgp
	BGP table version is 7, local router ID is 201.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1.1.1.0/24       2.2.2.2                                0 100 i
	 *>  2.2.2.0/24       0.0.0.0                  0         32768 ?
	 *>  100.0.0.0/24     2.2.2.2                                0 100 i
	 *>  101.0.0.0/24     2.2.2.2                                0 100 i
	 *>  200.0.0.0        0.0.0.0                  0         32768 ?
	 *>  201.0.0.0        0.0.0.0                  0         32768 ?
	RTC#
	
	! agora temos o prefixo 101.0.0.0/24 na topologia BGP do RTC.

### Tarefa 03
1. Acessar a console do ***RTA***.

2. Verificar prefixos IPv4/IPv6 no ***RTA***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTA#show bgp all
	For address family: IPv4 Unicast
	
	BGP table version is 7, local router ID is 101.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1.1.1.0/24       0.0.0.0                  0         32768 i
	 *>i 2.2.2.0/24       1.1.1.2                  0    100      0 200 ?
	 *>  100.0.0.0/24     0.0.0.0                  0         32768 i
	 *>  101.0.0.0/24     0.0.0.0                  0         32768 i
	 *>i 200.0.0.0        1.1.1.2                  0    100      0 200 ?
	 *>i 201.0.0.0        1.1.1.2                  0    100      0 200 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 7, local router ID is 101.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1::/64           ::                       0         32768 i
	 *>i 2::/64           1::2                     0    100      0 200 ?
	 *>  100::/64         ::                       0         32768 i
	 *>  101::/64         ::                       0         32768 i
	 *>i 200::/64         1::2                     0    100      0 200 ?
	 *>i 201::/64         1::2                     0    100      0 200 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTA#

3. Acessar a console do ***RTC***.

4. Verificar prefixos IPv4/IPv6 no ***RTC***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTC#show bgp all
	For address family: IPv4 Unicast
	
	BGP table version is 7, local router ID is 201.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1.1.1.0/24       2.2.2.2                                0 100 i
	 *>  2.2.2.0/24       0.0.0.0                  0         32768 ?
	 *>  100.0.0.0/24     2.2.2.2                                0 100 i
	 *>  101.0.0.0/24     2.2.2.2                                0 100 i
	 *>  200.0.0.0        0.0.0.0                  0         32768 ?
	 *>  201.0.0.0        0.0.0.0                  0         32768 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 7, local router ID is 201.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1::/64           2::2                                   0 100 i
	 *>  2::/64           ::                       0         32768 ?
	 *>  100::/64         2::2                                   0 100 i
	 *>  101::/64         2::2                                   0 100 i
	 *>  200::/64         ::                       0         32768 ?
	 *>  201::/64         ::                       0         32768 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTC#
	
	! observe que não temos nenhum prefixo originado no RTB, apenas as redes
	! injetadas pelos roteadores RTA e RTC.

5. Acessar a console do ***RTB***.

6. Verificar a configuração BGP no ***RTB***, execute o comando ***show running-config | section bgp***, compare com a saída de exemplo abaixo:

>
	RTB#show running-config | sec bgp
	router bgp 100
	 bgp log-neighbor-changes
	 no bgp default ipv4-unicast
	 neighbor 1::1 remote-as 100
	 neighbor 2::1 remote-as 200
	 neighbor 1.1.1.1 remote-as 100
	 neighbor 2.2.2.1 remote-as 200
	 !
	 address-family ipv4
	  neighbor 1.1.1.1 activate
	  neighbor 1.1.1.1 next-hop-self
	  neighbor 2.2.2.1 activate
	  neighbor 2.2.2.1 next-hop-self
	 exit-address-family
	 !
	 address-family ipv6
	  neighbor 1::1 activate
	  neighbor 1::1 next-hop-self
	  neighbor 2::1 activate
	  neighbor 2::1 next-hop-self
	 exit-address-family
	RTB#
	
	! podemos verificar que não temos nenhum comando network ou redistribute,
	! com isso nenhuma rede do RTB está sendo injetada no BGP.

7. Verificar prefixos IPv4/IPv6 no ***RTB***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTB#show bgp all
	For address family: IPv4 Unicast
	
	BGP table version is 7, local router ID is 2.2.2.2
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 r>i 1.1.1.0/24       1.1.1.1                  0    100      0 i
	 r>  2.2.2.0/24       2.2.2.1                  0             0 200 ?
	 *>i 100.0.0.0/24     1.1.1.1                  0    100      0 i
	 *>i 101.0.0.0/24     1.1.1.1                  0    100      0 i
	 *>  200.0.0.0        2.2.2.1                  0             0 200 ?
	 *>  201.0.0.0        2.2.2.1                  0             0 200 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 7, local router ID is 2.2.2.2
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 r>i 1::/64           1::1                     0    100      0 i
	 r>  2::/64           2::1                     0             0 200 ?
	 *>i 100::/64         1::1                     0    100      0 i
	 *>i 101::/64         1::1                     0    100      0 i
	 *>  200::/64         2::1                     0             0 200 ?
	 *>  201::/64         2::1                     0             0 200 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTB#
	
	! observando os prefixos na tologia BGP, nenhum é originado no RTB,
	! pois nenum tem como Next Hop 0.0.0.0 (IPv4) ou :: (IPv6).

8. Execute o script abaixo:

>
	configure terminal
	
	router bgp 100
	address-family ipv4
	redistribute connected
	exit
	address-family ipv6
	redistribute connected
	end
	write
	
	! diferente do RTA, optamos no RTB por utilizar redistribuição.

9. Verificar prefixos IPv4/IPv6 no ***RTB***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTB#show bgp all       
	For address family: IPv4 Unicast
	
	BGP table version is 11, local router ID is 2.2.2.2
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1.1.1.0/24       0.0.0.0                  0         32768 ?
	 * i                  1.1.1.1                  0    100      0 i
	 *>  2.2.2.0/24       0.0.0.0                  0         32768 ?
	 *                    2.2.2.1                  0             0 200 ?
	 *>i 100.0.0.0/24     1.1.1.1                  0    100      0 i
	 *>i 101.0.0.0/24     1.1.1.1                  0    100      0 i
	 *>  110.0.0.0/24     0.0.0.0                  0         32768 ?
	 *>  111.0.0.0/24     0.0.0.0                  0         32768 ?
	 *>  200.0.0.0        2.2.2.1                  0             0 200 ?
	 *>  201.0.0.0        2.2.2.1                  0             0 200 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 11, local router ID is 2.2.2.2
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1::/64           ::                       0         32768 ?
	 * i                  1::1                     0    100      0 i
	 *>  2::/64           ::                       0         32768 ?
	 *                    2::1                     0             0 200 ?
	 *>i 100::/64         1::1                     0    100      0 i
	 *>i 101::/64         1::1                     0    100      0 i
	 *>  110::/64         ::                       0         32768 ?
	 *>  111::/64         ::                       0         32768 ?
	 *>  200::/64         2::1                     0             0 200 ?
	 *>  201::/64         2::1                     0             0 200 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTB#
	
	! aqui podemos observar três coisas.
	
	! a primeira é que agora temos claramente prefixos originados no RTB.
	
	! a segunda é que os prefixos originados no RTB tem o Path = ?, isso
	! significa que a origem está incompleta, acontece quando o BGP não tem
	! certeza da origem do prefixo, geralmente está relacionado com rotas 
	! redistribuidas.
	
	! a terceira é que os prefixos orifinados no RTA tem o Path = i, quando a
	! origem é via um IGP, ou seja usando o comando network para sincronizar com
	! prefixos existentes na RIP, o BGP valida o prefixo e adiciona a origem ao
	! BGP.

10. Verificar prefixos IPv4/IPv6 no ***RTA***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTA#show bgp all
	For address family: IPv4 Unicast
	
	BGP table version is 10, local router ID is 101.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 * i 1.1.1.0/24       1.1.1.2                  0    100      0 ?
	 *>                   0.0.0.0                  0         32768 i
	 *>i 2.2.2.0/24       1.1.1.2                  0    100      0 ?
	 *>  100.0.0.0/24     0.0.0.0                  0         32768 i
	 *>  101.0.0.0/24     0.0.0.0                  0         32768 i
	 *>i 110.0.0.0/24     1.1.1.2                  0    100      0 ?
	 *>i 111.0.0.0/24     1.1.1.2                  0    100      0 ?
	 *>i 200.0.0.0        1.1.1.2                  0    100      0 200 ?
	 *>i 201.0.0.0        1.1.1.2                  0    100      0 200 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 10, local router ID is 101.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 * i 1::/64           1::2                     0    100      0 ?
	 *>                   ::                       0         32768 i
	 *>i 2::/64           1::2                     0    100      0 ?
	 *>  100::/64         ::                       0         32768 i
	 *>  101::/64         ::                       0         32768 i
	 *>i 110::/64         1::2                     0    100      0 ?
	 *>i 111::/64         1::2                     0    100      0 ?
	 *>i 200::/64         1::2                     0    100      0 200 ?
	 *>i 201::/64         1::2                     0    100      0 200 ?
	
	For address family: IPv4 Multicast
	
		
	For address family: MVPNv4 Unicast
	
	RTA#
	
	! veja que agora o RTA também tem os prefixos do RTB.

11. Verificar prefixos IPv4/IPv6 no ***RTC***, execute o comando ***show bgp all***, compare com a saída de exemplo abaixo:

>
	RTC#show bgp all
	For address family: IPv4 Unicast
	
	BGP table version is 10, local router ID is 201.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1.1.1.0/24       2.2.2.2                  0             0 100 ?
	 *   2.2.2.0/24       2.2.2.2                  0             0 100 ?
	 *>                   0.0.0.0                  0         32768 ?
	 *>  100.0.0.0/24     2.2.2.2                                0 100 i
	 *>  101.0.0.0/24     2.2.2.2                                0 100 i
	 *>  110.0.0.0/24     2.2.2.2                  0             0 100 ?
	 *>  111.0.0.0/24     2.2.2.2                  0             0 100 ?
	 *>  200.0.0.0        0.0.0.0                  0         32768 ?
	 *>  201.0.0.0        0.0.0.0                  0         32768 ?
	
	For address family: IPv6 Unicast
	
	BGP table version is 10, local router ID is 201.0.0.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 *>  1::/64           2::2                     0             0 100 ?
	 *   2::/64           2::2                     0             0 100 ?
	 *>                   ::                       0         32768 ?
	 *>  100::/64         2::2                                   0 100 i
	 *>  101::/64         2::2                                   0 100 i
	 *>  110::/64         2::2                     0             0 100 ?
	 *>  111::/64         2::2                     0             0 100 ?
	 *>  200::/64         ::                       0         32768 ?
	 *>  201::/64         ::                       0         32768 ?
	
	For address family: IPv4 Multicast
	
	
	For address family: MVPNv4 Unicast
	
	RTC#
	
	! veja que o RTC agora tem todos os prefixos do RTA e RTB.
	
	! agora já é possivel ter acesso do RTC até o RTA.

12. Execute um traceroute para o IPv4 100.0.0.1

>
	RTC#traceroute 100.0.0.1 numeric 
	Type escape sequence to abort.
	Tracing the route to 100.0.0.1
	VRF info: (vrf in name/id, vrf out name/id)
	  1 2.2.2.2 1 msec 0 msec 1 msec
	  2 1.1.1.1 [AS 100] 0 msec *  1 msec
	RTC#

13. Execute um traceroute para o IPv6 100::1

>
	RTC#traceroute 100::1         
	Type escape sequence to abort.
	Tracing the route to 100::1
	
	  1 2::2 1 msec 1 msec 0 msec
	  2 1::1 [AS 100] 1 msec 1 msec 1 msec
	RTC#

14. Execute um ping para o IPv4 100.0.0.1

>
	RTC#ping 101.0.0.1
	Type escape sequence to abort.
	Sending 5, 100-byte ICMP Echos to 101.0.0.1, timeout is 2 seconds:
	!!!!!
	Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
	RTC#

15. Execute um ping para o IPv6 100::1

>
	RTC#ping 101::1
	Type escape sequence to abort.
	Sending 5, 100-byte ICMP Echos to 101::1, timeout is 2 seconds:
	!!!!!
	Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
	RTC#