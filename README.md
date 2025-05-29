# FastAPI Deployment with CI/CD, Kubernetes and ArgoCD

Este projeto demonstra a implementação de uma aplicação FastAPI com um pipeline completo de CI/CD, incluindo testes automatizados, containerização com Docker, orquestração com Kubernetes e implantação contínua usando ArgoCD.

Aplicação foi feita a criação dos arquivos e organização

## 📋 Descrição do Projeto

A aplicação FastAPI contém três endpoints:
- `/` - Retorna uma mensagem "Hello World"
- `/square/{x}` - Deve retornar o quadrado de um número (corrigido no projeto)
- `/double/{x}` - Retorna o dobro de um número

O projeto inclui:
- Testes automatizados com pytest
- Pipeline CI/CD com GitHub Actions
- Containerização com Docker
- Configurações Kubernetes usando Kustomize
- Implantação contínua com ArgoCD

## 🛠️ Pré-requisitos

- Python 3.8+
- Docker
- kubectl
- Kind (Kubernetes in Docker)
- ArgoCD CLI (opcional)

## 🚀 Configuração Inicial

1. Clone o repositório:
   ```bash
   git clone https://github.com/Alexssandro49/AtividadePratica
   cd AtividadePratica
   ```

2. Crie e ative um ambiente virtual:
   ```bash
   python -m venv venv
   source venv/bin/activate  # Linux/Mac
   # ou 
   venv\Scripts\activate    # Windows
   ```

3. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```

## 🧪 Executando os Testes

```bash
pytest
```

## 🏃 Executando a Aplicação Localmente

```bash
uvicorn main:app --reload
```

Acesse: http://localhost:8000

## 🐳 Construindo e Executando com Docker

Construa a imagem:
```bash
docker build -t fastapi-app .
```

Execute o container:
```bash
docker run -p 8000:8000 fastapi-app
```

## ☸️ Configuração do Kubernetes

1. Crie um cluster local com Kind:
   ```bash
   kind create cluster --config kind-config.yaml
   ```

2. Verifique se o cluster está rodando:
   ```bash
   kubectl cluster-info
   ```

## 🔄 Configuração do ArgoCD

1. Instale o ArgoCD no cluster:
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

2. Acesse a interface do ArgoCD:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```
   Acesse: https://localhost:8080

3. Faça login (senha padrão pode ser obtida com):
   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
   ```

## 📦 Implantação com ArgoCD

1. Crie uma aplicação no ArgoCD apontando para este repositório e o diretório `k8s/`.

2. Sincronize a aplicação através da interface web ou CLI.

## 🔄 Fluxo de CI/CD

1. Push para o repositório dispara o workflow do GitHub Actions
2. Os testes são executados
3. Uma nova imagem Docker é construída e publicada no DockerHub
4. O arquivo kustomization.yaml é atualizado com a nova versão da imagem
5. ArgoCD detecta a mudança no repositório e sincroniza a aplicação no cluster

## 🌐 Acessando a Aplicação

Após a implantação, você pode acessar a aplicação com:
```bash
kubectl port-forward svc/fastapi-service 8000:8000
```
Acesse: http://localhost:8000

## 📌 Endpoints

- `GET /` - Mensagem de boas-vindas
- `GET /square/{x}` - Retorna o quadrado de x
- `GET /double/{x}` - Retorna o dobro de x

## 📄 Licença

Este projeto está licenciado sob a licença MIT.