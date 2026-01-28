# Segurança e Conformidade do NeuroGuard

## Visão Geral

O NeuroGuard implementa medidas rigorosas de segurança e privacidade para garantir conformidade com LGPD (Lei Geral de Proteção de Dados) e HIPAA (Health Insurance Portability and Accountability Act).

## Princípios de Segurança

### 1. Privacidade por Design

- Anonimização automática de dados sensíveis
- Minimização de dados coletados
- Criptografia end-to-end
- Acesso baseado em roles

### 2. Segurança em Repouso

- Criptografia AES-256-GCM para dados sensíveis
- Armazenamento seguro de chaves
- Backups criptografados

### 3. Segurança em Trânsito

- HTTPS/TLS para comunicação
- Autenticação de API
- Tokens JWT com expiração

### 4. Auditoria

- Logs de todas as operações sensíveis
- Rastreabilidade completa
- Alertas de segurança

## Módulos de Segurança

### Anonimização (`src/security/anonymization.py`)

#### Funcionalidades

1. **Anonimização de Identificadores**
   - Hash SHA-256 de IDs de pacientes
   - Pseudonimização reversível (com mapping)

2. **Generalização de Dados**
   - Datas: Remoção de dia (ano-mês apenas)
   - Idades: Categorização em faixas
   - Localização: Generalização geográfica

3. **K-Anonimização**
   - Garante que cada registro não pode ser distinguido de pelo menos k-1 outros
   - Generalização de quasi-identificadores

4. **Remoção de PHI**
   - Nomes
   - Emails
   - Telefones
   - Endereços
   - CPF/RG/SSN
   - Datas de nascimento

#### Exemplo de Uso

```python
from src.security.anonymization import DataAnonymizer

anonymizer = DataAnonymizer()

anonymized_data = anonymizer.anonymize_patient_data(
    data=df,
    patient_id_column="patient_id",
    date_columns=["birth_date", "exam_date"],
    sensitive_columns=["email", "phone"]
)
```

### Criptografia (`src/security/encryption.py`)

#### Funcionalidades

1. **Criptografia de Dados**
   - AES-256-GCM (recomendado)
   - Fernet (alternativa)
   - Suporte a strings, arrays numpy, DataFrames

2. **Criptografia de Arquivos**
   - Criptografia de arquivos completos
   - Descriptografia sob demanda

3. **Gerenciamento de Chaves**
   - Geração segura de chaves
   - Derivação de chaves a partir de senhas (PBKDF2)
   - Armazenamento seguro

#### Exemplo de Uso

```python
from src.security.encryption import DataEncryptor

# Criar criptografador
encryptor = DataEncryptor(algorithm="AES-256-GCM")

# Criptografar dados
encrypted = encryptor.encrypt_data("dados sensíveis")

# Descriptografar
decrypted = encryptor.decrypt_data(encrypted, output_type="str")

# Salvar chave
encryptor.save_key("keys/encryption.key")
```

## Conformidade com LGPD

### Direitos dos Titulares

1. **Acesso**: Pacientes podem acessar seus dados
2. **Correção**: Dados podem ser corrigidos
3. **Exclusão**: Dados podem ser deletados
4. **Portabilidade**: Dados podem ser exportados
5. **Revogação**: Consentimento pode ser revogado

### Medidas Implementadas

- Anonimização de dados pessoais
- Logs de acesso e modificação
- Política de retenção de dados
- Consentimento explícito

## Conformidade com HIPAA

### Regras de Segurança

1. **Administrative Safeguards**
   - Políticas e procedimentos
   - Treinamento de pessoal
   - Gestão de acessos

2. **Physical Safeguards**
   - Controle de acesso físico
   - Segurança de dispositivos
   - Backup e recuperação

3. **Technical Safeguards**
   - Controle de acesso
   - Auditoria
   - Integridade de dados
   - Transmissão segura

### PHI (Protected Health Information)

Dados considerados PHI:
- Nomes
- Datas (exceto ano)
- Números de telefone
- Endereços
- Números de identificação (SSN, CPF, etc.)
- Informações médicas

**Todos os PHI são removidos ou anonimizados antes do processamento.**

## Boas Práticas

### 1. Gestão de Chaves

- Nunca commitar chaves no código
- Usar variáveis de ambiente
- Rotação periódica de chaves
- Armazenamento seguro (HSM, Key Vault)

### 2. Logs e Auditoria

- Logs estruturados
- Retenção adequada
- Análise de logs
- Alertas de segurança

### 3. Acesso

- Princípio do menor privilégio
- Autenticação multi-fator
- Sessões com timeout
- Revogação de acesso

### 4. Backup e Recuperação

- Backups regulares
- Backups criptografados
- Testes de recuperação
- Plano de continuidade

## Checklist de Segurança

- [ ] Dados anonimizados antes do processamento
- [ ] Criptografia de dados sensíveis
- [ ] Chaves armazenadas de forma segura
- [ ] Logs de auditoria habilitados
- [ ] Acesso controlado por roles
- [ ] HTTPS/TLS configurado
- [ ] Backups criptografados
- [ ] Política de retenção definida
- [ ] Treinamento de pessoal
- [ ] Testes de segurança regulares

## Incidentes de Segurança

Em caso de incidente:

1. **Identificação**: Detectar e identificar o incidente
2. **Contenção**: Isolar sistemas afetados
3. **Eradicação**: Remover ameaças
4. **Recuperação**: Restaurar sistemas
5. **Lições Aprendidas**: Documentar e melhorar

## Recursos Adicionais

- [LGPD - Lei Geral de Proteção de Dados](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [HIPAA - Health Insurance Portability and Accountability Act](https://www.hhs.gov/hipaa/index.html)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

