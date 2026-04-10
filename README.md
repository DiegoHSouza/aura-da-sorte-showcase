# 🍀 Aura da Sorte | PWA & Resilient Lottery Ecosystem

[![Live Demo](https://img.shields.io/badge/Live_Demo-Disponível-success?style=for-the-badge)](https://auradasorte.com.br)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=white)
![TDD](https://img.shields.io/badge/Methodology-TDD-brightgreen?style=for-the-badge)

O **Aura da Sorte** é um Progressive Web App (PWA) de alta performance focado na gestão profissional de loterias, organização de bolões e oráculo inteligente de palpites.

Mais do que uma aplicação de sorteios, este projeto é um **case prático de arquitetura de software**, demonstrando como construir aplicações altamente resilientes, tolerantes a falhas de rede e com foco obsessivo na estabilidade da experiência do usuário (UX).

---

## 🚀 O Desafio Técnico

Aplicações web tradicionais falham silenciosamente quando a conexão de rede cai ou quando APIs de terceiros ficam instáveis. O objetivo arquitetural do Aura da Sorte foi construir um sistema que mantivesse 100% da usabilidade e integridade dos dados, independentemente do caos externo.

## 🧠 Soluções de Engenharia & Arquitetura

Para garantir confiabilidade de nível corporativo, apliquei padrões avançados de design de software:

### 1. Resiliência Offline-First & Sync Engine

A aplicação não depende de internet para funcionar. Toda a geração de jogos é persistida através de um motor de sincronização construído do zero sobre o `localStorage` e IndexedDB:

* **Background Syncing:** Dados salvos offline entram em uma fila (`PendingSync`).
* **Backoff Exponencial + Jitter:** Ao reconectar, o sistema tenta sincronizar com o Firebase usando espaçamento matemático de tempo para evitar o efeito manada (*thundering herd*) e não derrubar o banco de dados.
* **Dead-Letter Queue (DLQ):** Após atingir o limite de tentativas (`MAX_RETRIES`), falhas críticas são isoladas em uma DLQ local, impedindo loops infinitos. O usuário recebe um feedback visual amigável e pode forçar o reprocessamento manual com segurança.

### 2. Fallback Multi-Provider (Alta Disponibilidade)

O motor de geração de palpites baseados em astrologia consome APIs globais. Para mitigar indisponibilidades de terceiros, implementei uma esteira de resiliência:

* **Provider A** -> Falhou? -> **Provider B** assume instantaneamente.
* **Ambos falharam?** -> Um **Fallback Local** determinístico é acionado.
* **Resultado:** O SLA percebido pelo usuário é de virtualmente 100%.

### 3. Anti-Corruption Layer (ACL) & Segurança

Nenhum dado entra no ecossistema sem passar por uma rigorosa validação de domínio.

* Utilização da biblioteca **Zod** para criar esquemas estritos.
* *Fail Gracefully:* Payloads maliciosos ou estruturalmente corrompidos são interceptados na borda e descartados silenciosamente, protegendo a integridade do banco de dados e do estado do React.

### 4. Telemetria Estruturada

Para observabilidade em produção, a aplicação gera logs estruturados no padrão `[ORACLE_TELEMETRY]`, permitindo o monitoramento de *latência por provider*, *taxa de fallback* e *eventos da DLQ* diretamente em dashboards de cloud.

---

## 🛠️ Stack Tecnológica

* **Frontend & UI:** React, TypeScript, Tailwind CSS, PWA (Service Workers customizados).
* **BaaS & Backend:** Firebase (Firestore, Authentication, reCAPTCHA Enterprise).
* **Validação de Domínio:** Zod.
* **Qualidade & Testes:** Metodologia **TDD (Test-Driven Development)** garantindo que a lógica de negócios, a máquina de estados offline e as regras de fallback fossem validadas antes mesmo da interface gráfica existir. Suíte completa abrangendo testes unitários e testes E2E (*End-to-End*).

---

## 👨‍💻 Sobre o Desenvolvedor

A tecnologia deve existir para resolver problemas reais e agregar valor de negócio. Durante o desenvolvimento do **Aura da Sorte**, meu foco foi aliar código limpo (*Clean Code*) com visão de produto, entregando uma plataforma onde as regras de negócio são protegidas pela arquitetura e a conveniência do usuário está no centro das decisões técnicas.

Sinta-se à vontade para explorar a [versão em produção do Aura da Sorte](https://auradasorte.com.br) e conferir a performance na prática.

🔗 **[Acesse meu LinkedIn](https://www.linkedin.com/in/diego-h-souza/)** para batermos um papo sobre tecnologia, arquitetura de software e cultura de engenharia.
