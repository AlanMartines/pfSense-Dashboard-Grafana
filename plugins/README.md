# 🚀 Guia de Instalação e Uso dos Plugins

A maneira mais prática de instalar esses scripts no pfSense é através do pacote **Filer**.

Ao cadastrar um novo arquivo, especifique o caminho completo (ex: `/usr/local/bin/nome_do_plugin`) e cole o código correspondente.

> [!IMPORTANT]
> **Permissões de Execução:** Defina obrigatoriamente as permissões como **`0755`**. Por padrão, o sistema utiliza `0644`, que **NÃO** permite a execução dos scripts pelo Telegraf.

---

# 🛠️ telegraf_pfifgw.php (Script Principal)

*Este script consolida e substitui as versões antigas em Python (`telegraf_gateways.py`) e os scripts individuais de interface e gateway.*

Este é o motor de coleta do dashboard, responsável por extrair dados em tempo real diretamente das bibliotecas nativas do pfSense.

### 📊 Métricas de Interfaces:

* **Identificação:** Nome do sistema, Nome Amigável (Friendly Name) e Endereço MAC.
* **Rede:** Endereços IPv4/IPv6 e suas respectivas sub-redes.
* **Disponibilidade:** Status operacional em tempo real (Online, Offline, etc.).

### 🌐 Métricas de Gateways:

* **Conectividade:** IP de Monitoramento, IP de Origem e indicação de Gateway Padrão.
* **Performance:** Atraso (Delay/RTT), Desvio Padrão (Stddev) e Percentual de Perda (Loss).
* **Diagnóstico:** Status e Substatus detalhados (ex: Latency, Packetloss).

> **Diferencial Técnico:** Este script foi migrado de Python para **PHP Nativo**, permitindo chamadas diretas à função `return_gateways_status_text` do sistema (`/etc/inc/gwlb.inc`). Isso garante que o status exibido no Grafana seja **idêntico** ao status oficial do painel do pfSense, eliminando imprecisões.

---

# 📉 telegraf_unbound_lite.sh

#### *Nota: Este plugin não está em uso no meu sistema atual, mas permanece documentado por utilidade técnica.*

Ideal para quem utiliza o **Unbound DNS** e deseja monitorar apenas métricas essenciais de performance. Ele evita a coleta excessiva de dados, economizando espaço no banco InfluxDB e otimizando o processamento.

---

### 💡 Dica de Integração

Este conjunto de plugins é o que permite a **mágica do dashboard dinâmico**: o `telegraf_pfifgw.php` fornece os nomes das interfaces que serão rotulados como **WAN** ou **LAN** através das configurações que definimos no `additional_config.conf`.