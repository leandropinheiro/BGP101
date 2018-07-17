# BGP 101
## LAB 01

### Objetivo
Neste lab o aluno será apresentado as configurações iniciais do ***BGP***, assim como aos comandos básicos de peering ***BGP Dual-stack*** (IP v4 e v6).

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

