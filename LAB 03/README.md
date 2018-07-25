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
Neste lab o aluno será apresentado ao método para configurar um peering BGP utilizando interfaces Loopback, e aproveitando para mostrar como essa opção pode ser utilizada para utilizar multilinks entre os peers, assim como prover redundância de link para a sessão BGP.

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

Os roteador **RTA** não necessita de nenhuma configuração do aluno para este LAB. Neste equipamento o aluno pode fazer qualquer configuração, e executar quaisquer comandos.

O roteador **RTB** está configurado apenas com as configurações básicas, como *hostname*, *description* e endereço *IPv4* e *IPv6* das *interfaces*, assim como a configuração BGP do LAB 02. Neste equipamento o aluno pode fazer qualquer configuração, e executar quaisquer comandos.

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

## ATIVIDADES DO LAB 03
### Tarefa 01
1. Acessar a console do ***RTC***.

2. Execute um traceroute para o IPv4 100.0.0.1

>
	RTC#traceroute 100.0.0.1 numeric 
	Type escape sequence to abort.
	Tracing the route to 100.0.0.1
	VRF info: (vrf in name/id, vrf out name/id)
	  1 2.2.2.2 0 msec 0 msec 0 msec
	  2 1.1.1.1 [AS 100] 0 msec *  0 msec
	RTC#
	
	! observe cada salto utiliza apenas um link, a interface 3.3.3.x não está
	! sendo utilizada.

3. Acessar a console do ***RTB***.

4. Verificar a configuração BGP no ***RTB***, execute o comando ***show running-config | section bgp***, compare com a saída de exemplo abaixo:

>
	RTB#show run | section bgp
	router bgp 100
	 bgp log-neighbor-changes
	 no bgp default ipv4-unicast
	 neighbor 1::1 remote-as 100
	 neighbor 2::1 remote-as 200
	 neighbor 1.1.1.1 remote-as 100
	 neighbor 2.2.2.1 remote-as 200
	 !
	 address-family ipv4
	  redistribute connected
	  neighbor 1.1.1.1 activate
	  neighbor 1.1.1.1 next-hop-self
	  neighbor 2.2.2.1 activate
	  neighbor 2.2.2.1 next-hop-self
	 exit-address-family
	 !
	 address-family ipv6
	  redistribute connected
	  neighbor 1::1 activate
	  neighbor 1::1 next-hop-self
	  neighbor 2::1 activate
	  neighbor 2::1 next-hop-self
	 exit-address-family
	RTB#
	
	! veja que o peering com o RTC está sendo realizado diretamente pela
	! interface e0/1 (via 2.2.2.1), com isso todas os prefixos destinados ao 
	! RTC são direcionados para esta interface, pois a mesma é utilizada como
	! Next Hop.
	
	! além de não utiliza o outro link entre o RTB e RTC (3.3.3.x), para dados
	! no caso de queda do link 2.2.2.x o peering BGP também para de funcionar.

5. Execute o script abaixo:

>
	configure terminal
	
	! primeiro vamos desabilitar o ip route-cache para evitar que o roteador
	! amarre uma conexão a uma interface especifica.
	
	interface ethernet 0/1
	no ip route-cache
	interface ethernet 0/2
	no ip route-cache
	
	! depois vamos criar rotas estáticas para balancear o acesso a interface
	! Loopback 0 do RTC por ambas as interfaces e0/1 (2.2.2.2) e 0/2 (3.3.3.2)
	
	ip route 200.0.0.1 255.255.255.255 2.2.2.1 name RTC-Lo0-VIA-e0/0
	ip route 200.0.0.1 255.255.255.255 3.3.3.1 name RTC-Lo0-VIA-e0/1
	
	! mesma coisa para as redes IPv6.
	
	ipv6 route 200::1/128 2::1 name RTC-Lo0-VIA-e0/0
	ipv6 route 200::1/128 3::1 name RTC-Lo0-VIA-e0/1
	
	router bgp 100
	
	! vamos por em shutdown os neighbors para o RTC utilizando a interface e0/0
	
	neighbor 2.2.2.1 shutdown
	neighbor 2::1 shutdown
	
	! vamos adicionar os novos neighbors com a interface Loopback 0 do RTC
	
	neighbor 200.0.0.1 remote-as 200
	neighbor 200::1 remote-as 200
	
	! Vamos configurá-los para permitir multihop, para quando as interfaces
	! não estão diretamente conectadas, e também configurar o source como
	! sendo a interface local Loopback 0 do RTB, para que a origem dos pacotes
	! BGP utilizem o IP de Origem correto.
	
	neighbor 200.0.0.1 ebgp-multihop
	neighbor 200.0.0.1 update-source loopback 0
	neighbor 200::1 ebgp-multihop
	neighbor 200::1 update-source loopback 0
	
	address-family ipv4
	neighbor 200.0.0.1 activate
	
	address-family ipv6
	neighbor 200::1 activate
	
	end
	write
	

6. Acessar a console do ***RTC***.

7. Execute um traceroute para o IPv4 100.0.0.1

>
	RTC#traceroute 100.0.0.1 numeric 
	Type escape sequence to abort.
	Tracing the route to 100.0.0.1
	VRF info: (vrf in name/id, vrf out name/id)
	  1 2.2.2.2 0 msec
	    3.3.3.2 5 msec
	    2.2.2.2 4 msec
	  2 1.1.1.1 [AS 100] 5 msec *  1 msec
	RTC#
	
	! observe que agora o primeiro salto (RTC -> RTB) está utilizando os dois
	! liks e0/1 (2.2.2.2) e e0/1 (3.3.3.2).

8. Execute um traceroute para o IPv6 100::1

>
	RTC#traceroute 100::1 
	Type escape sequence to abort.
	Tracing the route to 100::1
	
	  1 3::2 0 msec
	    2::2 1 msec 1 msec
	  2 1::1 [AS 100] 0 msec 0 msec 1 msec
	RTC#
	
	! observe que temos o mesmo comportamento para o IPv6.
