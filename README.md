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

**Dá para usar, e o formato do arquivo não muda mais.** A 0.12 tem tudo o que estava planejado
para a v1.0: o formato está congelado e documentado, e todo banco criado desde a 0.3 vai abrir em
todas as versões 1.x. A partir da 0.12, uma versão antiga que recebe um banco mais novo o recusa sem tocar nele. Ela
ainda sai como pré-lançamento, antes do selo de v1.0.

## O que já tem

- **SQL do dia a dia:** CREATE TABLE/INDEX, INSERT, UPDATE, DELETE e SELECT com WHERE, ORDER BY e LIMIT, com os tipos e as conversões do SQLite.
- **JOINs, agregação e subconsultas:** `JOIN ... ON`, `CROSS JOIN`, `count`/`sum`/`avg`/`min`/`max`/`group_concat`, GROUP BY, HAVING, DISTINCT, `IN (SELECT ...)`, `EXISTS`.
- **`CASE`, `CAST` e funções de texto:** `substr`, `replace`, `trim`, `ltrim` e `rtrim`, com os detalhes do SQLite.
- **Restrições:** `NOT NULL`, `DEFAULT`, `UNIQUE`, `PRIMARY KEY` e `INTEGER PRIMARY KEY` (o rowid como coluna), com `PRAGMA table_info`.
- **Índices e planner**, com `EXPLAIN` e `EXPLAIN QUERY PLAN`.
- **Transações duráveis:** commit atômico mesmo com queda de energia ou `kill -9`.
- **Concorrência:** várias conexões, com locks e modo WAL.
- **Comandos preparados** (`prepare`/`bind`/`step`, com `?`, `?N` e `:nome`), recompilados sozinhos quando o schema muda, e códigos de erro com os números do SQLite.
- **Formato do arquivo congelado**, documentado byte a byte e com política de compatibilidade.
- **API C** no formato da API C do SQLite (ainda não distribuída nas releases, que trazem só o `edsqlite` de linha de comando).
- **Conferido contra o SQLite 3.53** em mais de 8 milhões de consultas geradas e em 6106 consultas do corpus oficial *sqllogictest*, sem divergência.
- **Testado com queda de energia** em toda a suíte (uns 5800 crashes simulados por execução), fuzzing noturno e 94,7% das linhas cobertas.

## Instalação

Baixe o `.tar.gz` do seu sistema na [última release](https://github.com/edersonmelo/edsqlite-releases/releases/latest):

| Sistema | Arquivo |
|---------|---------|
| macOS, Apple Silicon | `edsqlite-<versão>-macos-arm64.tar.gz` |
| macOS, Intel | `edsqlite-<versão>-macos-x86_64.tar.gz` |
| Linux, x86_64 (estático) | `edsqlite-<versão>-linux-x86_64.tar.gz` |
| Linux, ARM64 (estático) | `edsqlite-<versão>-linux-arm64.tar.gz` |

```bash
tar xzf edsqlite-0.12.0-macos-arm64.tar.gz
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

A 0.12 fecha tudo o que estava planejado para a **v1.0**. O próximo passo é um **driver JDBC**,
para abrir bancos EDSQLite em ferramentas como o DBeaver. Depois vêm `CHECK` e chaves
estrangeiras, `LEFT JOIN`, `UNION`, views, `ALTER TABLE`, mais funções (`instr`, `printf`, datas)
e locks no Windows. O roadmap
completo está na [página](https://edersonmelo.github.io/edsqlite-releases/#roadmap).

---

Desenvolvido por Ederson Melo · [edgo.app.br](https://edgo.app.br)
