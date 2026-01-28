# Pipeline de Dados do NeuroGuard

## Visão Geral

O pipeline de dados do NeuroGuard processa sinais neurológicos desde a aquisição até a geração de insights e predições.

## Etapas do Pipeline

### 1. Aquisição de Dados

**Formato**: Arquivos EDF (European Data Format)

**Localização**: `data/raw/`

**Características**:
- Múltiplos canais EEG
- Metadados (frequência de amostragem, canais, etc.)
- Informações do paciente (anonimizadas)

### 2. Leitura e Validação

**Módulo**: `src/data_engineering/edf_reader.py`

**Processos**:
- Validação do formato do arquivo
- Extração de metadados
- Verificação de integridade
- Conversão para formato interno

**Saída**: Objeto Raw do MNE-Python

### 3. Pré-processamento

**Módulo**: `src/data_engineering/preprocessing.py`

**Etapas**:

1. **Remoção de DC**
   - Remove componente de corrente contínua
   - Centraliza o sinal em zero

2. **Filtro Notch**
   - Remove ruído de linha (50/60 Hz)
   - Configurável por região

3. **Filtro Passa-Banda**
   - Filtra frequências relevantes (1-40 Hz típico)
   - Remove artefatos de alta e baixa frequência

4. **Normalização**
   - Normalização z-score
   - Padronização entre canais

5. **Remoção de Artefatos**
   - Detecção de valores extremos
   - Interpolação ou remoção

**Saída**: Dados pré-processados

### 4. Engenharia de Características

**Módulo**: `src/data_engineering/feature_engineering.py`

**Tipos de Características**:

#### Domínio do Tempo
- Média, desvio padrão, variância
- RMS (Root Mean Square)
- Peak-to-peak
- Crest factor
- Skewness, Kurtosis
- Zero crossing rate
- Parâmetros de Hjorth (Activity, Mobility, Complexity)

#### Domínio da Frequência
- Potência espectral total
- Potência por banda (delta, theta, alpha, beta, gamma)
- Frequência dominante
- Frequência média
- Spectral edge frequency

#### Estatísticas
- Percentis (10, 25, 50, 75, 90)
- IQR (Interquartile Range)
- MAD (Median Absolute Deviation)

#### Entropia
- Shannon entropy
- Sample entropy
- Approximate entropy

#### Conectividade
- Correlação entre canais
- Coerência espectral

**Saída**: DataFrame com características extraídas

### 5. Análise Exploratória

**Módulo**: `src/analytics/exploratory.py`

**Análises**:
- Estatísticas descritivas
- Avaliação de qualidade
- Análise espectral
- Visualizações

**Saída**: Relatório de análise

### 6. Modelagem

**Módulos**: `src/ml/classical_models.py`, `src/ml/deep_learning.py`

**Modelos Disponíveis**:
- Random Forest
- Gradient Boosting
- SVM
- Logistic Regression
- CNN
- LSTM
- Transformer

**Saída**: Modelo treinado

### 7. Avaliação

**Módulo**: `src/ml/evaluation.py`

**Métricas**:
- Accuracy
- Precision, Recall, F1-score
- ROC AUC
- Matriz de confusão

**Saída**: Relatório de avaliação

## Armazenamento

### Estrutura de Diretórios

```
data/
├── raw/              # Dados brutos (EDF)
├── processed/        # Dados processados
│   ├── preprocessed/ # Dados pré-processados
│   ├── features/     # Características extraídas
│   └── visualizations/ # Gráficos e visualizações
└── download_dataset.py # Script de download
```

### Modelos

```
models/
├── checkpoints/      # Checkpoints durante treinamento
└── final/           # Modelos finais treinados
```

## Segurança no Pipeline

1. **Anonimização**: Aplicada antes do processamento
2. **Criptografia**: Dados sensíveis criptografados
3. **Auditoria**: Logs de todas as operações
4. **Validação**: Verificação de integridade em cada etapa

## Exemplo de Uso

```python
from src.data_engineering.edf_reader import EDFReader
from src.data_engineering.preprocessing import Preprocessor
from src.data_engineering.feature_engineering import FeatureEngineer

# 1. Carregar dados
reader = EDFReader()
raw_data = reader.load_edf("data/raw/sample.edf")

# 2. Pré-processar
preprocessor = Preprocessor(
    sampling_freq=raw_data.info["sfreq"],
    notch_freq=50.0
)
processed_data = preprocessor.process(raw_data)

# 3. Extrair características
df = reader.get_data_as_dataframe(processed_data)
feature_engineer = FeatureEngineer(sampling_freq=raw_data.info["sfreq"])
features = feature_engineer.extract_all_features(df)

# 4. Salvar
features.to_csv("data/processed/features.csv", index=False)
```

## Otimizações

- **Processamento Paralelo**: Múltiplos canais processados em paralelo
- **Cache**: Resultados intermediários podem ser cacheados
- **Lazy Loading**: Dados carregados sob demanda
- **Batch Processing**: Processamento em lotes para grandes volumes

## Monitoramento

- Logs estruturados em cada etapa
- Métricas de performance
- Alertas para erros ou anomalias
- Rastreabilidade completa

