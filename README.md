# Dain Blog

Fonte do meu blog pessoal, feito com Hugo e Hextra.

## Rodar localmente

É necessário ter o Hugo Extended 0.146 ou mais recente.

```bash
hugo server -D
```

## Criar um post

```bash
hugo new content posts/nome-do-post/index.md
```

Edite o arquivo criado e troque `draft: true` por `draft: false` quando terminar. Para publicar uma versão em inglês, crie `index.en.md` na mesma pasta e use o mesmo `translationKey`.

## Publicar

```bash
git add .
git commit -m "Adiciona novo post"
git push
```

O GitHub Actions valida, gera e publica o site no GitHub Pages.

