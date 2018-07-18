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
*router bgp 100*|Cria/configura um processo BGP com o ASN 100
*bgp default ipv4-unicast*|Inclui os neighbors BGP automaticamente no *address-family ipv4* e ativa os mesmos, mesmo que o neighbor seja IPv6
*syncronization*|Sincroniza o FIB do BGP com a RIB criada por um IGP
*neighbor 1.1.1.1 remote-as 100*|Cria peering BGP no ASN 100 com o roteador 1.1.1.1
*address-family [ipv4][ipv6]*|Acessa a configuração BGP de uma versão especifica do protocolo IP
*neighbor 1.1.1.1 active*|Ativa uma conexão de peering BGP com o roteador 1.1.1.1
*neighbor 1.1.1.1 next-hop-self*|Subititui o endereço do *next hop* dos prefixos enviados ao peer 1.1.1.1, com o endereço ip local

## ATIVIDADES DOS LAB 01
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

8. Verificar neigbors IPv4 no RTA, execute o comando *show ip bgp summary*, compare com a saida de exemplo abaixo:

>
	RTA#show ip bgp summary
	BGP router identifier 1.1.1.1, local AS number 100
	BGP table version is 1, main routing table version 1
	
	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	1.1.1.2         4          100       0       0        1    0    0 never    Idle
	RTA#


