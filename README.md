# Configuração de Banco de Dados na Azure

Este documento reúne resumos, anotações e dicas práticas para criação e configuração de instâncias de **Banco de Dados SQL** na plataforma **Microsoft Azure**, com foco no uso por desenvolvedores backend.

---

## 📌 Sumário

- [1. Criando um Banco de Dados SQL](#1-criando-um-banco-de-dados-sql)
- [2. Configurando Acesso e Firewall](#2-configurando-acesso-e-firewall)
- [3. Conectando a Aplicação Backend](#3-conectando-a-aplicação-backend)
- [4. Dicas Gerais e Considerações de Custo](#4-dicas-gerais-e-considerações-de-custo)
---

## 1. Criando um Banco de Dados SQL

### Passos:

1. Acesse o [Portal Azure](https://portal.azure.com).
2. No menu lateral, pesquise por **Banco de Dados SQL** e clique em **Criar**.
3. Preencha os campos:
   - **Nome do banco de dados**: `db-backend`
   - **Servidor SQL**: Criar novo
     - Nome do servidor: `sql-backend-lab`
     - Login de administrador: `adminuser`
     - Senha: segura e anotada
     - Região: próxima da sua aplicação (ex: Brasil Sul)
4. Camada de preço:
   - Escolha "Basic" ou "Desenvolvimento/Teste"
5. Clique em **Revisar e Criar** → **Criar**

> ✅ Após alguns minutos, o banco estará disponível.

---

## 2. Configurando Acesso e Firewall

### 🔓 Liberando o IP para acesso externo:

1. Acesse o recurso do **servidor SQL** (não o banco em si).
2. Vá em **Firewalls e redes virtuais**.
3. Clique em **+ Adicionar IP do cliente atual** ou insira manualmente seu IP.
4. Clique em **Salvar**.

### 🛠 Conectando com ferramentas

- **Servidor**: `sql-backend-lab.database.windows.net`
- **Usuário**: `adminuser`
- **Senha**: (a que você definiu)
- **Banco de Dados**: `db-backend`
- Ferramentas recomendadas:
  - Azure Data Studio ✅
  - DBeaver, Beekeeper Studio ou SSMS

---

## 3. Conectando a Aplicação Backend

### 📄 Variáveis de ambiente (.env)

```env
DB_HOST=sql-backend-lab.database.windows.net
DB_PORT=1433
DB_USER=adminuser
DB_PASS=suaSenhaSegura
DB_NAME=db-backend
DB_DIALECT=mssql
```

## 4. Dicas Gerais e Considerações de Custo

### 💸 Custos

- O **Azure SQL Database** não possui camada gratuita.
- Use a camada **Basic** ou **Desenvolvimento/Teste** para economizar.
- Para evitar cobranças indesejadas:
  - Pause ou exclua instâncias quando não estiver utilizando.
  - Monitore o uso pelo portal da Azure ou pelo Azure Cost Management.

---

### 🛡️ Segurança

- Utilize **senhas fortes** e rotacione periodicamente.
- **Nunca** deixe o acesso liberado para "Todos os IPs" por tempo prolongado.
- Ative **firewall** e permita acesso apenas de IPs conhecidos.
- Prefira sempre **conexões criptografadas (SSL)**:
  - Para Node.js, use `encrypt: true` na string de conexão.
  - Para ferramentas gráficas, ative "Usar SSL" ao conectar.

---

### 🧰 Ferramentas Úteis

- **Azure Data Studio** – Leve, multiplataforma e recomendado pela Microsoft.
- **DBeaver** – Open-source, ideal para gerenciar múltiplos SGBDs.
- **Beekeeper Studio** – Interface moderna e fácil de usar.
- **SSMS (SQL Server Management Studio)** – Oficial e completo, mas só para Windows.

---

### 🚀 Próximos Passos para Produção

- Automatizar a criação da infraestrutura com **Terraform**, **Bicep** ou **ARM Templates**.
- Configurar:
  - **Backups automáticos**
  - **Failover** para alta disponibilidade
  - **Auditoria** e alertas com Azure Monitor
- Monitorar desempenho com:
  - **Query Performance Insight**
  - **SQL Analytics**

