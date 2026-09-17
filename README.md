# 🛠️ Central Administrativa Delta

O **Abertura de Chamados** é um ecossistema de gerenciamento de chamados de TI desenvolvido pela **Delta Cyber Security LTDA**. O sistema foi projetado para centralizar e otimizar as demandas técnicas da Prefeitura de Delta (Administração, Educação e Saúde), unindo eficiência operacional a rígidos padrões de cibersegurança.

## 🛡️ Segurança e Integridade (Core Cybersec)
Como um projeto focado em **Cybersecurity**, o sistema utiliza camadas de proteção críticas no Firebase:

* **Imutabilidade de Dados:** Implementamos a regra `allow delete: if false`, garantindo que nenhum chamado possa ser apagado. Isso mantém um histórico auditável e 100% íntegro para a prefeitura.
* **Rastreabilidade Total:** Validação obrigatória de **Nome e Sobrenome** em todos os formulários e registro automático de **data/hora** (timestamp) na captura e finalização dos chamados.
* **Isolamento de Acesso:** Regras de segurança que restringem a captura e resposta apenas ao técnico autenticado no chamado.

## 🚀 Funcionalidades Avançadas (v2.0)

* **🔄 Transferência de Chamados:** Capacidade de delegar ou transferir atendimentos entre os membros da equipe administrativa com segurança.
* **📄 Relatórios Auditáveis em PDF:** Geração de documentos oficiais com 8 colunas detalhadas, incluindo histórico de interações e soluções técnicas.
* **🔔 Alertas em Tempo Real:** Sistema de notificações visuais ("NOVA") e barras de SLA coloridas por urgência (Alta, Média, Baixa).
* **✅ Base de Conhecimento:** Exibição imediata da solução aplicada nos cards concluídos, facilitando consultas futuras.

## 👥 Equipe de Desenvolvimento (Delta Cyber Security)

* **Lucas** - Lead Cybersecurity & Backend Architecture
* **Ramon** - Fullstack Developer
* **Ezequias** - Sistemas de Informação (Uniube)
* **Jean** - Developer
* **Gustavo** - Engenharia da Computação (Uniube)

## 💻 Tecnologias Utilizadas

* **Frontend:** HTML5, CSS3 (Glassmorphism UI) e Bootstrap 5.
* **Banco de dados:** Firebase Firestore (NoSQL) para sincronização Realtime.
* **Anexos:** API própria em Flask (`server/app.py`) que salva os arquivos em disco na VPS — os anexos **não** vão para o Firebase (evita o custo de armazenamento). O Firestore guarda só os metadados (nome, tipo, tamanho, url).
* **Documentação:** jsPDF e AutoTable para geração de documentos oficiais.

## 🌐 Arquitetura & Deploy (VPS)

O site é servido pela **própria VPS** (nginx), e o Firebase é usado **apenas como banco de dados**. O nginx faz duas coisas: serve os estáticos de `public/` e faz **proxy de `/api` e `/uploads` para a API Flask** (`server/app.py`, em `127.0.0.1:5001`).

> ⚠️ Se faltarem os blocos `location /api/` e `location /uploads/` no nginx, o **envio e a exibição de anexos falham com 404** (o chat de texto continua funcionando, pois vai direto no Firestore).

**1. API de anexos (Flask):**
```bash
cd server
python3 -m venv venv && . venv/bin/activate
pip install -r requirements.txt
cp .env.example .env            # e preencha FIREBASE_CREDENTIALS_FILE
# coloque o JSON da service account do Firebase Admin dentro de server/
```
Para manter no ar, instale o serviço systemd de `server/delta-anexos.service` (instruções no topo do arquivo).

**2. nginx:** use `deploy/nginx.conf.example` como base (contém os blocos de proxy e o `client_max_body_size` necessário para uploads). Depois: `sudo nginx -t && sudo systemctl reload nginx`.

**3. Teste rápido:** `curl -i -X POST http://SEU_IP/api/chamados/teste/anexos` deve responder **401** (`{"erro":"Token de autenticação ausente."}`). Se responder **404**, o proxy do nginx ainda não está ativo.

---
*Gerenciado e Protegido por Delta Cyber Security LTDA.*