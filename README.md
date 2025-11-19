# Impacta Streaming – Aula Prática Kafka (Integrated Data Platforms)

Curso: MBA em Data Engineering  
Disciplina: Integrated Data Platforms  
Aluno: Vinícius Julio  
RA: 2500392

Descrição
Projeto da aula prática 5: ambiente de streaming usando Apache Kafka, Docker Compose, JupyterLab e Kafka UI. Simula uma "rede social" para demonstrar um fluxo completo de eventos: geração, transporte, consumo e persistência.

Objetivos
- Gerar eventos simulados (producer).  
- Ingerir e transportar eventos via Kafka (cluster com 2 brokers).  
- Consumir eventos (consumer).  
- Persistir mensagens em arquivos CSV para análise posterior.

Arquitetura da solução
Executada via Docker Compose com os seguintes componentes:
- Zookeeper — coordenação do cluster Kafka.  
- Kafka1 e Kafka2 — dois brokers para simular um cluster básico.  
- Kafka UI — interface web para inspecionar tópicos, partições e mensagens.  
- JupyterLab — ambiente para executar notebooks de producer/consumer.

Fluxo resumido
1. notebooks/producer.ipynb gera eventos simulados e publica em um tópico Kafka.  
2. notebooks/consumer.ipynb consome mensagens do tópico e grava em arquivos CSV na pasta data.  
3. Kafka UI (http://localhost:8080) permite inspecionar tópicos e mensagens.  
4. JupyterLab (http://localhost:8888) contém os notebooks e scripts de suporte.

Estrutura de pastas
impacta_streaming/
├─ docker-compose.yml  
├─ README.md  
├─ notebooks/  
│  ├─ producer.ipynb  
│  ├─ consumer.ipynb  
│  ├─ run_jupyterlab.sh  
│  └─ requirements.txt  
├─ data/                # CSV gerados pelo consumer (criado em runtime)  
└─ imagens/             # evidências (prints / vídeo)

Como executar (resumido)
Pré-requisitos:
- Docker Desktop instalado (Windows) e daemon ativo.  
- Docker Compose (v2) disponível via `docker compose`.  
- Python 3.8+ (se for rodar notebooks localmente).

Passos:
1. Abrir/ligar o Docker Desktop e garantir que o daemon está ativo.  
2. No diretório do projeto:
   - docker compose pull
   - docker compose up -d
3. Acesse:
   - JupyterLab: http://localhost:8888 (o script run_jupyterlab.sh remove o token)  
   - Kafka UI: http://localhost:8080
4. No JupyterLab, execute notebooks:
   - producer.ipynb — gera e publica eventos.  
   - consumer.ipynb — consome e grava CSV em data/.

Observações e soluções de problemas comuns
- Aviso: a chave `version:` no docker-compose.yml é obsoleta — pode ser removida sem impacto funcional.  
- Conflito de nomes: se houver erro "container name is already in use", remova o container conflitante:
  - docker rm -f zookeeper
  - docker compose up -d
- Se usar WSL2, habilitar integração no Docker Desktop (Settings > Resources > WSL Integration).  
- Se executar os notebooks no host Windows, use bootstrap.servers `localhost:9092,localhost:9093`. Se dentro do container, use `kafka1:9092,kafka2:9093`.

Boas práticas implementadas
- Consumer grava CSV em lote para reduzir overhead.  
- Script run_jupyterlab.sh instala dependências e inicia JupyterLab sem token para facilitar acesso local durante a aula.  
- Volumes mapeiam notebooks para facilitar edição no host.

Requisitos
- Docker e Docker Compose (v2)  
- Python 3.8+ (opcional, para execução local dos notebooks)  
- Navegador web para JupyterLab e Kafka UI

Licença
Uso acadêmico.
