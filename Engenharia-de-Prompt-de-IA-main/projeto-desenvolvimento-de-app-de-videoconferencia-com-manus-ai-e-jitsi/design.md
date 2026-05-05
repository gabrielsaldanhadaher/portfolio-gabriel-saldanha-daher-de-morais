# IT Connect - Design da Interface Mobile

## Visão Geral

O IT Connect é uma plataforma mobile para conectar profissionais de TI, permitindo a criação e gerenciamento de comunidades de programação com videoconferência integrada via Jitsi Meet. A interface segue os padrões iOS (Apple Human Interface Guidelines) com orientação portrait (9:16) e suporte a uso com uma mão.

---

## Lista de Telas

### Autenticação
1. **Splash Screen** - Logo e branding do app
2. **Login/Sign Up** - Tela de autenticação (email/senha ou OAuth)
3. **Perfil Inicial** - Configuração básica do perfil do usuário

### Navegação Principal (Tab Bar)
1. **Home** - Feed de comunidades e projetos recomendados
2. **Explorar** - Busca e descoberta de comunidades por categoria
3. **Minhas Comunidades** - Lista de comunidades do usuário
4. **Perfil** - Configurações e dados do usuário

### Categorias de Projetos (Explorar)
#### Front-End
- React / Vue / Angular
- Mobile (React Native, Flutter)
- UI/UX Design
- Testes Front-End

#### Back-End
- Node.js / Express
- Python / Django / FastAPI
- Java / Spring Boot
- Go / Rust
- Testes Back-End

#### Full Stack
- MERN Stack
- MEAN Stack
- Django + React
- Outros Full Stack

#### Banco de Dados
- SQL (PostgreSQL, MySQL)
- NoSQL (MongoDB, Firebase)
- Cache (Redis)
- Data Warehousing

#### Segurança
- Segurança de Aplicações
- Infraestrutura e DevOps
- Criptografia
- Segurança em Nuvem

#### Outras Áreas
- DevOps / CI-CD
- Cloud Computing
- Machine Learning / IA
- Arquitetura de Software

---

## Fluxos de Usuário Principais

### Fluxo 1: Descobrir e Entrar em uma Comunidade
1. Usuário abre "Explorar"
2. Seleciona uma categoria (ex: React)
3. Vê lista de comunidades disponíveis
4. Toca em uma comunidade
5. Visualiza detalhes (descrição, membros, privacidade)
6. Clica "Entrar" (se pública) ou "Solicitar Acesso" (se privada)
7. Entra na comunidade

### Fluxo 2: Criar uma Comunidade
1. Usuário vai para "Minhas Comunidades"
2. Clica no botão "+"
3. Preenche nome, descrição, categoria
4. Define privacidade (pública/privada)
5. Cria a comunidade
6. Aparece como admin automaticamente

### Fluxo 3: Iniciar Videoconferência
1. Usuário entra em uma comunidade
2. Clica no botão "Iniciar Reunião" ou "Entrar na Reunião"
3. Abre a interface do Jitsi Meet
4. Participa da videoconferência

### Fluxo 4: Gerenciar Comunidade (Admin)
1. Admin acessa a comunidade
2. Clica em "Configurações" (ícone de engrenagem)
3. Pode:
   - Editar nome/descrição
   - Gerenciar membros (ver lista, expulsar)
   - Transferir admin para outro membro
   - Remover admin de si mesmo
   - Alterar privacidade

---

## Estrutura de Telas Detalhada

### Home Screen
- **Header**: Logo IT Connect + ícone de notificações
- **Conteúdo Principal**:
  - Seção "Comunidades Recentes" (cards horizontais)
  - Seção "Recomendado para Você" (baseado em categorias de interesse)
  - Seção "Trending" (comunidades mais ativas)
- **Ações**: Botão flutuante para criar comunidade

### Explorar Screen
- **Header**: Barra de busca
- **Categorias**: Grid de categorias (Front-End, Back-End, etc.)
- **Conteúdo**: Lista de comunidades filtradas por categoria
- **Card de Comunidade**:
  - Ícone/cor da categoria
  - Nome da comunidade
  - Número de membros
  - Status (pública/privada)
  - Botão "Entrar" ou "Ver Detalhes"

### Detalhes da Comunidade
- **Header**: Nome da comunidade + ícone de menu
- **Seção de Informações**:
  - Descrição
  - Categoria
  - Número de membros
  - Status (pública/privada)
- **Seção de Ações**:
  - Botão "Entrar" / "Sair"
  - Botão "Iniciar Reunião" (se membro)
  - Botão "Solicitar Acesso" (se privada e não membro)
- **Membros**: Lista de membros com badges de admin
- **Configurações** (apenas para admin): Ícone de engrenagem

### Minhas Comunidades
- **Header**: "Minhas Comunidades" + botão "+"
- **Conteúdo**: Lista de comunidades do usuário
- **Card de Comunidade**:
  - Nome
  - Número de membros
  - Status (admin/membro)
  - Botão "Entrar" / "Configurações" (se admin)

### Painel de Admin
- **Abas**:
  - Informações: Editar nome, descrição, categoria, privacidade
  - Membros: Lista de membros com opções para remover ou promover a admin
  - Configurações: Outras opções (deletar comunidade, etc.)

### Tela de Videoconferência (Jitsi Meet)
- **Interface Jitsi**: Integrada nativamente
- **Controles**: Câmera, microfone, compartilhamento de tela
- **Participantes**: Lista de participantes
- **Chat**: Chat integrado (se disponível no Jitsi)

### Perfil do Usuário
- **Informações Pessoais**: Nome, email, foto de perfil
- **Preferências**: Categorias de interesse
- **Configurações**: Notificações, privacidade, tema (claro/escuro)
- **Logout**: Botão para sair

---

## Paleta de Cores

| Elemento | Cor | Uso |
|----------|-----|-----|
| Primary | #0a7ea4 (Azul) | Botões, links, destaques |
| Background | #ffffff (Branco) | Fundo principal (light mode) |
| Background Dark | #151718 (Cinza muito escuro) | Fundo principal (dark mode) |
| Surface | #f5f5f5 (Cinza claro) | Cards, superfícies elevadas (light mode) |
| Surface Dark | #1e2022 (Cinza escuro) | Cards, superfícies elevadas (dark mode) |
| Foreground | #11181C (Cinza muito escuro) | Texto principal (light mode) |
| Foreground Dark | #ECEDEE (Cinza claro) | Texto principal (dark mode) |
| Muted | #687076 (Cinza médio) | Texto secundário (light mode) |
| Muted Dark | #9BA1A6 (Cinza médio claro) | Texto secundário (dark mode) |
| Border | #E5E7EB (Cinza muito claro) | Bordas (light mode) |
| Border Dark | #334155 (Cinza escuro) | Bordas (dark mode) |
| Success | #22C55E (Verde) | Estados de sucesso |
| Warning | #F59E0B (Laranja) | Estados de aviso |
| Error | #EF4444 (Vermelho) | Estados de erro |

### Cores de Categoria
- **React**: #61DAFB (Azul claro)
- **Vue**: #4FC08D (Verde)
- **Angular**: #DD0031 (Vermelho)
- **Node.js**: #68A063 (Verde escuro)
- **Python**: #3776AB (Azul)
- **Java**: #007396 (Azul escuro)
- **PostgreSQL**: #336791 (Azul)
- **MongoDB**: #13AA52 (Verde)
- **DevOps**: #FF6B6B (Vermelho)
- **Machine Learning**: #FF9500 (Laranja)

---

## Componentes Reutilizáveis

1. **Card de Comunidade**: Exibe nome, membros, categoria, privacidade
2. **Card de Categoria**: Ícone + nome com cor específica
3. **Avatar de Usuário**: Foto redonda com iniciais como fallback
4. **Botão Primário**: Azul com texto branco
5. **Botão Secundário**: Fundo transparente com borda
6. **Badge**: Pequeno indicador (admin, novo, trending)
7. **Lista de Membros**: Avatar + nome + badge de admin
8. **Header Customizado**: Logo + título + ações

---

## Padrões de Interação

- **Tap**: Abrir detalhes, entrar em comunidade, iniciar reunião
- **Long Press**: Mostrar menu de contexto (copiar link, compartilhar, etc.)
- **Swipe**: Navegar entre abas, descartar cards
- **Pull to Refresh**: Atualizar lista de comunidades
- **Haptic Feedback**: Ao criar comunidade, entrar em reunião, promover admin

---

## Considerações de UX

- **Uma mão**: Todos os botões principais no terço inferior da tela
- **Acessibilidade**: Contraste adequado, tamanhos de toque mínimos de 44x44pt
- **Performance**: Lazy loading de listas, cache de imagens
- **Offline**: Mostrar comunidades em cache quando offline
- **Notificações**: Alertas para novas mensagens, convites, mudanças de comunidade

---

## Próximas Fases de Design

1. Wireframes detalhados das telas principais
2. Prototipagem interativa
3. Testes de usabilidade
4. Refinamento baseado em feedback
