# 📈 Meu Progresso

**▶️ Acesse: https://mrxavierdev.github.io/CRUD_Meu_Progresso/**

Acompanhe os **livros, jogos, cursos de programação, mangás e animes** que você está consumindo.

- **Cada usuário tem a própria lista**: crie sua conta com e-mail e senha e só você vê os seus itens
- **Tela inicial** apresentando o app para quem ainda não entrou, com "Criar conta grátis" e "Já tenho conta"

- **Uma aba e um visual para cada assunto**:
  - 🗂️ **Todos**: quadro com 3 colunas (Na fila → Em andamento → Concluídos)
  - 📚 **Livros**: estante com as capas nas prateleiras (Lendo / Quero ler / Lidos)
  - 🎮 **Jogos**: backlog escuro com barra de XP (Jogando / Backlog / Zerados)
  - 💻 **Cursos**: janela de terminal com `[x]` e barra de progresso em texto
  - 📖 **Mangás**: painéis de quadrinho com balão de fala e selo "FIM!"
  - 📺 **Animes**: fileiras de miniaturas no estilo streaming, com ▶ e barra de "assistido"
- **Resumo no topo** de cada aba: quantos já foram concluídos, com barra de progresso
- **+ Adicionar / ✏️ Editar** em uma janela (já vem com o tipo da aba aberta)
- **▶ Começar** move um item da fila para "em andamento"; **+10%** avança o progresso e, em 100%, conclui
- **Buscar** por título ou comentário

Tudo fica em um único arquivo: `index.html` (HTML + CSS + JavaScript).

## Requisitos do trabalho

| Requisito | Como foi feito |
|---|---|
| Criar projeto no Firebase | Projeto `projeto-ff561` |
| Vincular app no projeto | App da Web registrado no projeto |
| Configuração do SDK | Objeto `firebaseConfig` no `index.html` |
| Autenticação / método de login | **E-mail e senha** (Firebase Authentication), com "Criar conta" na própria tela |
| Banco de dados | **Cloud Firestore**, coleção `itens` |
| Regras de acesso | `firestore.rules`: cada usuário só lê, cria, edita e exclui os próprios itens; campos validados |
| CRUD local → nuvem | 1º commit salva no `localStorage`; 2º commit troca para o Firestore (ver abaixo) |
| Disponibilizar no GitHub | Este repositório + GitHub Pages |

## Do armazenamento local para a nuvem

Só o bloco **BANCO DE DADOS** no início do `<script>` mudou. O resto da tela continua igual:

| Operação | Antes (localStorage) | Depois (Firestore) |
|---|---|---|
| Create | `lista.push(...)` + `localStorage.setItem` | `addDoc(colecao, dados)` |
| Read | `JSON.parse(localStorage.getItem(...))` | `onSnapshot(query(colecao, orderBy("criadoEm")))` |
| Update | troca o item na lista + `setItem` | `updateDoc(doc(db, "itens", id), dados)` |
| Delete | `filter(...)` + `setItem` | `deleteDoc(doc(db, "itens", id))` |
| Login | não tinha | `signInWithEmailAndPassword` / `createUserWithEmailAndPassword` |

## Cada item no Firestore

```
itens/{id}
  titulo: "Código Limpo"
  tipo: "livro" | "jogo" | "curso" | "manga" | "anime"
  status: "fila" | "andamento" | "concluido"
  progresso: 0 a 100
  nota: 0 a 5
  comentario: "..."
  uid: id do usuário que criou
  autor: nome exibido
  criadoEm / atualizadoEm: data do servidor
```
