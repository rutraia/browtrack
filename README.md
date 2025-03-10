# BrowTrack

Sistema de gerenciamento de clientes, fichas de anamnese e agendamentos para profissionais de estética.

## Funcionalidades

- **Gerenciamento de Clientes**: Cadastro, edição e visualização de informações de clientes.
- **Fichas de Anamnese**: Criação e gerenciamento de fichas de anamnese para cada cliente.
- **Agendamentos**: Sistema de agendamento com visualização em calendário.
- **Impressão de Fichas**: Geração de PDFs com informações do cliente e ficha de anamnese.
- **Autenticação**: Sistema de login seguro com Supabase.

## Tecnologias Utilizadas

- **Frontend**: React, TypeScript, Vite
- **UI**: Shadcn/UI, Tailwind CSS
- **Banco de Dados**: Supabase (PostgreSQL)
- **Autenticação**: Supabase Auth
- **PDF**: jsPDF para geração de documentos
- **Animações**: Transições suaves no calendário e componentes

## Instalação

1. Clone o repositório:
```bash
git clone https://github.com/rutraia/browtrack.git
cd browtrack
```

2. Instale as dependências:
```bash
npm install
```

3. Configure as variáveis de ambiente:
Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:
```
VITE_SUPABASE_URL=sua_url_do_supabase
VITE_SUPABASE_ANON_KEY=sua_chave_anonima_do_supabase
```

4. Inicie o servidor de desenvolvimento:
```bash
npm run dev
```

## Configuração do Banco de Dados

O projeto utiliza Supabase como backend. É necessário criar as seguintes tabelas:

### Tabela de Clientes
```sql
CREATE TABLE IF NOT EXISTS clientes (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  nome VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  telefone VARCHAR(20) NOT NULL,
  endereco TEXT,
  observacoes TEXT,
  ultimo_atendimento TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  user_id UUID REFERENCES auth.users(id)
);
```

### Tabela de Anamnese
```sql
CREATE TABLE IF NOT EXISTS anamnese (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  cliente_id UUID NOT NULL REFERENCES clientes(id) ON DELETE CASCADE,
  tipo_pele VARCHAR(50),
  sensibilidade VARCHAR(50),
  oleosidade VARCHAR(50),
  espessura VARCHAR(50),
  textura VARCHAR(50),
  tonus VARCHAR(50),
  acne BOOLEAN DEFAULT FALSE,
  manchas BOOLEAN DEFAULT FALSE,
  melasma BOOLEAN DEFAULT FALSE,
  protetor_solar BOOLEAN DEFAULT FALSE,
  alergias TEXT,
  medicamentos TEXT,
  tratamentos_anteriores TEXT,
  reacoes_tratamentos TEXT,
  expectativas_tratamento TEXT,
  observacoes TEXT,
  data_avaliacao TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Tabela de Agendamentos
```sql
CREATE TABLE IF NOT EXISTS agendamentos (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  cliente_id UUID NOT NULL REFERENCES clientes(id) ON DELETE CASCADE,
  data DATE NOT NULL,
  horario VARCHAR(5) NOT NULL,
  servico VARCHAR(100) NOT NULL,
  duracao VARCHAR(10) NOT NULL,
  observacoes TEXT,
  status VARCHAR(20) NOT NULL DEFAULT 'agendado',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

## Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo LICENSE para mais detalhes.