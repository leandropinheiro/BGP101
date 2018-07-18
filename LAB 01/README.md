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
*no*|Nega/Desabilita/Inverte a função de um determinado comando
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

## ATIVIDADES DO LAB 01
### Tarefa 01
1. O aluno deve acessar o LAB 01.

2. Depois deve clicar em ***More actions***, e depois ***Start all nodes***

3. Aguardar os roteadores mudarem da cor cinza para azul.

4. ***Clicar no icone do RTA***.

![RTA](https://raw.githubusercontent.com/leandropinheiro/BGP101/master/img/RTA_console.png)

5. Deve abrir uma sessão de terminal com o *prompt* de *login* do roteador ***RTA***.

6. Efetue o login com as credênciais fornecidas acima.

7. Execute o script abaixo:

>
	! para entrar no modo de confiugração, a partir do prompt #
	!
	configure terminal
	! muda para o prompt (config)#
	!
	! cria o processo BGP no ASN 100
	!
	router bgp 100
	!
	! adiciona o peer IPv4 1.1.1.2 no ASN 100 (iBGP)
	!
	neighbor 1.1.1.2 remote-as 100
	!
	! adiciona o peer IPv6 1::2 no ASN 100 (iBGP)
	!
	neighbor 1::2 remote-as 100
	!
	! sai do modo de configuração, retorna ao prompt #
	!
	end
	!
	! salva a alteração de configuração
	!
	write

8. Verificar neigbors IPv4 no ***RTA***, execute o comando ***show ip bgp summary***, compare com a saída de exemplo abaixo:

>
	RTA#show ip bgp summary
	BGP router identifier 1.1.1.1, local AS number 100
	BGP table version is 1, main routing table version 1
	
	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	1.1.1.2         4          100       0       0        1    0    0 never    Idle
	RTA#
	
	! veja que no Up/Down = never.
	! isso significa que a conexão BGP nunca ficou ativa antes.
	
	! Veja que o State/PfxRcd = Idle.
	! isso significa que a conexão com peer está Down.

9. Verificar os prefixos IPv4 no ***RTA***, execute o comando ***show ip bgp***, compare com a saída de exemplo abaixo:

>
	RTA#show ip bgp         
	RTA#
	
	! veja que não há saída nenhuma.
	! isso significa que não há prefixo na Topologia do BGP
	! os motivos são dois: 1) não há conexão Up com nenhum peer
	! que possa injetar prefixos, 2) o roteador local não está
	! injetando nenhum prexio no BGP.

### Tarefa 02
1. ***Clicar no icone do RTB***.

2. Deve abrir uma sessão de terminal com o *prompt* de *login* do roteador ***RTB***.

3. Efetue o login com as credênciais fornecidas acima.

4. Execute o script abaixo:

>
	! para entrar no modo de confiugração, a partir do prompt #
	!
	configure terminal
	! muda para o prompt (config)#
	!
	! cria o processo BGP no ASN 100
	!
	router bgp 100
	!
	! adiciona o peer IPv4 1.1.1.1 no ASN 100 (iBGP)
	!
	neighbor 1.1.1.1 remote-as 100
	!
	! adiciona o peer IPv4 2.2.2.1 no ASN 200 (eBGP)
	!
	neighbor 2.2.2.1 remote-as 200
	!
	! adiciona o peer IPv6 1::1 no ASN 100 (iBGP)
	!
	neighbor 1::1 remote-as 100
	!
	! adiciona o peer IPv6 2::1 no ASN 200 (eBGP)
	!
	neighbor 2::1 remote-as 200
	!
	! sai do modo de configuração, retorna ao prompt #
	!
	end
	!
	! salva a alteração de configuração
	!
	write

5. Verificar neigbors IPv4 no ***RTB***, execute o comando ***show bgp ipv4 unicast summary***, compare com a saída de exemplo abaixo:

>
	RTB#show bgp ipv4 unicast summary
	BGP router identifier 2.2.2.2, local AS number 100
	BGP table version is 1, main routing table version 1
	3 network entries using 420 bytes of memory
	3 path entries using 240 bytes of memory
	1/0 BGP path/bestpath attribute entries using 144 bytes of memory
	1 BGP AS-PATH entries using 24 bytes of memory
	0 BGP route-map cache entries using 0 bytes of memory
	0 BGP filter-list cache entries using 0 bytes of memory
	BGP using 828 total bytes of memory
	BGP activity 3/0 prefixes, 3/0 paths, scan interval 60 secs

	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	1.1.1.1         4          100       2       2        1    0    0 00:00:08        0
	2.2.2.1         4          200       5       2        1    0    0 00:00:09        3
	RTB#
	
	! veja que no Up/Down = Tempo.
	! isso significa que a conexão BGP ficou ativa antes e caiu, ou está ativa.
	
	! veja que o State/PfxRcd = número.
	! isso significa que a conexão com peer está Aberta(Open), que é o estado
	! desejado, esse número representa a quantidade de prefixos recebidos.

6. Verificar neigbors IPv6 no ***RTB***, execute o comando ***show bgp ipv6 unicast summary***, compare com a saída de exemplo abaixo:

>
	RTB#show bgp ipv6 unicast summary 
	
	RTB#
	
	! veja que no não há saida nenhuma.
	! isso significa que o peering IPv6 não está configurado, ou não está
	! ativado corretamente.

9. Verificar os prefixos IPv4 no ***RTB***, execute o comando ***show ip bgp***, compare com a saída de exemplo abaixo:

>
	RTB#show ip bgp 
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
	RTB#
	
	! veja que há prefixos na Tolologia BGP.
	! isso significa que temos ao menos um peering ativo, ou que o roteador
	! local está injetando prefixos.
	
	! observe em Next Hop que o IP pertence ao RTC.
	! Se não houve manipulação do prefixo, isso significa que o prefixo
	! foi originado no RTC.
	
	! observe o Path, ele indica a quantidade de saltos de ASN do prefixo.
	! a leitura do Path deve ser realziada de trás para frente, o último
	! ASN é o primeiro ASN do Path, geralmente significa que o prefixo se
	! origina nesse ASN.
	
	! no nosso LAB todos os Prefixos são originados no RTC, pois apenas ele
	! está injetando prefixos no BGP.

### Tarefa 03
1. Acessar a console do ***RTA***.

2. Verificar os prefixos IPv4 no ***RTA***, execute o comando ***show ip bgp***, compare com a saída de exemplo abaixo:

>
	RTA#show ip bgp        
	BGP table version is 1, local router ID is 1.1.1.1
	Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
	              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
	              x best-external, a additional-path, c RIB-compressed, 
	Origin codes: i - IGP, e - EGP, ? - incomplete
	RPKI validation codes: V valid, I invalid, N Not found
	
	     Network          Next Hop            Metric LocPrf Weight Path
	 * i 2.2.2.0/24       2.2.2.1                  0    100      0 200 ?
	 * i 200.0.0.0        2.2.2.1                  0    100      0 200 ?
	 * i 201.0.0.0        2.2.2.1                  0    100      0 200 ?
	RTA#
	
	! veja que agora temos prefixos.
	! isso siginifica que temos ao menos um peering Open.
	!
	! veja que logo no inicio das linhas após o "*", temos um "i".
	! isso significa que o prefixo foi aprendido via um peer iBGP.
	!
	! o simbolo "?" no final da linha de cada prefixo, indica que
	! a origem do mesmo é incompleta, geralmente significa que foi
	! injetado no BGP via redistribuição.

### Tarefa 04
1. Acessar a console do ***RTC***.

2. Verificar neigbors IPv4 no ***RTC***, execute o comando ***show ip bgp summary***, compare com a saída de exemplo abaixo:

>
	RTC#show ip bgp summary
	BGP router identifier 2.2.2.1, local AS number 200
	BGP table version is 4, main routing table version 4
	3 network entries using 420 bytes of memory
	3 path entries using 240 bytes of memory
	1/1 BGP path/bestpath attribute entries using 144 bytes of memory
	0 BGP route-map cache entries using 0 bytes of memory
	0 BGP filter-list cache entries using 0 bytes of memory
	BGP using 804 total bytes of memory
	BGP activity 6/0 prefixes, 6/0 paths, scan interval 60 secs
	
	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	2.2.2.2         4          100       6       8        4    0    0 00:03:10        0
	RTC#

2. Verificar neigbors IPv4/IPv6 no ***RTC***, execute o comando ***show bgp all summary***, compare com a saída de exemplo abaixo:

>
	RTC#show bgp all summary
	For address family: IPv4 Unicast
	BGP router identifier 2.2.2.1, local AS number 200
	BGP table version is 4, main routing table version 4
	3 network entries using 420 bytes of memory
	3 path entries using 240 bytes of memory
	1/1 BGP path/bestpath attribute entries using 144 bytes of memory
	0 BGP route-map cache entries using 0 bytes of memory
	0 BGP filter-list cache entries using 0 bytes of memory
	BGP using 804 total bytes of memory
	BGP activity 6/0 prefixes, 6/0 paths, scan interval 60 secs
	
	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	2.2.2.2         4          100       7       9        4    0    0 00:03:39        0
	
	For address family: IPv6 Unicast
	BGP router identifier 2.2.2.1, local AS number 200
	BGP table version is 4, main routing table version 4
	3 network entries using 492 bytes of memory
	3 path entries using 312 bytes of memory
	1/1 BGP path/bestpath attribute entries using 144 bytes of memory
	0 BGP route-map cache entries using 0 bytes of memory
	0 BGP filter-list cache entries using 0 bytes of memory
	BGP using 948 total bytes of memory
	BGP activity 6/0 prefixes, 6/0 paths, scan interval 60 secs
	
	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	2::2            4          100       0       0        1    0    0 00:10:07 Idle
	RTC#
	
	! Veja que a saída do comando exibe os preficos IPv4 e IPv6 simultaneamente.

3. Verificar a configuração BGP no ***RTC***, execute o comando ***show running-config view full | section bgp***, compare com a saída de exemplo abaixo:

>
	RTC#show running-config view full | section bgp
	router bgp 200
	 bgp log-neighbor-changes
	 no bgp default ipv4-unicast
	 neighbor 2::2 remote-as 100
	 neighbor 2.2.2.2 remote-as 100
	 !
	 address-family ipv4
	  redistribute connected
	  neighbor 2.2.2.2 activate
	 exit-address-family
	 !
	 address-family ipv6
	  redistribute connected
	  neighbor 2::2 activate
	 exit-address-family
	RTC#
	
	! o comando é ligeiramente diferente aqui, conta com "view full",
	! essa parte server para mostra a configuração completa em um roteador
	! utilizando a função "parser view".
	
	! o "| section bgp" filtra a saída do comando para mostrar apenas a
	! parte referente a configuração BGP.

### Tarefa 05
1. Acessar a console do ***RTB***.

2. Ativar os peers IPv6 no roteador ***RTB***, execute o script abaixo:

>
	configure terminal
	
	router bgp 100
	address-family ipv6
	neighbor 1::1 activate
	neighbor 2::1 activate
	end
	write

3. Verificar a configuração BGP no ***RTB***, execute o comando ***show running-config | section bgp***, compare com a saída de exemplo abaixo:

>
	RTB#show running-config | section bgp
	router bgp 100
	 bgp log-neighbor-changes
	 neighbor 1::1 remote-as 100
	 neighbor 2::1 remote-as 200
	 neighbor 1.1.1.1 remote-as 100
	 neighbor 2.2.2.1 remote-as 200
	 !
	 address-family ipv4
	  no neighbor 1::1 activate
	  no neighbor 2::1 activate
	  neighbor 1.1.1.1 activate
	  neighbor 2.2.2.1 activate
	 exit-address-family
	 !
	 address-family ipv6
	  neighbor 1::1 activate
	  neighbor 2::1 activate
	 exit-address-family
	RTB#
	
	! veja que temos uma coisa estranha na cofiguração.
	
	! agora temos os peers separados em "address-family".
	! os peers IPv6 estão listados no "address-family ipv4", isso não
	! deveria estar acontecendo.
	
	! o comportamento padrão do BGP é inserir todos os peers no IPv4.
4. Verificar neigbors IPv4/IPv6 no ***RTB***, execute o comando ***show bgp all summary***, compare com a saída de exemplo abaixo:

>
	RTB#show bgp all summary
	For address family: IPv4 Unicast
	BGP router identifier 2.2.2.2, local AS number 100
	BGP table version is 4, main routing table version 4
	3 network entries using 420 bytes of memory
	3 path entries using 240 bytes of memory
	1/1 BGP path/bestpath attribute entries using 144 bytes of memory
	1 BGP AS-PATH entries using 24 bytes of memory
	0 BGP route-map cache entries using 0 bytes of memory
	0 BGP filter-list cache entries using 0 bytes of memory
	BGP using 828 total bytes of memory
	BGP activity 6/0 prefixes, 6/0 paths, scan interval 60 secs
	
	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	1.1.1.1         4          100      10      11        4    0    0 00:06:13        0
	2.2.2.1         4          200      11      10        4    0    0 00:06:14        3
	
	For address family: IPv6 Unicast
	BGP router identifier 2.2.2.2, local AS number 100
	BGP table version is 1, main routing table version 1
	3 network entries using 492 bytes of memory
	3 path entries using 312 bytes of memory
	1/0 BGP path/bestpath attribute entries using 144 bytes of memory
	1 BGP AS-PATH entries using 24 bytes of memory
	0 BGP route-map cache entries using 0 bytes of memory
	0 BGP filter-list cache entries using 0 bytes of memory
	BGP using 972 total bytes of memory
	BGP activity 6/0 prefixes, 6/0 paths, scan interval 60 secs
	
	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	1::1            4          100       0       0        1    0    0 never    Idle
	2::1            4          200       5       2        1    0    0 00:00:37        3
	RTB#
	
	! apesar da configuração do passo #3 estar com algumas coisas que podem
	! ser melhoradas.

	! podemos ver que agora temos peering do RTB com os roteadores RTA e RTC,
	! tanto no IPv4 quando no IPv6.

	! apesar do peering IPv6 com o RTA ainda estar em "never"
	! mas isso se deve a configuração incompleta no RTA.

5. Verificar os prefixos IPv4 no ***RTB***, execute o comando ***sh bgp ipv4 unicast***, compare com a saída de exemplo abaixo:

>
	RTB#sh bgp ipv4 unicast 
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
	RTB#

6. Verificar os prefixos IPv6 no ***RTB***, execute o comando ***sh bgp ipv6 unicast***, compare com a saída de exemplo abaixo:

>
	RTB#sh bgp ipv6 unicast 
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
	RTB#
	
	! veja que agora o RTB também conta com prefixos IPv6 no BGP.

