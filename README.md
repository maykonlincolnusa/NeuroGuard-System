# NeuroGuard

Sistema avançado de análise e tratamento de doenças neurológicas utilizando inteligência artificial e engenharia de dados.

## 🎯 Visão Geral

O NeuroGuard é uma plataforma completa que auxilia médicos no diagnóstico e tratamento de doenças neurológicas através de:
- Processamento avançado de sinais EEG/EDF
- Análise exploratória de dados neurológicos
- Modelos de machine learning clássicos e deep learning
- Segurança e conformidade com LGPD/HIPAA
- API RESTful para integração com sistemas hospitalares

## 🏗️ Arquitetura

```
NeuroGuard/
├── data/              # Dados brutos e processados
├── src/               # Código fonte principal
│   ├── data_engineering/  # Pipeline de dados
│   ├── analytics/        # Análises exploratórias
│   ├── ml/               # Modelos de ML/DL
│   ├── security/         # Segurança e privacidade
│   └── api/              # API REST
├── notebooks/         # Jupyter notebooks para análise
├── models/            # Modelos treinados
├── docs/              # Documentação técnica
└── tests/             # Testes unitários e de integração
```

## 🚀 Instalação

### Pré-requisitos
- Python 3.9+
- pip ou poetry

### Setup

1. Clone o repositório:
```bash
git clone <repository-url>
cd NeuroGuard
```

2. Crie um ambiente virtual:
```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate  # Windows
```

3. Instale as dependências:
```bash
pip install -r requirements.txt
```

4. Configure as variáveis de ambiente:
```bash
cp .env.example .env
# Edite .env com suas configurações
```

## 📊 Uso

### Processamento de Dados EDF
```python
from src.data_engineering.edf_reader import EDFReader
from src.data_engineering.preprocessing import Preprocessor

reader = EDFReader()
data = reader.load_edf("data/raw/sample.edf")

preprocessor = Preprocessor()
processed_data = preprocessor.process(data)
```

### Treinamento de Modelos
```python
from src.ml.deep_learning import NeuroGuardModel

model = NeuroGuardModel()
model.train(X_train, y_train)
predictions = model.predict(X_test)
```

### API REST
```bash
uvicorn src.api.app:app --reload
```

Acesse a documentação em: `http://localhost:8000/docs`

## 🔒 Segurança

- Criptografia de dados sensíveis
- Anonimização de pacientes
- Conformidade com LGPD e HIPAA
- Logs auditáveis

## 📚 Documentação

Consulte a pasta `docs/` para documentação detalhada:
- `architecture.md` - Arquitetura do sistema
- `data_pipeline.md` - Pipeline de dados
- `security_and_compliance.md` - Segurança e conformidade
- `roadmap.md` - Roadmap de desenvolvimento

## 🧪 Testes

```bash
pytest tests/
```

## 📝 Licença

Veja o arquivo LICENSE para mais detalhes.

## 👥 Contribuindo

Contribuições são bem-vindas! Por favor, leia as diretrizes de contribuição antes de submeter PRs.

## 📧 Contato

Para questões e suporte, abra uma issue no repositório.

