
## Lei LGPD

### Pontos Principais da LGPD para SaaS

- **Papéis Definidos**: Geralmente, o SaaS é o Operador (quem processa) e o cliente é o Controlador (quem decide o que fazer com o dado).

- **Consentimento Explícito**: A coleta de dados deve ter finalidade específica e clara, com consentimento explícito do usuário, especialmente em marketing.

- **Transparência**: Políticas de Privacidade e Termos de Uso devem ser claros, atualizados e detalhar quais dados são coletados e por que.

- **Segurança e Acesso**: Empresas devem garantir a segurança das informações para evitar vazamentos e permitir que titulares acessem ou excluam seus dados.


### Adequação no SaaS (Checklist)

- **Mapeamento de Dados**: Entender quais dados pessoais (nome, e-mail, IP, etc.) a plataforma coleta, onde armazena e quem tem acesso.

- **Atualização de Contratos**: Incluir cláusulas específicas de proteção de dados que definam a responsabilidade do SaaS como operador.

- **Segurança da Informação**: Adotar medidas técnicas (criptografia, controle de acesso) para proteger dados contra acessos não autorizados.

- **Política de Privacidade**: Criar ou atualizar a política de privacidade, informando de forma transparente o uso, compartilhamento e armazenamento dos dados.

- **Suboperadores**: Declarar se o SaaS utiliza terceiros (provedores de nuvem, ferramentas de e-mail) para processar os dados. 


### Tipos de Dados do Cidadão

- **Dados Pessoais**: Nome, CPF, data nascimento, endereço, histórico profissional e escolar. O currículo é composto majoritariamente por esses dados.

- **Dados Sensíveis**: São aqueles dados que podem gerar discriminação porque revelam: origem racial ou étnica, convicção religiosa, opinião política, saúde, Orientação sexual, Filiação sindical, dados genéticos, Tipo de deficiência. Se o seu currículo contiver, por exemplo, sua filiação sindical ou uma condição de saúde específica, ele passa a conter dados sensíveis por definição legal.

### Dados dos Cadastros:

**Candidato**:
	identificação: primeiro nome ou apelido
	localização: cidade, país (residência)
	contato: email, fone (celular)

	cover letter, resume

**Recrutador**:
	identificação: nome, cargo/função
	localização: cidade, país
	contato: email corporativo

	texto vaga

**Empresa**:
	identificação: nome fantasia, logo.jpg
	contato: domínio site, domínio email
	verificação: registro nacional.doc

	texto apresentação

* O Candidato/Recrutador deve consentir o armazenamento de dados pessoais e autorizar o envio de seus dados a candidatos / empresas
	ao efetuar cadastro (após login), deve consentir, ou não será permitido aplicar a vagas nem enviar mensagens

--------------------------------------------------------------------------------

Para adequar um SaaS tanto à **LGPD** quanto ao **GDPR**, você deve implementar um conjunto de funcionalidades técnicas que garantam a governança dos dados "por design".

Aqui está o resumo das features e procedimentos necessários, organizados por módulo de software:

---

## 1. Módulo de Gestão de Consentimento
O consentimento deve ser livre, informado e inequívoco. No GDPR, ele deve ser tão fácil de retirar quanto foi de dar.
* **Central de Preferências:** Um painel onde o usuário pode marcar/desmarcar finalidades específicas (ex: marketing, compartilhamento com parceiros, analytics).
* **Logs de Consentimento (Imutáveis):** Registro de *quem* consentiu, *quando*, *como* (qual versão dos termos) e *qual* foi o escopo.
* **Cookie Banner Granular:** Não use apenas "Aceito tudo". Permita que o usuário bloqueie cookies não essenciais antes que eles sejam carregados.

## 2. Direitos do Titular (Self-Service)
Automatizar esses pedidos evita gargalos operacionais e prazos estourados (lembre-se das 72 horas do GDPR).
* **Direito de Acesso e Portabilidade:** Botão para gerar um dump de todos os dados do usuário em formato **JSON** ou **CSV**.
* **Direito de Retificação:** Interface para correção imediata de dados incompletos ou inexatos.
* **Direito ao Esquecimento (Exclusão):** Fluxo de "Soft Delete" (desativação) seguido de "Hard Delete" (expurgo definitivo de DBs e Backups) ou anonimização irreversível.
* **Oposição ao Tratamento:** Opção para o usuário interromper o processamento de seus dados para fins específicos sem excluir a conta.

## 3. Segurança e Governança Técnica
Aqui entra a parte de infraestrutura e backend (especialmente relevante para quem trabalha com linguagens de alta performance).
* **Criptografia em Repouso e em Trânsito:** Implementação de TLS 1.3 e criptografia de discos/colunas sensíveis no banco de dados.
* **Pseudonimização:** Substituir campos identificáveis por identificadores artificiais em ambientes de staging ou analytics.
* **Data Retention Policy (Auto-purge):** Scripts que limpam automaticamente dados após o período de retenção definido (ex: excluir logs de acesso após 6 meses).
* **Logs de Auditoria de Acesso:** Registro de qual colaborador acessou quais dados de usuários e por quê.

## 4. Funcionalidades para "Decisões Automatizadas"
Especialmente importante se o seu SaaS usa IA para triagem ou scoring.
* **Human-in-the-loop:** Um fluxo que permite ao usuário solicitar que um humano revise uma decisão tomada automaticamente pelo sistema.
* **Explicabilidade:** Interface que explica, em linguagem simples, os critérios lógicos usados pelo algoritmo para chegar a um resultado sobre o usuário.

## 5. Documentação e Compliance (Admin Side)
* **DPIA (Data Protection Impact Assessment):** Funcionalidade para gerar relatórios de impacto sobre novos recursos que tratem dados sensíveis.
* **Registro de Operações (ROPA):** Um inventário digital dentro do seu sistema que mapeia o fluxo do dado: onde entra, onde é armazenado, quem acessa e quando é excluído.
* **Termos de Uso e Privacy Policy Dinâmicos:** Sistema de versionamento que obriga o usuário a aceitar novas versões antes de continuar usando o software após mudanças críticas.

---

### Checkpoint de Diferenciação
| Feature | Foco LGPD | Foco GDPR |
| :--- | :--- | :--- |
| **Portabilidade** | Exigida em formato legível. | Exigida em formato **interoperável**. |
| **Prazos** | Notificação em tempo "razoável". | Notificação de violação em **72h**. |
| **Base Legal** | Aceita "Legítimo Interesse" de forma ampla. | Muito restrita no uso de Legítimo Interesse. |
| **Representante** | Não exige representante na UE. | **Exige** se você coletar dados de europeus. |

---
> **Dica de Engenharia:** Se estiver usando **Linux Mint** para o desenvolvimento e **Rust/C++** no backend, aproveite ferramentas de análise estática para garantir que informações sensíveis não vazem em logs de erro (*core dumps*) ou fiquem presas em buffers de memória não limpos após o uso.

--------------------------------------------------------------------------------
--------------------------------------------------------------------------------

Para um SaaS compatível com a Lei Geral de Proteção de Dados Pessoais (LGPD) e o General Data Protection Regulation (GDPR), o foco não é só jurídico — é **funcionalidades concretas no sistema**.

Abaixo está um **resumo direto das features essenciais** que você precisa implementar.

---

# 1) Gestão de consentimento

* Coleta de consentimento claro (opt-in)
* Registro de prova:

  * timestamp
  * versão do termo
  * origem (IP/app/web)
* Tela para:

  * revogar consentimento
  * visualizar histórico

---

# 2) Base legal por operação

* Associar cada uso de dados a uma base legal:

  * consentimento
  * contrato
  * legítimo interesse, etc.
* Armazenar isso de forma rastreável

---

# 3) Direitos do titular (user self-service)

Endpoints ou UI para:

* Acesso aos dados (exportar tudo)
* Correção de dados
* Exclusão (“direito ao esquecimento”)
* Portabilidade (download estruturado, ex: JSON)
* Oposição ao tratamento
* Restrição de uso

---

# 4) Exclusão e retenção de dados

* Soft delete + hard delete
* Políticas automáticas de retenção
* Jobs para apagar dados expirados
* Anonimização quando exclusão total não for possível

---

# 5) Segurança (mínimo obrigatório)

* Criptografia:

  * em trânsito (TLS)
  * em repouso
* Controle de acesso (RBAC/ABAC)
* Hash seguro de senhas (bcrypt/argon2)
* Proteção contra acesso indevido

---

# 6) Auditoria e logs

* Log de:

  * acesso a dados pessoais
  * alterações
  * consentimentos
* Logs imutáveis (ou com trilha de integridade)
* Capacidade de auditoria

---

# 7) Data breach (incidente)

* Sistema para:

  * detectar vazamentos
  * registrar incidentes
* Capacidade de:

  * notificar autoridades
  * notificar usuários afetados

---

# 8) Privacy by Design & Privacy by Default

* Coletar **apenas o mínimo necessário**
* Configurações padrão com maior privacidade
* Campos opcionais claramente indicados

---

# 9) Mapeamento de dados (data inventory)

* Saber:

  * quais dados você coleta
  * onde estão armazenados
  * por quanto tempo
  * com quem compartilha

(geralmente implementado como documentação + metadata no sistema)

---

# 10) Controle de terceiros (integrações)

* Listar e controlar:

  * APIs externas
  * provedores (ex: cloud, analytics)
* Garantir contratos adequados (DPA)
* Possibilidade de desativar integrações

---

# 11) Transferência internacional

* Identificar onde os dados estão (região/país)
* Suportar:

  * segregação por região (ex: EU vs BR)
  * controle de fluxo de dados

---

# 12) Interface de transparência (privacy UI)

* Página de privacidade clara
* Dashboard para o usuário ver:

  * dados armazenados
  * permissões
  * consentimentos

---

# 13) Decisões automatizadas (se houver)

* Explicar decisões (ex: scoring, ranking)
* Permitir revisão humana

---

# 14) Gestão de DPO / Encarregado

* Canal de contato visível
* Registro de solicitações (tickets)
* SLA para resposta

---

# 15) DPIA / Relatório de impacto (nível sistema)

* Documentar:

  * fluxos de dados
  * riscos
  * medidas mitigadoras

(não é só código, mas o sistema deve permitir extrair essas informações)

---

# 16) Multi-tenant com isolamento

* Separação clara de dados entre clientes
* Evitar vazamento entre tenants

---

# 17) Versionamento de políticas

* Versionar:

  * termos de uso
  * políticas de privacidade
* Associar usuário à versão aceita

---

# Conclusão prática

Se você implementar:

* Consentimento rastreável
* Direitos do usuário automatizados
* Logs e auditoria
* Segurança forte
* Exclusão/retensão
* Transparência

Você cobre **~90% do necessário para LGPD + GDPR** em um SaaS.

--------------------------------------------------------------------------------
--------------------------------------------------------------------------------

Abaixo está uma **arquitetura técnica de referência** para um SaaS compatível com LGPD + GDPR, cobrindo modelo de dados, APIs e fluxos principais.

---

# 1) Modelo de dados (tabelas principais)

### `users`

```sql
id (pk)
email (unique)
password_hash
created_at
updated_at
deleted_at (nullable)
```

---

### `personal_data`

Armazena dados pessoais desacoplados do usuário (facilita anonimização)

```sql
id (pk)
user_id (fk)
data_type (ex: name, phone, address)
value (encrypted)
created_at
updated_at
```

---

### `consents`

```sql
id (pk)
user_id (fk)
consent_type (ex: marketing, analytics)
status (granted | revoked)
policy_version
ip_address
user_agent
created_at
revoked_at (nullable)
```

---

### `legal_basis`

```sql
id (pk)
operation (ex: email_marketing, billing)
basis (consent | contract | legitimate_interest)
description
```

---

### `data_processing_log`

Log de uso de dados pessoais

```sql
id (pk)
user_id (fk)
operation
data_type
legal_basis_id (fk)
performed_by (system | admin_id)
created_at
```

---

### `audit_logs`

```sql
id (pk)
actor_id
action
resource_type
resource_id
metadata (json)
created_at
```

---

### `data_requests`

Solicitações do titular

```sql
id (pk)
user_id (fk)
type (access | delete | correction | portability)
status (pending | processing | completed)
request_payload (json)
response_payload (json)
created_at
completed_at
```

---

### `breach_incidents`

```sql
id (pk)
title
description
severity
detected_at
reported_at
status
```

---

### `third_parties`

```sql
id (pk)
name
purpose
data_shared (json)
country
active (boolean)
```

---

# 2) APIs essenciais

### Consentimento

```http
POST /consents
GET /consents
PATCH /consents/{id}/revoke
```

---

### Direitos do usuário

```http
GET /me/data             # acesso
DELETE /me               # exclusão
PATCH /me                # correção
GET /me/export           # portabilidade (JSON/CSV)
POST /me/restrict        # restrição de uso
```

---

### Data requests (workflow interno)

```http
POST /data-requests
GET /data-requests/{id}
```

---

### Auditoria

```http
GET /audit-logs
```

---

### Incidentes

```http
POST /incidents
GET /incidents
```

---

# 3) Fluxos principais

## A) Registro de usuário

1. Criar `users`
2. Criar `personal_data`
3. Registrar consentimentos (`consents`)
4. Log em `audit_logs`

---

## B) Coleta de consentimento

1. Mostrar política (versionada)
2. Registrar:

   * versão
   * timestamp
   * IP
3. Salvar em `consents`

---

## C) Uso de dados

Sempre que usar dados pessoais:

1. Verificar base legal (`legal_basis`)
2. Registrar em `data_processing_log`

---

## D) Direito de acesso

1. Buscar:

   * `users`
   * `personal_data`
   * `consents`
   * logs relevantes
2. Gerar JSON estruturado
3. Retornar via `/me/export`

---

## E) Direito de exclusão

1. Marcar `deleted_at`
2. Remover/anonymizar:

   * `personal_data` → substituir por hash/null
3. Manter logs (sem identificação direta)

---

## F) Retenção automática

* Job periódico:

```pseudo
for record in personal_data:
  if expired(policy):
    anonymize(record)
```

---

## G) Data breach

1. Criar registro em `breach_incidents`
2. Identificar usuários afetados
3. Exportar lista para notificação

---

# 4) Segurança (implementação técnica)

* Criptografia:

  * `value` em `personal_data` com AES-256
* Senhas:

  * Argon2 ou bcrypt
* Tokens:

  * JWT com expiração curta
* Acesso:

  * RBAC (roles: user, admin, dpo)

---

# 5) Multi-tenant (SaaS)

Adicionar:

```sql
tenant_id (em todas as tabelas)
```

* Isolamento lógico (ou físico se necessário)
* Queries sempre filtradas por tenant

---

# 6) Versionamento de políticas

Tabela:

```sql
policies
id
version
content
created_at
```

Relacionar com `consents.policy_version`

---

# 7) Observabilidade e compliance

* Centralizar logs (ex: ELK, OpenSearch)
* Alertas:

  * acesso incomum a dados
  * falhas de autorização
* Métricas:

  * número de requests de dados
  * tempo de resposta (SLA)

---

# Conclusão estrutural

Essa arquitetura implementa diretamente:

* Consentimento rastreável
* Direitos do titular automatizados
* Auditoria completa
* Segurança e minimização
* Governança de dados

--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
