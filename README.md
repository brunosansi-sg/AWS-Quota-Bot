# AWS-Quota-Bot: Sistema de Automação de Cotas e Instâncias AWS

## 📋 Visão Geral

AWS-Quota-Bot é um sistema 100% automatizado para gerenciar cotas de serviço AWS, solicitar aumentos de cota e provisionar instâncias em múltiplas regiões e sub-contas com mínima intervenção humana.

O sistema utiliza as APIs da AWS para detectar automaticamente as cotas atuais, solicitar aumentos quando necessário, monitorar o status das solicitações e, após a aprovação, provisionar instâncias EC2 conforme configurado.

## ✨ Funcionalidades Principais

### 1. Auto-detecção e Configuração
- **Detecção automática de AMIs** para cada região, usando filtros para encontrar as mais recentes e adequadas
- **Descoberta automática de sub-contas** através da API Organizations
- **Configuração automática de IAM** para permissões necessárias
- **Validação de regiões** para verificar quais regiões estão disponíveis e habilitadas

### 2. Gerenciamento Inteligente de Cotas
- **Análise automática de cotas atuais** para vCPUs On-Demand e Spot
- **Solicitação progressiva e inteligente** - começando com 64 vCPUs, com fallback para 32 se necessário
- **Detecção de padrões de rejeição e suspensão** com estratégias de mitigação
- **Monitoramento contínuo do status de solicitações** com atualizações em tempo real

### 3. Provisionamento de Instâncias
- **Criação automática de instâncias c6a.16xlarge** após aprovação de cotas
- **Distribuição inteligente** entre sub-contas para maximizar recursos
- **Configuração automática** com script de inicialização personalizado
- **Paralelismo controlado** para respeitar limites de API

### 4. Interface Interativa
- **Dashboard moderno com tema escuro** e elementos visuais atraentes
- **Animações e transições suaves** entre telas
- **Indicadores de progresso** para operações de longa duração
- **Gráficos em tempo real** mostrando status de cotas e instâncias

## 🔧 Requisitos

- Python 3.6+
- Boto3
- AWS CLI configurado com credenciais válidas
- Permissões AWS para acessar Service Quotas, EC2 e Organizations (se usando sub-contas)

## 🚀 Instalação

### Para Windows (Recomendado)

1. Extraia todos os arquivos do AWS-Quota-Bot em uma pasta
2. Clique com o botão direito em `setup-windows.bat` e selecione "Executar como administrador"
3. Siga as instruções na tela para completar a configuração
4. Execute `start-quota-bot.bat` para iniciar o bot

Para instruções detalhadas, consulte [windows-setup-guide.md](windows-setup-guide.md).

### Instalação Manual

```bash
# Extrair os arquivos em uma pasta
# Abrir prompt de comando como administrador
cd caminho\para\aws-quota-bot

# Instalar dependências
pip install -r requirements.txt

# Configurar credenciais AWS (se ainda não configuradas)
aws configure

# Executar o bot
python run.py
```

## 🎮 Utilização

### Modo Interativo (UI)

```bash
# Iniciar com a interface de usuário
start-quota-bot.bat

# Iniciar e executar automaticamente o processo de solicitação
start-quota-bot.bat --auto-start
```

### Modo Headless

```bash
# Executar em modo headless (sem interface)
start-quota-bot.bat --headless

# Especificar arquivo de configuração personalizado
start-quota-bot.bat --config C:\path\to\config.json
```

## 🖥️ Interface do Usuário

A interface de linha de comando interativa oferece várias telas para gerenciar e monitorar o processo:

1. **Dashboard Principal**: Visão geral das contas, cotas e atividades recentes
2. **Detalhes de Cotas**: Informações detalhadas sobre cotas em todas as regiões
3. **Instâncias Provisionadas**: Lista de instâncias criadas pelo sistema
4. **Configurações**: Opções para personalizar o comportamento do bot
5. **Logs do Sistema**: Log completo de todas as atividades

## ⚙️ Configuração

O sistema usa um arquivo JSON para armazenar configurações. Por padrão, ele é salvo como `aws-quota-bot-state.json` no diretório atual. Você pode especificar um arquivo diferente usando a opção `--config`.

Configurações padrão:

```json
{
  "target_vcpus": 64,
  "fallback_vcpus": 32,
  "instance_type": "c6a.16xlarge",
  "max_parallel_requests": 5,
  "request_retry_interval": 15,
  "sub_account_access_role": "OrganizationAccountAccessRole"
}
```

## 🔒 Segurança

O AWS-Quota-Bot requer permissões para:

- Ler e solicitar alterações de Service Quotas
- Listar, criar e gerenciar instâncias EC2
- Listar contas em Organizations (se usando sub-contas)
- Assumir funções IAM em sub-contas (se aplicável)

O sistema configura automaticamente as permissões IAM necessárias durante a inicialização.

## ⚡ Prevenção de Problemas

### Evitando Suspensão de Conta
- Análise de padrões de comportamento
- Solicitações progressivas (32 vCPUs → 64 vCPUs)
- Detecção de sinais de risco
- Implementação de períodos de espera estratégicos

### Lidando com Limites de API
- Implementação de backoff exponencial
- Agendamento inteligente de solicitações
- Paralelismo controlado entre regiões
- Cache de resultados para reduzir chamadas desnecessárias

## 📊 Exemplo de Uso

```
# 1. Execute setup-windows.bat como administrador para configurar
# 2. Execute start-quota-bot.bat para iniciar o bot
# 3. No menu interativo:
#    - Selecione opção 1 para ver o dashboard detalhado
#    - Pressione 'r' para iniciar o processo automático
#    - Selecione opção 2 para ver o status detalhado das cotas
#    - Selecione opção 3 para ver as instâncias provisionadas
```

## 📄 Licença

Este projeto está licenciado sob a licença MIT.

---

⭐ AWS-Quota-Bot - Desenvolvido para facilitar o gerenciamento e provisionamento de recursos AWS em larga escala.
