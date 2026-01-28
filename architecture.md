# Arquitetura do NeuroGuard

## Visão Geral

O NeuroGuard é um sistema completo para análise e tratamento de doenças neurológicas, construído com arquitetura modular e escalável.

## Componentes Principais

### 1. Engenharia de Dados (`src/data_engineering/`)

#### EDF Reader (`edf_reader.py`)
- Leitura de arquivos EDF (European Data Format)
- Suporte a múltiplos canais EEG
- Conversão para formatos pandas/numpy
- Reamostragem e filtragem básica

#### Preprocessing (`preprocessing.py`)
- Remoção de componente DC
- Filtro notch (ruído de linha)
- Filtro passa-banda
- Normalização (z-score)
- Remoção de artefatos
- Segmentação de dados

#### Feature Engineering (`feature_engineering.py`)
- Características no domínio do tempo
- Características no domínio da frequência
- Características estatísticas
- Características de entropia
- Características de conectividade

### 2. Analytics (`src/analytics/`)

#### Exploratory Analysis (`exploratory.py`)
- Análise exploratória de dados
- Estatísticas por canal
- Avaliação de qualidade
- Análise espectral
- Visualizações (time series, spectrum, topomap)

### 3. Machine Learning (`src/ml/`)

#### Classical Models (`classical_models.py`)
- Random Forest
- Gradient Boosting
- SVM
- Logistic Regression
- Naive Bayes
- Tuning de hiperparâmetros
- Validação cruzada

#### Deep Learning (`deep_learning.py`)
- CNN (Convolutional Neural Network)
- LSTM (Long Short-Term Memory)
- Transformer
- Treinamento com PyTorch
- Suporte a GPU

#### Evaluation (`evaluation.py`)
- Métricas de avaliação
- Matriz de confusão
- Curvas ROC e Precision-Recall
- Relatórios completos

### 4. Segurança (`src/security/`)

#### Anonymization (`anonymization.py`)
- Anonimização de dados de pacientes
- Pseudonimização
- K-anonimização
- Remoção de PHI (Protected Health Information)
- Conformidade com LGPD e HIPAA

#### Encryption (`encryption.py`)
- Criptografia AES-256-GCM
- Criptografia Fernet
- Criptografia de arquivos
- Gerenciamento de chaves

### 5. API (`src/api/`)

#### FastAPI Application (`app.py`)
- API RESTful
- Documentação automática (Swagger/ReDoc)
- CORS configurável
- Logging estruturado

#### Routes (`routes.py`)
- Upload de arquivos EDF
- Pré-processamento
- Extração de características
- Análise exploratória
- Predições
- Anonimização

## Fluxo de Dados

```
Arquivo EDF
    ↓
EDF Reader → Raw Data
    ↓
Preprocessing → Processed Data
    ↓
Feature Engineering → Features
    ↓
ML Models → Predictions
    ↓
Evaluation → Metrics
```

## Segurança e Privacidade

1. **Anonimização**: Dados de pacientes são anonimizados antes do processamento
2. **Criptografia**: Dados sensíveis são criptografados em repouso
3. **Auditoria**: Logs de todas as operações sensíveis
4. **Conformidade**: LGPD e HIPAA compliant

## Escalabilidade

- **Processamento**: Suporte a processamento paralelo
- **Armazenamento**: Estrutura modular para diferentes backends
- **API**: Assíncrona e escalável com FastAPI
- **ML**: Suporte a GPU para deep learning

## Tecnologias

- **Python 3.9+**
- **MNE-Python**: Processamento de sinais neurológicos
- **PyTorch/TensorFlow**: Deep learning
- **scikit-learn**: Machine learning clássico
- **FastAPI**: API REST
- **Pandas/NumPy**: Manipulação de dados
- **Cryptography**: Segurança

## Próximos Passos

Ver `docs/roadmap.md` para detalhes sobre desenvolvimento futuro.

