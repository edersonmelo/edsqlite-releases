<p align="center">
  <img src="icon.svg" width="112" alt="Ícone do EDSQLite">
</p>

<h1 align="center">EDSQLite</h1>

<p align="center">
  Banco de dados relacional embutido, inspirado no SQLite, escrito em Rust do zero.<br>
  <a href="https://github.com/edersonmelo/edsqlite-releases/releases/latest"><b>Baixar a última versão</b></a> ·
  <a href="https://edersonmelo.github.io/edsqlite-releases/">Página, status e roadmap</a>
</p>

## É estável?

**Dá para usar, mas ainda não é uma versão estável.** A 0.9 serve para experimentar, aprender e
trabalhar com dados que você pode recriar. Para dados que não podem ser perdidos, espere a v1.0:
até lá, o formato do arquivo ainda pode mudar sem migração.

## O que já tem

- **SQL do dia a dia:** CREATE TABLE/INDEX, INSERT, UPDATE, DELETE e SELECT com WHERE, ORDER BY e LIMIT, com os tipos e as conversões do SQLite.
- **JOINs, agregação e subconsultas:** `JOIN ... ON`, `CROSS JOIN`, `count`/`sum`/`avg`/`min`/`max`/`group_concat`, GROUP BY, HAVING, DISTINCT, `IN (SELECT ...)`, `EXISTS`.
- **Índices e planner**, com `EXPLAIN` e `EXPLAIN QUERY PLAN`.
- **Transações duráveis:** commit atômico mesmo com queda de energia ou `kill -9`.
- **Concorrência:** várias conexões, com locks e modo WAL.
- **Conferido contra o SQLite 3.53** em mais de 8 milhões de consultas geradas, sem divergência.

## Instalação

Baixe o `.tar.gz` do seu sistema na [última release](https://github.com/edersonmelo/edsqlite-releases/releases/latest):

| Sistema | Arquivo |
|---------|---------|
| macOS, Apple Silicon | `edsqlite-<versão>-macos-arm64.tar.gz` |
| macOS, Intel | `edsqlite-<versão>-macos-x86_64.tar.gz` |
| Linux, x86_64 (estático) | `edsqlite-<versão>-linux-x86_64.tar.gz` |
| Linux, ARM64 (estático) | `edsqlite-<versão>-linux-arm64.tar.gz` |

```bash
tar xzf edsqlite-0.9.0-macos-arm64.tar.gz
sudo mv edsqlite /usr/local/bin/
edsqlite --version
```

No macOS, o binário não é assinado pela Apple. Se o sistema bloquear a execução, rode uma vez:

```bash
xattr -d com.apple.quarantine /usr/local/bin/edsqlite
```

O `SHA256SUMS` da release confere os arquivos (`shasum -a 256 -c SHA256SUMS`).

## Uso

```
$ edsqlite loja.db
edsqlite> CREATE TABLE vendas (produto TEXT, qtd INTEGER);
edsqlite> INSERT INTO vendas VALUES ('caneta', 10), ('caneta', 5), ('mochila', 1);
edsqlite> SELECT produto, sum(qtd) FROM vendas GROUP BY produto;
caneta|15
mochila|1
```

`edsqlite` sem argumento usa um banco temporário em memória. Os comandos terminam em `;`.
Comandos do REPL: `.headers on|off`, `.tables`, `.indexes [tabela]`, `.exit`. Para rodar um
script: `edsqlite dados.db < script.sql`.

## Roadmap

A próxima versão é a **v1.0**: formato do arquivo congelado e documentado, API com parâmetros
(`prepare`/`bind`/`step`), fuzzing, códigos de erro estáveis e mais testes. Depois vêm as
restrições (`NOT NULL`, `DEFAULT`, `INTEGER PRIMARY KEY`), `LEFT JOIN`, `UNION`, `CASE`, `CAST`,
`ALTER TABLE`, mais funções (`substr`, `replace`, `trim`, datas) e locks no Windows. O roadmap
completo está na [página](https://edersonmelo.github.io/edsqlite-releases/#roadmap).

---

Desenvolvido por Ederson Melo · [edgo.app.br](https://edgo.app.br)
