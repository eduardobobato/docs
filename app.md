# Documentação do Projeto - UseVision Mobile

## Índice
1. [Visão Geral do Projeto](#visão-geral-do-projeto)
2. [Personas](#personas)
3. [Histórias de Usuário](#histórias-de-usuário)
4. [Regras de Negócio](#regras-de-negócio)
5. [Fluxos de Processo](#fluxos-de-processo)
6. [Glossário](#glossário)
7. [Anexos](#anexos)

---

## Visão Geral do Projeto

### Objetivo
Desenvolver um aplicativo mobile que funcione como uma vitrine digital, reproduzindo automaticamente uma playlist de mídias (vídeos e imagens) em looping contínuo, com sincronização automática de conteúdo e monitoramento de reprodução.

### Escopo
**Incluído:**
- Sistema de autenticação via API (sem cadastro de usuários)
- Player de mídia com reprodução em looping
- Download e armazenamento local de mídias
- Sincronização automática de playlist
- Monitoramento e envio de histórico de reprodução
- Sistema de comandos remotos para reinicialização
- Gerenciamento inteligente de cache local

**Excluído:**
- Sistema de cadastro de usuários
- Edição de conteúdo pelo usuário
- Configurações avançadas pelo usuário final

### Stakeholders
- **Product Owner:** Responsável pelas decisões de produto e priorização
- **Equipe de Desenvolvimento:** Desenvolvimento e manutenção do aplicativo
- **Usuários Finais:** Operadores de vitrines digitais que utilizam o dispositivo
- **Administradores:** Responsáveis pelo gerenciamento de conteúdo via API

---

## Personas

### Persona 1: Operador de Vitrine
- **Descrição:** Profissional responsável por configurar e manter dispositivos de vitrine digital em pontos de venda
- **Necessidades:** Sistema simples de autenticação, reprodução automática sem intervenção, monitoramento de status
- **Frustrações:** Aplicativos complexos, necessidade de intervenção manual constante, falhas de conectividade
- **Objetivos:** Manter vitrine funcionando 24/7 com conteúdo sempre atualizado

### Persona 2: Administrador de Conteúdo
- **Descrição:** Responsável pelo gerenciamento de conteúdo e campanhas promocionais via sistema central
- **Necessidades:** Controle remoto de conteúdo, monitoramento de reprodução, comandos de manutenção
- **Frustrações:** Falta de visibilidade sobre reprodução, dificuldade para atualizar conteúdo remotamente
- **Objetivos:** Gerenciar eficientemente múltiplas vitrines e campanhas promocionais

---

## Histórias de Usuário

### Épico 1: Autenticação e Sessão

#### HU001 - Login no Sistema
- **Descrição:** Como um operador de vitrine, eu quero fazer login no aplicativo, para que eu possa acessar o sistema de reprodução de mídias.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve ter uma tela de login com campos para credenciais
  - [ ] O login deve ser realizado via API externa
  - [ ] Não deve haver opção de cadastro de novos usuários
  - [ ] Após login bem-sucedido, o usuário deve ser redirecionado para o player
  - [ ] O token de autenticação deve ser armazenado localmente
  - [ ] O usuário deve permanecer logado até o token expirar (status 403)
- **Prioridade:** Alta
- **Estimativa:** 8 pontos
- **Dependências:** Nenhuma
- **Notas:** Implementar gerenciamento seguro de tokens

#### HU002 - Gerenciamento de Sessão
- **Descrição:** Como um operador de vitrine, eu quero que minha sessão seja mantida automaticamente, para que eu não precise fazer login constantemente.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve detectar automaticamente quando o token expira (status 403)
  - [ ] Quando o token expira, o usuário deve ser redirecionado para a tela de login
  - [ ] O aplicativo deve tentar renovar o token automaticamente quando possível
  - [ ] A sessão deve ser mantida mesmo após reinicialização do aplicativo
- **Prioridade:** Alta
- **Estimativa:** 5 pontos
- **Dependências:** HU001
- **Notas:** Implementar refresh token se disponível na API

### Épico 2: Reprodução de Mídia

#### HU003 - Player de Mídia
- **Descrição:** Como um operador de vitrine, eu quero que o aplicativo reproduza automaticamente uma playlist de mídias, para que a vitrine funcione sem intervenção manual.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve reproduzir vídeos e imagens em sequência
  - [ ] A reprodução deve ser em looping contínuo
  - [ ] Não deve haver delay ou travamento entre as mídias
  - [ ] As imagens devem ter tempo configurável de exibição em segundos
  - [ ] Deve haver tipos de transição configuráveis (suave ou padrão)
  - [ ] A reprodução deve continuar mesmo sem conexão com internet
- **Prioridade:** Alta
- **Estimativa:** 13 pontos
- **Dependências:** HU001
- **Notas:** Foco na performance e transições suaves

#### HU004 - Download e Cache de Mídias
- **Descrição:** Como um operador de vitrine, eu quero que as mídias sejam baixadas e armazenadas localmente, para que a reprodução continue mesmo sem internet.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve baixar automaticamente vídeos e imagens da API
  - [ ] Cada mídia deve ter um nome único para identificação
  - [ ] Mídias já baixadas não devem ser baixadas novamente
  - [ ] Mídias não utilizadas nas últimas 24h devem ser removidas automaticamente
  - [ ] O armazenamento deve ser otimizado para economizar espaço
- **Prioridade:** Alta
- **Estimativa:** 8 pontos
- **Dependências:** HU003
- **Notas:** Implementar sistema inteligente de cache

### Épico 3: Sincronização e Atualização

#### HU005 - Sincronização Automática de Playlist
- **Descrição:** Como um operador de vitrine, eu quero que a playlist seja atualizada automaticamente, para que o conteúdo sempre esteja atual.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve consultar a API a cada 10 minutos para verificar atualizações
  - [ ] Novas mídias devem ser baixadas automaticamente
  - [ ] Mídias removidas da playlist devem ser removidas do dispositivo
  - [ ] A sincronização deve funcionar em background
  - [ ] Falhas de conectividade não devem interromper a reprodução atual
- **Prioridade:** Alta
- **Estimativa:** 8 pontos
- **Dependências:** HU004
- **Notas:** Implementar retry automático em caso de falha

#### HU006 - Envio de Histórico de Reprodução
- **Descrição:** Como um administrador de conteúdo, eu quero receber o histórico de reprodução das mídias, para que eu possa monitorar o desempenho das campanhas.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve registrar início e fim de cada mídia reproduzida
  - [ ] Os dados devem ser enviados para API a cada 5 minutos
  - [ ] Todas as datas devem ser tratadas em GMT 0
  - [ ] Em caso de falha de envio, os dados devem ser salvos em arquivo local
  - [ ] Após envio bem-sucedido, o arquivo local deve ser limpo
  - [ ] O sistema deve tentar reenviar dados pendentes na próxima tentativa
- **Prioridade:** Média
- **Estimativa:** 8 pontos
- **Dependências:** HU003
- **Notas:** Implementar sistema robusto de persistência local

### Épico 4: Comandos Remotos

#### HU007 - Sistema de Comandos Remotos
- **Descrição:** Como um administrador de conteúdo, eu quero enviar comandos remotos para o dispositivo, para que eu possa realizar manutenções sem deslocamento.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve consultar API de comandos a cada 5 minutos
  - [ ] Deve enviar timestamp da última chamada bem-sucedida (GMT 0)
  - [ ] Na primeira execução, deve enviar sem data
  - [ ] Deve processar comandos de reinicialização do dispositivo Android
  - [ ] Comandos devem ser executados imediatamente após recebimento
- **Prioridade:** Média
- **Estimativa:** 5 pontos
- **Dependências:** HU001
- **Notas:** Implementar sistema seguro de comandos remotos

#### HU008 - Inicialização Automática e Sempre em Primeiro Plano
- **Descrição:** Como um operador de vitrine, eu quero que o aplicativo inicie automaticamente quando o dispositivo ligar e permaneça sempre em primeiro plano, para que a vitrine funcione sem intervenção manual.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve abrir automaticamente quando o dispositivo ligar
  - [ ] O aplicativo deve solicitar permissão para sobrepor outros apps
  - [ ] O aplicativo deve permanecer sempre em primeiro plano
  - [ ] A tela deve permanecer ativa durante a reprodução
  - [ ] O aplicativo deve executar como serviço em primeiro plano
- **Prioridade:** Alta
- **Estimativa:** 8 pontos
- **Dependências:** HU001
- **Notas:** Implementar Boot Receiver e sistema de overlay nativo

#### HU009 - Comando de Logout
- **Descrição:** Como um administrador de conteúdo, eu quero enviar um comando de logout para o dispositivo, para que eu possa forçar o usuário a sair da sessão e retornar à tela de login quando necessário.
- **Critérios de Aceitação:**
  - [ ] O aplicativo deve processar comando `LOGOUT` via API
  - [ ] Deve limpar token de autenticação armazenado
  - [ ] Deve parar todos os serviços em execução
  - [ ] Deve navegar para a tela de login
  - [ ] Deve exibir mensagem informativa sobre o logout
  - [ ] Deve preservar dados importantes (histórico pendente, cache)
- **Prioridade:** Alta
- **Estimativa:** 5 pontos
- **Dependências:** HU007, HU001, HU002
- **Notas:** Implementar limpeza completa de sessão e navegação segura

---

## Regras de Negócio

### RN001 - Autenticação Obrigatória
- **Descrição:** O aplicativo não permite cadastro de novos usuários. Apenas usuários pré-cadastrados no sistema podem fazer login.
- **Justificativa:** Controle de acesso e segurança do sistema de vitrine digital.
- **Exceções:** Nenhuma.
- **Relacionamento com Histórias:** HU001
- **Status:** Ativa

### RN002 - Gerenciamento de Token
- **Descrição:** O usuário permanece logado até o token de autenticação expirar. Quando o token expira (status 403), o usuário é redirecionado para a tela de login.
- **Justificativa:** Segurança e controle de sessão adequado.
- **Exceções:** Nenhuma.
- **Relacionamento com Histórias:** HU001, HU002
- **Status:** Ativa

### RN003 - Reprodução Contínua
- **Descrição:** Não pode haver delay ou travamento entre a troca de mídias. O sistema deve funcionar como uma playlist em looping contínuo.
- **Justificativa:** Experiência do usuário e funcionamento adequado da vitrine digital.
- **Exceções:** Nenhuma.
- **Relacionamento com Histórias:** HU003
- **Status:** Ativa

### RN004 - Cache Inteligente de Mídias
- **Descrição:** Cada mídia possui um nome único. Mídias já baixadas não são baixadas novamente. Mídias não utilizadas nas últimas 24 horas são removidas automaticamente do dispositivo.
- **Justificativa:** Otimização de espaço de armazenamento e eficiência de download.
- **Exceções:** Nenhuma.
- **Relacionamento com Histórias:** HU004
- **Status:** Ativa

### RN005 - Sincronização Periódica
- **Descrição:** O aplicativo consulta a API a cada 10 minutos para verificar atualizações na playlist de mídias.
- **Justificativa:** Manter conteúdo sempre atualizado sem sobrecarregar a API.
- **Exceções:** Em caso de falha de conectividade, o intervalo pode ser ajustado automaticamente.
- **Relacionamento com Histórias:** HU005
- **Status:** Ativa

### RN006 - Funcionamento Offline
- **Descrição:** O aplicativo deve continuar reproduzindo mídias mesmo quando desconectado da internet, utilizando o cache local.
- **Justificativa:** Garantir funcionamento contínuo da vitrine independente da conectividade.
- **Exceções:** Nenhuma.
- **Relacionamento com Histórias:** HU003, HU004
- **Status:** Ativa

### RN007 - Monitoramento de Reprodução
- **Descrição:** O aplicativo deve registrar início e fim de cada mídia reproduzida e enviar esses dados para API a cada 5 minutos.
- **Justificativa:** Monitoramento e análise de desempenho das campanhas promocionais.
- **Exceções:** Em caso de falha de envio, os dados devem ser persistidos localmente.
- **Relacionamento com Histórias:** HU006
- **Status:** Ativa

### RN008 - Tratamento de Datas GMT
- **Descrição:** Todas as datas e timestamps devem ser tratados em GMT 0 (UTC) para padronização global.
- **Justificativa:** Consistência de dados em diferentes fusos horários.
- **Exceções:** Nenhuma.
- **Relacionamento com Histórias:** HU006, HU007
- **Status:** Ativa

### RN009 - Sistema de Comandos Remotos
- **Descrição:** O aplicativo consulta API de comandos a cada 5 minutos, enviando timestamp da última chamada bem-sucedida. Na primeira execução, envia sem data.
- **Justificativa:** Permitir manutenção remota e controle do dispositivo.
- **Exceções:** Nenhuma.
- **Relacionamento com Histórias:** HU007
- **Status:** Ativa

### RN010 - Comando de Logout Remoto
- **Descrição:** O aplicativo deve processar comandos de logout remoto, limpando completamente a sessão do usuário e retornando à tela de login. Dados importantes (histórico pendente, cache) devem ser preservados.
- **Justificativa:** Controle de acesso e segurança, permitindo forçar logout quando necessário.
- **Exceções:** Dados de histórico pendente e cache de mídia devem ser preservados para próxima sessão.
- **Relacionamento com Histórias:** HU009
- **Status:** Ativa

### RN011 - Persistência de Dados em Falha
- **Descrição:** Em caso de falha no envio de dados (histórico ou comandos), as informações devem ser salvas em arquivo local e reenviadas na próxima tentativa bem-sucedida.
- **Justificativa:** Garantir que nenhum dado seja perdido devido a falhas de conectividade.
- **Exceções:** Nenhuma.
- **Relacionamento com Histórias:** HU006, HU007
- **Status:** Ativa

---

## Fluxos de Processo

### Fluxo 1: Processo de Login e Inicialização
```
1. Usuário abre o aplicativo
2. Sistema verifica se existe token válido
   - Se SIM: Vai para passo 8
   - Se NÃO: Continua processo
3. Sistema exibe tela de login
4. Usuário insere credenciais
5. Sistema envia dados para API de autenticação
6. API valida credenciais
   - Se INVÁLIDAS: Exibe erro e volta para passo 3
   - Se VÁLIDAS: Continua processo
7. Sistema armazena token localmente
8. Sistema inicia processo de sincronização de mídias
9. Sistema inicia reprodução da playlist
10. Sistema inicia processos em background (monitoramento, comandos)
```

### Fluxo 2: Processo de Sincronização de Mídias
```
1. Sistema consulta API de playlist (a cada 10 minutos)
2. API retorna lista atualizada de mídias
3. Sistema compara com mídias locais
4. Para cada nova mídia:
   - Verifica se já existe localmente (por nome único)
   - Se NÃO existe: Baixa mídia
   - Se existe: Pula download
5. Para mídias removidas da playlist:
   - Remove do dispositivo
6. Sistema atualiza playlist local
7. Sistema continua reprodução com nova playlist
8. Em caso de falha de conectividade:
   - Continua reprodução com playlist atual
   - Agenda nova tentativa de sincronização
```

### Fluxo 3: Processo de Reprodução de Mídias
```
1. Sistema carrega playlist local
2. Para cada mídia na playlist:
   - Verifica se mídia existe localmente
   - Se existe: Inicia reprodução
   - Se não existe: Pula para próxima mídia
3. Durante reprodução:
   - Registra timestamp de início (GMT 0)
   - Reproduz mídia (vídeo ou imagem com tempo configurado)
   - Aplica transição configurada (suave ou padrão)
   - Registra timestamp de fim (GMT 0)
4. Após reprodução:
   - Vai para próxima mídia sem delay
   - Se última mídia: Volta para primeira (looping)
5. Sistema continua processo indefinidamente
```

### Fluxo 4: Processo de Monitoramento e Envio de Dados
```
1. Sistema registra dados de reprodução em memória
2. A cada 5 minutos:
   - Coleta dados pendentes de envio
   - Envia para API de monitoramento (GMT 0)
3. API processa dados
   - Se SUCESSO: Limpa dados locais
   - Se FALHA: Salva dados em arquivo local
4. Sistema agenda próxima tentativa de envio
5. Em próxima tentativa:
   - Tenta enviar dados do arquivo local
   - Se sucesso: Remove arquivo local
   - Se falha: Mantém arquivo para próxima tentativa
```

### Fluxo 5: Processo de Comandos Remotos
```
1. Sistema consulta API de comandos (a cada 5 minutos)
2. Envia timestamp da última chamada bem-sucedida (GMT 0)
   - Primeira execução: Envia sem data
3. API retorna lista de comandos pendentes
4. Para cada comando:
   - Processa comando imediatamente
   - Se comando de reinicialização: Reinicia dispositivo Android
5. Sistema registra timestamp da chamada bem-sucedida
6. Em caso de falha de conectividade:
   - Salva dados em arquivo local
   - Agenda nova tentativa
```

### Fluxo 6: Processo de Limpeza de Cache
```
1. Sistema executa limpeza (diariamente)
2. Para cada mídia local:
   - Verifica última utilização
   - Se não utilizada há mais de 24h: Remove do dispositivo
3. Sistema atualiza lista de mídias locais
4. Sistema continua funcionamento normal
```

---

## Glossário

| Termo | Definição |
|-------|-----------|
| **API** | Interface de Programação de Aplicações - sistema que permite comunicação entre o aplicativo e servidores externos |
| **Cache Local** | Armazenamento temporário de mídias no dispositivo para reprodução offline |
| **GMT 0** | Greenwich Mean Time - fuso horário padrão (UTC) usado para padronização de datas |
| **Looping** | Reprodução contínua que reinicia automaticamente após o fim da playlist |
| **Mídia** | Arquivo de vídeo ou imagem que será reproduzido na vitrine |
| **Playlist** | Lista ordenada de mídias que serão reproduzidas sequencialmente |
| **Status 403** | Código de resposta HTTP que indica token de autenticação expirado |
| **Token** | Chave de autenticação que permite acesso ao sistema |
| **Transição** | Efeito visual aplicado na mudança entre mídias (suave ou padrão) |
| **Vitrine Digital** | Sistema de exibição de conteúdo promocional em dispositivos móveis |

---

## Anexos

### Anexo A: Especificações Técnicas das APIs
- **API de Autenticação:** Endpoint para login e validação de credenciais
- **API de Playlist:** Endpoint para consulta de mídias disponíveis
- **API de Monitoramento:** Endpoint para envio de histórico de reprodução
- **API de Comandos:** Endpoint para recebimento de comandos remotos

### Anexo B: Estrutura de Dados
- **Formato de Mídia:** Especificação de tipos de arquivo suportados
- **Estrutura de Playlist:** Formato JSON da resposta da API de playlist
- **Formato de Histórico:** Estrutura dos dados de monitoramento enviados
- **Formato de Comandos:** Especificação dos comandos remotos suportados

---

## Histórico de Versões

| Versão | Data | Autor | Descrição das Alterações |
|--------|------|-------|-------------------------|
| 1.0 | 2024-12-19 | Equipe de Desenvolvimento | Versão inicial da documentação |

---

## Contato e Responsabilidades

- **Product Owner:** Responsável pelas decisões de produto e priorização das funcionalidades
- **Scrum Master:** Responsável pela gestão do processo de desenvolvimento
- **Equipe de Desenvolvimento:** Responsável pela implementação das funcionalidades
- **Arquiteto de Software:** Responsável pela definição da arquitetura técnica

---

*Este documento deve ser mantido atualizado conforme o projeto evolui e novos requisitos são identificados.*
