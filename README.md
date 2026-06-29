# Challenge GoodWe+FIAP

## 1. Identificação
**Equipe:** 19
**Integrantes:** Ellen Alves - RM570885, Rai da Silva - RM572070, Pietra Fanticelli - RM573229, Marco Junio - RM570834.

## 2. Contexto e Problema
O mercado brasileiro de veículos elétricos cresceu 89% entre 2023 e 2024. Esse volume gera uma pressão sobre condomínios e edifícios corporativos que precisam gerir a infraestrutura de recarga de forma justa. O **EV ChargeOps** surge para resolver a opacidade dos modelos de rateio tradicionais, implementando uma camada de gestão que captura dados reais de consumo via API GoodWe e protocolo OCPP, organizando a cobrança por kWh individualizado.

## 3. Arquitetura da Solução
A plataforma é estruturada em quatro camadas fundamentais:

*   **Camada Física (Hardware):** Carregador **GoodWe HCA G2** com autenticação RFID e sensores de efeito Hall para medição precisa de energia.
*   **Camada de Conectividade:** Transmissão via Wi-Fi/LAN utilizando a **API SEMS Portal** da GoodWe e protocolos abertos como OCPP.
*   **Camada de Aplicação:** Middleware responsável pela ingestão de dados, motor de regras de negócio e persistência em banco de dados **PostgreSQL**.
*   **Camada de Apresentação:** Aplicativo para o motorista e painel de controle para o gestor condominial.

## 4. Modelo de Rateio Dinâmico
Diferente do rateio fixo, nossa proposta utiliza uma **Matriz de Custos** baseada na origem do fluxo de energia:
*   **Equação Base:** $F_{i} = (E_{solar,i} \times C_{solar}) + (E_{rede,i} \times C_{rede}) + (E_{bateria,i} \times C_{bateria}) + T_{infra}$.
*   O sistema socializa custos em horários de pico através de uma tarifa média ponderada, garantindo justiça financeira mesmo em casos de flutuação de carga da bateria.

## 5. Implementações de Inteligência Artificial
Para otimizar a gestão, o sistema conta com três frentes de IA:
1.  **O Síndico Virtual (NLP/RAG):** Traduz dados técnicos de faturamento em relatórios gerenciais em linguagem natural.
2.  **Modelo Preditivo de Demanda:** Modelos de regressão para antecipar demanda e otimizar o uso de baterias e energia solar.
3.  **Segurança Operacional:** Algoritmos de detecção de anomalias para identificar fraudes ou falhas de medição.

## 6. Tratamento de Exceções
*   **Queda de Conexão:** O acumulador de hardware do carregador mantém os dados locais; o sistema recupera o consumo via cálculo de delta matemático assim que o sinal retorna.
*   **Múltiplos Veículos:** A segregação lógica entre `usuarios` e `rfids` no banco de dados permite que cada morador seja cobrado individualmente, mesmo compartilhando a mesma vaga.

## 7. Implementação e Prototipação Técnica

A implementação do **EV ChargeOps** segue uma estratégia de **Gêmeo Digital**, utilizando **Python (Pandas)** para simular payloads JSON da API SEMS e validar o motor de regras antes da integração física. 
O **middleware** será construído para realizar a ingestão de dados via *polling* agendado, tratando a persistência em um banco **PostgreSQL** que segrega logicamente `usuários` e `tags RFID` para viabilizar o faturamento em vagas compartilhadas. 

Para garantir a resiliência, o protótipo inclui lógica de **Cálculo de Delta Matemático**, recuperando o consumo acumulado no hardware após períodos de instabilidade de rede. As frentes de **IA** serão prototipadas utilizando arquiteturas **RAG (NLP)** para o **"Síndico Virtual"** e modelos de **Isolation Forests** para a detecção de anomalias elétricas, assegurando que a transição do carregador residencial para o modelo coletivo ocorra com total segurança operacional e transparência financeira.

**Pontos-chave da prototipagem:**
*   **Simulação com Python:** Uso de scripts para mimetizar o comportamento da API e validar a integridade dos dados antes do deploy.
*   **Segregação de Entidades:** Separação entre moradores e cartões RFID no banco de dados para gerir o uso múltiplo de uma mesma infraestrutura.
*   **Resiliência de Dados:** Tratamento analítico para evitar perda de faturamento durante quedas de conexão.
  
Arquitetura de Banco de Dados: A importância de separar RFIDs de usuários para tratar o cenário de múltiplos veículos
.
Resiliência (Delta): Como o sistema lida com quedas de conexão recuperando dados do hardware
.
Aplicação de IA: O uso de técnicas específicas (RAG e Isolation Forests) para resolver dores do síndico e segurança

## 8. Diagrama Oficial
<img width="1536" height="2752" alt="Jornada_do_Dado_de_Carregamento" src="https://github.com/user-attachments/assets/e5aecd98-e3da-43f6-adb2-a7c204e9800f" />

---
**Fontes Consultadas:** ABVE, ANEEL (RN 1.000/2021), GoodWe SEMS Portal API, IBGE.

**Utilização de IA:** Centralização de pesquisas e dados no NotebookLM para criação de relatório e diagrama oficial.
