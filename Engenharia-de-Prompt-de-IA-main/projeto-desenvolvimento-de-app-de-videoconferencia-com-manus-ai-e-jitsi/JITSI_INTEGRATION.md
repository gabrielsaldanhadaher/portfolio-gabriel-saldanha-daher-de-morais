# Integração com Jitsi Meet - IT Connect

## 📹 Visão Geral

O IT Connect integra o **Jitsi Meet** para fornecer videoconferência em tempo real nas comunidades. A integração permite que os usuários iniciem e participem de reuniões de vídeo diretamente dentro da plataforma.

## 🏗️ Arquitetura da Integração

### Componentes

1. **Tela de Videoconferência** (`app/video-meeting.tsx`)
   - Interface para iniciar/encerrar reuniões
   - Controles de câmera, microfone e compartilhamento de tela
   - Integração com Jitsi Meet

2. **Backend** (`server/routers.ts` e `server/db.ts`)
   - Rotas tRPC para gerenciar reuniões
   - Persistência de dados de reuniões no banco de dados
   - Histórico de reuniões

3. **Banco de Dados** (`drizzle/schema.ts`)
   - Tabela `videoMeetings` para armazenar informações de reuniões
   - Campos: id, communityId, jitsiRoomName, createdAt, endedAt, isActive

## 🚀 Como Funciona

### 1. Iniciar uma Reunião

```typescript
// Usuário clica em "Iniciar Reunião"
const handleStartMeeting = () => {
  const roomName = `community-${cId}-${Date.now()}`;
  
  createMeetingMutation.mutate({
    communityId: cId,
    jitsiRoomName: roomName,
  });
};
```

**Fluxo:**
1. Gera um nome único para a sala Jitsi
2. Cria registro no banco de dados
3. Abre Jitsi Meet em nova aba (web) ou integrado (mobile)

### 2. Participar de uma Reunião

```typescript
// Usuário acessa a tela de vídeo da comunidade
// Se houver reunião ativa, pode participar clicando no link
```

### 3. Encerrar uma Reunião

```typescript
// Apenas admins podem encerrar
const handleEndMeeting = () => {
  endMeetingMutation.mutate({ meetingId: activeMeeting.id });
};
```

## 🔗 URLs do Jitsi Meet

O Jitsi Meet é acessado através de:

```
https://meet.jit.si/{roomName}
```

**Exemplos:**
- `https://meet.jit.si/community-123-1715000000000`
- `https://meet.jit.si/itconnect-frontend-react`

## 🎮 Controles Disponíveis

| Controle | Descrição |
|----------|-----------|
| 🎤 Microfone | Ativar/desativar áudio |
| 📹 Câmera | Ativar/desativar vídeo |
| 📺 Compartilhamento | Compartilhar tela |
| 💬 Chat | Chat integrado do Jitsi |

## 📊 Dados de Reunião

### Estrutura no Banco de Dados

```typescript
interface VideoMeeting {
  id: number;
  communityId: number;
  jitsiRoomName: string;
  createdAt: Date;
  endedAt: Date | null;
  isActive: boolean;
}
```

### Exemplo de Registro

```json
{
  "id": 1,
  "communityId": 5,
  "jitsiRoomName": "community-5-1715000000000",
  "createdAt": "2026-05-05T00:00:00Z",
  "endedAt": null,
  "isActive": true
}
```

## 🔐 Segurança

### Controle de Acesso

- **Iniciar reunião**: Apenas membros da comunidade
- **Participar**: Membros públicos (comunidades públicas) ou membros aprovados (comunidades privadas)
- **Encerrar**: Apenas admins da comunidade

### Validações

```typescript
// Verificar se usuário é membro
const isMember = await db.isCommunityMember(communityId, userId);

// Verificar se usuário é admin
const isAdmin = await db.isCommunityAdmin(communityId, userId);
```

## 📱 Plataformas

### Web
- Abre Jitsi em nova aba
- Integração via iframe (opcional)
- Suporta compartilhamento de tela

### Mobile (Android/iOS)
- Integração com `react-native-jitsi-meet`
- Controles nativos
- Suporta câmera e microfone do dispositivo

## 🛠️ Rotas tRPC

### Criar Reunião

```typescript
trpc.meetings.create.useMutation({
  communityId: number,
  jitsiRoomName: string,
})
```

### Obter Reunião Ativa

```typescript
trpc.meetings.active.useQuery({
  communityId: number,
})
```

### Encerrar Reunião

```typescript
trpc.meetings.end.useMutation({
  meetingId: number,
})
```

## 📈 Histórico de Reuniões

Todas as reuniões são registradas no banco de dados, permitindo:

- Rastreamento de participação
- Relatórios de atividade
- Análise de engajamento
- Auditoria

## 🔄 Fluxo Completo

```
1. Usuário acessa comunidade
   ↓
2. Clica em "Iniciar Reunião"
   ↓
3. Sistema cria sala Jitsi
   ↓
4. Usuário é redirecionado para Jitsi Meet
   ↓
5. Outros membros podem participar
   ↓
6. Admin encerra reunião
   ↓
7. Registro salvo no banco de dados
```

## 🚀 Próximas Melhorias

- [ ] Notificações quando reunião inicia
- [ ] Gravação de reuniões
- [ ] Transcrição automática
- [ ] Integração com calendário
- [ ] Convites personalizados
- [ ] Relatórios de participação

## 📚 Referências

- [Jitsi Meet Documentation](https://jitsi.github.io/handbook/)
- [react-native-jitsi-meet](https://github.com/react-native-jitsi-meet/react-native-jitsi-meet)
- [Jitsi API](https://jitsi.github.io/handbook/docs/dev-guide/dev-guide-iframe)

## 🐛 Troubleshooting

### Reunião não inicia
- Verificar conexão com internet
- Validar nome da sala
- Verificar permissões de câmera/microfone

### Jitsi não abre
- Verificar se URL está correta
- Verificar bloqueio de pop-ups
- Tentar em navegador diferente

### Áudio/vídeo não funciona
- Verificar permissões do navegador
- Testar câmera/microfone em outro app
- Reiniciar navegador

---

**Desenvolvido com ❤️ para IT Connect**
