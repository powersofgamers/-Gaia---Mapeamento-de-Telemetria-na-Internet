# 🛰️ Gaia - Mapeamento de Telemetria na Internet

> “Assim como o Telescópio Espacial Gaia da ESA foi projetado para mapear as estrelas e decodificar a estrutura da nossa galáxia, o **Gaia* foi desenvolvido para escanear, mapear e dissecar a 'galáxia' de rastreadores, scripts de telemetria e coletores de dados ocultos na infraestrutura da Web.”*

O *Gaia* é uma ferramenta de inteligência e segurança cibernética (OSINT/Infosec) projetada para auditar a superfície de tráfego de aplicações web, identificando quais dados de telemetria e comportamento do usuário estão sendo coletados, por quem estão sendo processados e como mitigar essa exposição de forma cirúrgica.

---

## 🌌 A Importância do Gaia na Segurança da Informação

No cenário atual da segurança da informação, a telemetria comercial e os scripts de monitoramento de terceiros (Analytics, Pixels, WebSockets de gravação de sessão) tornaram-se vetores críticos em múltiplas vertentes:

1. *Privacidade e Governança de Dados (LGPD/GDPR):* Muitas aplicações capturam dados sensíveis e comportamentais de forma silenciosa. O Gaia atua como um auditor transparente para validar se um site está vazando dados de navegação para infraestruturas externas não autorizadas.
2. *Mitigação de Supply Chain Attacks:* Scripts de telemetria externos injetados dinamicamente em sites podem ser comprometidos na origem (como ataques do tipo Magecart). Mapear essas dependências com o *Scanner 1* é o primeiro passo para o endurecimento de superfícies (Attack Surface Management).
3. *Contramedidas e Sinkholing:* O *Scanner 2* não apenas expõe a estrutura física das requisições, mas traduz essa inteligência em regras acionáveis de mitigação local (Sinkholing via /etc/hosts), permitindo que operadores de segurança imunizem seus laboratórios e estações de trabalho contra coletas de dados invasivas.

---

## 🛠️ Arquitetura e Funcionamento
O ecossistema do Gaia é dividido em duas fases modulares e sequenciais de varredura:

[ Alvo: URL ]
│
├──> [ SCANNER 1: Mapeamento Superficial ]
│         │
│         └───> Identifica assinaturas (Regex) no HTML e scripts injetados.
│         └───> Salva relatório em: gaia_reports/scanner_1/
│
└──> [ SCANNER 2: Engenharia de Mitigação ]
│
└───> Força análise da infraestrutura, origens e payloads.
└───> Gera blocos de bloqueio/desativação (hosts/firewall).
└───> Salva relatório em: gaia_reports/scanner_2/


* *Scanner 1 (Mapeamento):* Executa uma requisição simulada com cabeçalhos de navegação comuns (User-Agent legítimo) para extrair e inspecionar a árvore do DOM. Ele busca por assinaturas conhecidas de grandes provedores de telemetria (Google Analytics, Meta Pixel, Hotjar, Clarity, TikTok, etc.).
* *Scanner 2 (Exploração e Desativação):* Processa os achados da primeira fase, contextualiza os endpoints de comunicação, documenta as origens corporativas dos coletores e constrói de forma automatizada regras de resolução nula (0.0.0.0 / 127.0.0.1) específicas para o alvo, prontas para aplicação em firewalls ou arquivos de roteamento locais.

---

## 🚀 Instalação e Requisitos

### Pré-requisitos
Certifique-se de ter o *Python 3.x* instalado em seu sistema operacional (como Arch Linux, Debian ou Windows).

### 1. Preparar o Repositório
git clone
mkdir -p ~/labs/gaia
cd ~/labs/gaia
# Salve o código principal como gaia.py nesta pasta


### 2. Instalar as Dependências
O Gaia utiliza bibliotecas consagradas e leves para requisições de rede e processamento estrutural de tags.

pip install requests beautifulsoup4


## 🕹️ Como Executar
Para iniciar o mapeamento de um alvo web, basta chamar o script principal através do seu terminal de comandos:
python gaia.py


### Fluxo de Uso:
 1. O programa exibirá o banner estilizado do *Gaia*.
 2. O ambiente preparará automaticamente a árvore de diretórios de relatórios.
 3. Forneça o link ou domínio desejado quando solicitado na tela:
   text
   Digite a URL do site que deseja mapear (ex: exemplo.com): adblock-tester.com
   
   
## 📊 Como Obter e Intermediar os Resultados
Após a finalização do processo, o Gaia organiza seus artefatos de inteligência de forma isolada e estruturada:
bash
gaia_reports/
├── scanner_1/
│   └── mapping_adblock-tester.com.txt       # Diagnóstico de telemetrias ativas
└── scanner_2/
    └── exploitation_adblock-tester.com.txt  # Regras de desativação e arquitetura


### Interpretando os Relatórios:
 * *Relatório do Scanner 1 (/scanner_1):* Ideal para auditorias de conformidade e triagem rápida. Indica se o alvo possui rastreadores ativos e quais marcas comerciais estão operando nele.
 * *Relatório do Scanner 2 (/scanner_2):* Contém a engenharia estrutural do tráfego. No final deste documento, ele gera dinamicamente uma sintaxe de bloqueio limpa:
   text
   # [Regras de Bloqueio geradas pelo Gaia para o seu arquivo de Hosts]
   127.0.0.1    google-analytics.com
   0.0.0.0      google-analytics.com
   127.0.0.1    connect.facebook.net
   
   
### Aplicando a Desativação no seu Acesso Local:
Para injetar as contramedidas simuladas pelo Gaia e mitigar o tráfego de telemetria a nível de sistema operacional na sua máquina:
 1. Abra o arquivo de hosts como superusuário (root/administrador):
   * *No Linux (Arch Linux / Debian):* /etc/hosts
   * *No Windows:* C:\Windows\System32\drivers\etc\hosts
 2. Copie as linhas fornecidas na seção final do relatório do *Scanner 2* e anexe-as ao fim do arquivo de hosts.
 3. Salve o arquivo. A partir deste instante, o seu tráfego local em direção a esses coletores será desviado para um buraco negro (Sinkhole), desativando por completo a telemetria do site sob sua perspectiva de rede.
Nota: Este software foi desenvolvido para fins estritamente educacionais, de pesquisa em segurança cibernética e auditoria de privacidade de dados.
