# 🏆 Sistema Polo Premiação Alunos - PoloCoin 🪙

Um sistema web completo de gestão escolar, engajamento e gamificação desenvolvido para a **Escola Estadual Unidade Polo Arapongas**. O **PoloCoin** permite a gestão de turmas e ocorrências, pré-conselho de classe, engajamento por meio de pontuação pedagógica e resgate de recompensas em uma loja virtual interna.

---

## 📌 Sumário

* [Funcionalidades Principais](https://www.google.com/search?q=%2523-funcionalidades-principais&utm_source=gemini)
* [Perfis de Acesso](https://www.google.com/search?q=%2523-perfis-de-acesso&utm_source=gemini)
* [Tecnologias Utilizadas](https://www.google.com/search?q=%2523-tecnologias-utilizadas&utm_source=gemini)
* [Estrutura do Banco de Dados (Supabase)](https://www.google.com/search?q=%2523-estrutura-do-banco-de-dados-supabase&utm_source=gemini)
* [Como Executar o Projeto](https://www.google.com/search?q=%2523-como-executar-o-projeto&utm_source=gemini)
* [Configuração do Supabase](https://www.google.com/search?q=%2523-configura%25C3%25A7%25C3%25A3o-do-supabase&utm_source=gemini)
* [Estrutura do Código](https://www.google.com/search?q=%2523-estrutura-do-c%25C3%25B3digo&utm_source=gemini)

---

## 🚀 Funcionalidades Principais

* **Gamificação Escolar (PoloCoins):** Sistema de pontuação para recompensar comportamentos positivos e gerenciar ocorrências disciplinares.
* **Pré-Conselho de Classe Eletrônico:** Módulo dedicado para compilação e impressão de ocorrências negativas por turma/período e registro de observações pedagógicas.
* **Ciência do Responsável (Pai/Mãe):** Notificação automática e sistema de assinatura/ciência digital para pais sobre ocorrências disciplinares dos filhos.
* **Loja de Recompensas Virtual:** Alunos podem trocar seus pontos acoplados por benefícios e itens cadastrados pela equipe gestora.
* **Personalização de Perfil:** Escolha de avatares interativos via emojis para os alunos.
* **Filtros e Relatórios Dinâmicos:** Consulta de histórico detalhado, relatórios em formato imprimível e dashboards por perfil.

---

## 👥 Perfis de Acesso

| Perfil | Permissões e Ações |
| --- | --- |
| **👑 Administrador (ADM)** | • Cadastro e gerenciamento de professores e vínculo com turmas.<br>

<br>• Cadastro de turmas, alunos e credenciais dos pais.<br>

<br>• Gerenciamento de itens da Loja de Recompensas.<br>

<br>• Controle e entrega dos pedidos resgatados.<br>

<br>• Edição completa dos dados dos alunos. |
| **👨‍🏫 Professor** | • Lançamento rápido de pontos positivos (participação, tarefas, respeito, etc.).<br>

<br>• Registro de descontos e ocorrências personalizadas.<br>

<br>• Gestão de pré-conselho de classe e emissão de relatórios de turmas para impressão.<br>

<br>• Consulta ao painel geral de relatórios e histórico de ocorrências lançadas.<br>

<br>• Edição de perfil e turmas lecionadas. |
| **🎓 Aluno** | • Acompanhamento em tempo real da pontuação total acumulada.<br>

<br>• Consulta ao histórico de extrato de pontos e ocorrências.<br>

<br>• Acesso à Loja de Recompensas para efetuar resgates.<br>

<br>• Personalização do avatar do perfil. |
| **👨‍👩‍👧 Pai / Responsável** | • Visualização da pontuação e histórico escolar do filho.<br>

<br>• Recebimento de alertas em modal para ocorrências pendentes.<br>

<br>• Confirmação digital de ciência nas ocorrências disciplinares. |

---

## 🛠️ Tecnologias Utilizadas

* **Frontend:** HTML5, CSS3 (Design responsivo, variáveis CSS, Flexbox, CSS Grid) e JavaScript puro (ES6+ Vanilla JS).
* **Backend as a Service (BaaS):** [Supabase](https://www.google.com/search?q=https://supabase.com/&utm_source=gemini) (PostgreSQL + Realtime JS Client).
* **Tipografia:** Google Fonts (*Inter* e *Plus Jakarta Sans*).

---

## 🗄️ Estrutura do Banco de Dados (Supabase)

O projeto se conecta ao Supabase utilizando o cliente JavaScript `@supabase/supabase-js`. As tabelas necessárias no PostgreSQL são:

### 1. `profiles`

Armazena as contas de administradores do sistema.

```sql
CREATE TABLE profiles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    username TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL,
    name TEXT NOT NULL,
    role TEXT NOT NULL DEFAULT 'admin'
);

```

### 2. `teachers`

Armazena o cadastro dos professores e as turmas atribuídas.

```sql
CREATE TABLE teachers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    full_name TEXT NOT NULL,
    username TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL,
    assigned_classes TEXT[] DEFAULT '{}'
);

```

### 3. `students`

Armazena os dados dos alunos, senhas de acesso, pontos e o JSON de ocorrências.

```sql
CREATE TABLE students (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    registration_number TEXT NOT NULL UNIQUE, -- RA do Aluno
    full_name TEXT NOT NULL,
    student_password TEXT DEFAULT '123',
    parent_username TEXT,
    parent_password TEXT,
    grade_level TEXT NOT NULL, -- Turma (ex: 8º Ano B)
    total_points INT DEFAULT 0,
    avatar TEXT DEFAULT '🐣',
    incidents_json JSONB DEFAULT '[]'::jsonb
);

```

### 4. `products`

Itens e benefícios disponíveis para resgate na loja.

```sql
CREATE TABLE products (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    points INT NOT NULL
);

```

### 5. `orders`

Registro dos resgates efetuados pelos alunos.

```sql
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES students(id) ON DELETE CASCADE,
    student_name TEXT NOT NULL,
    student_grade TEXT NOT NULL,
    product_name TEXT NOT NULL,
    points_spent INT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    delivered BOOLEAN DEFAULT FALSE
);

```

---

## 💻 Como Executar o Projeto

1. **Clonar o Repositório:**
```bash
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio

```


2. **Abrir o Projeto:**
Como o frontend é construído em HTML, CSS e JavaScript puros, basta abrir o arquivo `index.html` (ou renomear a versão desejada para `index.html`) diretamente no seu navegador web ou utilizar a extensão **Live Server** no VS Code.

---

## ⚙️ Configuração do Supabase

No arquivo HTML/JS do projeto, certifique-se de configurar a URL e a Chave Pública Anon (`SUPABASE_ANON_KEY`) de seu projeto no Supabase:

```javascript
const SUPABASE_URL = 'HTTPS_DA_SUA_INSTANCIA_SUPABASE';
const SUPABASE_ANON_KEY = 'SUA_CHAVE_PUBLICA_ANON';
const supabaseClient = supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

```

---

## 📄 Licença e Créditos

Desenvolvido pelas turmas de Desenvolvimento de Sistemas com apoio do **Prof. Robson Serpe** para a **Escola Estadual Unidade Polo Arapongas** - © 2026. Sistema de Gestão Escolar e Premiações.
