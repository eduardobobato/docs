# Especificação do Sistema de Gerenciamento de Telas Indoor

## Objetivo do Sistema
O sistema tem como objetivo gerenciar a exibição de mídias (imagens e vídeos) em telas indoor.  
As telas exibem playlists de mídias de forma contínua (loop infinito), controladas por clientes que possuem permissões e recursos definidos pelo administrador.

---

## Perfis de Usuário

### Administrador
- Responsável por gerenciar clientes e configurações globais.
- Possui todas as permissões do sistema sem limitações.
- Pode atuar como um cliente, mas com acesso total e irrestrito.

### Cliente
- Gerencia mídias, playlists, telas e subordinados.
- Possui limitações conforme os parâmetros definidos pelo administrador (quantidade de telas, armazenamento, validade de contrato, etc.).

---

## Funcionalidades do Administrador

### 1. Gestão de Clientes
Cadastro completo com os seguintes atributos:
- Nome, CNPJ/CPF, e-mail, telefone e endereço.
- Data de início e fim de contrato.
- Número máximo de telas permitidas.
- Limite de armazenamento (MB ou GB).

**Operações:**
- Cadastrar, listar, editar, excluir e detalhar clientes.

### 2. Configurações do Sistema
- Definir parâmetros globais como:
  - Tamanho máximo de arquivo de mídia.
  - Formatos aceitos (extensões permitidas para vídeo e imagem).
  - Tempo máximo padrão de exibição.
- Possibilidade de adicionar configurações futuras (ex: modo de notificação, limites adicionais).

### 3. Gerenciamento Geral
- Visualizar todas as mídias, playlists e telas dos clientes.
- Realizar operações administrativas sem restrições de limite.

---

## Funcionalidades do Cliente

### 1. Gestão de Mídias
Cadastro e gerenciamento de mídias (imagens e vídeos):
- Upload com validação de formato e tamanho.
- Para imagens:
  - Definição do tempo de exibição (em segundos).
  - Seleção do tipo de transição (fade, slide, zoom etc.).
- Para vídeos:
  - Pré-visualização e controle de duração.

**Funções:** listar, visualizar, editar e excluir.

### 2. Gestão de Playlists
Gerenciamento de playlists de exibição:
- Criação, edição e exclusão.
- Adição e ordenação de mídias (drag-and-drop).
- Definir nome, descrição e visualização prévia.
- Vinculação a uma ou mais telas.

### 3. Gestão de Telas
- Cadastro de telas com nome, local e observação opcional.
- Seleção da playlist vinculada à tela.
- Geração automática de login e senha únicos
  para autenticação no aplicativo da tela.

**Funções e Comandos:**
- Listar e buscar telas.
- Editar e mudar playlist.
- Enviar comandos remotos:
  - Reiniciar tela.
  - Fazer logout do dispositivo.
- Excluir tela e visualizar histórico de ações (logs).

### 4. Gestão de Subordinados
- Cadastrar usuários subordinados (nome, e-mail, permissões).
- O cliente só pode conceder permissões que ele também possui.
- As mesmas regras se aplicam ao administrador com seus próprios subordinados.

**Funções:** listar, editar, excluir, detalhar subordinados.

---

## Regras de Negócio
- Cada cliente respeita os limites configurados:
  - Armazenamento total.
  - Número máximo de telas.
- Uploads são bloqueados quando o limite de armazenamento é atingido.
- Contratos expirados bloqueiam o acesso até renovação.
- Telas possuem credenciais únicas para autenticação.
- O administrador pode suspender, reativar ou ajustar limites a qualquer momento.

---

## Interface e Experiência do Usuário (UX)

### Layout e Navegação
- **Dashboard** com cards informativos e atalhos principais.
- **Sidebar fixa** com ícones e labels curtos.
- **Listagens** com filtros dinâmicos e busca em tempo real.
- **Pré-visualização direta** de imagens e vídeos nas listagens.
- **Feedback visual** (loading, sucesso, erro) em todas as ações.
- **Interface responsiva** para desktop, tablet e mobile.

### Identidade Visual
- Estilo moderno e minimalista com ênfase em legibilidade e fluidez.

#### Paleta de Cores
- **Verde pastel:** botões primários, confirmações e elementos de destaque.  
- **Azul escuro:** fundos, cabeçalhos e menus laterais.

#### Modos de Exibição
- **Dark Mode**
  - Fundo cinza-azulado escuro com texto branco.
  - Verde pastel mantido como cor de destaque.
- **White Mode**
  - Fundo branco com textos e menus em azul escuro.
  - Verde pastel aplicado em botões e elementos interativos.

**Extras:**
- Alternância entre dark e white mode em tempo real.
- Preferência de modo salva por usuário.
- Feedback coerente entre modos (hover, foco, seleção).

---

## Estrutura de Módulos

| Módulo | Funcionalidades |
|--------|-----------------|
| Autenticação | Login, logout, recuperação de senha |
| Administração | Clientes, limites, configurações globais |
| Mídias | Upload, listagem, edição, exclusão |
| Playlists | Criação, ordenação, vinculação |
| Telas | Cadastro, comandos (reiniciar/logout) |
| Subordinados | Gestão de permissões e papéis |
| Relatórios | Consumo, status de telas, logs |

---

## Considerações Técnicas

### Stack Recomendada
- **Frontend:** React ou Vue.js com Material Design.
- **Backend:** Java Spring Boot ou Node.js (NestJS).
- **Banco de Dados:** PostgreSQL.
- **Armazenamento de Mídia:** S3 (ou similar).
- **Autenticação:** JWT.
- **Infraestrutura:** Docker + Kubernetes.

### Integração com Telas (Aplicativo Player)
- Conexão via WebSocket para envio de comandos:
  - Atualizar playlist.
  - Reiniciar ou realizar logout remoto.
- Sincronização automática de playlists e mídias associadas.

---

## Futuros Melhoramentos (Backlog)
- Módulo de relatórios e estatísticas detalhadas.
- Suporte a notificações push para telas.
- Integração com APIs externas de anúncios.
- Controle de versionamento de mídias e playlists.

---

© 2025 — Especificação completa do Sistema de Gerenciamento de Telas Indoor.
