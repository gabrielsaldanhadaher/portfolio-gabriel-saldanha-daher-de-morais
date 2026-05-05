# 🗄️ Otimizações de Banco de Dados

## Índices Recomendados

```sql
-- Índices para melhorar performance de queries

-- Communities
CREATE INDEX idx_communities_categoryId ON communities(categoryId);
CREATE INDEX idx_communities_creatorId ON communities(creatorId);
CREATE INDEX idx_communities_isPublic ON communities(isPublic);

-- Community Members
CREATE INDEX idx_communityMembers_communityId ON communityMembers(communityId);
CREATE INDEX idx_communityMembers_userId ON communityMembers(userId);
CREATE INDEX idx_communityMembers_isAdmin ON communityMembers(isAdmin);
CREATE UNIQUE INDEX idx_communityMembers_unique ON communityMembers(communityId, userId);

-- Access Requests
CREATE INDEX idx_accessRequests_communityId ON accessRequests(communityId);
CREATE INDEX idx_accessRequests_userId ON accessRequests(userId);
CREATE INDEX idx_accessRequests_status ON accessRequests(status);

-- Video Meetings
CREATE INDEX idx_videoMeetings_communityId ON videoMeetings(communityId);
CREATE INDEX idx_videoMeetings_isActive ON videoMeetings(isActive);
CREATE INDEX idx_videoMeetings_startedAt ON videoMeetings(startedAt);

-- Categories
CREATE INDEX idx_categories_parentId ON categories(parentId);
CREATE INDEX idx_categories_slug ON categories(slug);

-- Users
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_lastSignedIn ON users(lastSignedIn);
```

## Otimizações de Schema

### 1. Soft Delete
Adicionar coluna `deletedAt` para soft delete:

```sql
ALTER TABLE communities ADD COLUMN deletedAt TIMESTAMP NULL;
ALTER TABLE communityMembers ADD COLUMN deletedAt TIMESTAMP NULL;
ALTER TABLE videoMeetings ADD COLUMN deletedAt TIMESTAMP NULL;
```

### 2. Audit Log
Criar tabela para rastrear mudanças:

```sql
CREATE TABLE auditLogs (
  id INT AUTO_INCREMENT PRIMARY KEY,
  entityType VARCHAR(50) NOT NULL,
  entityId INT NOT NULL,
  action VARCHAR(20) NOT NULL,
  userId INT NOT NULL,
  changes JSON,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  INDEX idx_entityType (entityType),
  INDEX idx_userId (userId)
);
```

### 3. Cache Table
Tabela para cache de dados frequentes:

```sql
CREATE TABLE cache (
  key VARCHAR(255) PRIMARY KEY,
  value LONGTEXT NOT NULL,
  expiresAt TIMESTAMP,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  INDEX idx_expiresAt (expiresAt)
);
```

## Queries Otimizadas

### Listar comunidades com membros
```sql
SELECT 
  c.*,
  COUNT(cm.id) as memberCount,
  cat.name as categoryName
FROM communities c
LEFT JOIN communityMembers cm ON c.id = cm.communityId AND cm.deletedAt IS NULL
LEFT JOIN categories cat ON c.categoryId = cat.id
WHERE c.deletedAt IS NULL
GROUP BY c.id
ORDER BY c.createdAt DESC
LIMIT 20 OFFSET 0;
```

### Buscar comunidades por categoria
```sql
SELECT c.*, COUNT(cm.id) as memberCount
FROM communities c
LEFT JOIN communityMembers cm ON c.id = cm.communityId AND cm.deletedAt IS NULL
WHERE c.categoryId = ? 
  AND c.isPublic = true 
  AND c.deletedAt IS NULL
GROUP BY c.id
ORDER BY c.createdAt DESC
LIMIT 20;
```

### Verificar se usuário é membro
```sql
SELECT EXISTS(
  SELECT 1 FROM communityMembers 
  WHERE communityId = ? 
    AND userId = ? 
    AND deletedAt IS NULL
) as isMember;
```

## Paginação

Implementar paginação em todas as queries que retornam listas:

```typescript
interface PaginationParams {
  page: number;
  limit: number;
}

function getPaginationOffset(page: number, limit: number): number {
  return (page - 1) * limit;
}
```

## Cache Strategy

### Cache Keys
- `categories:all` - Todas as categorias (TTL: 1 hora)
- `categories:${id}` - Categoria específica (TTL: 1 hora)
- `communities:${categoryId}` - Comunidades por categoria (TTL: 5 minutos)
- `community:${id}` - Comunidade específica (TTL: 5 minutos)
- `members:${communityId}` - Membros da comunidade (TTL: 5 minutos)

### Invalidação de Cache
- Quando comunidade é criada/atualizada: invalidar `communities:*` e `categories:*`
- Quando membro é adicionado/removido: invalidar `members:${communityId}`
- Quando reunião é criada: invalidar `meetings:${communityId}`

## Monitoring

### Queries Lentas
Habilitar slow query log:

```sql
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 2;
```

### Índices Não Utilizados
```sql
SELECT * FROM performance_schema.table_io_waits_summary_by_index_usage
WHERE OBJECT_SCHEMA != 'mysql'
ORDER BY COUNT_READ, COUNT_WRITE;
```

## Backup Strategy

### Backup Automático
- Diário às 2 AM UTC
- Retenção: 30 dias
- Armazenamento: S3

### Restore Procedure
```sql
-- Restaurar de backup
RESTORE FROM 's3://backups/db-backup-2026-05-05.sql';
```

## Performance Targets

| Métrica | Target | Atual |
|---------|--------|-------|
| Query Time (P95) | <100ms | ? |
| Query Time (P99) | <500ms | ? |
| Cache Hit Rate | >80% | ? |
| DB Connection Pool | <10 | ? |
| Replication Lag | <1s | ? |

